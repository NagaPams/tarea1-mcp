# MCP frente a una API

Una API tradicional funciona como un contrato estricto entre programas, donde la decisión de qué *endpoint* invocar está escrita de antemano de forma rígida en el código (Fielding, 2000). En contraste, el *Model Context Protocol* (MCP) opera como un protocolo abierto basado en el estándar JSON-RPC 2.0 (JSON-RPC Working Group, 2010). En MCP, el servidor publica un catálogo de herramientas (*tools*) que detalla el nombre, la descripción y el esquema de parámetros; el modelo descubre estas capacidades en tiempo de ejecución y decide de manera autónoma cuál invocar (Anthropic, 2024).

## Tabla comparativa

La comparativa se articula en seis ejes fundamentales (Anthropic, 2024):

| Eje | API tradicional | MCP |
|---|---|---|
| Decisión de invocación | La toma el desarrollador | La toma el modelo de IA |
| Descubrimiento de capacidades | Documentación técnica estática | La IA descubre las herramientas dinámicamente |
| Acoplamiento cliente-servicio | Clientes fuertemente acoplados | Integración estandarizada para clientes universales |
| Formato de mensajes | Varía enormemente entre APIs | JSON-RPC 2.0 |
| Autenticación y consentimiento | Definidos por cada API | Opcional y solo definida para transportes HTTP (subconjunto de OAuth 2.1); con `stdio` se usan credenciales del entorno (Model Context Protocol, 2025) |
| Reutilización entre aplicaciones | Código de integración por cada aplicación | Un servidor MCP conecta con cualquier cliente compatible |

### Detalle: autenticación y consentimiento

MCP contempla la autorización como parte de su especificación, pero no la impone: es **opcional** y solo se define para los transportes basados en HTTP, donde se apoya en un subconjunto de OAuth 2.1. Cuando el transporte es `stdio`, la especificación indica explícitamente que no debe seguirse este mecanismo, sino obtener las credenciales del entorno de ejecución (por ejemplo, variables de entorno) (Model Context Protocol, 2025).

## MCP no sustituye a las APIs

Es importante aclarar que MCP no busca sustituir a las APIs; un servidor MCP generalmente actúa como una capa que envuelve una API existente para que la IA la pueda consumir de forma segura e inteligible (Anthropic, 2024).
