Toma todo el proyecto AngularJS 1.8 como contexto. Analiza todo el código fuente del proyecto y enfócate únicamente en el módulo llamado `EasyAfiliaciones`.
 
Tu objetivo es extraer y documentar toda la información relacionada exclusivamente con `EasyAfiliaciones`, sin suposiciones ni invenciones.
 
Genera una documentación técnica completa y precisa del módulo `EasyModulo1` incluyendo:
 
1. Nombre completo del módulo y cómo está registrado (por ejemplo: `angular.module('EasyAfiliaciones', [...])`).
2. Listado completo de sus dependencias (otros módulos, librerías externas, servicios, etc.).
3. Listado de componentes definidos en este módulo (controllers, factories, services, directives, filters, etc.) con:
   - Nombre
   - Tipo (controller, service, etc.)
   - Función que cumple
   - Ubicación del archivo
4. Vistas o templates asociados (si los hay), con ubicación y cómo se enlazan al módulo.
5. Rutas definidas en este módulo (por ejemplo, usando `$routeProvider`, `$stateProvider`, etc.), con:
   - URL
   - Template
   - Controller asociado
   - Resolves (si aplica)
6. Relaciones de este módulo con otros módulos del sistema (qué usa, qué lo usa).
7. Ejemplos representativos del uso de funciones clave del módulo.
8. Comentarios/documentación inline que ya exista en el código fuente.
9. Cualquier configuración específica, constantes, valores definidos, etc.
 
Importante:
- No inventes funciones ni estructuras que no existan en el código.
- Usa el código como fuente de verdad.
- No documentes cosas genéricas ni irrelevantes que no pertenezcan a `EasyModulo1`.
- Si no hay algo, indícalo explícitamente (por ejemplo: “Este módulo no define rutas propias”).
- Sé específico con rutas de archivos y nombres exactos.
 
Formato de salida sugerido: Markdown.
ubicacion del archivo: Mig19/documentacion/documentacion.md
 
Objetivo final: usar esta documentación como base para la futura migración del módulo a Angular 19 sin necesidad de escanear todo el proyecto nuevamente.
