# Guía de uso de Pluma

## 1. Ejecutar el editor

Descarga `editor-md.html` desde la raíz del repositorio y ábrelo con doble clic. La aplicación es autónoma: no requiere servidor de desarrollo ni paquetes.

Navegadores recomendados: versiones actuales de Chrome o Edge. Firefox y Safari pueden limitar el guardado directo, algunas operaciones de edición o las conexiones locales. Las políticas de `file://` y de `localStorage` difieren entre navegadores.

## 2. Crear, abrir y guardar

- **Nuevo:** selecciona una de las plantillas: **En blanco**, **Registro de Show & Tell**, **Acta de reunión** o **README de proyecto**.
- **Abrir:** selecciona un `.md`, `.markdown` o `.txt`. También puedes arrastrar un archivo compatible al editor.
- **Pestañas:** cada documento conserva su propio contenido y borrador. Al recuperar una sesión, comprueba que el navegador conserve el almacenamiento del origen.
- **Guardar / Ctrl+S:** utiliza File System Access API si está disponible y hay permiso; de lo contrario, descarga el `.md`. Comprueba la carpeta de descargas si no ves el archivo actualizado.

Se aceptan archivos Markdown de hasta 30 MB en el flujo de apertura; los documentos muy grandes pueden afectar el rendimiento.

## 3. Edición y formatos

**Visual** ofrece una hoja tipo papel y edición WYSIWYG; **Markdown** muestra el texto real; **Dividido** combina ambas superficies.

La cinta permite aplicar títulos H1–H3, negrita, cursiva, tachado, código en línea/bloque, listas numeradas y sin numerar, tareas, citas, tablas, enlaces, imágenes y líneas. Las tareas pueden marcarse; las tablas admiten operaciones sobre filas y columnas.

En Visual, Pluma intenta conservar las líneas Markdown originales que no has cambiado (por ejemplo, separadores y espaciado de tablas). Para cambios estructurales que el editor no puede asociar a un bloque anterior, utiliza una conversión de respaldo. **No es una garantía de preservación byte por byte para todos los Markdown posibles.**

## 4. Buscar y reemplazar

Usa `Ctrl+F` o `Ctrl+H`. El panel incluye navegación anterior/siguiente, coincidencia de mayúsculas y opción de expresiones regulares. Comprueba la expresión y las coincidencias antes de reemplazar todas.

## 5. Funciones compatibles con GitHub

- **Front matter YAML:** al comienzo del documento se presenta como un bloque plegable y se conserva al guardar.
- **Alertas:** sintaxis como `> [!NOTE]`, `> [!WARNING]` y `> [!TIP]`.
- **Mermaid:** los bloques cercados con `mermaid` se muestran como código con un aviso; **no se interpretan en diagramas gráficos** sin un motor externo.
- **Código:** coloreado local básico para JavaScript, Python, HTML, CSS, JSON y Bash. Es un tokenizador propio, no un parser completo.

## 6. Exportación

- **Markdown:** el formato fuente editable `.md`.
- **HTML:** documento independiente exportado desde el contenido actual.
- **Word:** archivo `.docx` OpenXML con títulos, listas, tablas, imágenes, estilos básicos, bloques de código y numeración de página en el pie.
- **PDF:** usa **Imprimir** (`Ctrl+P`) y selecciona «Guardar como PDF». La hoja se prepara con CSS de impresión.

Los estilos en Word son básicos y pueden variar ligeramente entre Microsoft Word y LibreOffice.

## 7. Historial

El panel de historial permite ver instantáneas, comparar dos puntos y restaurar una versión. Las versiones se mantienen normalmente en el navegador. La casilla **Guardar historial dentro del archivo** está desactivada de fábrica: si se activa, incorpora datos comprimidos dentro de un comentario `<!-- pluma:history ... -->`; no la actives para repositorios públicos si no necesitas ese contenido.

## 8. Pluma Mesh — colaboración experimental

**Preparación:** todos abren la misma versión de `editor-md.html`. Inicia una sesión sobre un documento que hayas guardado previamente como respaldo.

1. En el primer dispositivo, pulsa **Colaborar**, introduce nombre y selecciona **Crear sala**.
2. Copia el código de invitación completo (puede ser largo) y envíalo a la otra persona.
3. La otra persona abre **Colaborar**, pega la invitación en el área de códigos y la procesa.
4. Copia la **respuesta** generada y envíala a la primera persona.
5. El organizador pega y procesa la respuesta. Espera a que ambos vean el estado **conectado**.
6. Para otras personas, repite la invitación. El límite de sala previsto es de hasta 6 participantes.

El protocolo utiliza `RTCPeerConnection` y `RTCDataChannel`; al crear sala, **STUN está desactivado**. El intercambio de códigos no transfiere el documento a un servidor. Una red local puede requerir ajustes de navegador, firewall o conectividad. Las conexiones por internet pueden necesitar un relay TURN (no incluido), por lo que **no siempre podrán establecerse solo con STUN**.

La edición concurrente utiliza una secuencia CRDT por caracteres. **La reconexión, la sincronización después de periodos desconectados y los cursores remotos no están verificados extremo a extremo**. Conserva copias independientes antes de probarla. Usa **Salir de la sala** para cerrar las conexiones.

## 9. Asistente local con Ollama

Está **desactivado por defecto**. Opcionalmente, instala Ollama en tu propio equipo e inicia un modelo, por ejemplo:

```bash
ollama pull llama3.2
ollama serve
```

Abre el panel **Asistente**, activa la opción local y comprueba la conexión con `http://localhost:11434`. Permite resumir una selección, cambiar tono, proponer títulos o generar un índice. Si falla, el editor sigue funcionando sin IA.

Según navegador y configuración de Ollama, una página abierta como `file://` puede sufrir restricciones CORS/origen. No desactives medidas de seguridad del navegador sin comprender las consecuencias.

## 10. Solución de problemas

| Situación | Qué comprobar |
| --- | --- |
| Guardar descarga otro archivo | El navegador quizá no admite sobrescritura directa. Reemplaza manualmente el archivo anterior |
| Borrador no reaparece | Permisos y política de `localStorage` con `file://`, modo privado o limpieza del navegador |
| Mesh no conecta | Ambas respuestas completas, estado ICE, firewall, IP local, navegación segura y opción STUN |
| Ollama no responde | Servicio iniciado, modelo instalado, puerto 11434 y reglas de CORS |
| DOCX difiere del editor | Fuentes y motores Word/LibreOffice; conserva el Markdown como formato canónico |
| Un diff cambia más líneas de las esperadas | Confirma si editaste estructuras complejas y revisa el diff antes del commit |
