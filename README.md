# prompt-forge

> Skill para Claude Code que eleva cualquier prompt técnico al estándar profesional antes de ejecutarlo. Implementa el stack completo de técnicas avanzadas de [promptingguide.ai](https://www.promptingguide.ai).

---

## ¿Qué es?

**prompt-forge** es una disciplina de verificación y elevación de prompts técnicos para [Claude Code](https://claude.ai/code). No es un corrector de prompts vagos — es un estándar de calidad que intercepta **cualquier tarea técnica** antes de ejecutarla y la convierte en un prompt profesional, estructurado y blindado.

Hace dos cosas simultáneamente:

1. **Eleva** — aplica mejores prácticas de seguridad, arquitectura y documentación que el usuario puede no haber considerado
2. **Educa** — muestra exactamente qué faltaba en el prompt original y por qué, para que el usuario mejore con el tiempo

---

## Instalación

```bash
# Crear el directorio de la skill
mkdir -p ~/.claude/skills/prompt-forge

# Copiar el archivo de la skill
cp SKILL.md ~/.claude/skills/prompt-forge/SKILL.md
```

La skill se carga automáticamente en todas las sesiones de Claude Code a partir de ese momento.

---

## Cuándo se activa

**Siempre** ante cualquier tarea técnica, sin importar si el prompt parece completo o no:

| Se activa | No se activa |
|-----------|-------------|
| Construir: APIs, páginas, componentes, scripts, sistemas | `¿cómo abro un archivo?` |
| Refactorizar: limpiar, optimizar, reestructurar | `¿qué hay en esta carpeta?` |
| Depurar: bugs, errores, comportamientos inesperados | Charla casual sin intención técnica |
| Diseñar: arquitecturas, esquemas de datos, flujos | |
| Entrenar / configurar: modelos ML, pipelines, infraestructura | |
| Explicar: código, arquitecturas, conceptos complejos | |

---

## El Framework TCRO

Todo prompt se evalúa y eleva contra 4 componentes:

| Componente | Qué define | Sin él... |
|------------|------------|-----------|
| **T**area | Verbo específico + objeto técnico concreto + alcance | El modelo adivina el alcance |
| **C**ontexto | Stack real, versiones, entorno, propósito | El modelo elige tecnologías arbitrariamente |
| **R**estricciones | Directivas positivas + rol experto + CoT | El modelo asume patrones genéricos |
| **O**utput | Formato exacto + DoD + casos de borde | El resultado puede ser inutilizable |

---

## Técnicas implementadas

| # | Técnica | Qué hace |
|---|---------|----------|
| 1 | **TCRO** | Estructura obligatoria de 4 componentes para todo prompt |
| 2 | **Protocolo de Descubrimiento Activo** | Busca archivos de config reales (`package.json`, `tsconfig.json`, `Dockerfile`...) antes de proponer stack |
| 3 | **Rol Dinámico** | Asigna un rol experto específico según el dominio detectado |
| 4 | **Chain-of-Thought (CoT)** | Añade `Think step-by-step` obligatorio para reducir errores en arquitecturas complejas |
| 5 | **Few-shot en Output** | Incluye ejemplo concreto de la entrega esperada |
| 6 | **Definition of Done + Blindaje de Casos de Borde** | Define criterio de éxito y anticipa fallos por dominio |
| 7 | **Conversión de Restricciones** | Transforma `"no X"` en `"usa Y en su lugar"` con arquitectura alternativa |
| 8 | **Prompt Chaining** | Detecta tareas compuestas y recomienda dividirlas antes de ejecutar |
| 9 | **Verificabilidad de versiones** | Previene alucinaciones de librerías o versiones inexistentes |
| 10 | **Delimitación XML** | Envuelve código/datos con `<context_code>` para separar instrucciones de contenido |

---

## Flujo de ejecución

```
Prompt recibido
      │
      ▼
¿Tarea compuesta? ──yes──▶ Recomienda Prompt Chaining y pregunta por dónde empezar
      │ no
      ▼
Descubrimiento Activo de ecosistema
  ├── archivos de config encontrados  →  usa ecosistema real como fuente de verdad
  ├── dominio conocido sin archivos   →  propone stack moderno estándar
  └── dominio desconocido             →  activa Fallback de Especificidad
      │
      ▼
Reconstruye con TCRO + 5 Directivas
  ├── T: verbo específico + objeto concreto + alcance
  ├── C: ecosistema real/propuesto + delimitación XML si hay código
  ├── R: rol experto + restricciones positivas + CoT + verificabilidad
  └── O: formato + DoD + casos de borde + few-shot
      │
      ▼
Presenta: diagnóstico + prompt elevado completo
      │
      ▼
Espera aprobación del usuario
      │
      ▼
Ejecuta
```

---

## Ejemplos

### Prompt vago → elevado

**Input:**
```
haz una pagina web
```

**Prompt elevado por prompt-forge:**
```
### Tarea
Construye una landing page responsive con sección hero, características y CTA

### Contexto
Stack: React 18 + TypeScript + Vite (propuesto — sin archivos de config detectados, ajusta si usas otro)
Entorno: desarrollo local / deploy estático en Vercel o Netlify

### Restricciones / Rol
Actúa como Senior Frontend Engineer especializado en rendimiento y accesibilidad web
- Componentes funcionales con hooks de React
- Tailwind CSS, mobile-first
- Sin librerías de UI externas (shadcn, MUI, etc.)
Think step-by-step before providing the technical solution.
If a specific library version is suggested, ensure it exists in official documentation.

### Output
- Formato: componentes React separados (Hero.tsx, Features.tsx, CTA.tsx, App.tsx)
- Documentación: JSDoc en props de cada componente
- Error handling: Error boundary en componente raíz
- Casos de borde: estado de carga, error de red, imagen hero no disponible
- Ejemplo de estructura:
  src/
    components/Hero.tsx
    components/Features.tsx
    components/CTA.tsx
    App.tsx
```

---

### Prompt bien formado → enriquecido

**Input:**
```
crea una API REST con Node.js y Express para manejar autenticación de usuarios con JWT, que devuelva JSON
```

**Lo que prompt-forge añade:**
- Versiones específicas del stack (Node.js 20 LTS, Express 4, TypeScript 5)
- JWT de acceso (15min) + refresh token rotativo en httpOnly cookie
- Rate limiting: 5 intentos por IP cada 15 minutos
- Hashing con bcrypt (salt rounds: 12)
- Validación con Zod antes de tocar la base de datos
- Arquitectura en capas: router → controller → service → repository
- Esquema de respuesta tipado: `{ success: true, data: {...} }` / `{ success: false, error: {...} }`
- Casos de borde: 401, 403, 422, 429, refresh token expirado

---

### Dominio desconocido → Fallback de Especificidad

Para dominios no reconocidos (ML, DevOps, infraestructura), la skill activa el **Fallback de Especificidad**:

- **R:** el modelo debe listar todas sus suposiciones técnicas *antes* de entregar código
- **O:** código comentado + guía de ejecución paso a paso + `requirements.txt` / equivalente con versiones exactas

---

## Roles dinámicos por dominio

| Dominio | Rol asignado |
|---------|-------------|
| Frontend / web | Senior Frontend Engineer — rendimiento y accesibilidad |
| API / backend | Senior Backend Engineer — arquitecturas REST seguras |
| Full-stack | Full-Stack Architect — separación de responsabilidades |
| ML / IA | Machine Learning Engineer — pipelines reproducibles |
| DevOps / infraestructura | DevOps Engineer — infraestructura como código |
| Base de datos | Database Engineer — optimización de queries |
| Mobile | Mobile Engineer — rendimiento nativo y UX |
| Desconocido | Senior Software Engineer — declara suposiciones antes de implementar |

---

## Estructura del repositorio

```
prompt-forge/
├── SKILL.md    ← instalar en ~/.claude/skills/prompt-forge/
└── README.md   ← esta guía
```

---

## Requisitos

- [Claude Code](https://claude.ai/code) — CLI, desktop app, o extensión de VS Code / JetBrains
- Soporte de skills en `~/.claude/skills/` (disponible en Claude Code)

---

## Licencia

MIT
