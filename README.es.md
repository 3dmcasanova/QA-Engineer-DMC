[English](README.md) | **Español**

# Portafolio QA Engineer · David Morales

Portafolio de pruebas de software del programa **QA Engineer de TripleTen**: 10 sprints y un módulo de pruebas de aplicaciones móviles. Vengo del arte 3D y el desarrollo de videojuegos, y me estoy especializando en QA de videojuegos.

## Proyecto destacado: Urban Routes

Sprints 1 a 3. Pruebas manuales de **Urban Routes**, una aplicación web para pedir taxi: flujo funcional, red y consola, interfaz contra el diseño en dos navegadores, y validación de un formulario de pago.

| | |
|---|---|
| Verificaciones ejecutadas | **81** |
| Verificaciones fallidas | **26** |
| Bugs documentados | **21** · gravedad (sprint 1): 1 crítica, 1 alta · prioridad (sprints 2 y 3): 11 alta, 8 baja |
| Entornos | Chrome 800×600 y Firefox 1920×1080 |

**Bugs destacados**

- **URB-02 · Crítica.** La pantalla de espera del vehículo no se cierra con el botón X y la consola registra `Uncaught Error: Unimplemented method at onClick`.
- **URB-01 · Alta.** Al calcular la ruta, la tarifa Shine responde `503` en `/api/v1/tariffs/price`. La interfaz no lo muestra; solo aparece en la pestaña Network.
- **URTCBG-04 · Prioridad alta.** El campo Número de tarjeta acepta letras, no muestra error y habilita el botón Agregar.

**Técnicas:** casos de prueba paso a paso, checklist de interfaz, pruebas entre navegadores, particiones de equivalencia, análisis de valores límite y revisión de Network y Console en Chrome DevTools.

➡️ [Detalle del proyecto](01-03-urban-routes-manual-qa/README.es.md) · [Hoja en español con todos los casos y bug reports](https://docs.google.com/spreadsheets/d/1CdvvxNiFTvGbF5EXkdze9hxSYIwIRcebiG0vghuJWa0/edit)

## Pruebas de API: Urban Grocers

Sprint 4. Pruebas de la API de **Urban Grocers**, un servicio de entrega de supermercado, en Postman: paquetes de productos, costo y tiempo de entrega, y carrito de compras.

| | |
|---|---|
| Casos ejecutados | **46** |
| Casos fallidos | **26** |
| Bugs documentados | **26** (5 de gravedad crítica, 10 alta, 10 media y 1 baja) |
| Endpoints | `/api/v1/grocery-bundles/{id}/products`, `/speedy/v1/calculate`, `/api/v1/orders` |

**Bugs destacados**

- **UG-1-BUG-08 · Crítica.** Enviar `"productId": "Twelve"` responde `500` (`invalid input syntax for type integer`) y todas las peticiones posteriores a ese paquete fallan, incluso las válidas.
- **UG-2-BUG-01 · Alta.** Una entrega a las 22:00, fuera de horario, responde `isItPossibleToDeliver: true`. El código es `200`; el bug solo se ve en el body.
- **UG-3-BUG-02 · Alta.** Borrar un carrito existente con un token válido responde `404` "Cart with id=2 not found".

**Técnicas:** clases de equivalencia, análisis de valores límite, escenarios positivos y negativos, peticiones con y sin autorización, validación del body de la respuesta y comprobaciones automáticas con tests de Postman y Newman.

➡️ [Detalle del proyecto](04-api-testing/README.es.md) · [Hoja en español con todos los casos y bug reports](https://docs.google.com/spreadsheets/d/1kRWOYJvxgWPB4ujRQ1uBt8FCqq0NPA9MpzRmX1OOvK0/edit)

## Contenido

| Carpeta | Tema | Herramientas | Estado |
|---|---|---|---|
| [01-03-urban-routes-manual-qa](01-03-urban-routes-manual-qa/README.es.md) | QA manual web: casos de prueba, checklist de interfaz y formularios | Chrome DevTools, Google Sheets | ✅ Completo |
| [04-api-testing](04-api-testing/README.es.md) | Pruebas de API: paquetes de productos, entregas y carrito | Postman, Newman, Swagger, Google Sheets | ✅ Completo |
| [templates](templates/README.es.md) | Plantillas de caso de prueba y bug report | Markdown | ✅ |

**Próximamente:** SQL, Python con Pytest, automatización web con Selenium, pruebas de aplicaciones móviles y un proyecto final. Cada uno se agregará aquí al completarlo.

## Otros proyectos

- **Suppah Soccah:** plan de pruebas de un juego 3v3 en Roblox que estoy desarrollando: regresión funcional, anti-cheat, telemetría y Art QA. En preparación.

## Herramientas

Chrome DevTools · Postman · Jira · Google Sheets · SQL · Python · Pytest · Selenium · Git · Android Studio · Unreal Engine · Unity

## Idiomas

Español (nativo) · Inglés: C1 avanzado ([EF SET 69/100](https://cert.efset.org/en/goUbJe))

## Contacto

[LinkedIn: David Morales](https://www.linkedin.com/in/3dmcasanova/)

Correo: david.morales.dmqa@gmail.com
