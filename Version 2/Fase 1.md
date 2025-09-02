# Prompt - Fase 1: Descubrimiento e Inventario
 
## [ROL]  

Actúa como un **Senior Frontend Architect** especializado en migraciones de AngularJS a Angular moderno.
 
## [CONTEXTO]  

Estás comenzando la migración de una aplicación legada desarrollada en AngularJS (1.8).  

El objetivo es migrarla a Angular 19.2.14 utilizando arquitectura moderna basada en:

- Componentes standalone

- Lazy loading

- RxJS para manejo reactivo

- Formularios reactivos

- Estricto control de dependencias y buenas prácticas de DX

Además, como referencia hay un archivo referencia-package.json que contiene el una lista de dependencias que deberá tener la nueva versión.

Repositorio fuente: `src\Aplicaciones\EasyAfiliaciones`  

Ruta de trabajo:  

`/Mig19/_insumos-migracion/00_descubrimiento/`
 
## [TAREA]  

Explora y analiza el código fuente actual para generar un diagnóstico técnico que sirva como base para toda la migración.
 
### Debes entregar:
 
1. **Estructura de carpetas (hasta 3 niveles)** del proyecto actual: `src`, `app`, `views`, `services`, etc.

2. **Inventario detallado de elementos AngularJS** (con rutas relativas):

   - Módulos (`angular.module(...)`)

   - Controllers

   - Services / Factories

   - Directives

   - Filters

   - Templates HTML

3. **Mapa de rutas**:

   - Path

   - Parámetros (`:id`, `:slug`, etc.)

   - Resolves simulados

   - Guards (autenticación/autorización)

4. **Dependencias externas**:

   - Nombre del paquete

   - Versión exacta

   - Uso dentro del proyecto

   - Nivel de criticidad: crítica / útil / obsoleta

5. **Anti-patrones detectados**:

   - Uso excesivo de `$scope`

   - Watchers innecesarios o anidados

   - jQuery directo

   - Lógica en controllers

   - Acoplamientos fuertes entre templates y lógica

6. **Requisitos no funcionales**:

   - i18n / traducción

   - Accesibilidad básica (ARIA, roles, foco)

   - Compatibilidad con navegadores específicos

   - Límites de rendimiento o “performance budgets”
 
 
## [RESTRICCIONES]  

- No modifiques el código fuente original.  

- No uses Git todavía (no crear commits ni ramas).  

- No inventes componentes, módulos o rutas no existentes.  

- Toda la información debe derivarse únicamente del código fuente.  

- Usa solo Markdown como formato.  

- Usa enlaces relativos entre archivos si es necesario.  

- Usa tablas para inventarios y listas.

- No utilices adornos como emoticones.
 
---
 
## Entregables esperados (coloca cada uno en `/00_descubrimiento/`)
 
| Archivo                    | Contenido esperado |

|----------------------------|---------------------|

| `MIGRATION_PLAN.md`        | Resumen del sistema actual, dependencias críticas, anti-patrones, riesgos técnicos, enfoque inicial sugerido. |

| `ROUTES_MAP.md`            | Tabla de rutas de AngularJS con: path, params, resolves simulados, guards, archivo origen. |

| `COMMIT_PLAN.md`           | Rama sugerida (sin ejecutarla), lista de pasos de commit esperados por entregable. |

| `comandos_sugeridos.md`    | Lista de comandos esperados en futuras fases (ng new, npm install, linters, etc). No ejecutarlos aún. |
 
##  Archivos globales a actualizar
 
| Archivo                      | Acción |

|-----------------------------|--------|

| `README.md`                 | Agregar estado: “Fase 1 – Descubrimiento completada” |

| `MIGRATION_LOG.md`          | Nueva entrada con fecha, entregables y resumen |

| `CHECKLIST_DE_PARIDAD.md`  | Marcar ítems correspondientes como "LISTO" o "PENDIENTE" |
 
---
 
##  Criterios de aceptación
 
- Los entregables están completos y organizados.  

- Los archivos se encuentran en la carpeta correcta.  

- La cobertura de rutas y elementos AngularJS supera el 90%.  

- No se introdujo código nuevo.  

- Toda la información es verificable en el código original.

 
