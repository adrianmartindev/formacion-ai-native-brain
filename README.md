# Company brain

La documentación de negocio de la empresa en Markdown, organizada por departamento y con un archivo por tema. Es la memoria compartida del equipo y de sus agentes: Claude Code, Codex o Cursor la leen antes de trabajar y citan de qué archivo sale cada dato.

## Cómo funciona

- **Entra información:** el material nuevo (PDF, presentaciones, notas, exportaciones) se deja en `INGESTA/` y se procesa con `/ingesta-informacion`. La skill lo convierte a Markdown, lo reparte por departamento sin resumirlo y regenera los índices.
- **Se guarda ordenado:** cada departamento tiene su carpeta `NN-Nombre/`, un `README.md` con qué va y qué no, y `_plantillas/` con la estructura de cada tipo de documento. Un dato vive en un solo archivo; el resto enlaza a él.
- **Se consulta:** se pregunta en lenguaje natural desde la terminal. El agente responde citando el archivo de origen y, si el dato no está, lo dice.
- **Se mantiene:** los documentos superados se marcan `SUPERSEDED`, las cifras que no cuadran se dejan con su origen y los huecos se marcan `[PENDIENTE]`. Git guarda cada cambio, así que todo se puede revisar y deshacer.

Las reglas que siguen los agentes están en [`AGENTS.md`](AGENTS.md). `CLAUDE.md` solo lo importa para Claude Code.

## Índice

| Departamento | Contenido | Documentos |
|---|---|---|
| [01-Direccion](01-Direccion/README.md) | La empresa: qué es, problema, solución, público y metas | 0 |
| [02-Finanzas](02-Finanzas/README.md) | Plan de negocio, modelo financiero, costes y supuestos | 0 |
| [03-Estrategia-y-Mercado](03-Estrategia-y-Mercado/README.md) | Mercado y competencia | 0 |
| [04-Producto](04-Producto/producto-y-planificacion/README.md) | Producto o servicio y planificación | 0 |
| [04-Producto](04-Producto/validacion-con-clientes/README.md) | Validación con clientes y entrevistas | 0 |
| [05-Marketing](05-Marketing/estrategia-y-canales/README.md) | Estrategia de marketing y canales | 0 |
| [05-Marketing](05-Marketing/plan-mes-aaaa/README.md) | Plan de marketing de un periodo | 0 |
| [06-Ventas](06-Ventas/README.md) | Precios, embudo, playbook y objeciones | 0 |
| [07-Marca-y-Diseno](07-Marca-y-Diseno/README.md) | Marca, voz y tono, identidad visual | 0 |
| [08-Operaciones-y-Equipo](08-Operaciones-y-Equipo/README.md) | Equipo, herramientas y decisiones internas | 0 |
