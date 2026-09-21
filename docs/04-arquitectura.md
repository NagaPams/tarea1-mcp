# Arquitectura de MCP

La arquitectura se divide en tres roles clave: Host, Cliente y Servidor. En un entorno local, herramientas como Claude Code asumen los roles simultáneos de host y cliente, mientras que el **servidor** es un proceso independiente —por ejemplo, el paquete `@modelcontextprotocol/server-filesystem`— que el host lanza como subproceso hijo y con el que se comunica por `stdio`; es ese proceso, y no el sistema de archivos en sí, el que opera sobre el disco y responde a las peticiones JSON-RPC (Model Context Protocol, 2025). El sistema de archivos es el recurso sobre el que actúa el servidor, no el servidor mismo.

Las primitivas de la arquitectura se dividen estrictamente en:

* **Primitivas del servidor:** *Tools* (funciones ejecutables que modifican estado), *Resources* (datos inyectados únicamente como contexto) y *Prompts* (plantillas preconstruidas para flujos de trabajo de la IA) (Anthropic, 2024).
* **Primitivas del cliente:** *Roots* (límites declarados de acceso a datos locales para prevenir lecturas fuera de alcance) y *Elicitation* (solicitudes iniciadas por el servidor hacia el usuario para pedir datos o confirmación).

Para la comunicación, se establecen dos transportes estandarizados:

* **`stdio`**, para servidores locales: el cliente lanza el servidor como subproceso y ambos intercambian mensajes JSON-RPC por entrada y salida estándar.
* **Streamable HTTP**, para servidores remotos: sustituyó al transporte anterior *HTTP+SSE* de la versión 2024-11-05 del protocolo. El servidor expone un único endpoint HTTP que acepta peticiones POST y GET, y **puede usar opcionalmente** *Server-Sent Events* (SSE) para transmitir varios mensajes en una misma respuesta; no está "basado" en SSE, sino que SSE es uno de los dos formatos de respuesta posibles (el otro es un objeto JSON simple) (Model Context Protocol, 2025).

La versión vigente de la especificación es **2025-11-25**, identificada con el formato AAAA-MM-DD que indica la fecha del último cambio incompatible hacia atrás; la documentación principal del estándar se mantiene en *modelcontextprotocol.io* (Model Context Protocol, 2025).
