# TripleTen QA Engineer · David Morales

Portafolio de pruebas de software del programa **QA Engineer de TripleTen**: 10 sprints y un módulo de pruebas de aplicaciones móviles. Vengo del arte 3D y el desarrollo de videojuegos, y me estoy especializando en QA de videojuegos.

## Proyecto destacado: Urban Routes

Pruebas manuales de **Urban Routes**, una aplicación web para pedir taxi: flujo funcional, red y consola, interfaz contra el diseño en dos navegadores, y validación de un formulario de pago.

| | |
|---|---|
| Verificaciones ejecutadas | **81** |
| Verificaciones fallidas | **26** |
| Bugs documentados | **21** (1 crítico, 1 mayor, 11 de prioridad alta, 8 de prioridad baja) |
| Entornos | Chrome 800×600 y Firefox 1920×1080 |

**Bugs destacados**

- **URB-02 · Crítico.** La pantalla de espera del vehículo no se cierra con el botón X y la consola registra `Uncaught Error: Unimplemented method at onClick`.
- **URB-01 · Mayor.** Al calcular la ruta, la tarifa Shine responde `503` en `/api/v1/tariffs/price`. La interfaz no lo muestra; solo aparece en la pestaña Network.
- **URTCBG-04 · Alta.** El campo Número de tarjeta acepta letras, no muestra error y habilita el botón Agregar.

**Técnicas:** casos de prueba paso a paso, checklist de interfaz, pruebas entre navegadores, particiones de equivalencia, análisis de valores límite y revisión de Network y Console en Chrome DevTools.

➡️ [Detalle del proyecto](01-03-urban-routes-qa-manual/README.md) · [Hoja con todos los casos y bug reports](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit)

## Contenido

| Carpeta | Tema | Herramientas | Estado |
|---|---|---|---|
| [01-03-urban-routes-qa-manual](01-03-urban-routes-qa-manual/) | QA manual web: casos de prueba, checklist de interfaz y formularios | Chrome DevTools, Google Sheets | ✅ Completo |
| [04-pruebas-api](04-pruebas-api/) | Pruebas de API | Postman | ⏳ En curso |
| [06-bases-de-datos](06-bases-de-datos/) | Fundamentos de bases de datos | SQL, consola | ⏳ Pendiente |
| [07-python-automatizacion](07-python-automatizacion/) | Python para automatización de pruebas | Python, PyCharm | ⏳ Pendiente |
| [08-entorno-automatizacion](08-entorno-automatizacion/) | Entorno para automatización de pruebas | Pytest, Git | ⏳ Pendiente |
| [09-automatizacion-web](09-automatizacion-web/) | Automatización de pruebas de aplicaciones web | Selenium WebDriver | ⏳ Pendiente |
| [10-proyecto-final](10-proyecto-final/) | Proyecto final | Por definir | ⏳ Pendiente |
| [modulo-pruebas-moviles](modulo-pruebas-moviles/) | Pruebas de aplicaciones móviles | Android Studio | ⏳ Pendiente |
| [plantillas](plantillas/) | Plantillas de caso de prueba y bug report | Markdown | ✅ |

El sprint 5 es el curso de preparación de carrera, sin proyecto de pruebas.

## Otros proyectos

- **Suppah Soccah:** plan de pruebas de un juego 3v3 en Roblox que estoy desarrollando: regresión funcional, anti-cheat, telemetría y Art QA. En preparación.

## Herramientas

Chrome DevTools · Postman · Jira · Google Sheets · SQL · Python · Pytest · Selenium · Git · Android Studio · Unreal Engine · Unity
