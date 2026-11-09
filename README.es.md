[English](README.md) | **Español**

# Portafolio QA Engineer · David Morales

Portafolio de pruebas de software del programa **QA Engineer de TripleTen**: 10 sprints y un módulo de pruebas de aplicaciones móviles. Vengo del arte 3D y el desarrollo de videojuegos, y me estoy especializando en QA de videojuegos.

## Proyecto destacado: Urban Routes

Sprints 1 a 3. Pruebas manuales de **Urban Routes**, una aplicación web para pedir taxi: flujo funcional, red y consola, interfaz contra el diseño en dos navegadores, y validación de un formulario de pago.

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

➡️ [Detalle del proyecto](01-03-urban-routes-manual-qa/README.es.md) · [Hoja en español con todos los casos y bug reports](https://docs.google.com/spreadsheets/d/1CdvvxNiFTvGbF5EXkdze9hxSYIwIRcebiG0vghuJWa0/edit)

## Pruebas de API: Urban Grocers

Sprint 4. Pruebas de la API de **Urban Grocers**, un servicio de entrega de supermercado, en Postman: paquetes de productos, costo y tiempo de entrega, y carrito de compras.

| | |
|---|---|
| Casos ejecutados | **46** |
| Casos fallidos | **26** |
| Bugs documentados | **26** (5 críticos, 10 de severidad alta, 10 media y 1 baja) |
| Endpoints | `/api/v1/grocery-bundles/{id}/products`, `/speedy/v1/calculate`, `/api/v1/orders` |

**Bugs destacados**

- **UG-1-BUG-08 · Crítica.** Enviar `"productId": "Twelve"` responde `500` (`invalid input syntax for type integer`) y todas las peticiones posteriores a ese paquete fallan, incluso las válidas.
- **UG-2-BUG-01 · Alta.** Una entrega a las 22:00, fuera de horario, responde `isItPossibleToDeliver: true`. El código es `200`; el bug solo se ve en el body.
- **UG-3-BUG-02 · Alta.** Borrar un carrito existente con un token válido responde `404` "Cart with id=2 not found".

**Técnicas:** clases de equivalencia, análisis de valores límite, escenarios positivos y negativos, peticiones con y sin autorización, y validación del body de la respuesta.

➡️ [Detalle del proyecto](04-api-testing/README.es.md) · [Hoja en español con todos los casos y bug reports](https://docs.google.com/spreadsheets/d/1Acz0HfCy_edp4G604z_HCaaMaRX2kvuu_1aAvDZB4Mw/edit)

## Contenido

| Carpeta | Tema | Herramientas | Estado |
|---|---|---|---|
| [01-03-urban-routes-manual-qa](01-03-urban-routes-manual-qa/README.es.md) | QA manual web: casos de prueba, checklist de interfaz y formularios | Chrome DevTools, Google Sheets | ✅ Completo |
| [04-api-testing](04-api-testing/README.es.md) | Pruebas de API: paquetes de productos, entregas y carrito | Postman, Swagger, Google Sheets | ✅ Completo |
| [06-databases](06-databases/README.es.md) | Fundamentos de bases de datos | SQL, consola | ⏳ Pendiente |
| [07-python-for-test-automation](07-python-for-test-automation/README.es.md) | Python para automatización de pruebas | Python, PyCharm | ⏳ Pendiente |
| [08-test-automation-environment](08-test-automation-environment/README.es.md) | Entorno para automatización de pruebas | Pytest, Git | ⏳ Pendiente |
| [09-web-test-automation](09-web-test-automation/README.es.md) | Automatización de pruebas de aplicaciones web | Selenium WebDriver | ⏳ Pendiente |
| [10-final-project](10-final-project/README.es.md) | Proyecto final | Por definir | ⏳ Pendiente |
| [mobile-app-testing](mobile-app-testing/README.es.md) | Pruebas de aplicaciones móviles | Android Studio | ⏳ Pendiente |
| [templates](templates/README.es.md) | Plantillas de caso de prueba y bug report | Markdown | ✅ |

El sprint 5 es el curso de preparación de carrera, sin proyecto de pruebas.

## Otros proyectos

- **Suppah Soccah:** plan de pruebas de un juego 3v3 en Roblox que estoy desarrollando: regresión funcional, anti-cheat, telemetría y Art QA. En preparación.

## Herramientas

Chrome DevTools · Postman · Jira · Google Sheets · SQL · Python · Pytest · Selenium · Git · Android Studio · Unreal Engine · Unity

## Idiomas

Español (nativo) · Inglés: C1 avanzado ([EF SET 69/100](https://cert.efset.org/en/goUbJe))

## Contacto

[LinkedIn: David Morales](https://www.linkedin.com/in/3dmcasanova/)
