# MCP e Integraciones en Claude — Contenido de Clase

> **Programa "Claude para Liderazgo" — Qaracter**
> **Audiencia:** Directores y Senior Managers, sin experiencia previa en IA
> **Versión:** Mayo 2026

---

## Índice

1. [De Conectores a MCP: el salto de nivel](#1-de-conectores-a-mcp-el-salto-de-nivel)
2. [Qué es MCP (Model Context Protocol)](#2-qué-es-mcp-model-context-protocol)
3. [Arquitectura de MCP explicada sin jerga](#3-arquitectura-de-mcp-explicada-sin-jerga)
4. [MCP en el stack de Qaracter](#4-mcp-en-el-stack-de-qaracter)
   - 4.1 [Microsoft 365](#41-microsoft-365)
   - 4.2 [Atlassian (Jira, Confluence, Rovo)](#42-atlassian-jira-confluence-rovo)
   - 4.3 [Devin — el agente autónomo de desarrollo](#43-devin--el-agente-autónomo-de-desarrollo)
   - 4.4 [Otros sistemas relevantes](#44-otros-sistemas-relevantes)
5. [Conectores prefabricados vs. MCP: cuándo usar cada uno](#5-conectores-prefabricados-vs-mcp-cuándo-usar-cada-uno)
6. [Casos de uso reales para directivos de Qaracter](#6-casos-de-uso-reales-para-directivos-de-qaracter)
7. [Rovo Agents: la inteligencia nativa de Atlassian](#7-rovo-agents-la-inteligencia-nativa-de-atlassian)
8. [Seguridad, gobernanza y control directivo](#8-seguridad-gobernanza-y-control-directivo)
9. [Hoja de ruta y visión de futuro](#9-hoja-de-ruta-y-visión-de-futuro)
10. [Resumen ejecutivo y puntos clave](#10-resumen-ejecutivo-y-puntos-clave)
11. [Anexo — Glosario para directivos](#11-anexo--glosario-para-directivos)
12. [Anexo — Fuentes y recursos](#12-anexo--fuentes-y-recursos)

---

## 1. De Conectores a MCP: el salto de nivel

En sesiones anteriores hemos visto cómo los **Conectores de Claude** (Gmail, Drive, Calendar, M365…) eliminan la "fontanería" entre Claude y nuestras herramientas de trabajo. Un Conector es una conexión prefabricada: Anthropic la construye, la publica en su directorio y nosotros la activamos con un clic.

Eso funciona perfectamente cuando el sistema al que queremos conectar Claude es uno de los grandes nombres: Google, Microsoft, Salesforce, Slack. Pero las empresas reales — y Qaracter es un ejemplo perfecto — no viven solo en esos ecosistemas. Tenemos sistemas propietarios, ERPs, bases de datos internas, aplicaciones legacy, herramientas de nicho sectorial. Para esos sistemas no existe un Conector prefabricado en el directorio de Claude.

Aquí es donde aparece **MCP — Model Context Protocol**. MCP es el estándar abierto que permite construir conexiones personalizadas entre Claude (o cualquier otro modelo de IA) y cualquier sistema, servicio o fuente de datos. Si los Conectores son las "apps preinstaladas" de Claude, MCP es la "App Store" que permite instalar cualquier integración imaginable.

La diferencia fundamental: con los Conectores, dependes de lo que Anthropic decida publicar. Con MCP, tu equipo técnico puede construir la integración exacta que necesita tu negocio, en días, no en meses.

---

## 2. Qué es MCP (Model Context Protocol)

### Definición para directivos

MCP es un **protocolo estándar y abierto** — creado por Anthropic y adoptado por la industria — que define cómo un modelo de IA se comunica con sistemas externos de forma segura y controlada.

Pensemos en una analogía concreta: cuando un navegador web quiere cargar una página, usa el protocolo HTTP. No importa si el navegador es Chrome o Firefox, ni si el servidor es de Google o de una tienda local — HTTP es el idioma común. MCP hace lo mismo, pero para la comunicación entre modelos de IA y herramientas externas.

### Las tres cosas que MCP permite hacer a Claude

| Capacidad | Qué significa | Ejemplo Qaracter |
|---|---|---|
| **Leer contexto** (Resources) | Claude puede consultar información de un sistema externo | Leer el estado de un proyecto en Jira, consultar una página de Confluence, ver un documento en SharePoint |
| **Ejecutar acciones** (Tools) | Claude puede realizar operaciones en un sistema externo | Crear un ticket en Jira, actualizar una página de Confluence, enviar una notificación en Teams |
| **Recibir instrucciones** (Prompts) | El sistema externo puede sugerir a Claude plantillas de trabajo predefinidas | Jira sugiere a Claude la plantilla "resumen de sprint" cuando detecta que estás en contexto de proyecto |

### Por qué MCP importa para directivos (no solo para técnicos)

MCP no es algo que vayáis a configurar personalmente. Pero sí es algo que necesitáis entender por tres razones:

1. **Conversaciones con IT.** Cuando pidáis "quiero que Claude acceda a nuestro sistema X", IT os dirá "lo hacemos por MCP". Saber qué es os permite tener una conversación informada sobre plazos, alcance y limitaciones.

2. **Decisiones de inversión.** Construir un servidor MCP para un sistema interno es un proyecto técnico con coste y plazo. Vosotros decidís si el retorno justifica la inversión.

3. **Visión estratégica.** MCP es el estándar que va a definir cómo interactúan los modelos de IA con los sistemas empresariales durante los próximos años. Entenderlo os permite anticipar oportunidades y riesgos.

---

## 3. Arquitectura de MCP explicada sin jerga

### Los tres roles

Imaginemos una conversación telefónica a tres bandas:

```
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   MCP Host  │ ───► │ MCP Client  │ ───► │ MCP Server  │
│  (Claude)   │      │ (el cable)  │      │ (la herram.) │
└─────────────┘      └─────────────┘      └─────────────┘
   "Quiero          "Traduzco la         "Tengo los datos
   información"     petición al           y ejecuto las
                    formato correcto"     acciones"
```

- **Host (Claude):** Es quien necesita información o quiere ejecutar una acción. Es vuestro asistente inteligente.
- **Client (conector MCP):** Es el intermediario técnico. Traduce lo que Claude pide al formato que entiende el sistema destino. Vosotros no lo veis, pero está ahí.
- **Server (la herramienta):** Es el sistema externo — Jira, Confluence, SharePoint, vuestro ERP, una base de datos interna. Recibe la petición, la ejecuta y devuelve el resultado.

### El flujo en lenguaje llano

1. Vosotros le decís a Claude: *"Dame el resumen del sprint actual del proyecto Alfa en Jira"*.
2. Claude detecta que necesita datos de Jira → invoca la herramienta MCP de Jira.
3. El client MCP traduce la petición al formato que Jira entiende (API REST).
4. El server MCP ejecuta la consulta en Jira con **vuestras credenciales**.
5. Jira devuelve los datos → el client los pasa a Claude.
6. Claude os presenta el resumen en lenguaje natural con referencias a los tickets originales.

Todo esto sucede en segundos, de forma transparente, con vuestros permisos y sin que vosotros hayáis tenido que abrir Jira, buscar el proyecto, filtrar por sprint, leer 40 tickets y hacer el resumen mental.

### Seguridad por diseño

Tres principios que debéis conocer:

- **Consentimiento explícito.** Claude nunca ejecuta una acción MCP sin que vosotros lo hayáis autorizado. La primera vez que Claude quiere usar un servidor MCP, os pide permiso.
- **Permisos heredados.** Claude accede solo a lo que vosotros ya podéis ver. Si no tenéis acceso a un proyecto en Jira, Claude tampoco.
- **Auditabilidad.** Cada acción MCP queda registrada. IT puede ver qué se consultó, cuándo y por quién.

---

## 4. MCP en el stack de Qaracter

Qaracter trabaja principalmente con tres ecosistemas tecnológicos: **Microsoft 365**, **Atlassian** y **herramientas de desarrollo/operaciones**. Veamos cómo MCP conecta Claude con cada uno.

### 4.1 Microsoft 365

Microsoft es el ecosistema central de comunicación y documentación de Qaracter. La integración con Claude opera en dos niveles:

#### Nivel 1: Conector M365 prefabricado (ya disponible)

Desde abril 2026, existe un conector oficial de Microsoft 365 para Claude, publicado en el Microsoft Marketplace. Este conector es **solo lectura** y cubre:

| Servicio | Qué puede leer Claude | Qué NO puede hacer |
|---|---|---|
| **Outlook** | Hilos de email, patrones de comunicación, búsquedas por remitente/fecha/tema | No envía emails ni crea borradores |
| **Teams** | Chats, mensajes de canal, transcripciones de reuniones, grabaciones, insights de IA | No publica mensajes ni crea reuniones |
| **SharePoint** | Documentos en sitios y bibliotecas según los permisos del usuario | No crea ni edita documentos |
| **OneDrive** | Archivos personales del usuario | No sube ni modifica archivos |
| **Calendar** | Eventos, calendarios compartidos | No crea ni edita eventos |

**Requisito técnico para Qaracter:** Un Administrador Global de Microsoft Entra (antes Azure AD) debe dar consentimiento de tenant antes de que cualquier usuario pueda conectar. Esto crea dos service principals en el tenant de Qaracter: `M365 MCP Server for Claude` y `M365 MCP Client for Claude`.

**Punto importante sobre SharePoint:** Si un sitio de SharePoint tiene activado Restricted Content Discovery (RCD) — una función que muchas empresas usan para limitar el acceso de Microsoft Copilot — esa misma restricción aplica a Claude. Los documentos protegidos con etiquetas de sensibilidad cifradas (Azure Information Protection) tampoco son accesibles.

#### Nivel 2: MCP personalizado sobre Microsoft Graph

Más allá del conector prefabricado, el equipo técnico puede construir servidores MCP personalizados que acceden a Microsoft Graph API para operaciones de **escritura** o para sistemas no cubiertos por el conector estándar:

- **Crear y enviar emails** desde Outlook (no solo leer).
- **Publicar mensajes** en canales de Teams.
- **Crear documentos** en SharePoint o OneDrive.
- **Gestionar tareas** en Microsoft Planner/To-Do.
- **Acceder a Power BI** para consultar dashboards y métricas.
- **Consultar Dynamics 365** para datos de CRM/ERP.

Esto requiere desarrollo técnico (días, no meses) pero amplía radicalmente lo que Claude puede hacer dentro del ecosistema Microsoft.

#### Caso práctico: preparación de comité de dirección

Sin MCP:
> Abres Outlook, buscas los emails relevantes. Abres Teams, buscas las transcripciones de las últimas reuniones. Abres SharePoint, localizas los informes financieros. Abres Excel, cruzas datos manualmente. Tardas 2-3 horas en preparar el briefing.

Con MCP (M365):
> Le dices a Claude: *"Prepárame el briefing del comité de mañana. Revisa los emails de los últimos 15 días con el equipo directivo, las transcripciones de las reuniones de proyecto de esta semana en Teams, y los últimos informes de SharePoint del área financiera. Dame un one-pager con estado, riesgos y decisiones pendientes."*
>
> Claude consulta Outlook, Teams y SharePoint simultáneamente, cruza la información y genera el briefing en 90 segundos. Con citas al email, la transcripción o el documento original de cada afirmación.

---

### 4.2 Atlassian (Jira, Confluence, Rovo)

Atlassian es el ecosistema de gestión de proyectos y conocimiento corporativo de Qaracter. La integración con Claude vía MCP es especialmente potente porque Atlassian ha apostado fuerte por MCP como estándar.

#### Jira — gestión de proyectos y tickets

| Capacidad MCP | Qué hace | Ejemplo |
|---|---|---|
| **Buscar issues** | Consulta tickets con filtros avanzados (JQL) | *"Dame todos los bugs críticos del proyecto Alfa abiertos más de 7 días"* |
| **Leer detalle** | Accede al contenido completo de un ticket: descripción, comentarios, adjuntos, historial | *"Resúmeme la discusión del ticket ALFA-342"* |
| **Crear issues** | Crea tickets nuevos con todos los campos | *"Crea un ticket de tarea en el proyecto Alfa, prioridad alta, asignado a María, con esta descripción…"* |
| **Actualizar issues** | Modifica campos, transiciona estados, añade comentarios | *"Mueve el ticket ALFA-342 a 'En revisión' y añade un comentario con el resumen de la reunión"* |
| **Consultar dashboards** | Lee tableros y métricas de velocity, burndown, etc. | *"¿Cómo va el sprint actual del equipo Backend? ¿Vamos en riesgo de no cumplir?"* |

**Para directivos, el valor principal es doble:**

1. **Visibilidad sin entrar en Jira.** Podéis preguntar a Claude sobre el estado de cualquier proyecto sin necesidad de aprender a navegar Jira, crear filtros JQL o interpretar tableros Kanban.

2. **Acción desde la conversación.** Si durante una reunión decidís que hay que crear un ticket urgente, no necesitáis salir del contexto — Claude lo crea directamente en Jira con todos los campos correctos.

#### Confluence — la wiki corporativa

| Capacidad MCP | Qué hace | Ejemplo |
|---|---|---|
| **Buscar páginas** | Búsqueda semántica en todo el espacio de Confluence accesible | *"Busca en Confluence la política de viajes actualizada"* |
| **Leer páginas** | Accede al contenido completo incluyendo tablas, macros y adjuntos | *"Lee la página de onboarding del área comercial y resúmeme los pasos"* |
| **Crear páginas** | Genera páginas nuevas con formato completo | *"Crea una página de acta de reunión en el espacio del Comité con la información que acabamos de discutir"* |
| **Actualizar páginas** | Edita contenido existente | *"Actualiza la página de estado del proyecto Alfa con los datos del último sprint"* |

**El valor para Qaracter:** Confluence es donde vive el conocimiento corporativo. Sin MCP, ese conocimiento solo es útil si alguien sabe dónde está y se toma el tiempo de buscarlo. Con MCP, Claude puede consultar Confluence como si tuviera toda la wiki en la cabeza — y citaros la fuente exacta.

#### La combinación Jira + Confluence

El poder real aparece cuando Claude puede acceder a **ambos sistemas simultáneamente**:

> *"Revisa el proyecto Alfa en Jira: dame el estado del sprint actual, los tickets bloqueados y sus motivos. Después busca en Confluence si existe documentación sobre los bloqueos técnicos mencionados en los tickets. Prepárame un informe ejecutivo que combine ambas fuentes."*

Claude cruza datos de gestión operativa (Jira) con conocimiento documentado (Confluence) y genera un informe que antes requería que alguien hiciera ese cruce manualmente — abriendo 15 tickets, leyendo 5 páginas de Confluence y sintetizando todo en un documento. Ahora son 2 minutos.

---

### 4.3 Devin — el agente autónomo de desarrollo

Devin es una pieza diferente en el ecosistema. No es una herramienta a la que Claude se conecta, sino un **agente autónomo de desarrollo de software** que Qaracter ya utiliza para tareas técnicas. Devin usa MCP internamente para conectarse a múltiples sistemas:

- **GitHub:** gestión de código, pull requests, revisión de código.
- **Jira/Atlassian:** lectura de tickets para entender requisitos.
- **Herramientas de calidad:** SonarQube, Trivy, Fortify para análisis de seguridad.
- **Confluence:** consulta de documentación técnica.

**Lo que esto significa para directivos:** Cuando un equipo técnico usa Devin para ejecutar una tarea de desarrollo, Devin no trabaja aislado — se conecta vía MCP a todo el ecosistema de herramientas que el equipo ya usa. Lee el ticket de Jira, consulta la documentación en Confluence, escribe el código, crea el pull request en GitHub y ejecuta las verificaciones de calidad. Todo orquestado mediante MCP.

Devin es la prueba tangible dentro de Qaracter de que MCP funciona en producción y a escala, y de que los agentes de IA son más potentes cuantos más sistemas tienen conectados.

---

### 4.4 Otros sistemas relevantes

Más allá de Microsoft y Atlassian, estos son otros sistemas del ecosistema Qaracter donde MCP puede aportar valor:

| Sistema | Tipo de integración | Valor para directivos |
|---|---|---|
| **Salesforce / CRM** | Conector prefabricado + MCP custom | Pipeline comercial, estado de oportunidades, briefings de cuenta |
| **Slack** | Conector prefabricado | Resúmenes de canales, búsqueda de conversaciones, publicación |
| **GitHub** | MCP server | Seguimiento de entregas técnicas, estado de despliegues |
| **Bases de datos internas** | MCP custom | Consultas directas a datos operativos sin necesidad de saber SQL |
| **ERPs propietarios** | MCP custom | Datos financieros, de RRHH o de operaciones accesibles vía lenguaje natural |
| **Herramientas de BI (Power BI)** | MCP custom sobre Microsoft Graph | Métricas de negocio, dashboards, KPIs consultables desde Claude |

---

## 5. Conectores prefabricados vs. MCP: cuándo usar cada uno

Esta es una de las preguntas más prácticas para tomar decisiones informadas:

| Criterio | Conector prefabricado | MCP personalizado |
|---|---|---|
| **Disponibilidad** | Ya existe, se activa con un clic | Hay que construirlo (días a semanas) |
| **Mantenimiento** | Lo mantiene Anthropic | Lo mantiene IT de Qaracter |
| **Coste** | Incluido en la licencia Enterprise | Coste de desarrollo + mantenimiento |
| **Flexibilidad** | Hace lo que Anthropic decide | Hace exactamente lo que necesitamos |
| **Sistemas cubiertos** | ~50 servicios principales | Cualquier sistema con API |
| **Permisos** | OAuth estándar (clic del usuario) | Configurables a medida |
| **Escritura** | Variable (muchos son solo lectura) | Completa si se diseña así |
| **Caso ideal** | Herramientas estándar del mercado | Sistemas internos o necesidades específicas |

**Regla práctica para directivos:**

1. Si el sistema tiene conector prefabricado → **usad el conector**. Es más rápido, más estable y no consume recursos de IT.
2. Si no hay conector pero el sistema tiene API → **evaluad un MCP custom**. Preguntad a IT: ¿cuánto cuesta construirlo? ¿Cuánto tiempo ahorramos?
3. Si el sistema no tiene API → MCP no aplica directamente. Habría que construir una capa intermedia, lo cual es un proyecto mayor.

---

## 6. Casos de uso reales para directivos de Qaracter

Estos son escenarios concretos donde la combinación de Claude + MCP + stack Qaracter genera impacto medible.

### Caso 1: Briefing diario automatizado

**Sin MCP:** El directivo dedica 30-45 minutos cada mañana a revisar emails en Outlook, mensajes en Teams, tickets actualizados en Jira y documentos compartidos en SharePoint. Luego sintetiza mentalmente qué es urgente.

**Con MCP:**

> *"Buenos días, Claude. Dame mi briefing de hoy:*
> *1. Los 5 emails más importantes de ayer en Outlook que requieren mi acción.*
> *2. Mensajes en Teams donde me mencionan o mencionan mis proyectos.*
> *3. Tickets de Jira que cambiaron de estado ayer en mis proyectos.*
> *4. Documenta un resumen en una página nueva de Confluence en mi espacio personal."*

Claude consulta M365 (Outlook + Teams), Atlassian (Jira) y escribe el resultado en Confluence. 3 minutos en lugar de 45. Cada mañana. Multiplicado por 21 directivos = **más de 150 horas mensuales recuperadas** solo con este caso.

### Caso 2: Seguimiento de proyecto cross-funcional

**El problema:** Un Director que supervisa 3 proyectos necesita una vista consolidada semanal. Los datos están repartidos entre Jira (gestión técnica), Confluence (documentación), SharePoint (informes financieros) y Teams (comunicaciones del equipo).

**Con MCP:**

> *"Claude, prepárame el informe semanal del Proyecto Migración OPICS:*
> *- Estado del sprint actual en Jira (tickets completados, bloqueados, en riesgo).*
> *- Últimas actualizaciones en Confluence del equipo.*
> *- Resumen de las conversaciones relevantes en el canal de Teams del proyecto esta semana.*
> *- El documento de seguimiento financiero más reciente en SharePoint.*
> *Formato: informe ejecutivo de 1 página con semáforo (verde/amarillo/rojo) por área y las 3 decisiones más urgentes."*

Claude accede a 4 sistemas simultáneamente, cruza la información y genera un informe ejecutivo con semáforo visual. Con citas a las fuentes originales en cada afirmación.

### Caso 3: Preparación de propuesta comercial

**Con MCP (Jira + Confluence + SharePoint + CRM):**

> *"Tengo que preparar una propuesta para el cliente [X]. Necesito:*
> *1. Busca en Confluence las propuestas anteriores que hayamos hecho a este cliente.*
> *2. Revisa en Jira los proyectos que hemos ejecutado para ellos — alcance, duración, resultado.*
> *3. Mira en SharePoint si hay algún acuerdo marco vigente.*
> *4. En Salesforce/CRM, dame el historial de facturación y el estado de la relación.*
> *5. Con todo eso, genera un borrador de propuesta siguiendo nuestra plantilla estándar de Confluence."*

Lo que antes era medio día de trabajo de búsqueda y compilación se convierte en una conversación de 5 minutos donde Claude hace todo el trabajo de "fontanería documental".

### Caso 4: Onboarding acelerado de un nuevo directivo

**Con MCP:**

> *"Acabo de incorporarme como Director del área [Y]. Necesito ponerme al día rápido:*
> *1. Busca en Confluence la documentación del área: organigramas, procesos, guías.*
> *2. En Jira, muéstrame los proyectos activos con su estado actual.*
> *3. En Outlook, busca los últimos hilos importantes del equipo con dirección.*
> *4. Prepárame un documento de onboarding con: quién es quién, qué proyectos hay, qué está en riesgo, y qué debería preguntar en mi primera semana."*

Un onboarding que normalmente lleva 2-3 semanas de absorción pasiva se acelera enormemente porque Claude puede consumir y sintetizar toda la información disponible en los sistemas de la empresa.

### Caso 5: Auditoría rápida de cumplimiento

**Con MCP (Confluence + Jira + SharePoint):**

> *"Vamos a tener auditoría de calidad el mes que viene. Necesito saber:*
> *1. ¿Están actualizados los procedimientos en Confluence? Dame una lista de los que llevan más de 6 meses sin actualizar.*
> *2. En Jira, ¿hay tickets abiertos de tipo 'incidencia de calidad' sin resolver?*
> *3. En SharePoint, ¿están los informes de revisión por dirección de los últimos 2 trimestres?*
> *4. Genera un informe de gaps que pueda presentar al comité."*

---

## 7. Rovo Agents: la inteligencia nativa de Atlassian

Además de MCP, hay que entender un concepto complementario dentro del ecosistema Atlassian: **Rovo Agents**.

### Qué es Rovo

Rovo es la **capa de inteligencia artificial nativa de Atlassian**. Está integrada directamente en Jira y Confluence y ofrece tres capacidades principales:

| Capacidad | Qué hace | Ejemplo |
|---|---|---|
| **Rovo Search** | Búsqueda semántica que entiende lenguaje natural en todo el contenido Atlassian | *"¿Cuál es nuestra política de trabajo remoto?"* — busca en Confluence sin necesidad de conocer la página exacta |
| **Rovo Chat** | Asistente conversacional dentro de Jira/Confluence que responde preguntas con contexto | *"¿Qué decidimos en la última retrospectiva del equipo Backend?"* — busca en páginas de Confluence y tickets de Jira |
| **Rovo Agents** | Agentes automatizados que ejecutan flujos de trabajo completos | Un agente que automáticamente clasifica tickets nuevos, asigna prioridad y notifica al equipo |

### Rovo Agents vs. Claude + MCP: ¿compiten?

No compiten, se complementan:

| | Rovo Agents | Claude + MCP |
|---|---|---|
| **Dónde viven** | Dentro de Jira/Confluence | En Claude (web, desktop, móvil) |
| **Alcance** | Solo datos Atlassian | Cualquier sistema conectado |
| **Fortaleza** | Automatización de flujos dentro de Atlassian | Análisis cross-sistema, generación de contenido, síntesis |
| **Quién los configura** | Admins de Atlassian | IT de Qaracter + usuarios con Claude Enterprise |
| **Caso ideal** | Automatizar procesos repetitivos en Jira/Confluence | Consultas complejas que cruzan múltiples sistemas |

**La visión para Qaracter:** Usad Rovo Agents para la automatización interna de Atlassian (clasificación automática de tickets, alertas, flujos de aprobación) y Claude + MCP para el trabajo cross-sistema que requiere síntesis inteligente (briefings que cruzan Outlook + Jira + Confluence, informes que combinan datos de SharePoint + Jira).

### Rovo Agents — ejemplos prácticos dentro de Qaracter

1. **Agente de triaje de tickets:** Cada vez que se crea un ticket en un proyecto, el agente Rovo analiza la descripción, asigna prioridad, categoriza por componente y lo enruta al equipo correcto. Sin intervención humana para el 80% de los casos.

2. **Agente de resumen de sprint:** Al finalizar un sprint, el agente genera automáticamente una página en Confluence con el resumen: tickets completados, pendientes, métricas de velocity y recomendaciones para el siguiente sprint.

3. **Agente de Knowledge Base:** Cuando un empleado hace una pregunta repetida (ej: "¿cómo pido vacaciones?"), el agente busca en Confluence la respuesta actualizada y la entrega sin que nadie de RRHH tenga que responder manualmente.

4. **Notebooks de Confluence:** Son páginas especiales que actúan como "memoria persistente" para los agentes. Permiten dar contexto permanente a Rovo o a Claude sobre un proyecto, un cliente o un proceso — de forma que no tengáis que explicar el contexto cada vez.

---

## 8. Seguridad, gobernanza y control directivo

### Modelo de seguridad de MCP: 3 capas

| Capa | Quién la controla | Qué decide |
|---|---|---|
| **1. Administrador de tenant** | IT de Qaracter | Qué servidores MCP están habilitados a nivel organización. Si IT no lo habilita, nadie lo usa. |
| **2. Permisos de usuario** | Cada directivo (OAuth) | Cada persona autoriza individualmente el acceso con sus credenciales. Claude hereda los permisos del usuario, no los del admin. |
| **3. Política de uso** | Dirección + Legal de Qaracter | Qué se puede hacer con los datos accedidos, qué tipos de información no deben pasar por Claude, protocolos de revisión para acciones con efecto externo. |

### Privacidad y tratamiento de datos

Estas son las respuestas a las preguntas que os van a hacer (o que debéis poder responder ante el comité):

| Pregunta | Respuesta |
|---|---|
| ¿Anthropic entrena sus modelos con nuestros datos? | **No.** Claude Enterprise tiene política contractual de no-retención y no-entrenamiento. Los datos accedidos vía MCP o Conectores no se usan para entrenar modelos. |
| ¿Dónde se procesan los datos? | En la infraestructura de Anthropic (AWS), cifrados en tránsito y en reposo. Asociados al chat — si borráis el chat, se borran los datos accedidos. |
| ¿Quién más puede ver mis datos? | Nadie. Ni otros usuarios de Qaracter con Claude, ni Anthropic (salvo en investigaciones de abuso bajo orden legal). |
| ¿Hay logs de auditoría? | Sí. Claude Enterprise genera audit logs de cada acción de conector/MCP. Exportables al SIEM corporativo. |
| ¿Cumplimos GDPR? | Anthropic tiene Data Processing Agreement (DPA) que cumple GDPR. Los datos no salen de regiones autorizadas. |
| ¿Y las regulaciones sectoriales de nuestros clientes? | Depende del sector. Para datos ultra-sensibles (datos bancarios, sanitarios), consultad con Legal antes de usar Conectores/MCP con esa información. |

### Los 5 errores de gobernanza que hay que evitar

**Error 1: Conectar todo el día 1.**
Activar todos los conectores y servidores MCP sin una estrategia de adopción. Genera confusión y potenciales problemas de permisos.
→ **Mejor:** adopción progresiva, empezando por un caso de uso validado.

**Error 2: No definir qué datos NO deben pasar por Claude.**
Si vuestra empresa maneja datos clasificados (defensa, bancarios, sanitarios bajo regulación), necesitáis una política explícita de qué espacios de SharePoint, proyectos de Jira o carpetas de Drive quedan excluidos.
→ **Mejor:** lista blanca de sistemas y espacios autorizados, no lista negra.

**Error 3: Compartir Projects con conectores activos sin pensar.**
Si creáis un Project en Claude con conectores activos y lo compartís con alguien, esa persona ejecuta las consultas con sus propios permisos (no los vuestros). Pero el contexto de la conversación SÍ se comparte — y podría contener información sensible que Claude extrajo con vuestros permisos.
→ **Mejor:** Projects con conectores = privados por defecto.

**Error 4: Asumir que Claude siempre tiene razón.**
Claude puede malinterpretar un email irónico, confundir dos clientes con nombre similar o no detectar un cambio reciente que aún no se ha indexado. Los conectores y MCP mejoran el acceso a la información, no eliminan la necesidad de juicio humano.
→ **Mejor:** revisad siempre las fuentes que Claude cita, especialmente en decisiones de alto impacto.

**Error 5: No medir el impacto.**
Si no medís cuánto tiempo ahorráis con las integraciones, no podéis justificar la inversión ni ampliar la adopción.
→ **Mejor:** cada directivo registra 1 caso de uso semanal con el tiempo ahorrado estimado. En 4 semanas tenéis datos reales para el comité.

---

## 9. Hoja de ruta y visión de futuro

### Dónde estamos hoy (mayo 2026)

```
Conectores prefabricados ───────────── ✅ Disponibles
├─ Google Workspace (Gmail, Calendar,
│  Drive)                                    Lectura + escritura parcial
├─ Microsoft 365 (Outlook, Teams,
│  SharePoint, OneDrive, Calendar)           Solo lectura
├─ Salesforce, HubSpot, Jira, Slack,
│  DocuSign, Notion, Confluence…             Lectura + escritura variable
│
MCP personalizado ──────────────────── ✅ Disponible
├─ Cualquier sistema con API                 Lectura + escritura completa
├─ Requiere desarrollo por IT                Días a semanas por integración
│
Rovo Agents (Atlassian) ───────────── ✅ Disponibles
├─ Automatización dentro de Jira/Confluence  Configurables por admins
├─ Complementarios a Claude + MCP
│
Agentes autónomos (Devin) ─────────── ✅ En uso en Qaracter
├─ Tareas de desarrollo de software          Usa MCP internamente
└─ Orquestación multi-herramienta
```

### Hacia dónde va esto

1. **Más conectores con escritura.** El conector M365 es hoy solo lectura. Es razonable esperar que Anthropic amplíe capacidades de escritura progresivamente (primero borradores de email, luego eventos de calendar).

2. **MCP como estándar de industria.** MCP ya es adoptado por Google (Gemini), OpenAI, Microsoft y otros. Esto significa que los servidores MCP que IT construya para Claude también funcionarán con otros modelos de IA — no es inversión cautiva.

3. **Agentes más autónomos.** La tendencia es que los agentes como Claude o Devin ejecuten flujos completos (recibir ticket → entender requisito → ejecutar → entregar) con supervisión humana en puntos de decisión, no en cada paso.

4. **Integración nativa Claude + Atlassian.** Atlassian ya tiene un marketplace de MCP servers oficiales. Es cuestión de tiempo que Claude aparezca como integración nativa dentro de Jira y Confluence, sin necesidad de ir a la web de Claude.

5. **Personalización por rol.** Cada directivo tendrá un perfil de conectores y servidores MCP configurado para su rol específico. Un Director Comercial tendrá CRM + email + calendar; un Director de Tecnología tendrá Jira + GitHub + herramientas de calidad.

---

## 10. Resumen ejecutivo y puntos clave

### Las 5 ideas que debéis llevaros de esta sesión

**1. MCP es el estándar que conecta la IA con vuestros sistemas reales.**
Los Conectores prefabricados cubren Google, Microsoft y las grandes plataformas. MCP permite conectar Claude con cualquier sistema que tenga API — incluidos los internos y propietarios de Qaracter.

**2. El valor de Claude se multiplica con cada sistema conectado.**
Claude sin integraciones es un asistente inteligente pero ciego. Claude con MCP es un asistente inteligente con acceso a todo el conocimiento y las herramientas de la empresa. La diferencia no es incremental, es transformacional.

**3. El stack de Qaracter ya es compatible.**
Microsoft 365, Atlassian (Jira, Confluence), Slack, GitHub — todos tienen integraciones disponibles o construibles vía MCP. No es ciencia ficción: es configuración.

**4. La seguridad está diseñada para entornos corporativos.**
Permisos heredados, consentimiento explícito, revocabilidad, audit logs, no-entrenamiento con datos corporativos. El modelo de seguridad de MCP está pensado para que Legal y Compliance digan que sí.

**5. Empezad por un caso, medid y ampliad.**
No intentéis conectar todo el día 1. Elegid un caso de uso que os duela (briefings de reunión, seguimiento de proyectos, preparación de propuestas), validadlo durante 2 semanas, medid el ahorro de tiempo y usad esos datos para justificar la siguiente ampliación.

### La regla de oro

> **MCP y los Conectores son herramientas de aceleración, no de sustitución del juicio directivo. Eliminan la fontanería entre sistemas para que podáis dedicar vuestro tiempo a lo que realmente importa: decidir, liderar y crear valor.**

---

## 11. Anexo — Glosario para directivos

| Término | Definición |
|---|---|
| **MCP (Model Context Protocol)** | Estándar abierto que define cómo un modelo de IA se comunica con herramientas externas. Creado por Anthropic, adoptado por la industria. |
| **Conector (Connector)** | Integración prefabricada entre Claude y un servicio externo (Gmail, Jira, etc.). Se activa con un clic. Es una implementación concreta de MCP. |
| **Servidor MCP (MCP Server)** | El componente que "habla" con el sistema externo. Puede ser prefabricado (publicado por Anthropic o el proveedor) o personalizado (construido por IT). |
| **OAuth** | Protocolo estándar de autorización. Es lo que veis cuando dais permiso a una app para acceder a vuestra cuenta de Google o Microsoft. Seguro y revocable. |
| **Tenant** | La instancia corporativa de un servicio. El "tenant de Qaracter en Microsoft" es el entorno M365 de toda la empresa. |
| **Rovo** | La capa de IA nativa de Atlassian, integrada en Jira y Confluence. Incluye búsqueda inteligente, chat y agentes automatizados. |
| **Rovo Agent** | Un agente automatizado dentro de Atlassian que ejecuta flujos de trabajo (triaje de tickets, resúmenes, clasificación). |
| **Notebook (Confluence)** | Página especial de Confluence que actúa como memoria persistente para agentes de IA. Proporciona contexto sin tener que repetirlo en cada conversación. |
| **JQL** | Jira Query Language. El lenguaje de consultas de Jira. Con MCP, Claude escribe las consultas JQL por vosotros — no necesitáis aprenderlo. |
| **API** | Application Programming Interface. La "puerta" técnica que permite a un sistema comunicarse con otro. MCP usa las APIs de cada herramienta. |
| **Microsoft Graph** | La API unificada de Microsoft que da acceso a Outlook, Teams, SharePoint, OneDrive, Calendar, Planner, etc. El conector M365 y los MCP custom usan Microsoft Graph internamente. |
| **Service Principal** | Una identidad técnica (no humana) que representa a una aplicación en Microsoft Entra. Es lo que permite a Claude acceder al tenant M365 de Qaracter. |
| **RCD (Restricted Content Discovery)** | Función de Microsoft que restringe qué contenido es descubrible por herramientas de IA (Copilot, Claude). Si está activo en un sitio de SharePoint, Claude no puede acceder. |
| **AIP (Azure Information Protection)** | Etiquetas de sensibilidad cifradas de Microsoft. Los documentos protegidos con AIP cifrado no son accesibles por Claude. |
| **Audit log** | Registro de todas las acciones realizadas. En Claude Enterprise, cada consulta a un conector/MCP queda registrada con quién, cuándo, a qué sistema y qué datos. |
| **ACU (Agent Compute Unit)** | Unidad de consumo de recursos de agentes de IA. Métrica para medir y presupuestar el uso de agentes como Devin. |
| **Devin** | Agente autónomo de desarrollo de software utilizado en Qaracter. Usa MCP internamente para conectarse a GitHub, Jira, Confluence y herramientas de calidad. |
| **Playbook** | Plantilla de instrucciones reutilizable para agentes de IA. Define cómo ejecutar una tarea específica de forma consistente. |

---

## 12. Anexo — Fuentes y recursos

### Documentación oficial de Anthropic
- [Use Google Workspace connectors — Claude Help Center](https://support.claude.com/en/articles/10166901-use-google-workspace-connectors)
- [Enable and use the Microsoft 365 connector — Claude Help Center](https://support.claude.com/en/articles/12542951-enable-and-use-the-microsoft-365-connector)
- [Model Context Protocol — Especificación oficial](https://modelcontextprotocol.io/)
- [Trust Center de Anthropic](https://trust.anthropic.com/) — seguridad y compliance

### Microsoft
- [M365 Connector for Claude — Microsoft Marketplace](https://marketplace.microsoft.com/en-us/product/saas/anthropic.microsoft-365-connector-for-claude)
- [Microsoft Graph API Documentation](https://learn.microsoft.com/en-us/graph/overview)
- [Using the Microsoft 365 Connector for Claude — Office 365 IT Pros](https://office365itpros.com/2026/04/08/microsoft-365-connector-for-claude/)

### Atlassian
- [Rovo — Atlassian Intelligence](https://www.atlassian.com/software/rovo)
- [Atlassian MCP Servers — Marketplace](https://marketplace.atlassian.com/)
- [Confluence Notebooks Documentation](https://support.atlassian.com/confluence-cloud/)

### Análisis de terceros
- [Claude AI Connectors: One-Click Tool Integrations 2026](https://max-productive.ai/blog/claude-ai-connectors-guide-2025/)
- [Claude Cowork Connectors Guide](https://claudeimplementation.com/blog-claude-cowork-connectors-guide)

### Recursos internos Qaracter
- Programa "Claude para Liderazgo" v4.0 — documento base del workshop
- Contenido Teórico del programa de Agent Engineer — módulos sobre MCP y Rovo
- Lista de impacto personal (W1) — referencia para elegir el primer caso de uso

---

*Documento preparado para el programa "Claude para Liderazgo" — Qaracter, Mayo 2026.*
*Versión 1.0 — Contenido detallado sobre MCP e integraciones, alineado con el stack corporativo de Qaracter (Microsoft 365, Atlassian, herramientas de desarrollo).*
