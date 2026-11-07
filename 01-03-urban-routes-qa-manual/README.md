# Urban Routes · QA manual web

Urban Routes es la aplicación web de práctica del programa QA Engineer de TripleTen: el usuario ingresa origen y destino, elige un modo de transporte y una tarifa, agrega un método de pago y pide el taxi.

Todo el trabajo está en una sola hoja de cálculo: [ver la hoja completa](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit).

## Resultados

| Sprint | Ejecutadas | Aprobadas | Fallidas | Bugs |
|---|---|---|---|---|
| 1 · Funcional, red y consola | 11 casos | 9 | 2 | 2 |
| 2 · Checklist de interfaz | 48 verificaciones (24 × 2 entornos) | 36 | 12 | 7 |
| 3 · Formulario de tarjeta | 22 casos | 10 | 12 | 12 |
| **Total** | **81** | **55** | **26** | **21** |

## Sprint 1 · Flujo de pedido y respuestas del servidor

Recorrí el flujo principal (dirección, ruta, modos Óptimo, Flash y Personal, verificación del teléfono con código SMS, mensaje al conductor y pedido) revisando también Network y Console en Chrome DevTools.

[Casos de prueba](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=688606946) · [Bug reports](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=2029093293)

| ID | Bug | Gravedad |
|---|---|---|
| URB-01 | La tarifa Shine responde `503` en `/api/v1/tariffs/price` al calcular la ruta | Mayor |
| URB-02 | La pantalla de espera no se cierra con el botón X; la consola muestra `Uncaught Error: Unimplemented method at onClick` | Crítico |

## Sprint 2 · Interfaz contra el diseño en dos entornos

Comparé la pantalla de reserva con el diseño en 24 puntos, en **Chrome 800×600** y **Firefox 1920×1080**. Cada entorno falla en puntos distintos: Chrome recorta y superpone elementos a baja resolución, y Firefox recorta el ícono de pago.

[Checklist](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=1506116036) · [Bug reports](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=719725667)

| ID | Bug | Entorno | Evidencia |
|---|---|---|---|
| URBG-01 | Falta el ícono del peatón junto al tiempo estimado | Ambos | [captura](https://drive.google.com/file/d/1sC3-kiLtL3Hkk-tJXhtJX3dQdR7lJom8/view?usp=sharing) |
| URBG-02 | La tarjeta Camping no muestra el equipaje sobre el auto | Ambos | [captura](https://drive.google.com/file/d/1Fil3EZ_sBztmgGDuOHJyXVH9IBJpO-4F/view?usp=sharing) |
| URBG-03 | La tarjeta De lujo no muestra las estrellas junto al auto | Ambos | [captura](https://drive.google.com/file/d/1CTphS509Q2oZPlbAZFjZh1rTdsZ8aoXt/view?usp=sharing) |
| URBG-04 | La descripción de la tarifa Camping no coincide con el diseño | Ambos | [captura](https://drive.google.com/file/d/1VdpPvO5HcGXjecC_WZN6c3g4oUYH7SwI/view?usp=sharing) |
| URBG-05 | El ícono de tarjeta del método de pago se recorta | Firefox 1920×1080 | [captura](https://drive.google.com/file/d/1n2Fmjo38SdZKV956tPpwCNHwEvutMhNc/view?usp=sharing) |
| URBG-06 | El botón Agregar licencia de conducir y reservar se recorta | Chrome 800×600 | [captura](https://drive.google.com/file/d/1FpQxb4UefoQx8psmQZo2MR0HTkVn-QMy/view?usp=sharing) |
| URBG-07 | Los botones de borrar Desde y Hasta se superponen al menú de automóvil reservado | Chrome 800×600 | [captura](https://drive.google.com/file/d/18M8Ja1Ipz94MLR7a5-O4FH3ren2qp4py/view?usp=sharing) |

Todos con prioridad baja: son diferencias visuales que no bloquean el pedido.

## Sprint 3 · Validación del formulario de tarjeta

Diseñé 20 escenarios con **particiones de equivalencia** y **análisis de valores límite** para los campos Número de tarjeta (12 dígitos) y Código (2 dígitos, de 01 a 99), y los convertí en 22 casos de prueba.

[Escenarios de validación](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=1579365076) · [Casos de prueba](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=1057185272) · [Bug reports](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=328919447)

Valores límite del campo Código:

| Clase | Dato | Esperado | Resultado |
|---|---|---|---|
| Bajo el rango | `00` | Rechazar | ❌ Acepta |
| Mínimo y mínimo +1 | `01`, `02` | Aceptar | ✅ Acepta |
| Máximo −1 y máximo | `98`, `99` | Aceptar | ✅ Acepta |
| Longitud 1 y 3 | `1`, `123` | Rechazar | ❌ Acepta |
| Letras y símbolos | `AA`, `!@` | Rechazar | ❌ Acepta |

El campo Número de tarjeta muestra el mismo patrón: acepta 5, 11 y 13 dígitos, letras, símbolos y espacios manuales, y habilita el botón Agregar en todos esos casos. Por eso documenté 12 bugs (11 de prioridad alta) y recomendé una ronda de pruebas más exhaustiva sobre el flujo de pago.

## Lo que demuestra este proyecto

- Cada caso fallido tiene su bug report, con pasos para reproducirlo, resultado esperado y obtenido, gravedad y evidencia.
- Reviso la red y la consola además de la pantalla.
- Pruebo en más de un entorno para separar fallos de diseño de fallos de resolución o navegador.
- Diseño los datos de prueba con técnicas formales en lugar de probar al azar.
