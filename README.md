**English** | [Español](README.es.md)

# QA Engineer Portfolio · David Morales

Software testing portfolio built during the **TripleTen QA Engineer** program: 10 sprints plus a mobile app testing module. I come from 3D art and game development, and I'm specializing in game QA.

## Featured project: Urban Routes

Sprints 1 to 3. Manual testing of **Urban Routes**, a ride-hailing web app: functional flow, network and console checks, UI against the design in two browsers, and validation of a payment form.

| | |
|---|---|
| Checks executed | **81** |
| Checks failed | **26** |
| Bugs reported | **21** · severity (sprint 1): 1 critical, 1 high · priority (sprints 2 and 3): 11 high, 8 low |
| Environments | Chrome 800×600 and Firefox 1920×1080 |

**Highlighted bugs**

- **URB-02 · Critical.** The vehicle waiting screen does not close with the X button, and the console logs `Uncaught Error: Unimplemented method at onClick`.
- **URB-01 · High.** When the route is calculated, the Shine tariff returns `503` from `/api/v1/tariffs/price`. The UI hides it; it only shows up in the Network tab.
- **URTCBG-04 · High priority.** The card number field accepts letters, shows no error and enables the Add button.

**Techniques:** step-by-step test cases, UI checklist, cross-browser testing, equivalence partitioning, boundary value analysis, and Network and Console review in Chrome DevTools.

➡️ [Project details](01-03-urban-routes-manual-qa/README.md) · [Spreadsheet with every test case and bug report](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit)

## API testing: Urban Grocers

Sprint 4. API testing of **Urban Grocers**, a grocery delivery API, in Postman: product bundles, delivery cost and time, and the shopping cart.

| | |
|---|---|
| Test cases executed | **46** |
| Test cases failed | **26** |
| Bugs reported | **26** (5 critical, 10 high, 10 medium, 1 low) |
| Endpoints | `/api/v1/grocery-bundles/{id}/products`, `/speedy/v1/calculate`, `/api/v1/orders` |

**Highlighted bugs**

- **UG-1-BUG-08 · Critical.** Sending `"productId": "Twelve"` returns `500` (`invalid input syntax for type integer`), and every later request to that bundle fails, even valid ones.
- **UG-2-BUG-01 · High.** A delivery at 22:00, outside business hours, returns `isItPossibleToDeliver: true`. The status code is `200`; the bug is only in the body.
- **UG-3-BUG-02 · High.** Deleting an existing cart with a valid token returns `404` "Cart with id=2 not found".

**Techniques:** equivalence classes, boundary value analysis, positive and negative scenarios, requests with and without authorization, response body validation, and automated checks with Postman tests and Newman.

➡️ [Project details](04-api-testing/README.md) · [Spreadsheet with every test case and bug report](https://docs.google.com/spreadsheets/d/1AIelYIYvlfqz0BGgRB3tkbC1-ZJSi9gmikdd0O0Wuzo/edit)

## Contents

| Folder | Topic | Tools | Status |
|---|---|---|---|
| [01-03-urban-routes-manual-qa](01-03-urban-routes-manual-qa/) | Manual web QA: test cases, UI checklist and forms | Chrome DevTools, Google Sheets | ✅ Done |
| [04-api-testing](04-api-testing/) | API testing: product bundles, delivery and shopping cart | Postman, Newman, Swagger, Google Sheets | ✅ Done |
| [templates](templates/) | Test case and bug report templates | Markdown | ✅ |

**Coming next:** SQL, Python with Pytest, web automation with Selenium, mobile app testing and a final project. Each one will be added here when it's done.

## Other projects

- **Suppah Soccah:** test plan for a 3v3 Roblox game I'm developing: functional regression, anti-cheat, telemetry and art QA. In progress.

## Tools

Chrome DevTools · Postman · Jira · Google Sheets · SQL · Python · Pytest · Selenium · Git · Android Studio · Unreal Engine · Unity

## Languages

English: C1 Advanced ([EF SET 69/100](https://cert.efset.org/en/goUbJe)) · Spanish (native)

## Contact

[LinkedIn: David Morales](https://www.linkedin.com/in/3dmcasanova/)

Email: david.morales.dmqa@gmail.com
