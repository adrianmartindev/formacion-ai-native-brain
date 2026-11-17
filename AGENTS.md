# [PENDIENTE: nombre de la empresa]

## Project

[PENDIENTE: qué hace la empresa y para quién, en una frase. Modelo de negocio y fase (bootstrap, ronda, rentable…). Prioridad actual (velocidad, retención, margen…).]

> **Copy al cliente:** [PENDIENTE: reglas que se cumplen siempre en todo lo que lee el cliente (web, producto, redes, emails, guiones de venta): vocabulario obligatorio o prohibido, tú o usted, qué no se menciona nunca]. Los documentos internos de estrategia SÍ reflejan la realidad completa.

> **Contexto completo de la empresa:** [`01-Direccion/01-resumen.md`](01-Direccion/01-resumen.md)

No es un repositorio de código: es la documentación de negocio de la empresa en Markdown, un archivo por tema, agrupados por departamento. Toda la documentación está en español. Mantenla así.

Las secciones marcadas `[PENDIENTE]` en este archivo se rellenan cuando el brain tiene el dato: al terminar una ingesta, si el material nuevo aporta algo a Project, Principios, Convenciones o Key decisions, propón al usuario el cambio en este archivo (una línea por regla, enlazando al documento fuente). Mientras siga `[PENDIENTE]`, no supongas nada: dilo.

## Structure

```
01-Direccion/              → qué es la empresa, problema, solución, público, principios y metas
02-Finanzas/               → plan de negocio, modelo financiero, costes, supuestos y riesgos
03-Estrategia-y-Mercado/   → mercado, competidores, tablas comparativas y fuentes
04-Producto/
  producto-y-planificacion/  → producto o servicio, alcance, roadmap y decisiones de producto
  validacion-con-clientes/   → entrevistas, hipótesis y aprendizajes
05-Marketing/
  estrategia-y-canales/      → estrategia, posicionamiento, canales, contenido y KPIs
  plan-mes-aaaa/             → plan de marketing de un periodo (renombrar a plan-mes-año al crearlo)
06-Ventas/                 → precios, embudo, guion de demo y objeciones
07-Marca-y-Diseno/         → historia de marca, voz y tono, identidad visual
08-Operaciones-y-Equipo/   → equipo, herramientas, rutinas y registro de decisiones
INGESTA/                   → bandeja de entrada de material sin procesar (queda vacía tras cada ingesta)
.claude/agents/            → agentes por departamento
.claude/skills/            → skills del proyecto (ingesta-informacion)
```

### Claude Code y otros agentes

`AGENTS.md` es la única fuente de reglas: lo leen Codex, Cursor y cualquier agente compatible. `CLAUDE.md` solo contiene `@AGENTS.md` para que Claude Code lo importe. No se convierte en symlink (se rompe al clonar en Windows) ni se duplica su contenido.

- Cada carpeta tiene un `README.md`: cabecera escrita a mano (qué va, qué no, fuentes) y una lista `## Contenido` que se genera sola. No se edita a mano.
- `_plantillas/` en cada departamento: la estructura de cada tipo de documento. Antes de crear un documento, mira si hay plantilla y úsala. Las plantillas no son documentos y no salen en los índices.
- Departamento nuevo (`09-...`) solo si nada encaja, y avisando al usuario.

## Principios

Viven en [`01-Direccion/06-principios.md`](01-Direccion/06-principios.md). Aquí solo los que cambian cómo trabaja el agente:

1. [PENDIENTE: principio que cambia cómo se trabaja, p. ej. simple antes que completo]
2. [PENDIENTE]
3. [PENDIENTE]

## Agent orchestration (MANDATORY)

> **CRITICAL RULE:** Eres un orquestador. Para CUALQUIER tarea de un departamento,
> DEBES delegar en su agente con la herramienta Agent y su `subagent_type`.
> NO hagas tú el trabajo del departamento, aunque parezca sencillo: delega y luego integra.

### Enrutado por departamento

> **Estado:** ningún departamento tiene todavía agente propio. Todos delegan en `general-purpose` con el encargo limitado a su carpeta hasta que tengan el suyo.
> **Al crear cada agente** en `.claude/agents/`, sustituye `general-purpose` por su `subagent_type` en la fila de su departamento y completa sus triggers. El alcance y los límites del agente viven en su propio archivo; esta tabla solo enruta y nunca copia su descripción.

| Departamento | Carpeta | `subagent_type` | Triggers (delegar cuando la tarea implique) |
|---|---|---|---|
| **Dirección** | `01-Direccion/` | `general-purpose` (limitado a la carpeta) | Visión, problema, público objetivo, propuesta de valor, diferencial, metas |
| **Finanzas** | `02-Finanzas/` | `general-purpose` (limitado a la carpeta) | Plan de negocio, modelo financiero, supuestos, costes, unit economics, escenarios, impacto financiero de una decisión |
| **Estrategia y Mercado** | `03-Estrategia-y-Mercado/` | `general-purpose` (limitado a la carpeta) | Tamaño de mercado, competidores, posicionamiento, research |
| **Producto** | `04-Producto/` | `general-purpose` (limitado a la carpeta) | Producto o servicio, alcance, roadmap y backlog, especificaciones, entrevistas e hipótesis |
| **Marketing** | `05-Marketing/` | `general-purpose` (limitado a la carpeta) | Posicionamiento, canales, plan del periodo y calendario de contenidos, contenido, SEO, KPIs de marketing |
| **Ventas** | `06-Ventas/` | `general-purpose` (limitado a la carpeta) | Precios, embudo, prospección, demo, objeciones, cierre |
| **Marca y Diseño** | `07-Marca-y-Diseno/` | `general-purpose` (limitado a la carpeta) | Historia de marca, logo, color, tipografía, voz y tono, taglines, assets |
| **Operaciones y Equipo** | `08-Operaciones-y-Equipo/` | `general-purpose` (limitado a la carpeta) | Equipo, herramientas, rutinas, registro de decisiones |
| **Ingesta** | `INGESTA/` | skill `/ingesta-informacion` | Material nuevo que hay que convertir en documentos de departamento |
| Búsqueda entre departamentos (solo lectura) | varios | `Explore` | Encontrar dónde vive un dato antes de enrutar el trabajo |

### Cómo enrutar

1. **Identifica el departamento** por el contenido de la petición, no por el verbo.
2. **Un departamento:** lanza su agente.
3. **Varios departamentos independientes:** lanza un agente por departamento, en paralelo, en un solo mensaje. A cada uno dile qué pidió el usuario, que otros agentes lo miran desde su departamento y que señale implicaciones para los demás.
4. **Varios dependientes** (B necesita lo que produce A, p. ej. finanzas fija objetivos → producto los convierte en roadmap): lanza A y pasa su resultado de forma explícita en el encargo de B.
5. **Ambiguo:** haz una pregunta corta al usuario antes de lanzar nada.
6. **Integra:** si los agentes coinciden, una recomendación unificada; si chocan, muestra ambas posturas, señala la tensión y deja decidir al usuario. Cada dato queda en un solo archivo.

### Contrato de delegación

1. **La orquestadora recoge.** Nunca termina un turno con "esperando a los agentes": espera a todos, integra y responde.
2. **Cada encargo se entiende solo.** El agente solo sabe lo que pone su encargo: objetivo, archivos o carpeta que leer, lo que ya está descartado y cómo es el resultado terminado.
3. **Lo que dice un agente es un informe, no una prueba.** Comprueba los archivos que haya escrito antes de decir que está hecho.
4. **Los agentes no escriben fuera de su carpeta.** Los cambios entre departamentos pasan por la orquestadora.
5. **Manos fuera tras delegar.** Si hace falta seguir (aplicar un cambio, profundizar), se continúa el mismo agente con `SendMessage`; no se abre otro ni se hace a mano.

### Sin agente (lo hace la sesión principal)

- Preguntas sobre dónde está un dato o qué contiene un archivo
- Operaciones de git (commits, ramas, diffs)
- Búsquedas y lecturas rápidas
- Memoria (guardar, recordar, actualizar)
- Conversación que no requiere trabajo de departamento

## MCPs permitidos (MANDATORY)

> **CRITICAL RULE:** En este proyecto solo se pueden usar los servidores MCP definidos en [`.mcp.json`](.mcp.json).
> Cualquier otro MCP disponible en la sesión (de usuario, de plugins o conectores de claude.ai) está prohibido aquí, aunque esté conectado y aunque parezca útil.

- La regla aplica a la sesión principal y a todos los agentes y skills.
- Si una tarea necesita un MCP que no está en `.mcp.json`, no se usa: se avisa al usuario y se propone añadirlo a `.mcp.json`.
- Añadir o quitar un MCP se hace solo editando `.mcp.json`, con confirmación del usuario. La lista vigente vive en ese archivo y no se copia aquí.
- No cuentan como MCP externo las herramientas propias de la app de Claude Code (tareas, sesiones, vistas, navegador integrado).

## Ingesta (`/ingesta-informacion`)

Usa la skill siempre que haya material nuevo en `INGESTA/`. Su `SKILL.md` es la referencia del proceso. Resumen:

1. Convertir a Markdown (PDF con el script de la skill; Word, PowerPoint y HTML con `markitdown`). Imágenes, vídeos y hojas de cálculo no se convierten: se mueven a su departamento con su nombre original.
2. Decidir el departamento de cada capítulo por su contenido, no por su título.
3. Repartir en un archivo por sección, con la cabecera estándar.
4. Verificar que no se ha perdido texto.
5. Regenerar los índices `README.md`.
6. Enseñar la lista de originales que se van a borrar de `INGESTA/` y borrarlos solo con confirmación.
7. Proponer los cambios a las secciones `[PENDIENTE]` de este `AGENTS.md` que el material nuevo permita rellenar.

## Formato de cada documento

```markdown
# Título del documento

> Departamento: [Departamento] · Origen: [documento o persona de la que sale] ([fecha])

[Contenido]
```

Si existe plantilla para ese tipo de documento en `_plantillas/`, se sigue su estructura.

## Convenciones de edición

- **Una sola fuente por dato: la información NUNCA se duplica.** Cada dato vive en un único archivo; todo lo demás (otros documentos, agentes, skills, este AGENTS.md) enlaza a ese archivo. Si un documento nuevo repite algo ya documentado, se actualiza el existente. Si encuentras un duplicado, avisa y propón cuál se queda como fuente.
- **Nombres:** archivos en minúsculas, sin tildes y con guiones (`NN-tema.md`). Carpetas de departamento: `NN-Nombre`. Subcarpetas: minúsculas con guiones.
- **La ingesta conserva el texto completo:** no se resume ni se reescribe. Si un material nuevo actualiza un documento, se actualiza ese archivo con la fecha y el origen nuevos.
- **Cifras que no cuadran:** se dejan las dos con su origen. No se reconcilian hasta que el usuario decida.
- **Plan no es dato real:** las cifras de un plan o un presupuesto no se presentan como resultados medidos.
- **Documentos superados:** se marcan `SUPERSEDED (fecha)` al principio y se conservan como histórico. Nunca se usan como regla vigente.
- **`[SUPUESTO]`** marca cifras que salen de un plan o estimación, no de datos medidos.
- **Huecos:** `[PENDIENTE: qué falta y quién lo sabe]`. Nunca se inventa para rellenar.
- **Citar siempre:** cualquier respuesta sobre la empresa dice de qué archivo sale cada dato. Si algo no está, se dice ("no está en el brain").
- **Precios:** [PENDIENTE: cómo se escriben (moneda, IVA incluido o no, por mercado)].
- **Vocabulario:** [PENDIENTE: términos obligatorios y prohibidos en copy al cliente].

## Qué NO entra en el brain

- Datos de contacto de clientes o prospectos (teléfonos, emails, perfiles personales). Viven en [PENDIENTE: CRM o herramienta].
- Contraseñas, claves de API, credenciales e identificadores internos.
- Datos personales del equipo y de los clientes, y documentación legal sensible.

El contenido es interno y confidencial: no se sube a servicios externos.

## Key decisions

Decisiones vigentes que afectan a cualquier tarea. El detalle (motivo y fecha) vive en [`08-Operaciones-y-Equipo/registro-de-decisiones.md`](08-Operaciones-y-Equipo/registro-de-decisiones.md); aquí solo la regla, una línea cada una.

- [PENDIENTE: primera decisión vigente, p. ej. precio, canal principal o mercado]
