# Seguridad

La exposición de operaciones al entorno de un modelo introduce vectores de ataque críticos (OWASP Foundation, 2025):

* **Prompt injection desde contenido:** El modelo lee un archivo local cuyo texto contiene comandos diseñados para hackear o sobreescribir las instrucciones originales de la IA.
* **Path traversal y symlinks:** Intentos de evadir las rutas permitidas usando navegación de directorios (ej. `../`) o enlaces simbólicos mal configurados para acceder a archivos sensibles fuera del *Root*.
* **Escrituras o borrados no deseados:** Daños al sistema originados por alucinaciones o interpretaciones erróneas del modelo.

Frente a esto, la especificación **recomienda** —no exige— mantener siempre a un humano en el ciclo (*human in the loop*) con capacidad de rechazar la invocación de una herramienta: el texto usa explícitamente el verbo modal "SHOULD", propio de una buena práctica fuertemente aconsejada pero no obligatoria (Model Context Protocol, 2025). Junto a esta recomendación, se sugiere delimitar rígidamente el alcance (*scope*) de cada servidor y utilizar modos de solo lectura cuando la modificación de archivos no sea estrictamente necesaria.
