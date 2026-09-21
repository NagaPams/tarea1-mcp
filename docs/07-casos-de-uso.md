# Casos de uso

El ecosistema de agentes de codificación aprovecha la estandarización de MCP para interactuar con el sistema de archivos y otras herramientas de forma predecible y limitada. Entre los más representativos:

* **Claude Code**: agente de terminal de Anthropic que asume simultáneamente los roles de host y cliente MCP; al tener acceso directo al shell y al sistema de archivos del proyecto, puede leer, modificar y ejecutar pruebas sobre un repositorio completo en el propio disco del desarrollador (Anthropic, 2025).
* **Cursor**: editor de código basado en VS Code con un modo agente que se conecta a servidores MCP para consultar documentación, bases de datos o control de versiones, y aplica los cambios directamente sobre los archivos abiertos del proyecto (Cursor, 2025).
* **Google Antigravity**: entorno de desarrollo agéntico de Google, impulsado por Gemini 3, que coordina múltiples agentes capaces de planear, escribir y validar tareas de software de forma autónoma, con un panel de gestión de agentes y conectividad profunda con el navegador para probar los cambios (Google, 2025).

En los tres casos, la edición de un repositorio completo no requiere que el usuario suba manualmente los archivos al modelo: el agente, actuando como cliente MCP, invoca herramientas de un servidor como `server-filesystem` (u otro equivalente integrado en la propia aplicación) que opera directamente sobre las rutas del disco autorizadas mediante *Roots* o argumentos de arranque. El texto viaja del disco al modelo y de vuelta como llamadas a herramientas —lecturas, ediciones y escrituras puntuales—, no como una carga masiva de archivos por parte del usuario.
