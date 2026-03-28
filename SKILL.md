---
name: prompt-forge
description: Use before executing any technical or complex task to verify, structure, and elevate prompt quality using the TCRO framework, ensuring professional standards and specific output indicators — covers building, refactoring, debugging, explaining, designing, and training tasks
---

# Prompt Forge

## Overview

Un prompt vago produce resultados genéricos. Esta skill hace dos cosas simultáneamente:

1. **Eleva el prompt** — toma la intención del usuario y la convierte en un prompt completo, ejecutable y de alta calidad usando defaults de mejores prácticas
2. **Educa al usuario** — explica qué componentes faltaban y por qué cada uno importa, para que el usuario aprenda a construir mejores prompts con el tiempo

**Principio central:** No interrogues al usuario con preguntas abiertas. Propón un prompt elevado y completo, luego pide confirmación.

**Violating the letter of this process is violating the spirit of quality.**

## La Ley de Hierro

```
NINGUNA TAREA COMPLEJA SE EJECUTA SIN VERIFICACIÓN TCRO PREVIA
```

Si detectaste al menos un síntoma y no ejecutaste el proceso, violaste la disciplina.

## When to Use

**Activar SIEMPRE ante cualquier tarea técnica**, independientemente de si el prompt parece completo o no. El objetivo no es solo reparar prompts rotos — es elevar incluso los buenos.

Tipos de tarea que siempre activan esta skill:
- **Construir**: componentes, APIs, scripts, modelos, páginas, sistemas
- **Refactorizar**: mejorar, limpiar, optimizar, reestructurar código existente
- **Depurar**: arreglar bugs, diagnosticar errores, investigar comportamientos
- **Explicar**: describir código, arquitecturas, conceptos técnicos complejos
- **Diseñar**: arquitecturas, esquemas de datos, flujos, sistemas
- **Entrenar / configurar**: modelos ML, pipelines, entornos, infraestructura

**No activar únicamente en:**
- Preguntas triviales de configuración de la herramienta (`¿cómo abro un archivo?`)
- Comandos de navegación de archivos (`¿qué hay en esta carpeta?`)
- Charla casual sin intención técnica (`¿recuerdas la skill que creamos?`)

**Regla de elevación:** Incluso si el prompt ya tiene T, C, O y R, la skill interviene para enriquecer — sugerir mejores prácticas de seguridad, esquemas más robustos, estándares adicionales que el usuario no consideró.

## El Proceso TCRO

### Paso 1 — Analiza
Evalúa el prompt del usuario contra los 4 componentes TCRO.
Identifica cuáles están ausentes o son débiles.

### Paso 2 — Reconstruye

**Tres reglas de oro (promptingguide.ai):**
1. **Separadores `###`** — delimitan instrucciones de contextos
2. **Tarea primero** — el componente T va siempre al inicio del prompt refinado
3. **Few-shot en Output** — si la tarea es compleja, incluir ejemplo de salida esperada

**Verificación de Atomicidad — ANTES de reconstruir:**
Evalúa si la tarea es atómica o compuesta. Si el prompt implica múltiples sub-tareas independientes (ej: "haz un sistema completo con auth, dashboard y pagos"), NO reconstruyas en un solo bloque. En su lugar, interviene en el diagnóstico con:

> "⚠ Esta tarea es compuesta — contiene [N] sub-tareas independientes. Recomiendo dividirla en prompts atómicos encadenados (Prompt Chaining) para mayor precisión y control:
> 1. [Sub-tarea 1]
> 2. [Sub-tarea 2]
> 3. [Sub-tarea N]
> ¿Arrancamos por cuál?"

Regla: una tarea es compuesta si tiene más de una responsabilidad que podría fallar independientemente.

**Más cinco Directivas Técnicas obligatorias:**

**Directiva 1 — Protocolo de Descubrimiento Activo (aplica a C):**

El objetivo es nunca adivinar el entorno — siempre descubrirlo o declarar las suposiciones explícitamente.

**Paso 1 — Buscar archivos de configuración conocidos:**
`tsconfig.json`, `package.json`, `pom.xml`, `build.gradle`, `pyproject.toml`, `Cargo.toml`, `.nvmrc`, `docker-compose.yml`, `Dockerfile`, `terraform.tf`, `requirements.txt`, `go.mod`

**Paso 2 — Si el dominio no es frontend/backend/scripts estándar, exploración activa:**
Buscar en el directorio del proyecto palabras clave relacionadas con la tarea (ej: para ML buscar `model`, `train`, `dataset`; para DB buscar `schema`, `migration`, `query`; para DevOps buscar `pipeline`, `deploy`, `service`).

**Tres casos de resultado:**

- **Archivos de config encontrados:** usa el ecosistema detectado como fuente de verdad. Si hay discrepancia con el prompt, señalar con `⚠`.

- **Dominio conocido sin archivos** (frontend/backend/mobile/scripts): propón el stack moderno estándar:
  - Frontend/web → React 18 + TypeScript + Vite
  - API/backend → Node.js + Express + TypeScript
  - Script/automatización → Python 3.11+
  - Mobile → React Native + TypeScript
  - Full-stack → Next.js 14 + TypeScript + Tailwind

- **Dominio desconocido sin archivos** (ML, DevOps, arquitectura, DB, etc.): activa el **Fallback de Especificidad** — añade en `### Restricciones` la siguiente directiva obligatoria:
  > "Antes de entregar cualquier código o solución, el modelo debe listar explícitamente las suposiciones técnicas que está haciendo sobre [Dominio X]: versiones, herramientas, entorno de ejecución y dependencias asumidas."

**Directiva 2 — Definition of Done en Output (aplica a O):**
El componente Output siempre incluye los 3 elementos del DoD:
- **Formato**: estructura exacta (JSON, Markdown, función, clase, bloque de código, etc.)
- **Estándar de documentación**: JSDoc / docstrings / comentarios inline / ninguno
- **Estrategia de error handling**: excepciones tipadas / valores por defecto / logging / ninguno

Para tareas de tipo **bug-fix o diagnóstico**, añadir un 4º elemento obligatorio:
- **Diagnóstico previo**: 3-5 líneas explicando la causa raíz antes del código corregido.
  Regla: nunca entregar un fix sin identificar primero qué estaba mal y por qué.

**Inferencia de Formato para dominio desconocido:**
Si el Fallback de Especificidad fue activado (dominio no reconocido), el Output por defecto es siempre:
- **Código comentado**: cada bloque no obvio con comentario inline explicando el qué y el por qué
- **Guía de ejecución**: pasos numerados para correr la solución desde cero
- **Requisitos de entorno**: lista de dependencias, versiones y comandos de instalación necesarios

**Directiva 3 — Conversión de Restricciones + Rol Dinámico (aplica a R):**
Toda prohibición del prompt original debe convertirse en arquitectura alternativa concreta.

```
Regla estricta: "no X" → "usa Y en su lugar"
Nunca dejar una restricción en negativo.
```

Ejemplos de conversión:
- "no uses async/await" → "usa Promises con `.then()/.catch()`"
- "no uses clases" → "usa funciones puras y hooks de React"
- "no uses Redux" → "usa Context API + useReducer para estado global"

Cuando hay múltiples restricciones, verificar **consistencia arquitectural**:
las restricciones convertidas deben formar una arquitectura coherente, no directivas que se contradigan.
Si dos restricciones convertidas son incompatibles, señalarlo con `⚠` al usuario antes de presentar el refinamiento.

**Rol Dinámico — asignación obligatoria según dominio detectado:**
Todo prompt elevado debe comenzar `### Restricciones / Rol` con un rol experto específico al dominio:

| Dominio detectado | Rol a asignar |
|-------------------|---------------|
| Frontend / web | Actúa como Senior Frontend Engineer especializado en rendimiento y accesibilidad web |
| API / backend | Actúa como Senior Backend Engineer especializado en arquitecturas REST seguras y escalables |
| Full-stack | Actúa como Full-Stack Architect con foco en separación de responsabilidades y DX |
| ML / IA | Actúa como Machine Learning Engineer especializado en pipelines reproducibles y evaluación de modelos |
| DevOps / infraestructura | Actúa como DevOps Engineer especializado en infraestructura como código y pipelines CI/CD seguros |
| Base de datos | Actúa como Database Engineer especializado en optimización de queries y diseño de esquemas |
| Mobile | Actúa como Mobile Engineer especializado en rendimiento nativo y UX en dispositivos |
| Dominio desconocido | Actúa como Senior Software Engineer con criterio para identificar y declarar suposiciones técnicas antes de implementar |

El rol se escribe como primera línea de `### Restricciones / Rol`, seguido de las demás restricciones.

**Chain-of-Thought (CoT) — instrucción obligatoria en R:**
Todo prompt elevado debe incluir como última línea de `### Restricciones / Rol`:
> `"Think step-by-step before providing the technical solution."`

Esto garantiza que el razonamiento lógico preceda a la generación de código, reduciendo errores en arquitecturas complejas. No es opcional — va en todos los prompts técnicos sin excepción.

**Verificabilidad de versiones — instrucción obligatoria en R:**
Cuando el prompt elevado menciona librerías, frameworks o herramientas con versión específica, añadir en `### Restricciones / Rol`:
> `"If a specific library or framework version is suggested, ensure it exists in official documentation before using it."`

Esta directiva previene alucinaciones de versiones inexistentes, especialmente en dominios desconocidos o stacks emergentes.

**Directiva 4 — Blindaje de Casos de Borde (aplica a T y O):**
Todo prompt elevado debe anticipar explícitamente los estados de error e inputs inválidos relevantes para la tarea.

Regla: antes de cerrar el prompt elevado, identificar y agregar en `### Output` los casos de borde obligatorios según el tipo de tarea:

| Tipo de tarea | Casos de borde a cubrir obligatoriamente |
|---------------|------------------------------------------|
| API / endpoints | Input inválido (422), no autenticado (401), no autorizado (403), recurso no encontrado (404), límite de tasa (429) |
| Formularios / UI | Campos vacíos, formatos inválidos, respuesta de servidor lenta, estado de carga, error de red |
| Funciones / scripts | Argumentos nulos, tipos incorrectos, archivos inexistentes, permisos insuficientes, timeouts |
| ML / modelos | Dataset vacío, features faltantes, valores fuera de rango, modelo no convergente |
| DB / queries | Registros duplicados, foreign key violations, conexión caída, query sin resultados |
| Infraestructura | Recursos ya existentes, permisos IAM insuficientes, límites de cuota, estado de drift |

Si la tarea no encaja en ninguna categoría, agregar como mínimo: `input nulo`, `estado inesperado del sistema` y `fallo de dependencia externa`.

**Directiva 5 — Delimitación Estricta de Inputs (aplica a T y C):**
Cuando la tarea implica analizar, refactorizar o corregir código o datos existentes, el prompt elevado debe envolver ese material en etiquetas explícitas para evitar que el modelo confunda las instrucciones con el contenido a procesar.

Etiquetas obligatorias según el tipo de input:

| Tipo de input | Etiqueta a usar |
|---------------|----------------|
| Código a analizar / refactorizar | `<context_code> ... </context_code>` |
| Datos de entrada (JSON, CSV, etc.) | `<input_data> ... </input_data>` |
| Esquema de base de datos | `<db_schema> ... </db_schema>` |
| Logs o trazas de error | `<error_logs> ... </error_logs>` |
| Documentación de referencia | `<reference_docs> ... </reference_docs>` |

Regla de posición: las etiquetas van SIEMPRE dentro de `### Contexto`, después de la descripción del ecosistema y antes de las restricciones. Las instrucciones de qué hacer van en `### Tarea` — nunca mezcladas con el contenido delimitado.

Ejemplo de uso correcto:
```
### Contexto
Stack: Node.js 20 + TypeScript 5

<context_code>
// código existente que debe ser refactorizado
function login(u, p) { ... }
</context_code>
```

### Estructura del prompt refinado

```
### Tarea
[Verbo de acción + instrucción atómica y específica]

### Contexto
[Ecosistema detectado en archivos de config + entorno técnico confirmado]
[⚠ Si hay discrepancia: "Detecté X en el proyecto pero mencionaste Y — ¿cuál aplica?"]

### Restricciones / Rol
[Toda prohibición convertida a directiva positiva con arquitectura alternativa]
[Estándar técnico si aplica: TDD, Clean Code, SOLID, etc.]

### Output
[Formato exacto de entrega]
[Estándar de documentación]
[Estrategia de error handling]
[Ejemplo few-shot si la tarea es compleja]
```

### Paso 3 — Eleva, educa y presenta

Muestra en este orden exacto:

**A) Diagnóstico breve** — tabla de qué faltaba y por qué importa:

| Componente | Estado | Lo que faltaba | Por qué importa |
|------------|--------|---------------|-----------------|
| Tarea | ⚠ | Verbo sin objeto técnico específico | Sin objeto concreto el modelo adivina el alcance |
| Contexto | ✗ | Stack no confirmado | Sin stack el modelo elige tecnologías arbitrariamente |
| Output | ✗ | Sin formato ni DoD | Sin formato el resultado puede ser inutilizable |
| Restricciones | — | No aplica / [lo que faltaba] | [por qué importa] |

**B) Prompt elevado** — completo, ejecutable, sin placeholders:

```
### Tarea
[Verbo específico + objeto técnico concreto + alcance definido]

### Contexto
[Stack propuesto o detectado — si es propuesta, indicar "Propongo X — ajusta si usas otro"]
[Propósito, entorno, dependencias relevantes]

### Restricciones / Rol
[Directivas positivas + estándares técnicos aplicables]

### Output
[Formato exacto]
[Estándar de documentación]
[Estrategia de error handling]
[Ejemplo few-shot si la tarea es compleja]
```

**C) Pregunta de confirmación** — una sola línea:
> "¿Procedo con esta versión o ajustamos algo?"

**Regla de presentación:** El prompt elevado debe ser ejecutable tal como está. Si el usuario dice "sí", debe poder ejecutarse sin modificaciones adicionales.

### Paso 4 — Espera aprobación
NO ejecutes la tarea hasta recibir confirmación del usuario.
Si el usuario ajusta algo → vuelve al Paso 2 con los cambios.
Solo cuando el usuario apruebe → procede con la ejecución.

## Errores Comunes

| Error | Realidad |
|-------|----------|
| "El prompt tiene verbo, por lo tanto T está cubierto" | Un verbo genérico ('haz', 'crea', 'arregla') sin objeto técnico específico = T ausente. 'Haz una página web' no define stack, alcance ni tipo de página. |
| "El prompt es corto pero claro" | Corto ≠ claro. Verifica los 4 componentes + DoD. |
| "Asumo React/Node porque es lo más popular" | Detecta el ecosistema. No asumas jamás. |
| "Las restricciones negativas son suficiente dirección" | Conviértelas siempre a arquitectura alternativa. |
| "Puedo cubrir los casos más comunes de error" | Sin DoD, "casos comunes" = adivinanza. |
| "El usuario puede adaptar después" | Adaptar después = iteraciones desperdiciadas. |
| "Preguntar parece ineficiente" | Adivinar es más ineficiente que una pregunta. |
| "La tarea es simple, no necesita output estructurado" | Si es compleja para el modelo, necesita DoD. |

## Señales de Alerta — PARA y activa la skill

- Estás a punto de asumir el lenguaje o framework sin buscar archivos de config
- El prompt dice "no hagas X" y no tienes un "haz Y en su lugar" concreto
- No tienes los 3 elementos del DoD (formato + documentación + error handling)
- La tarea tiene más de una responsabilidad (no es atómica)
- Estás pensando "el usuario puede adaptar esto"
