# APLICACIÓN ORIGINAL

Este proyecto, denominado PAS-EasyCashWeb, es una aplicación web de tipo "monolito" que alberga múltiples aplicaciones financieras o bancarias bajo un mismo paraguas. La estructura del código sugiere que es una plataforma de "marca blanca" (white-labeling) que puede ser personalizada para diferentes instituciones financieras, como lo demuestran las hojas de estilo específicas para bancos como Pichincha, JEP, Fomento, Produbanco, entre otros.
El objetivo principal parece ser proporcionar una solución de "Cash Management" o banca empresarial, permitiendo a los usuarios realizar operaciones como pagos, transferencias, consulta de saldos, y más. La presencia de un directorio EasyAfiliaciones sugiere también funcionalidades para la afiliación de nuevos clientes o servicios.
## Componentes Técnicos y Arquitectura
La aplicación tiene una arquitectura de varias capas, aunque desplegada como un monolito en el frontend.
### Frontend:
- Tecnologías Principales: El núcleo del frontend está construido con AngularJS (basado en la estructura de archivos y la presencia de angular-strap). 
- Task Runner: Utiliza Grunt.js (gruntfile.js) para la automatización de tareas como la compilación, minificación y concatenación de archivos JavaScript y CSS.
- Estructura de Aplicaciones: El código fuente principal reside en src. Dentro de Aplicaciones, se encuentran las diferentes sub-aplicaciones (Cash, EasyAfiliaciones, EasySeguridad, etc.). Cada una de estas parece ser un módulo de AngularJS.
- Aplicación Móvil (Híbrida): El directorio app contiene una configuración para Ionic/Cordova (ionic.config.json, config.xml). Esto indica que una parte del proyecto es o fue una aplicación móvil híbrida, probablemente para iOS y Android, que reutiliza parte de la lógica web.
- Backend (BFF - Backend for Frontend): Los directorios bff/ y bffidentidad/ contienen archivos web.config, lo que indica la presencia de servicios backend desarrollados con tecnología de Microsoft, muy probablemente ASP.NET.
Estos servicios actúan como una capa intermedia (BFF) que se comunica con los sistemas core del banco y expone los datos de una manera que el frontend (la aplicación AngularJS) puede consumir fácilmente. bffidentidad sugiere un servicio dedicado a la autenticación y gestión de la identidad del usuario.
- Servidor Web: La presencia de web.config en la raíz y en los directorios BFF sugiere que la aplicación está diseñada para ser desplegada en un servidor IIS (Internet Information Services) de Microsoft.

## Aplicaciones y Módulos
El proyecto está compuesto por varias "aplicaciones" o módulos, que se encuentran en Aplicaciones:
BancaEmpresas / Cash: Probablemente el módulo principal para operaciones de cash management.
EasyAfiliaciones: Gestión de afiliaciones de clientes.
EasyConfigurador: Panel de configuración de la aplicación.
EasySeguridad: Módulo para la administración de usuarios, roles y permisos.
EasyTeller: Podría ser una aplicación para cajeros o personal del banco.
MulticanalAdmin: Panel de administración general de la plataforma.
Produnet: Posiblemente una integración o módulo específico para Produbanco.
## Prácticas de Desarrollo y Despliegue
Gestión de Dependencias: Se utiliza package.json para gestionar las dependencias de desarrollo de JavaScript (como Grunt y sus plugins).
Build Process: gruntfile.js define cómo se construye la aplicación. Probablemente concatena y minifica todos los archivos .js y .css de las diferentes aplicaciones en archivos únicos (como libs.js y index.js en Aplicaciones) para optimizar la carga en el navegador.
Manejo de Datos: El directorio datos/ contiene numerosos archivos .json. Estos podrían ser utilizados para desarrollo y pruebas en local, simulando las respuestas de la API del backend (mocking), lo que permite a los desarrolladores de frontend trabajar sin depender de un backend activo.
Migración: Existe un archivo MIGRATION_PLAN.md (aunque vacío). Esto sugiere que puede haber planes o intentos pasados de migrar esta aplicación monolítica a una arquitectura más moderna (por ejemplo, micro-frontends o una reescritura completa con un framework más nuevo como React, Vue o una versión reciente de Angular).
## Resumen
En resumen, PAS-EasyCashWeb es una plataforma de banca en línea monolítica, construida principalmente con AngularJS y ASP.NET (BFF), y diseñada para ser personalizable para diferentes bancos. Incluye una gran cantidad de funcionalidades de "Cash Management" y administración. La presencia de herramientas como Grunt y una estructura de aplicación móvil híbrida con Ionic/Cordova pintan el cuadro de una aplicación madura, desarrollada con tecnologías populares de hace varios años. Los indicios de un plan de migración sugieren que se reconoce la necesidad de modernizar su arquitectura y base tecnológica.

---
# OBJETIVO
Reescribir la plataforma desde AngularJS en el framework Angular 19.2.14 utilizando las dependencias y versiones del archivo "referencia-package.json".

## Análisis

### Angular 19: 
Saltar directamente a una de las versiones más recientes de Angular, abandonando por completo AngularJS. Esto implica que no se puede reutilizar casi nada del código del framework (controladores, scopes, directivas de AngularJS).
### Micro-Frontends (Module Federation): 
La dependencia @angular-architects/module-federation y el script run:all son el indicio más claro. La nueva arquitectura no será un monolito como la actual. Se basará en micro-frontends, donde diferentes partes de la aplicación (como Seguridad, Cash, Afiliaciones) pueden ser desarrolladas y desplegadas de forma independiente como aplicaciones Angular separadas que se integran en un "shell" o contenedor principal.
### Componentes de UI:
* PrimeNG: Has elegido PrimeNG como la biblioteca de componentes de UI principal. Esto reemplazará cualquier componente personalizado, Bootstrap o angular-strap que se usaba en la versión de AngularJS. Toda la interfaz de usuario deberá ser reconstruida con componentes de PrimeNG.
* Herramientas de Desarrollo (Tooling):
* Angular CLI: Reemplaza completamente a Grunt.js para la construcción, el desarrollo local y la generación de código.
* Testing: Se adopta Jest como el framework de pruebas unitarias, una elección moderna y popular en el ecosistema de Angular.
* Calidad de Código: La inclusión de ESLint, Prettier y Stylelint demuestra un fuerte enfoque en mantener un código limpio, consistente y formateado automáticamente, lo cual es una mejora significativa sobre las prácticas comunes en proyectos AngularJS antiguos.
### Dependencias Clave:
* Librerías Locales (@ngx-controlbase): Sigues dependiendo de librerías locales (.tgz). Esto indica que tienes una o más bibliotecas de componentes o lógica de negocio compartida que se gestionan internamente. Los scripts para copiar desde una unidad de red (\\172.10.90.22\NPMLocal) y luego instalar localmente confirman este flujo de trabajo.
* Estado y Comunicación: pubsub-js se mantiene, probablemente para facilitar la comunicación entre micro-frontends o componentes desacoplados.
* Internacionalización: La inclusión de @ngx-translate sugiere que la nueva aplicación será multi-idioma desde su concepción.

## Plan de Migración (Reescritura)
Esta no es una migración automática. Es un proyecto de reescritura que debe ser planificado cuidadosamente.
### Fase 1: Configuración y Arquitectura Base (Shell)
1. Crear el Workspace: Utiliza Angular CLI para crear un nuevo workspace. No intentes hacerlo sobre el proyecto existente.
2. Crear la Aplicación "Shell": Dentro del workspace, crea la aplicación principal que actuará como contenedor de los micro-frontends.
3. Instalar Dependencias: Instala todas las dependencias de tu package.json objetivo en el nuevo proyecto shell. Configura Jest, Prettier y ESLint.
4. Configurar Module Federation: Añade @angular-architects/module-federation al proyecto shell y configúralo. Este será el host que cargará las demás aplicaciones.
5. Desarrollar el Layout Principal: Crea el layout base en la aplicación shell: la cabecera, el menú de navegación principal, el pie de página y el área de contenido (<router-outlet>). Este layout debe ser agnóstico a los módulos que se cargarán.

### Fase 2: Reescritura de Módulos como Micro-Frontends
Elige un módulo del proyecto AngularJS para empezar, idealmente el más simple o el de autenticación.
Crear el Micro-Frontend (MFE): Dentro del mismo workspace, genera una nueva aplicación Angular para el módulo.
Configurar como MFE: Configura seguridad como un micro-frontend (remote) usando Module Federation, exponiendo su módulo principal.
Reescribir Funcionalidades:
1. Análisis: Revisa las vistas, controladores y servicios del módulo EasySeguridad en el proyecto AngularJS.
2. Componentes: Reconstruye las vistas utilizando componentes de Angular y PrimeNG.
3. Lógica: Migra la lógica de los controladores de AngularJS a los componentes y servicios de Angular. La lógica de negocio (validaciones, cálculos) a menudo se puede migrar con pocos cambios, pero la interacción con el framework (como $scope o $http) debe ser reemplazada por la sintaxis de Angular (Input/Output, RxJS, HttpClient).
4. Servicios: Recrea los servicios para la comunicación con el BFF. Utiliza el HttpClient de Angular y tipa las respuestas con interfaces.
5. Integrar en el Shell: Configura el enrutamiento en la aplicación shell para que cargue perezosamente el módulo seguridad cuando el usuario navegue a la ruta correspondiente (ej: /seguridad).
6. Repetir: Repite los pasos 1-4 para cada módulo del monolito original (Cash, EasyAfiliaciones, etc.), convirtiendo cada uno en un micro-frontend.

### Fase 3: Despliegue y Finalización
Refinar la Comunicación: Asegúrate de que la comunicación entre micro-frontends (si es necesaria) esté bien gestionada, usando pubsub-js o una librería de estado compartida.
Pruebas End-to-End: Realiza pruebas exhaustivas de la aplicación integrada para asegurar que la navegación y la interacción entre módulos funcionen correctamente.


## Observaciones y Consideraciones Clave
Reescritura, no Actualización
Gestión de Librerías Locales: El flujo de trabajo con las librerías .tgz copiadas desde una unidad de red es frágil. Considera seriamente configurar un registro privado de NPM (como Verdaccio, Nexus o GitHub Packages) para gestionar estas dependencias de forma más robusta y versionada.
Autenticación: La estrategia de autenticación y gestión de sesión debe ser definida tempranamente, ya que afectará a todos los micro-frontends. La sesión debe ser compartida de forma segura entre ellos.
Estilos: La migración a PrimeNG implica rehacer toda la capa de CSS/SCSS. Aprovecha para unificar y limpiar los estilos, eliminando los CSS específicos de cada "marca blanca" y gestionándolos a través de los temas de PrimeNG y variables de SCSS.
