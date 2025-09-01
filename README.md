# Migración Angular por Partes

### 0) Semilla de contexto (descubrimiento)

Objetivo: Inventariar AngularJS y fijar restricciones.
Prompt:

Actúa como Senior Frontend Architect. Lee el repo AngularJS ({{repo_url}}). Genera:

Árbol de carpetas (3 niveles).

Lista de módulos, controllers, services/factories, directives, filters, templates.

Mapa de rutas (path, params, resolves/guards).

Dependencias (nombre, versión, uso).

Anti-patterns detectados.
Entrega MIGRATION_PLAN.md y ROUTES_MAP.md. Si falta info, asume y documenta.
Aceptación: Existen MIGRATION_PLAN.md y ROUTES_MAP.md con >90% de cobertura.

### 1) Auditoría de dependencias

Objetivo: Decidir conservar/reemplazar.
Prompt:

A partir del inventario, produce DEPENDENCY_MATRIX.md con columnas: Paquete actual, Estado (mantener / reemplazar / eliminar), Sustituto (si aplica), Versión NPM exacta, Motivo, Notas de migración.
Incluye recomendaciones para: i18n, UI (Bootstrap/Material), fecha/hora, gráficas, notificaciones, utilidades. Sin librerías obsoletas.
Aceptación: Matriz completa y ejecutable (nombres/versions válidos).

### 2) Bootstrap del workspace en /Mig19

Objetivo: Crear base Angular 19.2.14.
Prompt:

Genera comandos reproducibles para:

ng new Mig19 --standalone --routing --style=scss --strict

Instalar dependencias decididas en la matriz.

Configurar ESLint, Prettier, path aliases, browserslist, HMR.

Crear MIGRATION_LOG.md (bitácora) y CONVENTIONS.md (naming, estructura por features).
Devuelve los comandos y los archivos resultantes (diffs clave de angular.json, tsconfig.json, package.json).
Aceptación: Proyecto compila con ng serve; linters configurados.

### 3) Estructura espejo por features

Objetivo: Reflejar la organización funcional.
Prompt:

Con base en MIGRATION_PLAN.md, crea la estructura en /Mig19/src/app:

core/ (auth, http, interceptors, guards)  
shared/ (ui atoms, directives, pipes)  
features/{featureA,featureB,...}/


Genera SCAFFOLDING_REPORT.md con el árbol propuesto, stubs vacíos (componentes standalone, servicios, pipes), y un CHECKLIST_DE_PARIDAD.md por feature (pantallas, rutas, formularios, i18n).
Aceptación: Árbol creado; stubs y checklist por feature listos.

### 4) Mapa de rutas 1:1

Objetivo: Replicar paths y params.
Prompt:

A partir de ROUTES_MAP.md, entrega app.routes.ts con rutas equivalentes (mismos paths y params). Usa lazy con loadComponent, guards para antiguos resolves/roles. Genera ROUTES_DIFF.md con cualquier desviación y motivo.
Aceptación: Navegación básica compilando; sin rupturas de deep-links conocidos.

### 5) Reglas de traducción (codemods/heurísticas)

Objetivo: Automatizar lo mecánico.
Prompt:

Propón y entrega scripts (regex/ts-morph/jscodeshift) para transformar:

ng-repeat→*ngFor, ng-if→*ngIf, ng-class→[ngClass].

$http→HttpClient, promesas→Observables (switchMap, catchError).

$scope/controllerAs→estado en clase/signal() cuando aplique.

filters→pipes, directives con template→components.
Devuelve CODEMODS.md y ejemplos sobre 1–2 archivos de muestra.
Aceptación: Code snippets de antes/después correctos y aplicables.

### 6) Piloto de migración (1 feature extremo a extremo)

Objetivo: Validar la estrategia con una feature representativa.
Prompt:

Selecciona la feature más representativa (CRUD + formulario + tabla + modal). Migra: componentes, servicios, pipes, directivas y estilos. Incluye:

main.ts con bootstrapApplication.

Interceptor HTTP (Auth, manejo de errores).

Formulario reactivo equivalente a ng-model.

i18n (pipe o Angular i18n/ngx-translate).
Entrega diffs y PILOT_REPORT.md con riesgos y tiempos estimados por feature.
Aceptación: La feature piloto funciona en /Mig19; checklist de paridad en verde.

### 7) Servicios y contratos API

Objetivo: Consolidar llamadas y errores.
Prompt:

Migra todos los servicios $http/$resource a HttpClient con tipado fuerte. Crea ApiClientService, interceptores (headers, retry opcional, mapeo de errores), y helpers RxJS. Entrega API_GUIDE.md con patrones (cancelación, caching, forkJoin, paginación).
Aceptación: Servicios compilan, tipados, sin any; flujos críticos funcionando.

### 8) Formularios, validaciones y accesibilidad

Objetivo: Paridad UX en entradas de datos.
Prompt:

Convierte formularios basados en ng-model a Reactive Forms: FormGroup, validaciones síncronas/async, mensajes de error accesibles. Entrega FORMS_GUIDE.md + componentes ejemplo (form-field compartido), y pruebas unitarias mínimas.
Aceptación: Validaciones y UX equivalentes; tests pasan.

### 9) UI/estilos y librerías visuales

Objetivo: Pixel-fidelity razonable.
Prompt:

Integra el framework UI ({{Bootstrap/Material/u otro}}), migra variables/mixins SASS, y restituye layouts. Si había jQuery/UI-Bootstrap, encapsula en adapters o sustituye por {{ng-bootstrap/ngx-bootstrap/Material}}. Entrega STYLE_PARITY.md con capturas/observaciones de brechas.
Aceptación: Páginas clave visualmente equivalentes (desviaciones documentadas).

### 10) Pruebas, calidad y DX

Objetivo: Garantizar paridad y mantenibilidad.
Prompt:

Configura Jest (o mantén Karma si imprescindible) y Cypress. Genera TEST_PLAN.md con casos unitarios y E2E para flujos críticos (login, navegación, CRUDs, errores). Agrega budgets de performance en angular.json, preloading, división por rutas, HMR en dev.
Incluye CI básico (lint → build → test → e2e) y QUALITY_GATE.md.
Aceptación: Lint/build/tests/E2E OK; budgets configurados.

### 11) Migración masiva por lotes

Objetivo: Escalar el piloto al resto de features.
Prompt:

Planifica lotes de features (Lote1, Lote2, …) con orden y dependencias. Ejecuta las reglas de CODEMODS.md + patrones del PILOT_REPORT.md. Actualiza CHECKLIST_DE_PARIDAD.md por feature y MIGRATION_LOG.md con commits sugeridos (por lote) y mensajes.
Aceptación: % de avance por feature visible; sin dead ends técnicos.

### 12) Auditoría final y hardening

Objetivo: Cerrar brechas y preparar release.
Prompt:

Ejecuta auditoría final: accesibilidad, i18n, errores globales, estados vacíos, loading/skeletons, seguridad básica (DomSanitizer, CSP sugerida), limpieza de TODOs, dependencia sin usar. Entrega FINAL_GAP_REPORT.md, RELEASE_NOTES.md y FOLLOW_UP.md (tareas post-migración).
Aceptación: Brechas residuales explícitas con plan claro; build de release listo.
