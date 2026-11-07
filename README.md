**English** | [Español](README.es.md)

# QA Engineer Portfolio · David Morales

Software testing portfolio built during the **TripleTen QA Engineer** program: 10 sprints plus a mobile app testing module. I come from 3D art and game development, and I'm specializing in game QA.

## Featured project: Urban Routes

Sprints 1 to 3. Manual testing of **Urban Routes**, a ride-hailing web app: functional flow, network and console checks, UI against the design in two browsers, and validation of a payment form.

| | |
|---|---|
| Checks executed | **81** |
| Checks failed | **26** |
| Bugs reported | **21** (1 critical, 1 major, 11 high priority, 8 low priority) |
| Environments | Chrome 800×600 and Firefox 1920×1080 |

**Highlighted bugs**

- **URB-02 · Critical.** The vehicle waiting screen does not close with the X button, and the console logs `Uncaught Error: Unimplemented method at onClick`.
- **URB-01 · Major.** When the route is calculated, the Shine tariff returns `503` from `/api/v1/tariffs/price`. The UI hides it; it only shows up in the Network tab.
- **URTCBG-04 · High.** The card number field accepts letters, shows no error and enables the Add button.

**Techniques:** step-by-step test cases, UI checklist, cross-browser testing, equivalence partitioning, boundary value analysis, and Network and Console review in Chrome DevTools.

➡️ [Project details](01-03-urban-routes-manual-qa/README.md) · [Spreadsheet with every test case and bug report](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit)

## Contents

| Folder | Topic | Tools | Status |
|---|---|---|---|
| [01-03-urban-routes-manual-qa](01-03-urban-routes-manual-qa/) | Manual web QA: test cases, UI checklist and forms | Chrome DevTools, Google Sheets | ✅ Done |
| [04-api-testing](04-api-testing/) | API testing | Postman | ⏳ In progress |
| [06-databases](06-databases/) | Database fundamentals | SQL, command line | ⏳ Pending |
| [07-python-for-test-automation](07-python-for-test-automation/) | Python for test automation | Python, PyCharm | ⏳ Pending |
| [08-test-automation-environment](08-test-automation-environment/) | Test automation environment | Pytest, Git | ⏳ Pending |
| [09-web-test-automation](09-web-test-automation/) | Web app test automation | Selenium WebDriver | ⏳ Pending |
| [10-final-project](10-final-project/) | Final project | To be defined | ⏳ Pending |
| [mobile-app-testing](mobile-app-testing/) | Mobile app testing | Android Studio | ⏳ Pending |
| [templates](templates/) | Test case and bug report templates | Markdown | ✅ |

Sprint 5 is the career preparation course and has no testing project.

## Other projects

- **Suppah Soccah:** test plan for a 3v3 Roblox game I'm developing: functional regression, anti-cheat, telemetry and art QA. In progress.

## Tools

Chrome DevTools · Postman · Jira · Google Sheets · SQL · Python · Pytest · Selenium · Git · Android Studio · Unreal Engine · Unity

## Languages

English ([EF SET certificate](https://cert.efset.org/en/goUbJe)) · Spanish (native)

## Contact

[LinkedIn: David Morales](https://www.linkedin.com/in/3dmcasanova/)
