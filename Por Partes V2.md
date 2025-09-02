---

# Bloque común (pegar al inicio de **cada** prompt)

```
PROTOCOLO (INSUMOS, SIN GIT, CONFIDENCIALIDAD PACKAGE):
0) Preflight de inicialización:
   - Verifica si existe /Mig19/_insumos-migracion/.
     * Si NO existe:
       - Genera /Mig19/_insumos-migracion/INITIALIZATION_PLAN.md con comandos para inicializar.
       - Guarda scripts sin ejecutarlos:
         /Mig19/_insumos-migracion/scripts/initialize_insumos.sh
         /Mig19/_insumos-migracion/scripts/initialize_insumos.ps1
       - Indica al developer que ejecute manualmente uno y reinicie este paso.
   - Verifica /Mig19/_insumos-migracion/package.json (TARGET_PACKAGE_JSON).
     * Si existe: úsalo como fuente de verdad de deps/versions/scripts/engines/overrides.
     * Si NO: registra TODO en /Mig19/_insumos-migracion/MIGRATION_LOG.md y continúa con supuestos mínimos.

1) Confidencialidad:
   - NO imprimas el contenido completo del TARGET_PACKAGE_JSON. Solo listas agregadas, diffs/patches sintéticos o nombres/versión cuando sea imprescindible.

2) Idempotencia:
   - Entrega SIEMPRE diffs o .patch aplicables.
   - Solo escribe en /Mig19 (código) y /Mig19/_insumos-migracion (insumos).

3) Progreso:
   - Actualiza /Mig19/_insumos-migracion/MIGRATION_LOG.md (timestamp ISO, acciones, % avance por feature).
   - Mantén /Mig19/_insumos-migracion/CHECKLIST_DE_PARIDAD.md al día por feature.

4) Plan de commits (manual):
   - NO ejecutes git.
   - Genera /Mig19/_insumos-migracion/XX_*/COMMIT_PLAN.md con:
     rama sugerida, lista/orden de parches, mensajes de commit sugeridos,
     checklist de validaciones previas (build/tests).

5) Validaciones:
   - Indica comandos esperados (ng build, npm test, npm run e2e), sin ejecutarlos.
   - Documenta cómo interpretar fallos comunes.

6) Versiones y rutas:
   - Objetivo Angular 19.2.14; Node LTS derivado de engines/peers del TARGET_PACKAGE_JSON.
   - Código en /Mig19; insumos en /Mig19/_insumos-migracion/.
```

---

# Prompts por paso (listos para pegar)

## 0) Descubrimiento e inventario

**Guardar en:** `/Mig19/_insumos-migracion/00_descubrimiento/`

```
[Usa el PROTOCOLO (INSUMOS, SIN GIT, CONFIDENCIALIDAD PACKAGE)]

Objetivo: Inventariar la app AngularJS y fijar restricciones.
Tareas:
1) Entregar árbol de carpetas (3 niveles) del proyecto AngularJS original.
2) Listar módulos, controllers, services/factories, directives, filters, templates (con paths).
3) Mapa de rutas (path, params, resolves/similares).
4) Dependencias actuales (nombre, versión, uso, criticidad).
5) Anti-patterns ($scope, watchers intensivos, jQuery directo, lógica en controllers…).
6) Requisitos no funcionales (i18n, accesibilidad, soportes de browser, performance budgets).

Entregables (en esta carpeta):
- MIGRATION_PLAN.md
- ROUTES_MAP.md
- COMMIT_PLAN.md (rama sugerida, sin git; no hay parches aún)
- Nota con comandos esperados a futuro (solo listado, no ejecutar).

Actualiza índices globales: README.md, MIGRATION_LOG.md, CHECKLIST_DE_PARIDAD.md.
```

---

## 1) Auditoría de dependencias

**Guardar en:** `/Mig19/_insumos-migracion/01_deps_auditoria/`

```
[Usa el PROTOCOLO]

Objetivo: Definir qué mantener/reemplazar/eliminar y amarrar versiones al TARGET_PACKAGE_JSON (si existe).
Tareas:
1) Leer TARGET_PACKAGE_JSON (sin imprimirlo) y extraer lista agregada de paquetes objetivo.
2) Construir DEPENDENCY_MATRIX.md (AngularJS→Angular 19):
   columnas: Paquete actual | Estado (mantener/reemplazar/eliminar) | Sustituto moderno | Versión exacta (del target) | Motivo | Notas
3) Señalar peerDeps/engines clave inferidas (TS, RxJS, zone.js, @angular/*, builder).
4) Si aplica, proponer overrides en overrides_sugeridos.md.

Entregables:
- DEPENDENCY_MATRIX.md
- overrides_sugeridos.md (si aplica)
- COMMIT_PLAN.md (qué se parcheará luego en package.json del workspace; no ejecutar git)

Actualiza ../00_descubrimiento/MIGRATION_PLAN.md con decisiones clave.
```

---

## 2) Bootstrap del workspace en `/Mig19`

**Guardar en:** `/Mig19/_insumos-migracion/02_bootstrap/`

```
[Usa el PROTOCOLO]

Objetivo: Definir comandos y parches para crear/alinéar el workspace Angular 19.2.14.
Tareas:
1) Documentar comandos (no ejecutarlos):
   - Node LTS recomendado (derivado de peers/engines del TARGET_PACKAGE_JSON).
   - @angular/cli@19
   - ng new Mig19 --standalone --routing --style=scss --strict
2) Comparar el package.json que generaría el CLI vs TARGET_PACKAGE_JSON y entregar:
   - package.json.diff (o package-json.align.patch)
3) Entregar diffs sugeridos coherentes con el target:
   - tsconfig.json.diff (reglas TS estrictas, path aliases)
   - angular.json.diff (budgets, builder esperado)
4) Entregar revisiones de coherencia:
   - PACKAGE_VET.md (Angular 19.2.14 OK, TS OK, RxJS/zone peers OK, ESLint/Prettier/test runner OK)
   - CONSTRAINTS_CHECKLIST.md (peer deps/engines verificados)
5) CONVENTIONS.md (naming, estructura por features, aliases)

Entregables:
- package.json.diff
- tsconfig.json.diff
- angular.json.diff
- PACKAGE_VET.md
- CONSTRAINTS_CHECKLIST.md
- CONVENTIONS.md
- COMMIT_PLAN.md (orden para aplicar parches, validaciones: ng build, npm run lint)
```

---

## 3) Estructura espejo por features

**Guardar en:** `/Mig19/_insumos-migracion/03_scaffolding/`

```
[Usa el PROTOCOLO]

Objetivo: Reflejar organización funcional original en /Mig19/src/app.
Tareas:
1) Proponer árbol:
   core/ (auth, http, interceptors, guards)
   shared/ (ui atoms, directives, pipes)
   features/{featureA,featureB,...}/
2) Generar stubs (standalone) como parches idempotentes:
   - scaffolding_core.patch
   - scaffolding_shared.patch
   - scaffolding_features.patch
3) Actualizar CHECKLIST_DE_PARIDAD.md (por feature).

Entregables:
- SCAFFOLDING_REPORT.md
- scaffolding_core.patch
- scaffolding_shared.patch
- scaffolding_features.patch
- COMMIT_PLAN.md (orden de aplicación y validaciones esperadas)
```

---

## 4) Rutas 1:1 (paths/params)

**Guardar en:** `/Mig19/_insumos-migracion/04_routing/`

```
[Usa el PROTOCOLO]

Objetivo: Replicar navegación (paths/params).
Tareas:
1) A partir de ROUTES_MAP.md, producir app.routes.ts con lazy via loadComponent y guards equivalentes a resolves/roles.
2) Entregar app.routes.ts.patch.
3) Documentar diferencias en ROUTES_DIFF.md.

Entregables:
- app.routes.ts.patch
- ROUTES_DIFF.md
- COMMIT_PLAN.md (aplicar parche; validar: ng build y navegación básica)
```

---

## 5) Reglas de traducción (codemods/heurísticas)

**Guardar en:** `/Mig19/_insumos-migracion/05_codemods/`

```
[Usa el PROTOCOLO]

Objetivo: Automatizar transformaciones mecánicas.
Tareas:
1) CODEMODS.md con reglas:
   - ng-repeat→*ngFor, ng-if→*ngIf, ng-class→[ngClass]
   - $http/$resource→HttpClient (Observables)
   - Promesas→RxJS (switchMap, catchError, forkJoin)
   - $scope/controllerAs→propiedades de clase / signals cuando aplique
   - filters→pipes; directivas con template→components
   - adapters mínimos para jQuery si es imprescindible
2) Parches ejemplo (idempotentes):
   - templates.patch
   - services_http.patch
   - scope_to_class.patch
   - filters_to_pipes.patch
   - adapters_jquery.patch

Entregables:
- CODEMODS.md
- templates.patch
- services_http.patch
- scope_to_class.patch
- filters_to_pipes.patch
- adapters_jquery.patch
- COMMIT_PLAN.md (orden y validaciones)
```

---

## 6) Servicios y contratos API

**Guardar en:** `/Mig19/_insumos-migracion/06_services_api/`

```
[Usa el PROTOCOLO]

Objetivo: Estandarizar acceso HTTP tipado.
Tareas:
1) API_GUIDE.md (interceptor auth/errores, cancelación, caching, retry opcional, paginación, forkJoin).
2) api_client.patch (ApiClientService + interceptores).
3) models_catalog.md (inventario de interfaces/types por endpoint).

Entregables:
- API_GUIDE.md
- api_client.patch
- models_catalog.md
- COMMIT_PLAN.md (parches + validación esperada ng build)
```

---

## 7) Formularios y validaciones (a11y)

**Guardar en:** `/Mig19/_insumos-migracion/07_forms/`

```
[Usa el PROTOCOLO]

Objetivo: Convertir formularios a Reactive Forms con UX equivalente.
Tareas:
1) FORMS_GUIDE.md (FormGroup/FormControl, validaciones sync/async, mensajes accesibles).
2) Parches:
   - form_field_shared.patch
   - reactive_forms_examples.patch
   - tests_validations.patch

Entregables:
- FORMS_GUIDE.md
- form_field_shared.patch
- reactive_forms_examples.patch
- tests_validations.patch
- COMMIT_PLAN.md (orden + validaciones: npm test)
```

---

## 8) UI/estilos y librerías visuales

**Guardar en:** `/Mig19/_insumos-migracion/08_ui_styles/`

```
[Usa el PROTOCOLO]

Objetivo: Lograr paridad visual razonable con libs modernas definidas en el TARGET_PACKAGE_JSON.
Tareas:
1) STYLE_PARITY.md (brechas tolerables; notas de diseño).
2) styles_migration.patch (styles.scss, imports por componente, DS elegido).
3) Si el target define Material/ng-bootstrap/otra, alinear helpers/temas.

Entregables:
- STYLE_PARITY.md
- styles_migration.patch
- COMMIT_PLAN.md (aplicar parche; validar: ng build y revisión visual)
```

---

## 9) Pruebas, calidad y DX

**Guardar en:** `/Mig19/_insumos-migracion/09_quality/`

```
[Usa el PROTOCOLO]

Objetivo: Asegurar paridad y mantenibilidad.
Tareas:
1) TEST_PLAN.md (flujos críticos: login, navegación, CRUD, errores).
2) QUALITY_GATE.md (umbrales lint/coverage/perf) compatibles con devDeps del TARGET_PACKAGE_JSON.
3) Parches/config:
   - jest_config.patch o preserva Karma según target
   - cypress_scaffold.patch
   - budgets_config.diff (angular.json)

Entregables:
- TEST_PLAN.md
- QUALITY_GATE.md
- jest_config.patch
- cypress_scaffold.patch
- budgets_config.diff
- COMMIT_PLAN.md (orden; validaciones: npm run lint, npm test, npm run e2e)
```

---

## 10) Migración completa (sin piloto)

**Guardar en:** `/Mig19/_insumos-migracion/10_full_migration/`

```
[Usa el PROTOCOLO]

Objetivo: Ejecutar la migración total de todas las features.
Tareas:
1) Aplicar reglas de CODEMODS.md a todo el código objetivo.
2) Sustituir controllers/directives/filters/services por components/pipes/services modernos.
3) Conectar todo a app.routes.ts.
4) Reconstruir formularios con Reactive Forms (FORMS_GUIDE.md).
5) Ajustar estilos (STYLE_PARITY.md) con mínima desviación visual.
6) Alinear deps del workspace con TARGET_PACKAGE_JSON usando package.json.diff (y diffs/patches adicionales si emergen).

Entregables:
- resumen_por_feature.md (estado final y notas)
- patches_por_feature/ (subcarpeta con .patch por feature, en orden recomendado)
- issues_resueltos_y_pendientes.md (incluye propuesta de overrides si hay choques)
- COMMIT_PLAN.md (rama, orden exacto de parches; validaciones: npm run lint, ng build, npm test, npm run e2e)
```

---

## 11) Auditoría final y hardening

**Guardar en:** `/Mig19/_insumos-migracion/11_hardening/`

```
[Usa el PROTOCOLO]

Objetivo: Cerrar brechas y preparar release.
Tareas:
1) Auditoría: a11y (roles/labels/foco/contraste), i18n (claves faltantes), estados vacíos/skeletons, errores globales, seguridad (DomSanitizer, recomendaciones CSP), limpieza de TODOs y deps no usadas.
2) Notas de release y seguimiento.

Entregables:
- FINAL_GAP_REPORT.md
- RELEASE_NOTES.md
- FOLLOW_UP.md
- COMMIT_PLAN.md (mensajes de cierre; validar build de release)
```

---

## 12) Entrega y cierre

**Guardar en:** `/Mig19/_insumos-migracion/12_close/`

```
[Usa el PROTOCOLO]

Objetivo: Consolidar y cerrar la migración.
Tareas:
1) RESUMEN_EJECUTIVO.md (≤15 líneas).
2) ESTADO_POR_FEATURE.md (tabla AngularJS→Angular 19).
3) VERSIONS_FINAL.md (tabla paquete→versión, convergente con TARGET_PACKAGE_JSON; si hay lockfile, incluir hash/nota sin exponer su contenido).
4) Actualizar índices globales (README.md, MIGRATION_LOG.md, CHECKLIST_DE_PARIDAD.md).

Entregables:
- RESUMEN_EJECUTIVO.md
- ESTADO_POR_FEATURE.md
- VERSIONS_FINAL.md
- COMMIT_PLAN.md (mensajes finales; no ejecutar git)
```

---

## Mini-plantilla para `COMMIT_PLAN.md` (usar en cada paso)

```
# COMMIT_PLAN — step XX <nombre>
Rama sugerida: mig19/step-XX-<slug>
Parches a aplicar (en orden):
1) <primer.patch>
2) <segundo.patch>
Mensajes sugeridos:
- chore(mig19): step XX - <resumen corto>
Checklist antes del commit:
- ng build OK
- npm run lint OK
- npm test / npm run e2e (si aplica) OK
Observaciones:
- <notas de integración, riesgos, follow-ups>
```
