# Las 5 Directivas Técnicas Obligatorias

Aplicar en el Paso 2 (Reconstruye) de cada prompt elevado.

---

## Directiva 1 — Protocolo de Descubrimiento Activo (aplica a C)

Nunca adivinar el entorno — siempre descubrirlo o declarar las suposiciones.

**Paso 1 — Buscar archivos de configuración:**
`tsconfig.json`, `package.json`, `pom.xml`, `build.gradle`, `pyproject.toml`, `Cargo.toml`, `.nvmrc`, `docker-compose.yml`, `Dockerfile`, `terraform.tf`, `requirements.txt`, `go.mod`

**Paso 2 — Exploración activa para dominios no estándar:**
Buscar palabras clave en el proyecto: ML → `model, train, dataset` / DB → `schema, migration, query` / DevOps → `pipeline, deploy, service`

**Tres casos:**
- **Archivos encontrados** → usa ecosistema real. Discrepancia con el prompt → señalar con `⚠`
- **Dominio conocido sin archivos** → proponer stack estándar (ver `roles-and-defaults.md`)
- **Dominio desconocido sin archivos** → activar Fallback de Especificidad: añadir en R: *"Antes de entregar, listar explícitamente las suposiciones técnicas sobre [Dominio]: versiones, herramientas, entorno y dependencias."* Y en O: código comentado + guía de ejecución + requisitos de entorno.

---

## Directiva 2 — Definition of Done en Output (aplica a O)

El componente Output siempre incluye los 3 elementos del DoD:
- **Formato**: estructura exacta (JSON, función, clase, bloque de código, etc.)
- **Documentación**: JSDoc / docstrings / comentarios inline / ninguno
- **Error handling**: excepciones tipadas / valores por defecto / logging / ninguno

Para **bug-fix o diagnóstico**, añadir 4º elemento obligatorio:
- **Diagnóstico previo**: 3-5 líneas de causa raíz antes del código corregido. Nunca entregar fix sin identificar qué estaba mal y por qué.

---

## Directiva 3 — Conversión de Restricciones + Rol + CoT + Verificabilidad (aplica a R)

**Conversión de prohibiciones:**
```
"no X" → "usa Y en su lugar"
Nunca dejar restricción en negativo.
```
Ejemplos: `"no uses Redux"` → `"usa Context API + useReducer"` / `"no uses clases"` → `"usa funciones puras y hooks"`

Si múltiples restricciones son incompatibles entre sí → señalar con `⚠` antes de presentar.

**Rol Dinámico** — primera línea de `### Restricciones / Rol` (ver tabla completa en `roles-and-defaults.md`)

**Chain-of-Thought — última línea obligatoria en R:**
> `Think step-by-step before providing the technical solution.`

**Verificabilidad — cuando hay versiones específicas:**
> `If a specific library or framework version is suggested, ensure it exists in official documentation before using it.`

---

## Directiva 4 — Blindaje de Casos de Borde (aplica a O)

Agregar en `### Output` los casos de borde según tipo de tarea (ver tabla completa en `roles-and-defaults.md`).

Mínimo universal si el tipo no encaja en ninguna categoría: `input nulo`, `estado inesperado del sistema`, `fallo de dependencia externa`.

---

## Directiva 5 — Delimitación de Inputs (aplica a C)

Cuando la tarea implica analizar o refactorizar código/datos existentes, envolver el material en etiquetas dentro de `### Contexto`:

| Tipo de input | Etiqueta |
|---------------|---------|
| Código a analizar | `[context_code] ... [/context_code]` |
| Datos (JSON, CSV) | `[input_data] ... [/input_data]` |
| Esquema de DB | `[db_schema] ... [/db_schema]` |
| Logs / trazas | `[error_logs] ... [/error_logs]` |
| Documentación | `[reference_docs] ... [/reference_docs]` |

Las instrucciones van en `### Tarea` — nunca mezcladas con el contenido delimitado.
