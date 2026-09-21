# Tarea 1: MCP y servidor de sistema de archivos

## Datos de identificación

- **Nombre:** Hervey Gabriel Gutierrez Prats
- **Boleta:** 2022630373
- **Grupo:** 7CV

## Resumen de la actividad



## Índice de documentos

1. [Evolución de los modelos](docs/01-evolucion.md)
2. [El problema del aislamiento](docs/02-aislamiento.md)
3. [MCP frente a una API](docs/03-mcp-vs-api.md)
4. [Arquitectura de MCP](docs/04-arquitectura.md)
5. [El servidor de sistema de archivos](docs/05-servidor-fs.md)
6. [Seguridad](docs/06-seguridad.md)
7. [Casos de uso](docs/07-casos-de-uso.md)

## MCP vs API

| Eje | API tradicional | MCP |
|---|---|---|
| Decisión de invocación | La toma el desarrollador | La toma el modelo de IA |
| Descubrimiento de capacidades | Documentación técnica estática | La IA descubre las herramientas dinámicamente |
| Acoplamiento cliente-servicio | Clientes fuertemente acoplados | Integración estandarizada para clientes universales |
| Formato de mensajes | Varía enormemente entre APIs | JSON-RPC 2.0 |
| Autenticación y consentimiento | Definidos por cada API | Opcional y solo definida para transportes HTTP (subconjunto de OAuth 2.1); con `stdio` se usan credenciales del entorno (Model Context Protocol, 2025) |
| Reutilización entre aplicaciones | Código de integración por cada aplicación | Un servidor MCP conecta con cualquier cliente compatible |

## Instalación paso a paso

_Pendiente: sistema operativo, versiones, cliente elegido (Claude Code) y justificación, contenido de `config/`._

## Evidencias

_Pendiente: capturas en `img/` (listar, leer, crear, modificar, buscar y prueba del límite de seguridad)._

## Conclusiones personales



## Referencias

* Anthropic. (2024). *Model Context Protocol (MCP) Documentation*. Recuperado de [https://modelcontextprotocol.io](https://modelcontextprotocol.io)
* Anthropic. (2025). *Claude Code*. Recuperado de [https://docs.anthropic.com/en/docs/agents-and-tools/claude-code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code)
* Cursor. (2025). *Cursor — The AI Code Editor*. Recuperado de [https://cursor.com](https://cursor.com)
* Fielding, R. T. (2000). *Architectural Styles and the Design of Network-based Software Architectures* [Disertación doctoral, University of California, Irvine].
* Google. (2025). *Google Antigravity*. Recuperado de [https://antigravity.google](https://antigravity.google)
* JSON-RPC Working Group. (2010). *JSON-RPC 2.0 Specification*. Recuperado de [https://www.jsonrpc.org/specification](https://www.jsonrpc.org/specification)
* Model Context Protocol. (2025). *Specification, versión 2025-11-25*. Recuperado de [https://modelcontextprotocol.io/specification](https://modelcontextprotocol.io/specification)
* Model Context Protocol. (2025). *servers* [Repositorio], paquete `@modelcontextprotocol/server-filesystem`. Recuperado de [https://github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
* OpenAI. (2024, septiembre). *Learning to reason with LLMs*. Recuperado de [https://openai.com/index/learning-to-reason-with-llms/](https://openai.com/index/learning-to-reason-with-llms/)
* OWASP Foundation. (2025). *Top 10 for Large Language Model Applications*. Recuperado de [https://owasp.org/www-project-top-10-for-large-language-model-applications/](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
