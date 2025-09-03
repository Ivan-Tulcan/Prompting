[ROL]
Actúa como desarrollador Angular Senior encargado de migrar un directorio completo de código AngularJS 1.8 hacia Angular 19.2, siguiendo un enfoque incremental por features.
[CONTEXTO]
Dispones de:
Un directorio base (easyAfiliaciones) con el código fuente AngularJS 1.8 (módulos, controllers, servicios, templates, estilos).
Rutas ya inventariadas, codemods definidos y convenciones de migración establecidas.
Debes crear el proyecto Angular 19.2 en la carpeta Mig19 únicamente como estructura base (sin ejecutar comandos, solo generando los archivos y configuraciones necesarias).
El objetivo es migrar feature por feature (pantalla + lógica + estilos) desde el directorio legado en este mismo workspace.
[TAREA]
Preparación del workspace
Crear la carpeta Mig19 como contenedor del proyecto Angular 19.2.
Generar dentro de ella los archivos base (package.json, tsconfig.json, angular.json, src/app/...) ya alineados con Angular 19.2.
Ajustar estas configuraciones según el target y las dependencias definidas.
Migración por features
Para cada feature detectado en el directorio AngularJS 1.8:
Identificar controllers, services, directives, filters y templates asociados.
Aplicar codemods relevantes (componentes standalone, servicios modernos, templates Angular).
Migrar formularios (ng-model) a Reactive Forms.
Migrar estilos y componentes visuales según el UI framework definido en el target.
Conectar rutas en app.routes.ts mediante loadComponent y guards equivalentes.
Entregables
patches_por_feature/featureA.patch, featureB.patch, etc.
CHECKLIST_DE_PARIDAD.md actualizado (estado de cada feature respecto al original).
resumen_por_feature.md con notas de migración, decisiones y desviaciones.
Ubicación de entregables
Todo debe guardarse en:
/Mig19/_insumos-migracion/10_full_migration/.
[RESTRICCIONES]
No usar any en servicios o modelos.
Mantener compatibilidad visual razonable (no pixel-perfect).
No dejar código muerto ni TODOs.
La migración debe partir directamente del directorio legado AngularJS 1.8.
No ejecutar comandos en ningún momento, únicamente crear y documentar los archivos y carpetas necesarios.
