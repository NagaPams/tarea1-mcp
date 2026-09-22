# Tarea 1: MCP y servidor de sistema de archivos

## Datos de identificación

- **Nombre:** Hervey Gabriel Gutierrez Prats
- **Boleta:** 2022630373
- **Grupo:** 7CV4

## Resumen de la actividad

Esta actividad investiga cómo un modelo de lenguaje pasa de estar aislado (solo recibe y devuelve texto) a poder operar sobre archivos locales mediante el *Model Context Protocol* (MCP), y documenta la instalación, verificación y prueba de límites del servidor MCP de referencia para sistema de archivos, conectado a Claude Code.


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

### Entorno utilizado

| Componente | Versión |
|---|---|
| Sistema operativo | Arch Linux, kernel 7.2.6-arch2-1 |
| Node.js | v26.9.0 |
| npm / npx | 12.0.2 |
| Cliente MCP | Claude Code 2.1.278 |
| Servidor | `@modelcontextprotocol/server-filesystem` 0.2.0, ejecutado vía `npx` |

### Elección del cliente

Se eligió **Claude Code** por ser el cliente MCP disponible en esta máquina que ya se usa como entorno de desarrollo en terminal, lo que permite asumir simultáneamente los roles de host y cliente, y porque su configuración vía `.mcp.json` es reproducible y versionable en el repositorio.

### 1. Crear el directorio de trabajo autorizado

Se creó `workspace-mcp/` dentro del repositorio, exclusivo para esta tarea (ni la raíz del disco ni la carpeta de usuario completa):

```bash
mkdir -p workspace-mcp
```

### 2. Registrar el servidor filesystem

```bash
cd workspace-mcp
claude mcp add filesystem --scope project -- npx -y @modelcontextprotocol/server-filesystem /home/pams/GitHub/tarea1-mcp/workspace-mcp
```

Esto genera un archivo `.mcp.json` **dentro de `workspace-mcp/`**, con el siguiente contenido (también disponible en [`config/mcp.json`](config/mcp.json), sin credenciales):

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/home/pams/GitHub/tarea1-mcp/workspace-mcp"
      ],
      "env": {}
    }
  }
}
```

> **Nota importante sobre *Roots*:** el servidor filesystem, si el cliente soporta el mecanismo MCP de *Roots*, reemplaza los directorios pasados por línea de comandos con el directorio raíz que declara el cliente (que en Claude Code es el directorio desde el que se abre la sesión). Por eso el `.mcp.json` debe colocarse dentro de `workspace-mcp/` y Claude Code debe abrirse **desde esa carpeta**, y no desde la raíz del repositorio; de lo contrario el servidor queda delimitado al repositorio completo en vez de al directorio pensado para la tarea. Ver evidencia en [`img/antes-de-corregir-root/`](img/antes-de-corregir-root/).

### 3. Abrir Claude Code desde el directorio autorizado y verificar

```bash
cd ~/GitHub/tarea1-mcp/workspace-mcp
claude
```

Al iniciar, Claude Code solicita aprobar el servidor declarado en `.mcp.json`. Dentro de la sesión, `/mcp` confirma que `filesystem` está conectado y expone 14 herramientas (véase [`docs/05-servidor-fs.md`](docs/05-servidor-fs.md) para el detalle de cada una).

Verificación adicional del límite, pidiendo al modelo usar `list_allowed_directories` del servidor: responde únicamente `/home/pams/GitHub/tarea1-mcp/workspace-mcp`.

## Evidencias

| Captura | Descripción |
|---|---|
| [`img/01-mcp-list-servers.png`](img/01-mcp-list-servers.png) | `/mcp` dentro de Claude Code: servidor `filesystem` conectado, 14 herramientas |
| [`img/02-claude-mcp-list-cli.png`](img/02-claude-mcp-list-cli.png) | `claude mcp list` desde la terminal, con la ruta autorizada |
| [`img/03-mcp-list-corregido.png`](img/03-mcp-list-corregido.png) | `/mcp` ya con `.mcp.json` dentro de `workspace-mcp/` |
| [`img/04-allowed-directories.png`](img/04-allowed-directories.png) | `list_allowed_directories`: confirma que solo `workspace-mcp/` está autorizado |
| [`img/05-listar-directorio.png`](img/05-listar-directorio.png) | Operación: listar el contenido del directorio autorizado (`list_directory`) |
| [`img/06-leer-archivo.png`](img/06-leer-archivo.png) | Operación: leer un archivo existente (`read_text_file` sobre `notas.txt`) |
| [`img/07-crear-archivo.png`](img/07-crear-archivo.png) | Operación: crear un archivo nuevo con contenido (`write_file` crea `nuevo.txt`) |
| [`img/08-modificar-archivo.png`](img/08-modificar-archivo.png) | Operación: modificar un archivo existente (`edit_file` cambia "manzana" por "pera" en `notas.txt`) |
| [`img/09-buscar-archivo.png`](img/09-buscar-archivo.png) | Operación: buscar un archivo por nombre (`search_files`, coincidencia "nota") |
| [`img/10-prueba-limite-seguridad.png`](img/10-prueba-limite-seguridad.png) | Prueba del límite de seguridad: solicitud de lectura de `/etc/hostname`, rechazada por el servidor |

### Prueba del límite de seguridad

Se solicitó al modelo leer `/etc/hostname` usando explícitamente `read_text_file` del servidor `filesystem` (fuera de `workspace-mcp/`). La operación fue rechazada con el mensaje:

> No se pudo: `/etc/hostname` está fuera del directorio permitido para el servidor filesystem (`/home/pams/GitHub/tarea1-mcp/workspace-mcp`).

El mecanismo que impidió la operación no es un permiso de Claude Code, sino la **validación de rutas del propio servidor MCP**: antes de ejecutar cualquier herramienta, el proceso `server-filesystem` compara la ruta solicitada contra su lista de directorios permitidos (la definida por `.mcp.json` o, si el cliente soporta *Roots*, la que este declaró) y rechaza toda ruta que quede fuera de esa lista, sin llegar a tocar el disco.

## Conclusiones personales
La implementación del servidor filesystem mediante MCP evidencia un cambio arquitectónico crucial para los agentes de IA, destacando dos principios fundamentales:

MCP reemplaza las integraciones a medida con un contrato universal. Esto permite al modelo descubrir dinámicamente sus herramientas y pasar de ser un simple procesador de texto aislado a un agente capaz de interactuar con el entorno local.

La prueba de límite intentando leer /etc/hostname confirmó que la seguridad no depende de las "intenciones" de la IA (que es no determinista y manipulable), sino del servidor MCP. Este aplica un modelo Zero Trust, actuando como un árbitro estricto que confina toda operación exclusivamente al directorio autorizado.


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
