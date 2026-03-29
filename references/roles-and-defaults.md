# Roles, Stacks y Casos de Borde por Dominio

---

## Roles Dinámicos por Dominio

| Dominio detectado | Rol a asignar en ### Restricciones / Rol |
|-------------------|------------------------------------------|
| Frontend / web | Actúa como Senior Frontend Engineer especializado en rendimiento y accesibilidad web |
| API / backend | Actúa como Senior Backend Engineer especializado en arquitecturas REST seguras y escalables |
| Full-stack | Actúa como Full-Stack Architect con foco en separación de responsabilidades y DX |
| ML / IA | Actúa como Machine Learning Engineer especializado en pipelines reproducibles y evaluación de modelos |
| DevOps / infraestructura | Actúa como DevOps Engineer especializado en infraestructura como código y pipelines CI/CD seguros |
| Base de datos | Actúa como Database Engineer especializado en optimización de queries y diseño de esquemas |
| Mobile | Actúa como Mobile Engineer especializado en rendimiento nativo y UX en dispositivos |
| Dominio desconocido | Actúa como Senior Software Engineer con criterio para identificar y declarar suposiciones técnicas antes de implementar |

---

## Stacks Estándar por Dominio (cuando no hay archivos de config)

| Dominio | Stack propuesto |
|---------|----------------|
| Frontend / web | React 18 + TypeScript + Vite |
| API / backend | Node.js 20 LTS + Express 4 + TypeScript 5 |
| Script / automatización | Python 3.11+ |
| Mobile | React Native + TypeScript |
| Full-stack | Next.js 14 + TypeScript + Tailwind CSS |

---

## Casos de Borde Obligatorios por Tipo de Tarea

| Tipo de tarea | Casos de borde a cubrir |
|---------------|------------------------|
| API / endpoints | Input inválido (422), no autenticado (401), no autorizado (403), recurso no encontrado (404), límite de tasa (429) |
| Formularios / UI | Campos vacíos, formatos inválidos, servidor lento, estado de carga, error de red |
| Funciones / scripts | Argumentos nulos, tipos incorrectos, archivos inexistentes, permisos insuficientes, timeouts |
| ML / modelos | Dataset vacío, features faltantes, valores fuera de rango, modelo no convergente |
| DB / queries | Registros duplicados, foreign key violations, conexión caída, query sin resultados |
| Infraestructura | Recursos ya existentes, permisos IAM insuficientes, límites de cuota, estado de drift |
| Universal (fallback) | Input nulo, estado inesperado del sistema, fallo de dependencia externa |
