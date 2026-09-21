# El problema del aislamiento

Mantener una arquitectura aislada responde a dos tipos de motivos distintos:

* *Razones de diseño*: desacoplar el ciclo de vida del modelo respecto al de los servicios externos, de modo que cada servidor MCP pueda actualizarse, reiniciarse o sustituirse sin afectar al modelo ni a los demás servidores, favoreciendo la modularidad y la responsabilidad única de cada componente.
* *Razones de ciberseguridad*: confinar la ejecución de las herramientas a entornos controlados o *sandboxes*, de forma que un fallo, una alucinación o una inyección de instrucciones queden contenidos y no puedan escapar al sistema operativo anfitrión ni a otros procesos.
