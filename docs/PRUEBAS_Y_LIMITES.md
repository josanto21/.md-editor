# Pruebas, criterios de aceptación y limitaciones

Este documento distingue **comprobado**, **comprobación previa reportada** y **pendiente**. No debe interpretarse como certificación de calidad de producción.

## Comprobación al publicar esta versión

- **Inspección estática:** el archivo es HTML UTF-8 autónomo, contiene un único bloque `<script>` inline y no declara bibliotecas en etiquetas `script src` ni `link href`.
- **Sintaxis:** el JavaScript extraído del bloque inline pasó `node --check` con Node.js 22.
- **Fuente:** se publica el HTML completo ya elaborado; no se reescribió el editor para crear la documentación.
- **GitHub:** el commit de publicación se contrasta con la existencia del archivo y de los documentos en el repositorio.

## Comprobaciones efectuadas en la iteración anterior

Durante el desarrollo anterior se informó de pruebas sobre:

- Edición localizada de una celda sin modificar separadores Markdown vecinos.
- Buscar/reemplazar, cambio de modos y deshacer/rehacer.
- Marcadores YAML y alertas de GitHub.
- Pestañas y recuperación simulada de borradores.
- Exportación DOCX estructural (XML válido, lectura en LibreOffice, imágenes y pie).
- Renderizado y estilos en pantalla móvil.
- Aplicación de operaciones CRDT concurrentes en una simulación.
- Escapado de algunos casos de HTML y enlaces activos.

**Estas pruebas no se han repetido en todos los navegadores al publicar este commit.** La validación XML y LibreOffice tampoco equivalen a abrir el archivo en Microsoft Word.

## Matriz de aceptación pendiente de completar

| Prueba | Estado | Cómo verificar |
| --- | --- | --- |
| Abrir y volver a guardar `.md` sencillo | Parcial | Comparar archivo antes/después |
| Editar una palabra en tabla sin normalizar otras líneas | Comprobación previa | `git diff --word-diff` y `git diff` |
| Visual → Markdown → Dividido | Comprobación previa | Verificar estilos, listas y tareas |
| Deshacer/rehacer tras cambiar de vista | Comprobación previa | Realizar una secuencia mixta |
| Pestañas + cierre/reapertura del navegador | Parcial | Editar dos documentos, cerrar y recuperar |
| DOCX abierto en Microsoft Word | Pendiente | Abrir, inspeccionar tablas, listas e imágenes |
| PDF imprime solamente la hoja | Comprobación previa | Imprimir a PDF y revisar páginas |
| Dos PCs intercambian código WebRTC | **Pendiente** | Probar en LAN con distinto navegador |
| Presencia/cursor remoto en menos de 5 s | **Pendiente** | Cronometrar después de completar señalización |
| Concurrente en misma línea, documentos idénticos | Simulación previa | Dos sesiones reales escribiendo simultáneamente |
| Desconectar, editar ambos, reconectar y converger | **Pendiente** | Comparar hashes del Markdown final |
| Restaurar instantánea histórica | Parcial | Crear dos versiones y volver a la anterior |
| Markdown exportado sin `pluma:history` por defecto | Implementado, revisar | Buscar marcador en archivo guardado |
| Ollama local en `file://` | **Pendiente** | Activar Ollama y comprobar CORS/localidad |
| Seguridad con Markdown arbitrario hostil | **Pendiente** | Auditoría de inyección y protocolos |

## Limitaciones conocidas

1. La conversión Visual → Markdown puede normalizar estructuras difíciles de identificar; confirma diferencias antes de hacer commits.
2. El funcionamiento de `localStorage` bajo `file://` varía con las políticas del navegador; su cuota puede agotarse.
3. Mesh usa señalización manual. Sin TURN, algunas redes/NAT no permitirán conexión; tampoco se garantiza la convergencia después de una desconexión prolongada sin pruebas adicionales.
4. Los cursores remotos y selecciones dependen del estado de edición y pueden no representar todas las estructuras complejas.
5. Mermaid se muestra como código, no como diagrama.
6. El resaltado de sintaxis es básico; no equivale a un analizador de lenguajes.
7. El exportador DOCX conserva estructura básica pero puede diferir visualmente de Word o LibreOffice.
8. La privacidad local no implica cifrado en reposo. Evita usar el prototipo con datos confidenciales sin revisión adicional.
9. Mesh limita los documentos a aproximadamente 100 000 caracteres por sala, incluidas imágenes base64, según la implementación actual.

## Pruebas sugeridas para contribuciones

1. Guardar un Markdown de referencia y obtener su hash.
2. Editar exactamente una palabra en Visual; guardar y verificar que `git diff` solo muestre la línea editada.
3. Intercalar cambios en Visual y Markdown; usar deshacer/rehacer repetidamente.
4. Abrir dos pestañas y dos PCs reales: conectividad Mesh y ediciones simultáneas.
5. Desconectar el canal, editar de ambos lados, restablecerlo y comparar el Markdown byte a byte.
6. Probar un documento con front matter, alertas, tablas, tareas, imágenes, código y enlaces maliciosos; validar exportaciones y ausencia de ejecución HTML.
7. Ejecutar las pruebas con modo oscuro, pantalla pequeña y sin conexión de internet.
