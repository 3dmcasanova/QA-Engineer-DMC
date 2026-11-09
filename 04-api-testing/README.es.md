[English](README.md) | **Español**

# Urban Grocers · Pruebas de API

Sprint 4. Urban Grocers es la API de práctica de entrega de supermercado del programa QA Engineer de TripleTen. Probé tres historias de usuario en Postman a partir de la documentación de Swagger: agregar productos a un paquete, calcular el costo y el tiempo de entrega, y gestionar el carrito de compras.

Todo el trabajo está en una sola hoja de cálculo: [ver la hoja completa](https://docs.google.com/spreadsheets/d/1Acz0HfCy_edp4G604z_HCaaMaRX2kvuu_1aAvDZB4Mw/edit).

## Resultados

| Historia de usuario | Endpoint | Casos | Aprobados | Fallidos | Bugs |
|---|---|---|---|---|---|
| UG-1 · Agregar productos a un paquete | `POST /api/v1/grocery-bundles/{id}/products` | 11 | 3 | 8 | 8 |
| UG-2 · Calcular el costo y el tiempo de entrega | `POST /speedy/v1/calculate` | 22 | 6 | 16 | 16 |
| UG-3 · Gestionar el carrito de compras (opcional) | `/api/v1/orders` (POST, PUT, GET, DELETE) | 13 | 11 | 2 | 2 |
| **Total** | | **46** | **20** | **26** | **26** |

Bugs por gravedad: **5 críticos**, 10 mayores, 10 menores y 1 trivial.

## Ejecuta las pruebas tú mismo

Las tres colecciones de Postman están en [postman/](postman/). Cada petición tiene aserciones `pm.test` con el resultado esperado de la hoja. Cada colección crea su propio usuario, y sus propios paquetes o carritos, así que no necesita preparación y no depende de lo que otras personas hayan hecho en el servidor.

```bash
npx newman run 04-api-testing/postman/ug-2-delivery.postman_collection.json
```

También puedes importarlas en Postman y usar el Collection Runner.

Las aserciones con el nombre de un bug, como `UG-2-BUG-01 · isItPossibleToDeliver is false`, comprueban el comportamiento correcto, así que fallan hasta que se corrija ese bug. Última ejecución con Newman, noviembre de 2026:

| Colección | Peticiones | Aserciones | Fallidas | Qué falla |
|---|---|---|---|---|
| [UG-1 · Paquetes de productos](postman/ug-1-product-bundles.postman_collection.json) | 13 | 15 | 9 | Los 8 bugs reportados (UG-1-BUG-08 tiene dos aserciones) |
| [UG-2 · Costo y tiempo de entrega](postman/ug-2-delivery.postman_collection.json) | 22 | 31 | 16 | Los 16 bugs reportados |
| [UG-3 · Carrito de compras](postman/ug-3-shopping-cart.postman_collection.json) | 14 | 17 | 5 | UG-3-BUG-01 y UG-3-BUG-02, más 2 hallazgos nuevos: TC-02 responde `409` en lugar de `400`, y TC-03 responde `500` (`productsList.reduce is not a function`) |

## UG-1 · Paquetes de productos

Cubrí el flujo válido, valores límite de `productId` y `quantity`, tipos inválidos (valor vacío, caracteres especiales, letras, texto), un body con estructura inválida, más de 30 productos en un paquete y un paquete que no existe.

[Casos de prueba](https://docs.google.com/spreadsheets/d/1Acz0HfCy_edp4G604z_HCaaMaRX2kvuu_1aAvDZB4Mw/edit#gid=997693036) · [Bug reports](https://docs.google.com/spreadsheets/d/1Acz0HfCy_edp4G604z_HCaaMaRX2kvuu_1aAvDZB4Mw/edit#gid=1474709818)

| ID | Bug | Gravedad | Evidencia |
|---|---|---|---|
| UG-1-BUG-01 | `"productId": " "` responde `500` y daña el paquete: las peticiones válidas posteriores también responden `500` | Crítico | [captura](https://drive.google.com/file/d/1NM_0j7kKWVmsXNk6s0xHFsvMGKaR-EWg/view?usp=sharing) |
| UG-1-BUG-02 | `"quantity": " "` responde `200` y altera `productsCount`, que suma espacios con cada petición | Mayor | [captura](https://drive.google.com/file/d/1J4qv2zhtZoaxZqGJ2MGCe5hFd_R7lmyE/view?usp=sharing) |
| UG-1-BUG-03 | Un `productId` inexistente (9999) se agrega al paquete sin nombre, precio ni peso | Menor | [captura](https://drive.google.com/file/d/1ghzXYlWnO4kZYmMQ8qWrd7s6MlxHtzZo/view?usp=sharing) |
| UG-1-BUG-04 | `"quantity": 999999` se acepta sin comprobar stock ni disponibilidad | Mayor | [captura](https://drive.google.com/file/d/1pZozyWDWjZLagGhX_S8RV8ojp9e6TPP4/view?usp=sharing) |
| UG-1-BUG-05 | `"productId": "@!?"` responde `500` y daña el paquete | Crítico | [captura](https://drive.google.com/file/d/1uUXv9mZggoQxVGTx979IOonb8pfHmTAX/view?usp=sharing) |
| UG-1-BUG-06 | `"productId": "uno"` responde `500` y daña el paquete | Crítico | [captura](https://drive.google.com/file/d/1r1Q31ok20r_tRtJN6mprJ0xh1iWYi20f/view?usp=sharing) |
| UG-1-BUG-07 | `productsList` enviado como objeto en lugar de array responde `500` (`productsList.map is not a function`) en lugar de `400` | Trivial | [captura](https://drive.google.com/file/d/1V4UswKgkgTBi0zYymQjCUCVHQaYfjpFO/view?usp=sharing) |
| UG-1-BUG-08 | `"productId": "Twelve"` responde `500` y todas las peticiones posteriores a ese paquete fallan, incluso las válidas | Crítico | [petición](https://drive.google.com/file/d/1psl3a-OzOA0giO4x-b9VXy8gkOYaOtdk/view?usp=sharing) · [siguiente petición válida](https://drive.google.com/file/d/1Un61kl8SXFhxDdUms6p4QdQnf3oxDTCU/view?usp=sharing) |

La API no valida el tipo de `productId` ni de `quantity`. Los valores de texto llegan a la base de datos (`invalid input syntax for type integer`) y dejan el paquete inservible. Por eso ejecuté cada uno de estos casos en un paquete nuevo, para que un fallo no ocultara ni provocara el siguiente.

## UG-2 · Costo y tiempo de entrega

El servicio Speedy solo entrega entre las 7 y las 21. Diseñé los valores de `deliveryTime` con **análisis de valores límite** y revisé `isItPossibleToDeliver` en el body de la respuesta, no solo el código de estado.

[Casos de prueba](https://docs.google.com/spreadsheets/d/1Acz0HfCy_edp4G604z_HCaaMaRX2kvuu_1aAvDZB4Mw/edit#gid=1772128769) · [Bug reports](https://docs.google.com/spreadsheets/d/1Acz0HfCy_edp4G604z_HCaaMaRX2kvuu_1aAvDZB4Mw/edit#gid=808547673)

| Clase | `deliveryTime` | Esperado | Resultado |
|---|---|---|---|
| Negativo | `-1` | `400` | ❌ `200` |
| Fuera de rango | `1` | `isItPossibleToDeliver: false` | ❌ `true` |
| Límite inferior −1 | `6` | `isItPossibleToDeliver: false` | ❌ `true` |
| Límite inferior y límite inferior +1 | `7`, `8` | `isItPossibleToDeliver: true` | ✅ `true` |
| Intermedio | `14` | `isItPossibleToDeliver: true` | ✅ `true` |
| Límite superior −1 y límite superior | `20`, `21` | `isItPossibleToDeliver: true` | ✅ `true` |
| Límite superior +1 | `22` | `isItPossibleToDeliver: false` | ❌ `true` |
| Letras y símbolos | `"ocho"`, `"@!?"` | `400` | ❌ `200` |

`productId` y `quantity` siguen el mismo patrón: aceptan letras, símbolos, texto vacío, `null` y números negativos, y un pedido de 999 999 unidades se puede entregar. Solo el body con una estructura JSON rota recibe un `400`.

| ID | Bug | Gravedad | Evidencia |
|---|---|---|---|
| UG-2-BUG-01 | `deliveryTime` 22 (fuera de horario) responde `isItPossibleToDeliver: true` | Mayor | [captura](https://drive.google.com/file/d/1brYcdl802B49LPPhRlEN3HE3aBAZ0fe0/view?usp=sharing) |
| UG-2-BUG-02 | `deliveryTime` 6 (antes del horario) responde `isItPossibleToDeliver: true` | Mayor | [captura](https://drive.google.com/file/d/1ptEzYG8GYQlSF4PPBiJhTT5MK3N2tHox/view?usp=sharing) |
| UG-2-BUG-03 | Un producto sin `quantity` responde `200` | Menor | [captura](https://drive.google.com/file/d/1ki09r4qGvr7sU7qIkaDkFbJoRJR1lZOR/view?usp=sharing) |
| UG-2-BUG-04 | `"productId": "uno"` responde `200` | Menor | [captura](https://drive.google.com/file/d/1QPKsASbf1n9hCp3yAHZvuQSaZPlCHVoP/view?usp=sharing) |
| UG-2-BUG-05 | `"quantity": "uno"` responde `200` | Menor | [captura](https://drive.google.com/file/d/1LFBrg5A0CIDgcb2M-O5WBOTbuWkVpy51/view?usp=sharing) |
| UG-2-BUG-06 | `"productId": "@!?"` responde `200` | Menor | [captura](https://drive.google.com/file/d/10ef1FlmsxJ2a_KOHAqDAnRl5hsD1BO0W/view?usp=sharing) |
| UG-2-BUG-07 | `"quantity": "@!?"` responde `200` | Menor | [captura](https://drive.google.com/file/d/1L-lOweO547wVqL-n-1zS5LcFWDZ3Llq9/view?usp=sharing) |
| UG-2-BUG-08 | `"productId": ""` responde `200` | Menor | [captura](https://drive.google.com/file/d/1o-j9pen8qUMBW-ihwiXMxzW7AO9hDMiy/view?usp=sharing) |
| UG-2-BUG-09 | `"quantity": ""` responde `200` | Menor | [captura](https://drive.google.com/file/d/1Y-hqa3kNJ_ZcQLX4RjanIxLuETBc4_c7/view?usp=sharing) |
| UG-2-BUG-10 | `"quantity": 999999` responde `isItPossibleToDeliver: true` | Mayor | [captura](https://drive.google.com/file/d/1DxRPBJxNEi-8z1GWVkiyQ2_Mxro2S6Eq/view?usp=sharing) |
| UG-2-BUG-11 | `"quantity": null` responde `200` | Mayor | [captura](https://drive.google.com/file/d/1BU7IKBTlHLlR9m5WJi8jwtuHmdPEFV3b/view?usp=sharing) |
| UG-2-BUG-12 | `"quantity": -1` responde `200` | Mayor | [captura](https://drive.google.com/file/d/1mZ8YTpRg0N2dT_nwrZAdSxWH07kshOeU/view?usp=sharing) |
| UG-2-BUG-13 | `"deliveryTime": -1` responde `200` | Mayor | [captura](https://drive.google.com/file/d/1YfKefta3JdcJz4vPF8liOfHlU8omqRzI/view?usp=sharing) |
| UG-2-BUG-14 | `deliveryTime` 1 (fuera de rango) responde `isItPossibleToDeliver: true` | Mayor | [captura](https://drive.google.com/file/d/1YYWnzgpfEYukupYEjG9kckcEJDlRGJuK/view?usp=sharing) |
| UG-2-BUG-15 | `"deliveryTime": "ocho"` responde `200` | Menor | [captura](https://drive.google.com/file/d/1MXvIIapkAUa1-CoDj1ex2ukFKNsYZzfe/view?usp=sharing) |
| UG-2-BUG-16 | `"deliveryTime": "@!?"` responde `200` | Menor | [captura](https://drive.google.com/file/d/102aSFxxpMjPL7WAK60eTPIYMeuVks2qD/view?usp=sharing) |

## UG-3 · Carrito de compras (opcional)

Probé las cinco operaciones del carrito (crear, agregar productos, consultar, borrar y completar el pedido) con y sin token de autorización, con un body inválido y con carritos que no existen.

[Casos de prueba](https://docs.google.com/spreadsheets/d/1Acz0HfCy_edp4G604z_HCaaMaRX2kvuu_1aAvDZB4Mw/edit#gid=1823327897) · [Bug reports](https://docs.google.com/spreadsheets/d/1Acz0HfCy_edp4G604z_HCaaMaRX2kvuu_1aAvDZB4Mw/edit#gid=1721365376)

| ID | Bug | Gravedad | Evidencia |
|---|---|---|---|
| UG-3-BUG-01 | Agregar productos válidos a un carrito existente (`PUT /api/v1/orders/{id}`) responde `409` "No hay un almacén para procesar tu pedido" | Crítico | [captura](https://drive.google.com/file/d/159sGVlqfXxA_oYWKQr-D7hcIoY3SZsHQ/view?usp=sharing) |
| UG-3-BUG-02 | Borrar un carrito existente con un token válido (`DELETE /api/v1/orders/{id}`) responde `404` "Cart with id=2 not found" | Mayor | [captura](https://drive.google.com/file/d/17xtmmNxO6UguqYQ2EBKzGgr_NNOFWFsi/view?usp=sharing) |

El mismo carrito responde `409` al `PUT` y `404` al `DELETE`, así que el carrito existe y el `404` viene de la operación de borrado.

## Qué demuestra este proyecto

- Diseño los datos de prueba de una API con clases de equivalencia y valores límite para cada campo del body.
- Reviso el body de la respuesta, no solo el código de estado: varios bugs responden `200` con un valor incorrecto (`isItPossibleToDeliver`, `productsCount`).
- Aíslo las pruebas que dañan datos, para que un fallo no oculte ni provoque otro.
- Toda la suite se vuelve a ejecutar con un solo comando de Newman, y cada aserción que falla apunta a su ID de bug.
- Cada caso fallido tiene su bug report, con pasos para reproducir, resultado esperado y actual, severidad y una captura de Postman como evidencia.
