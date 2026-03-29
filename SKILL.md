---
name: prompt-forge
description: Use when receiving any technical task — building, refactoring, debugging, explaining, designing, or configuring — before executing, regardless of whether the prompt seems complete or not
---

# Prompt Forge

## Overview

Un prompt vago produce resultados genéricos. Esta skill hace dos cosas simultáneamente:

1. **Eleva el prompt** — convierte la intención del usuario en un prompt completo y ejecutable usando defaults de mejores prácticas
2. **Educa al usuario** — explica qué componentes faltaban y por qué, para que mejore con el tiempo

**Principio central:** No interrogues con preguntas abiertas. Propón un prompt elevado y completo, luego pide confirmación.

**Violating the letter of this process is violating the spirit of quality.**

## La Ley de Hierro

```
NINGUNA TAREA COMPLEJA SE EJECUTA SIN VERIFICACIÓN TCRO PREVIA
```

## When to Use

**Activar SIEMPRE ante cualquier tarea técnica**, sin importar si el prompt parece completo.

Activa en: construir, refactorizar, depurar, explicar, diseñar, entrenar/configurar.

**No activar en:** preguntas triviales de herramienta, navegación de archivos, charla casual.

**Regla de elevación:** Incluso prompts bien formados se enriquecen con mejores prácticas de seguridad, arquitectura y estándares que el usuario no consideró.

## El Proceso TCRO

Los 4 componentes que todo prompt debe tener:

| Componente | Qué define |
|------------|------------|
| **T**area | Verbo específico + objeto técnico concreto + alcance |
| **C**ontexto | Stack real, versiones, entorno, propósito |
| **R**estricciones | Directivas positivas + rol experto + CoT |
| **O**utput | Formato exacto + DoD + casos de borde |

### Paso 1 — Analiza
Evalúa el prompt contra los 4 componentes. Identifica ausencias y debilidades.

### Paso 2 — Reconstruye

**Verificación de Atomicidad — ANTES de reconstruir:**
Si la tarea tiene múltiples sub-tareas independientes, NO reconstruyas en un bloque. Interviene con:

> "⚠ Tarea compuesta — recomiendo dividirla en prompts atómicos encadenados (Prompt Chaining):
> 1. [Sub-tarea 1]
> 2. [Sub-tarea 2]
> ¿Arrancamos por cuál?"

**Aplicar las 5 Directivas Técnicas** (ver `references/directives.md`):
1. Protocolo de Descubrimiento Activo — detectar ecosistema real antes de proponer stack
2. Definition of Done — formato + documentación + error handling en Output
3. Conversión de Restricciones + Rol Dinámico + CoT + Verificabilidad
4. Blindaje de Casos de Borde — anticipar fallos por tipo de tarea
5. Delimitación de Inputs — envolver código/datos en etiquetas dentro de Contexto

Para roles por dominio y stacks estándar → ver `references/roles-and-defaults.md`

**Estructura del prompt elevado:**
```
### Tarea
[Verbo específico + objeto concreto + alcance]

### Contexto
[Stack detectado o propuesto]
[Propósito, entorno, dependencias]

### Restricciones / Rol
[Rol experto del dominio]
[Directivas positivas + estándares técnicos]
Think step-by-step before providing the technical solution.

### Output
[Formato exacto]
[Estándar de documentación]
[Error handling]
[Casos de borde]
[Ejemplo few-shot si aplica]
```

### Paso 3 — Eleva, educa y presenta

Mostrar en este orden:

**A) Diagnóstico** — tabla de componentes: estado / lo que faltaba / por qué importa

**B) Prompt elevado** — completo, ejecutable, sin placeholders

**C) Confirmación:**
> "¿Procedo con esta versión o ajustamos algo?"

El prompt elevado debe ser ejecutable tal como está. Si el usuario dice "sí", se ejecuta sin cambios adicionales.

### Paso 4 — Espera aprobación
NO ejecutes hasta recibir confirmación. Si el usuario ajusta → vuelve al Paso 2. Solo al aprobar → ejecuta.

## Errores Comunes

| Error | Realidad |
|-------|----------|
| "El prompt tiene verbo, T está cubierto" | Verbo genérico sin objeto técnico = T ausente |
| "El prompt es corto pero claro" | Corto ≠ claro. Verifica los 4 componentes |
| "Asumo React/Node por ser popular" | Detecta el ecosistema. No asumas jamás |
| "Las restricciones negativas son suficientes" | Conviértelas a arquitectura alternativa |
| "El usuario puede adaptar después" | Adaptar después = iteraciones desperdiciadas |
| "La tarea es simple, no necesita DoD" | Si es compleja para el modelo, necesita DoD |

## Señales de Alerta — PARA y activa la skill

- Estás a punto de asumir stack sin buscar archivos de config
- El prompt dice "no hagas X" sin un "haz Y en su lugar"
- No tienes los 3 elementos del DoD
- La tarea tiene más de una responsabilidad independiente
- Estás pensando "el usuario puede adaptar esto"
