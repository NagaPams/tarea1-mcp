# El servidor de sistema de archivos

El servidor de sistema de archivos es una implementación de referencia oficial y no forma parte central del protocolo (Anthropic, 2024). Se distribuye como el paquete `@modelcontextprotocol/server-filesystem`, dentro del repositorio `modelcontextprotocol/servers`, y expone entre otras las siguientes herramientas (Model Context Protocol, 2025):

* `read_text_file` / `read_file` (este último está marcado como obsoleto en la documentación del paquete), `read_media_file` y `read_multiple_files`: lectura de uno o varios archivos, de texto o multimedia.
* `write_file`: creación de un archivo nuevo o sobreescritura de uno existente.
* `edit_file`: ediciones selectivas basadas en coincidencia de líneas o bloques de texto.
* `create_directory`, `list_directory`, `list_directory_with_sizes` y `directory_tree`: creación y exploración de directorios.
* `move_file`, `search_files`, `get_file_info` y `list_allowed_directories`: movimiento, búsqueda, metadatos y consulta de los directorios habilitados.

Al consultar el servidor con `tools/list` (versión 0.2.0, protocolo 2025-06-18) se obtuvieron 14 herramientas. Solo `write_file`, `edit_file`, `create_directory` y `move_file` modifican el disco; las demás están anotadas como de solo lectura.

Para gestionar la seguridad, el propio README del proyecto especifica que el servidor **solo permitirá operaciones dentro de los directorios indicados como argumentos** al iniciarlo (Model Context Protocol, 2025). Si no existiera este límite, el modelo tendría acceso indiscriminado de lectura y escritura sobre toda la jerarquía de archivos del equipo anfitrión, lo cual expone la máquina a la manipulación no intencionada o maliciosa (OWASP Foundation, 2025).
