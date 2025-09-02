[ROL]  

Actúa como experto en refactorización automática y codemods (JS/TS).
 
[CONTEXTO]  

Se están definiendo reglas para automatizar la migración de AngularJS a Angular 19.2. Se requiere transformar estructuras heredadas a sus equivalentes modernos.
 
[TAREA]  

1. Define reglas de transformación en `CODEMODS.md`, incluyendo:

   - `ng-repeat` → `*ngFor`, `ng-if` → `*ngIf`, `ng-class` → `[ngClass]`.

   - `$http` → `HttpClient` con Observables (usando `switchMap`, `catchError`, etc.).

   - Promesas → RxJS; `$scope` → clase/componente con `signals()` si aplica.

   - `filters` → `pipes`, directivas con template → componentes standalone.

   - Si es necesario, incluye adaptadores mínimos para jQuery.
 
2. Genera parches ejemplo:

   - `templates.patch`, `services_http.patch`, `scope_to_class.patch`, `filters_to_pipes.patch`, `adapters_jquery.patch`.
 
3. Documenta todo en `/Mig19/_insumos-migracion/05_codemods/` con:

   - `CODEMODS.md`

   - Todos los parches anteriores

   - `COMMIT_PLAN.md`
 
[RESTRICCIONES]  

- Los parches deben ser idempotentes y comentados.

- No incluir transformaciones que requieran rediseño lógico.

- Usa ejemplos realistas del código original.

 
[ROL]  

Actúa como desarrollador Angular Senior encargado de migrar features de manera incremental.
 
[CONTEXTO]  

Ya existen rutas definidas, codemods disponibles y el scaffolding del workspace. Se desea migrar cada feature (pantalla + lógica + estilos) de AngularJS a Angular moderno.
 
[TAREA]  

1. Para cada feature, aplica:

   - Codemods relevantes (componentes, servicios, templates).

   - Reemplazo de formularios (ng-model → Reactive Forms).

   - Migración de estilos/componentes visuales (según UI framework definido).

   - Conexión de rutas (`app.routes.ts`) vía `loadComponent`.
 
2. Entrega:

   - `patches_por_feature/featureA.patch`, etc.

   - `CHECKLIST_DE_PARIDAD.md` actualizado por feature.

   - `resumen_por_feature.md` con estado y notas.
 
3. Guarda todo en `/Mig19/_insumos-migracion/10_full_migration/`.
 
[RESTRICCIONES]  

- No usar `any` en servicios o modelos.

- Mantener compatibilidad visual razonable (no pixel-perfect).

- No dejar código muerto ni TODOs.

 
