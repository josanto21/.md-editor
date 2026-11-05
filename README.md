# Pluma — Editor Markdown offline

**Pluma** es un editor Markdown visual en español, inspirado en la experiencia de escritura de Microsoft Word. Está diseñado para ejecutarse desde **un único archivo HTML**, sin instalación, servidor propio, frameworks ni CDN.

> **Estado del proyecto:** versión funcional en evolución. La edición local es el núcleo del proyecto; **Pluma Mesh** (colaboración WebRTC) se considera **experimental** hasta completar pruebas entre navegadores y equipos reales.

## Comenzar en 30 segundos

1. Descarga [`editor-md.html`](./editor-md.html) (en GitHub, abre el archivo y elige **Download raw**).
2. Ábrelo con doble clic en un navegador moderno, preferentemente Chrome o Edge.
3. Escribe en **Visual**, alterna a **Markdown** o usa **Dividido**.
4. Usa **Guardar** o `Ctrl+S` para obtener tu archivo `.md`.

**No necesitas instalar Node.js, Python ni conectar internet para editar archivos locales.** Algunas API varían según el navegador; si no está disponible el guardado directo, Pluma descarga el archivo.

## Funcionalidades

| Área | Qué ofrece |
| --- | --- |
| Escritura | WYSIWYG con hoja centrada, títulos, estilos, citas, listas anidadas, tareas, enlaces, imágenes, código y tablas |
| Markdown real | Modos Visual / Markdown / Dividido y actualización localizada para reducir cambios innecesarios en diffs |
| Productividad | Buscar y reemplazar, expresiones regulares, pestañas, plantillas, esquema, zoom, atajos y arrastrar y soltar |
| GitHub | Front matter YAML, alertas tipo `[!NOTE]` / `[!WARNING]` / `[!TIP]` y bloques Mermaid mostrados como código |
| Archivos | Abrir y guardar `.md`; exportar a `.html` y `.docx`; PDF mediante impresión |
| Apariencia | Interfaz española, tema claro/oscuro/automático y fuente del documento seleccionable |
| Historial | Versiones locales, comparación y restauración; inclusión opcional del historial en un comentario Markdown |
| Colaboración | **Pluma Mesh (experimental):** códigos manuales, WebRTC DataChannel y sincronización CRDT, con STUN opcional |
| IA | Asistente local opcional mediante Ollama (`http://localhost:11434`); desactivado de fábrica |

El editor está pensado para documentos Markdown habituales. La edición de elementos estructurales complejos puede normalizar parte del formato original. **Revisa el diff de Git antes de confirmar cambios importantes.**

## Atajos principales

| Acción | Atajo |
| --- | --- |
| Negrita / cursiva | `Ctrl+B` / `Ctrl+I` |
| Insertar enlace | `Ctrl+K` |
| Guardar | `Ctrl+S` |
| Deshacer / rehacer | `Ctrl+Z` / `Ctrl+Y` |
| Buscar / reemplazar | `Ctrl+F` / `Ctrl+H` |
| Imprimir / guardar PDF | `Ctrl+P` |

En macOS, algunos atajos pueden depender del navegador.

## Pluma Mesh (experimental)

Dos participantes pueden intentar conectarse sin cuentas ni servidor propio:

1. Quien organiza abre **Colaborar → Crear sala** y copia el código de invitación.
2. La otra persona abre Pluma, entra en **Colaborar**, pega el código y genera la **respuesta**.
3. La respuesta vuelve a quien organiza, que la pega para completar la señalización.
4. Una vez que el navegador indique **conectado**, ambos pueden editar el Markdown.

Para sumar participantes se generan invitaciones adicionales. La topología de malla y la reconexión necesitan pruebas reales antes de considerarse fiables. En una LAN puede funcionar sin internet, según la configuración de WebRTC y red. **STUN público está desactivado por defecto**; al activarlo, ese servidor recibe datos de conectividad (no el documento).

**No se garantiza** conexión en cinco segundos, funcionamiento entre todas las redes ni recuperación perfecta tras desconexiones prolongadas.

## Privacidad y almacenamiento

- Los borradores, pestañas e historial normal se guardan en el `localStorage` del navegador, sujeto a disponibilidad y cuota.
- De forma predeterminada, el Markdown exportado **no contiene datos de Pluma ni historial incrustado**.
- Las colaboraciones usan canales WebRTC directos; los códigos se intercambian manualmente por el medio que elijas.
- La opción STUN realiza una petición externa para descubrir conectividad. Ollama se consulta únicamente en `localhost` cuando lo activas.
- `localStorage` no está cifrado por Pluma: evita equipos compartidos para material sensible.

## Estructura del repositorio

```text
.
├── editor-md.html          # Aplicación completa: HTML, CSS y JavaScript
├── README.md               # Presentación y arranque rápido
├── CHANGELOG.md            # Registro de cambios
├── .gitignore
└── docs/
    ├── GUIA_DE_USO.md      # Guía práctica del usuario
    ├── ARQUITECTURA.md     # Diseño técnico y decisiones
    └── PRUEBAS_Y_LIMITES.md # Verificación y límites conocidos
```

## Documentación

- [Guía de uso](./docs/GUIA_DE_USO.md)
- [Arquitectura](./docs/ARQUITECTURA.md)
- [Pruebas, aceptación y límites](./docs/PRUEBAS_Y_LIMITES.md)
- [Registro de cambios](./CHANGELOG.md)

## Contribuir

Se aceptan informes reproducibles de errores y propuestas mediante *Issues* y *Pull Requests*. En especial, interesa validar WebRTC en dos equipos, la convergencia tras una desconexión y los cambios mínimos en archivos Markdown complejos. Acompaña cada problema con navegador, sistema operativo, pasos y resultado esperado.

**Licencia:** todavía no se ha elegido ni incluido una licencia de distribución. No se debe asumir una licencia de reutilización por el hecho de que el repositorio sea público.
