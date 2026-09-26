# Andrés Cuello

**I build complete applications — domain model, API and interface as one system.**

C# and .NET on the backend. I care most about where the business rules live and how a
system behaves when someone tries to break them, so each project ships with its rules
and failure cases documented next to the code that enforces them.

| | |
|---|---|
| **Backend** | C# · .NET 8 · ASP.NET Core Minimal APIs · System.Text.Json with custom converters · built-in DI container · ProblemDetails middleware |
| **Architecture** | Domain-centric layering · domain-driven modelling · separation of concerns · composition root · unit-of-work write boundaries · REST with RFC 7807 |
| **Frontend** | React · Javascript · CSS — responsive interfaces |
| **Quality** | Reqnroll (BDD) · NUnit · Playwright · RestSharp · Page Object Model |
| **Delivery** | Docker multi-stage builds · GitHub Actions |

## How the projects are structured

**The dependency rule holds.** Entities, invariants and validation live in `Domain/`,
which has no reference to infrastructure or to the web layer, dependencies point
inward only. Field-level validation maps straight onto RFC 7807 Problem Details.

**One write boundary per application.** `MutateAsync` takes a delegate, applies it to
freshly loaded state under a single writer, then commits atomically.

**Composition at the edge.** Services are registered in one composition root and
injected into handlers. The HTTP layer routes, serialises and returns status codes; it
holds no business logic.

## Projects

**[CarIs RD](https://github.com/AndresCuello001/CarIsRD)** — Vehicle history for the
Dominican used-car market. Source-attributed timelines, protected by rules that reject
odometer rollbacks, future-dated events and duplicate plates or VINs.

**[StockPilot](https://github.com/AndresCuello001/StockPilot)** — Inventory and sales
for small retail. Every stock change is a recorded movement, so the current quantity is
always explainable. Multi-line sales validate every line before touching inventory,
then commit as one unit.

**[TurnoFlow](https://github.com/AndresCuello001/TurnoFlow)** — Appointment scheduling.
End times derive from service duration and overlapping bookings for the same staff
member are rejected. Cancelled slots free themselves, because availability is computed
rather than stored.

**[QualityFlow · CarIs RD](https://github.com/AndresCuello001/QualityFlow-CarIsRD)** —
BDD regression suite for CarIs RD. Spanish Gherkin on Reqnroll and NUnit, Playwright
for the browser, RestSharp for the API. The application under test ships inside the
repository, so one script gives a full run.

---

<sub>Each repository documents its architecture, API surface and setup.
`REQUIREMENTS.md` records what is in scope, what is deliberately out of it, and where
the product would go next.</sub>
