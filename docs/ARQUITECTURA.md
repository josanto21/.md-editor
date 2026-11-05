# Arquitectura y decisiones técnicas

## Objetivo

Pluma concentra interfaz, estilos, renderizado, persistencia, exportadores y módulos opcionales en **`editor-md.html`**. No hay proceso de compilación, framework ni CDN.

## Flujo de datos

```text
         Abrir archivo .md / escribir Markdown
                         |
                         v
                 state.md (canónico)
                    /         \
                   v           v
       Parseo a bloques    Edición Markdown
              |                 |
              v                 |
      Hoja contenteditable      |
              |                 |
              +---- cambios ----+
                         |
                         v
            Borrador / historial / .md
                         |
              +----------+----------+
              |          |          |
           HTML/PDF      DOCX      Mesh
```

**Fuente de verdad:** el Markdown en `state.md`. El modo Visual representa bloques derivados de ese texto. Al editar Visual se intenta aplicar el cambio al fragmento de origen en vez de serializar todo el documento; cuando no es seguro, se hace una conversión de respaldo. Esto reduce ruido en Git, pero no cubre todas las construcciones Markdown.

## Secciones principales del HTML

- **Interfaz y diseño:** CSS con variables de tema, cinta tipo Word, hoja y paneles.
- **Parseador / renderizador:** Markdown a bloques y nodos visuales; tratamiento de enlaces, tablas, listas, tareas, citas, código, YAML y alertas.
- **Conversión Visual → Markdown:** extracción del DOM editable y reconciliación de cambios; conversión completa en casos no compatibles.
- **Archivos y preferencias:** apertura por selector/arrastrar, descarga, File System Access API, `localStorage` bajo `try/catch`.
- **Exportación:** HTML autónomo, DOCX generado en OpenXML con ZIP desde JavaScript y PDF mediante CSS `@media print`.
- **Pestañas e historial:** registro por documento y versiones locales restaurables. Inclusión opcional de historial comprimido en comentarios Markdown.
- **Búsqueda y colores de código:** motor de búsqueda reemplazo y tokenizadores locales sin dependencias.
- **Mesh:** WebRTC DataChannel, señalización manual, estados de pares y CRDT de texto.
- **Asistente:** integración optativa con Ollama en `localhost`.

## Pestañas y recuperación

El programa mantiene pestañas con su propio Markdown, historial de cambios y metadatos. Las estructuras de sesión y preferencias se serializan a `localStorage`. La disponibilidad del almacenamiento depende del navegador, origen y espacio libre; no sustituye guardar el `.md` en disco.

## Historial

Las instantáneas se conservan por defecto localmente. La opción de incrustación utiliza un comentario `pluma:history` con contenido comprimido (LZW y base64). El exportador no lo incluye cuando la casilla está desactivada. Este contenido **no es cifrado**.

## Pluma Mesh: diseño, no promesa de conectividad universal

- La señalización SDP se copia manualmente como invitación y respuesta. No existe backend de señalización.
- Cada conexión utiliza `RTCPeerConnection` + `RTCDataChannel`.
- Hay lógica para una malla de participantes (2 a 6) y difusión entre pares; el organizador coordina invitaciones y descubrimiento de participantes.
- Las operaciones de texto siguen un modelo **RGA/CRDT** por carácter con IDs, orden lógico y marcas de eliminación. Los participantes intercambian operaciones y sincronizan instantáneas de operaciones.
- El estado de presencia y los cursores se transmiten como información auxiliar de colaboración.
- El canal aplica troceado de mensajes para documentos grandes; la sala tiene un límite de aproximadamente 100 000 caracteres.

**Riesgos técnicos:** ICE puede no descubrir pares entre determinados navegadores/redes, la presencia puede desfasarse y la recuperación tras cierres/desconexiones no está demostrada de extremo a extremo. No hay servidor TURN integrado. STUN público es opcional y desactivado de fábrica. La seguridad del canal depende de WebRTC; los datos locales e invitaciones requieren igual cuidado.

## Seguridad y privacidad

- El HTML no incluye scripts externos ni estilos remotos.
- La representación de contenido intenta escapar HTML e impedir esquemas activos, por ejemplo `javascript:`. Esto no sustituye una auditoría especializada de seguridad frente a Markdown malicioso, archivos grandes o participantes no confiables.
- La colaboración transmite el documento **a los participantes** por canales P2P. Por tanto, «sin nube» no significa «sin transferencia de datos».
- Si se activa STUN, un servidor externo recibe metadatos de conectividad (sin el cuerpo del documento).
- Si se activa Ollama, el texto solicitado se envía al proceso local de Ollama. La seguridad de ese servicio y del modelo dependen de su configuración.
- Borradores e historial se almacenan en claro en el almacenamiento del navegador; no hay cifrado local explícito.

## Desarrollo

Puedes editar el propio `editor-md.html` en cualquier editor de texto. Evita extraer dependencias online: conservar la operación offline es un requisito del proyecto. Al cambiar el parser o la conversión visual, añade casos de prueba de **diff mínimo** y **ida y vuelta Markdown–Visual–Markdown**. Al cambiar Mesh, verifica con dos navegadores/PCs reales, pérdida de conexión, reingreso y edición concurrente.

No existe aún un conjunto automatizado de pruebas de regresión integrado en este repositorio.
