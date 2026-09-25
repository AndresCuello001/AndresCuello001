# Andrés Cuello

**I build complete applications — domain model, API and interface as one system.**

Most of my work starts with the same question: *where do the business rules live, and
what happens when someone tries to break them?* The projects below are working
products built around that question — each one ships with its requirements, its
business rules and its failure cases written down next to the code that enforces
them.

---

## What I build with

**Backend** · C# · .NET 8 · ASP.NET Core Minimal APIs · System.Text.Json with custom converters
**Frontend** · JavaScript · HTML · CSS — responsive interfaces built without a framework
**Quality** · Reqnroll (BDD) · NUnit · Playwright · RestSharp · Page Object Model
**Delivery** · Docker multi-stage builds · GitHub Actions

---

## How I approach a system

**Rules belong in the domain, not in the controller.** Field-level validation lives on
the request models and maps straight onto RFC 7807 Problem Details. Rules that need to
see existing state — a duplicate SKU, an odometer rollback, a double-booked staff
member — raise domain exceptions inside the write transaction and surface as 409.
Malformed input and state conflicts stay distinguishable to whoever is calling the API.

**A write either happens completely or not at all.** Each project serialises access
through a single writer, applies the change to freshly-loaded state, then swaps the
data file into place atomically. A rejected operation leaves storage untouched — a
five-line sale with one short line writes nothing.

**Invariants are computed, not remembered.** Staff availability, reorder alerts and
inventory value are derived from current state rather than stored and kept in sync, so
there is nothing to drift.

**Acceptance criteria should execute.** Requirements written for a product owner and
the tests that verify them are the same artefact where it matters most — Gherkin
scenarios in the language the product is actually spoken in.

---

## Projects

### [CarIs RD](https://github.com/AndresCuello001/CarIsRD) · Verifiable vehicle history

A vehicle history platform for the Dominican used-car market. Each vehicle carries a
source-attributed timeline of maintenance, inspections, claims and ownership changes,
protected by rules that reject the edits which would make a history untrustworthy —
odometer rollbacks, future-dated events, duplicate plates and VINs.

`ASP.NET Core 8` · `Minimal APIs` · `Domain validation` · `Atomic JSON persistence` · `Docker`

### [StockPilot](https://github.com/AndresCuello001/StockPilot) · Inventory and sales for small retail

Stock control for shops that currently run on a notebook. Every stock change is a
recorded movement, so the current quantity is always explainable by the history that
produced it. Multi-line sales validate every line before touching inventory, then
commit as one unit.

`ASP.NET Core 8` · `Transactional writes` · `Movement ledger` · `Low-stock alerts` · `Docker`

### [TurnoFlow](https://github.com/AndresCuello001/TurnoFlow) · Appointment scheduling

Booking and daily operations for salons, barbershops and clinics. Appointment end times
are derived from service duration, and overlapping intervals for the same staff member
are rejected outright. Cancelled bookings release their slot automatically, because
availability is computed rather than stored.

`ASP.NET Core 8` · `Interval conflict detection` · `Custom JsonConverter` · `Docker`

### [QualityFlow · CarIs RD](https://github.com/AndresCuello001/QualityFlow-CarIsRD) · BDD regression framework

An executable acceptance-test layer for CarIs RD. Spanish Gherkin scenarios on Reqnroll
and NUnit, driving the browser with Playwright and the API with RestSharp. Step
definitions carry no selectors, UI and API suites are separated by tag, and failures
capture a full-page screenshot attached to the test result. The application under test
ships inside the repository, so a clone and one script gives a full regression run.

`Reqnroll` · `NUnit` · `Playwright` · `RestSharp` · `Page Object Model` · `GitHub Actions`

---

<sub>Each repository documents its own architecture, API surface, business rules and
setup. `REQUIREMENTS.md` in each project records what is in scope, what is deliberately
out of it, and where the product would go next.</sub>
