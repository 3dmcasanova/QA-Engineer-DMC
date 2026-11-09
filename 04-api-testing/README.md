**English** | [Español](README.es.md)

# Urban Grocers · API testing

Sprint 4. Urban Grocers is the practice grocery delivery API of the TripleTen QA Engineer program. I tested three user stories in Postman against the Swagger documentation: adding products to a product bundle, calculating delivery cost and time, and managing the shopping cart.

All the work lives in one spreadsheet: [open the full spreadsheet](https://docs.google.com/spreadsheets/d/1AIelYIYvlfqz0BGgRB3tkbC1-ZJSi9gmikdd0O0Wuzo/edit).

## Results

| User story | Endpoint | Test cases | Passed | Failed | Bugs |
|---|---|---|---|---|---|
| UG-1 · Add products to a product bundle | `POST /api/v1/grocery-bundles/{id}/products` | 11 | 3 | 8 | 8 |
| UG-2 · Calculate delivery cost and time | `POST /speedy/v1/calculate` | 22 | 6 | 16 | 16 |
| UG-3 · Manage the shopping cart (optional) | `/api/v1/orders` (POST, PUT, GET, DELETE) | 13 | 11 | 2 | 2 |
| **Total** | | **46** | **20** | **26** | **26** |

Bugs by severity: **5 critical**, 10 high, 10 medium, 1 low.

## Run the tests yourself

The three Postman collections are in [postman/](postman/). Every request has `pm.test` assertions for the expected result in the spreadsheet. Each collection creates its own user, and its own bundles or carts, so it needs no setup and does not depend on what other people did on the server.

```bash
npx newman run 04-api-testing/postman/ug-2-delivery.postman_collection.json
```

You can also import them into Postman and use the Collection Runner.

Assertions named after a bug, such as `UG-2-BUG-01 · isItPossibleToDeliver is false`, check the correct behavior, so they fail until that bug is fixed. Last run with Newman, November 2026:

| Collection | Requests | Assertions | Failed | What fails |
|---|---|---|---|---|
| [UG-1 · Product bundles](postman/ug-1-product-bundles.postman_collection.json) | 13 | 15 | 9 | The 8 reported bugs (UG-1-BUG-08 has two assertions) |
| [UG-2 · Delivery cost and time](postman/ug-2-delivery.postman_collection.json) | 22 | 31 | 16 | The 16 reported bugs |
| [UG-3 · Shopping cart](postman/ug-3-shopping-cart.postman_collection.json) | 14 | 17 | 5 | UG-3-BUG-01 and UG-3-BUG-02, plus 2 new findings: TC-02 returns `409` instead of `400`, and TC-03 returns `500` (`productsList.reduce is not a function`) |

## UG-1 · Product bundles

I covered the valid flow, boundary values for `productId` and `quantity`, invalid types (empty value, special characters, letters, strings), a body with an invalid structure, more than 30 products in one bundle and a bundle that does not exist.

[Test cases](https://docs.google.com/spreadsheets/d/1AIelYIYvlfqz0BGgRB3tkbC1-ZJSi9gmikdd0O0Wuzo/edit#gid=997693036) · [Bug reports](https://docs.google.com/spreadsheets/d/1AIelYIYvlfqz0BGgRB3tkbC1-ZJSi9gmikdd0O0Wuzo/edit#gid=1474709818)

| ID | Bug | Severity | Evidence |
|---|---|---|---|
| UG-1-BUG-01 | `"productId": " "` returns `500` and breaks the bundle: later valid requests to it also return `500` | Critical | [screenshot](https://drive.google.com/file/d/1NM_0j7kKWVmsXNk6s0xHFsvMGKaR-EWg/view?usp=sharing) |
| UG-1-BUG-02 | `"quantity": " "` returns `200` and corrupts `productsCount`, which gains extra spaces with every request | High | [screenshot](https://drive.google.com/file/d/1J4qv2zhtZoaxZqGJ2MGCe5hFd_R7lmyE/view?usp=sharing) |
| UG-1-BUG-03 | A non-existent `productId` (9999) is added to the bundle without name, price or weight | Medium | [screenshot](https://drive.google.com/file/d/1ghzXYlWnO4kZYmMQ8qWrd7s6MlxHtzZo/view?usp=sharing) |
| UG-1-BUG-04 | `"quantity": 999999` is accepted with no stock or availability check | High | [screenshot](https://drive.google.com/file/d/1pZozyWDWjZLagGhX_S8RV8ojp9e6TPP4/view?usp=sharing) |
| UG-1-BUG-05 | `"productId": "@!?"` returns `500` and breaks the bundle | Critical | [screenshot](https://drive.google.com/file/d/1uUXv9mZggoQxVGTx979IOonb8pfHmTAX/view?usp=sharing) |
| UG-1-BUG-06 | `"productId": "uno"` returns `500` and breaks the bundle | Critical | [screenshot](https://drive.google.com/file/d/1r1Q31ok20r_tRtJN6mprJ0xh1iWYi20f/view?usp=sharing) |
| UG-1-BUG-07 | `productsList` sent as an object instead of an array returns `500` (`productsList.map is not a function`) instead of `400` | Low | [screenshot](https://drive.google.com/file/d/1V4UswKgkgTBi0zYymQjCUCVHQaYfjpFO/view?usp=sharing) |
| UG-1-BUG-08 | `"productId": "Twelve"` returns `500` and every later request to that bundle fails, even valid ones | Critical | [request](https://drive.google.com/file/d/1psl3a-OzOA0giO4x-b9VXy8gkOYaOtdk/view?usp=sharing) · [next valid request](https://drive.google.com/file/d/1Un61kl8SXFhxDdUms6p4QdQnf3oxDTCU/view?usp=sharing) |

The API does not validate the type of `productId` or `quantity`. Text values reach the database (`invalid input syntax for type integer`) and leave the bundle unusable. Because of that, I ran each of these cases on a new bundle, so that one failure would not hide or cause the next one.

## UG-2 · Delivery cost and time

The Speedy service only delivers between 7 and 21. I designed the `deliveryTime` values with **boundary value analysis** and checked `isItPossibleToDeliver` in the response body, not just the status code.

[Test cases](https://docs.google.com/spreadsheets/d/1AIelYIYvlfqz0BGgRB3tkbC1-ZJSi9gmikdd0O0Wuzo/edit#gid=1772128769) · [Bug reports](https://docs.google.com/spreadsheets/d/1AIelYIYvlfqz0BGgRB3tkbC1-ZJSi9gmikdd0O0Wuzo/edit#gid=808547673)

| Class | `deliveryTime` | Expected | Result |
|---|---|---|---|
| Negative | `-1` | `400` | ❌ `200` |
| Out of range | `1` | `isItPossibleToDeliver: false` | ❌ `true` |
| Lower limit −1 | `6` | `isItPossibleToDeliver: false` | ❌ `true` |
| Lower limit and lower limit +1 | `7`, `8` | `isItPossibleToDeliver: true` | ✅ `true` |
| Middle | `14` | `isItPossibleToDeliver: true` | ✅ `true` |
| Upper limit −1 and upper limit | `20`, `21` | `isItPossibleToDeliver: true` | ✅ `true` |
| Upper limit +1 | `22` | `isItPossibleToDeliver: false` | ❌ `true` |
| Letters and symbols | `"ocho"`, `"@!?"` | `400` | ❌ `200` |

`productId` and `quantity` show the same pattern: they accept letters, symbols, empty strings, `null` and negative numbers, and an order of 999,999 units can still be delivered. Only the body with a broken JSON structure gets a `400`.

| ID | Bug | Severity | Evidence |
|---|---|---|---|
| UG-2-BUG-01 | `deliveryTime` 22 (after hours) returns `isItPossibleToDeliver: true` | High | [screenshot](https://drive.google.com/file/d/1brYcdl802B49LPPhRlEN3HE3aBAZ0fe0/view?usp=sharing) |
| UG-2-BUG-02 | `deliveryTime` 6 (before hours) returns `isItPossibleToDeliver: true` | High | [screenshot](https://drive.google.com/file/d/1ptEzYG8GYQlSF4PPBiJhTT5MK3N2tHox/view?usp=sharing) |
| UG-2-BUG-03 | A product without `quantity` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/1ki09r4qGvr7sU7qIkaDkFbJoRJR1lZOR/view?usp=sharing) |
| UG-2-BUG-04 | `"productId": "uno"` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/1QPKsASbf1n9hCp3yAHZvuQSaZPlCHVoP/view?usp=sharing) |
| UG-2-BUG-05 | `"quantity": "uno"` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/1LFBrg5A0CIDgcb2M-O5WBOTbuWkVpy51/view?usp=sharing) |
| UG-2-BUG-06 | `"productId": "@!?"` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/10ef1FlmsxJ2a_KOHAqDAnRl5hsD1BO0W/view?usp=sharing) |
| UG-2-BUG-07 | `"quantity": "@!?"` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/1L-lOweO547wVqL-n-1zS5LcFWDZ3Llq9/view?usp=sharing) |
| UG-2-BUG-08 | `"productId": ""` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/1o-j9pen8qUMBW-ihwiXMxzW7AO9hDMiy/view?usp=sharing) |
| UG-2-BUG-09 | `"quantity": ""` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/1Y-hqa3kNJ_ZcQLX4RjanIxLuETBc4_c7/view?usp=sharing) |
| UG-2-BUG-10 | `"quantity": 999999` returns `isItPossibleToDeliver: true` | High | [screenshot](https://drive.google.com/file/d/1DxRPBJxNEi-8z1GWVkiyQ2_Mxro2S6Eq/view?usp=sharing) |
| UG-2-BUG-11 | `"quantity": null` returns `200` | High | [screenshot](https://drive.google.com/file/d/1BU7IKBTlHLlR9m5WJi8jwtuHmdPEFV3b/view?usp=sharing) |
| UG-2-BUG-12 | `"quantity": -1` returns `200` | High | [screenshot](https://drive.google.com/file/d/1mZ8YTpRg0N2dT_nwrZAdSxWH07kshOeU/view?usp=sharing) |
| UG-2-BUG-13 | `"deliveryTime": -1` returns `200` | High | [screenshot](https://drive.google.com/file/d/1YfKefta3JdcJz4vPF8liOfHlU8omqRzI/view?usp=sharing) |
| UG-2-BUG-14 | `deliveryTime` 1 (out of range) returns `isItPossibleToDeliver: true` | High | [screenshot](https://drive.google.com/file/d/1YYWnzgpfEYukupYEjG9kckcEJDlRGJuK/view?usp=sharing) |
| UG-2-BUG-15 | `"deliveryTime": "ocho"` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/1MXvIIapkAUa1-CoDj1ex2ukFKNsYZzfe/view?usp=sharing) |
| UG-2-BUG-16 | `"deliveryTime": "@!?"` returns `200` | Medium | [screenshot](https://drive.google.com/file/d/102aSFxxpMjPL7WAK60eTPIYMeuVks2qD/view?usp=sharing) |

## UG-3 · Shopping cart (optional)

I tested the five cart operations (create, add products, view, delete and complete the order) with and without the authorization token, with an invalid body and with carts that do not exist.

[Test cases](https://docs.google.com/spreadsheets/d/1AIelYIYvlfqz0BGgRB3tkbC1-ZJSi9gmikdd0O0Wuzo/edit#gid=1823327897) · [Bug reports](https://docs.google.com/spreadsheets/d/1AIelYIYvlfqz0BGgRB3tkbC1-ZJSi9gmikdd0O0Wuzo/edit#gid=1721365376)

| ID | Bug | Severity | Evidence |
|---|---|---|---|
| UG-3-BUG-01 | Adding valid products to an existing cart (`PUT /api/v1/orders/{id}`) returns `409` "No hay un almacén para procesar tu pedido" (no warehouse available) | Critical | [screenshot](https://drive.google.com/file/d/159sGVlqfXxA_oYWKQr-D7hcIoY3SZsHQ/view?usp=sharing) |
| UG-3-BUG-02 | Deleting an existing cart with a valid token (`DELETE /api/v1/orders/{id}`) returns `404` "Cart with id=2 not found" | High | [screenshot](https://drive.google.com/file/d/17xtmmNxO6UguqYQ2EBKzGgr_NNOFWFsi/view?usp=sharing) |

The same cart answers `409` to `PUT` and `404` to `DELETE`, so the cart exists and the `404` comes from the delete operation itself.

## What this project shows

- I design API test data with equivalence classes and boundary values for every field in the body.
- I check the response body, not only the status code: several bugs return `200` with a wrong value (`isItPossibleToDeliver`, `productsCount`).
- I isolate tests that corrupt data, so one failure does not hide or cause another.
- The whole suite runs again with one Newman command, and each failing assertion points to its bug ID.
- Every failed test case has its bug report, with steps to reproduce, expected and actual results, severity and a Postman screenshot as evidence.
