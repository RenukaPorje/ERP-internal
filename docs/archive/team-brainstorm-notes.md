# Internal ERP Platform: Brainstorm Summary

_Date: 2026-11-04 · Team working notes, not part of the leadership pack_

> **Note for readers:** these are the delivery team's early brainstorm notes. Words like "Chosen" and "Rejected" below record the **team's leaning**, not a decision. The build vs buy decision belongs to leadership. See [04-options-build-vs-buy.md](../stage-1-problem-and-vision/04-options-build-vs-buy.md) for the neutral options.

## Problem
Our in-house HR and resourcing tool on Frappe Cloud has usability issues, missing or incorrect features, security problems and a poor UI. We want to replace it with a new architecture and a new design.

## Options Explored
1. **Clean Slate:** build everything custom on a modern stack. Effort: high. Risk: medium. **Chosen.**
2. **Frappe Foundation:** use Frappe HR as the base and add a custom resourcing app. Effort: low. Risk: medium. Rejected because it repeats today's platform and its limits.
3. **Buy HR, Build the Brain:** a SaaS HR system holds the sensitive data, and we build only resourcing. Effort: medium. Risk: low. Not chosen.
4. **Ask-Anything agent:** a Teams/Slack AI layer for questions and reminders to keep data current. Effort: medium. Risk: medium. Deferred.
5. **Strangler Fig:** migrate one module at a time. Effort: medium. Risk: low. Rollout question deferred.
6. **No-UI ERP:** Teams as the main interface. Effort: medium. Risk: high. Not chosen, since the decision is web app only.

## Chosen Direction
A clean-slate **modular monolith**, delivered as **one responsive web app**, with **HR basics first** and resourcing after. Security is a launch requirement, not a later phase.

## Architecture

```
                   ┌───────────────────────────────┐
                   │   Web App (responsive SPA)    │
                   └───────────────┬───────────────┘
                                   │ HTTPS / API
┌──────────────────────────────────▼───────────────────────────────────┐
│                       Modular Monolith (one deployable)              │
│                                                                      │
│  ┌──────────── Platform (cross-cutting) ────────────────────────┐    │
│  │ Identity & Access · Audit Log · Notifications · Files        │    │
│  └──────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  ┌──────────┐  ┌──────────────┐  ┌──────────────┐  ┌────────────┐    │
│  │  People  │  │Leave&Holidays│  │ On/Offboard  │  │ Resourcing │    │
│  │  (core)  │◄─┤              │  │              │  │  (phase 2) │    │
│  └────▲─────┘  └──────────────┘  └──────────────┘  └────────────┘    │
│       └──────── all modules depend on People via its public API ─────┘
│                                                                      │
│  ┌──────────── Integration ─────────────────────────────────────┐    │
│  │ Frappe Importer (one-time migration) · Exports (payroll etc.)│    │
│  └──────────────────────────────────────────────────────────────┘    │
└──────────────────────────────────┬───────────────────────────────────┘
                                   │
                 ┌─────────────────▼─────────────────┐
                 │ PostgreSQL: one schema per module │
                 │ (no cross-schema joins)           │
                 └───────────────────────────────────┘
```

### Architecture rules
- **Each module has a public API.** Modules call each other only through that API, never through another module's tables. Each module owns its database schema.
- **People is the core.** It owns the employee identity. Other modules refer to employees by ID only.
- **Side effects go through in-process domain events** (`EmployeeJoined`, `LeaveApproved`, `EmployeeExited`). For example, onboarding starts on `EmployeeJoined` and notifications react to `LeaveApproved`. Add a broker only when a real need appears.
- **Security is built in:** SSO with Microsoft Entra ID (the M365 identity service), role- and scope-based permissions, field-level protection for sensitive data, and an append-only audit log of every write and every read of sensitive data.
- **Ready to split later:** strict boundaries mean any module can become its own service when there is a concrete reason, such as different scaling needs or a separate team.

## High-Level Modules

| Module | Responsibility | Phase |
|---|---|---|
| **Identity & Access** | Entra SSO, roles (Employee, Manager, HR, Admin), scoped permissions (own record, reports, everyone), session security | v1 |
| **Audit Log** | Append-only record of who changed or viewed what and when, with sensitive-field reads included | v1 |
| **People** | Employee master record, org structure and reporting lines, designations, locations, employment history, documents, emergency contacts; sensitive fields (salary, IDs, bank details) isolated and encrypted | v1 |
| **Leave & Holidays** | Leave types and policies, accrual and balance engine, request and approval workflow, holiday calendars per location, team leave calendar | v1 |
| **Onboarding / Offboarding** | Checklist templates, tasks assigned to HR/IT/manager, asset handover, document collection, exit workflow | v1 |
| **Notifications** | Email (M365) and in-app notifications, triggered by domain events | v1 |
| **Integration** | One-time Frappe REST importer, exports for payroll and accounting inputs | v1 |
| **Reporting** | Headcount, attrition, leave usage; read-only queries over module APIs or read models | v1.5 |
| **Resourcing** | Skills catalogue, project allocations, bench view, roll-off forecast, staffing search | v2 |

## Key Insights
- **This is a people platform (HR system plus resource planner), not a classic ERP.** Finance (general ledger, tax, invoicing) stays out of scope, which removes the hardest, most regulated layer.
- **Security and sensitive HR data are tightly linked.** Starting with HR means personal data from day one, so access control, field-level protection and audit logging belong in the first release.
- **Frappe's generic form-over-table interface is likely the source of the UX pain.** The new design should be built around workflows (request leave, approve, onboard someone) rather than screens per database table.
- **Migration is tractable:** Frappe exposes every DocType through REST, so data can be extracted cleanly.

## Open Questions
- Rollout strategy: strangler cutover vs. parallel run vs. security shell first (deferred by choice).
- Tech stack (language, framework, hosting) has not been chosen.
- Does payroll stay external? What format do the payroll and accounting exports need?
- Data residency and compliance requirements, such as India's DPDP Act, for employee personal data.
- Should the Teams/Slack "ask-anything" resourcing agent come later, on top of Resourcing v2?
