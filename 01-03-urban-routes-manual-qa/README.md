**English** | [Español](README.es.md)

# Urban Routes · Manual web QA

Urban Routes is the practice web app of the TripleTen QA Engineer program: the user enters a pickup and a destination, picks a transport mode and a tariff, adds a payment method and orders a taxi.

All the work lives in one spreadsheet: [open the full spreadsheet](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit).

## Results

| Sprint | Executed | Passed | Failed | Bugs |
|---|---|---|---|---|
| 1 · Functional, network and console | 11 test cases | 9 | 2 | 2 |
| 2 · UI checklist | 48 checks (24 × 2 environments) | 36 | 12 | 7 |
| 3 · Card form | 22 test cases | 10 | 12 | 12 |
| **Total** | **81** | **55** | **26** | **21** |

## Sprint 1 · Ordering flow and server responses

I walked the main flow (address, route, Optimal, Flash and Personal modes, phone verification with an SMS code, message to the driver and the order itself) while also checking Network and Console in Chrome DevTools.

[Test cases](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=688606946) · [Bug reports](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=2029093293)

| ID | Bug | Severity |
|---|---|---|
| URB-01 | The Shine tariff returns `503` from `/api/v1/tariffs/price` when the route is calculated | Major |
| URB-02 | The waiting screen does not close with the X button; the console shows `Uncaught Error: Unimplemented method at onClick` | Critical |

## Sprint 2 · UI against the design in two environments

I compared the booking screen with the design on 24 points, in **Chrome 800×600** and **Firefox 1920×1080**. Each environment fails on different points: Chrome clips and overlaps elements at low resolution, and Firefox clips the payment icon.

[Checklist](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=1506116036) · [Bug reports](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=719725667)

| ID | Bug | Environment | Evidence |
|---|---|---|---|
| URBG-01 | The pedestrian icon is missing next to the estimated time | Both | [screenshot](https://drive.google.com/file/d/1sC3-kiLtL3Hkk-tJXhtJX3dQdR7lJom8/view?usp=sharing) |
| URBG-02 | The Camping card does not show the luggage on top of the car | Both | [screenshot](https://drive.google.com/file/d/1Fil3EZ_sBztmgGDuOHJyXVH9IBJpO-4F/view?usp=sharing) |
| URBG-03 | The Deluxe card does not show the stars next to the car | Both | [screenshot](https://drive.google.com/file/d/1CTphS509Q2oZPlbAZFjZh1rTdsZ8aoXt/view?usp=sharing) |
| URBG-04 | The Camping tariff description does not match the design | Both | [screenshot](https://drive.google.com/file/d/1VdpPvO5HcGXjecC_WZN6c3g4oUYH7SwI/view?usp=sharing) |
| URBG-05 | The card icon in the payment method box gets clipped | Firefox 1920×1080 | [screenshot](https://drive.google.com/file/d/1n2Fmjo38SdZKV956tPpwCNHwEvutMhNc/view?usp=sharing) |
| URBG-06 | The Add driver's license and book button gets clipped | Chrome 800×600 | [screenshot](https://drive.google.com/file/d/1FpQxb4UefoQx8psmQZo2MR0HTkVn-QMy/view?usp=sharing) |
| URBG-07 | The clear buttons for From and To overlap the booked car menu | Chrome 800×600 | [screenshot](https://drive.google.com/file/d/18M8Ja1Ipz94MLR7a5-O4FH3ren2qp4py/view?usp=sharing) |

All low priority: they are visual differences that do not block the order.

## Sprint 3 · Card form validation

I designed 20 scenarios with **equivalence partitioning** and **boundary value analysis** for the Card number field (12 digits) and the Code field (2 digits, 01 to 99), and turned them into 22 test cases.

[Validation scenarios](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=1579365076) · [Test cases](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=1057185272) · [Bug reports](https://docs.google.com/spreadsheets/d/1gI-mDF3BTTqg-HQ6AgfDhJoS6o1kyaI-cY1NLv57GcE/edit#gid=328919447)

Boundary values for the Code field:

| Class | Input | Expected | Result |
|---|---|---|---|
| Below range | `00` | Reject | ❌ Accepted |
| Minimum and minimum +1 | `01`, `02` | Accept | ✅ Accepted |
| Maximum −1 and maximum | `98`, `99` | Accept | ✅ Accepted |
| Length 1 and 3 | `1`, `123` | Reject | ❌ Accepted |
| Letters and symbols | `AA`, `!@` | Reject | ❌ Accepted |

The Card number field shows the same pattern: it accepts 5, 11 and 13 digits, letters, symbols and manual spaces, and enables the Add button in every case. I reported 12 bugs (11 high priority) and recommended a more thorough round of testing on the payment flow.

## What this project shows

- Every failed test case has its bug report, with steps to reproduce, expected and actual results, severity or priority, and evidence.
- I check the network and the console, not just the screen.
- I test in more than one environment to separate design defects from resolution or browser defects.
- I design test data with formal techniques instead of testing at random.
