# Product Requirements (PRD)

*Stage 3 · For leadership review · Draft*

## Summary

Incubyte's people tool, built on Frappe Cloud, is hard to use, slow, sometimes wrong, missing features, and shows people more than their role should. We will replace it with **one secure, simple web platform for everyday people work**, built around Microsoft 365. It ships in three releases, starting with the modules everyone uses: Profile, Leaves and Expenses.

Background and goals: [Stage 1](../stage-1-problem-and-vision/README.md). How work happens today: [Stage 2](../stage-2-discovery/README.md). Module overview: [module map](01-module-map-and-requirements.md).

## Who uses it

| Role | What they do |
|---|---|
| **Employee** | Keeps their profile up to date, applies for leave, claims expenses, gives and receives feedback |
| **People Success Team (PST)** | Approves leave, runs onboarding and offboarding |
| **HR** | Owns employee records, leave rules, review cycles, PIPs, policies and letters |
| **Manager** | Reviews their team, runs PIPs with HR, sees who's away |
| **Pod Lead** | Sees their pod's members and availability |
| **Finance** | Approves expense claims and sends them for payout |
| **TA / Buddy** | Complete their onboarding tasks |
| **Admin** | Manages roles and settings. Never sees personal data unless also HR. |

## Release 1: Core

### Employee Profile
- Employees see their profile and update their own personal and contact details.
- HR maintains the rest: role, department, location, manager, joining date.
- Colleagues see only a basic directory. Personal and sensitive details are visible only to the employee and HR.

### Leaves
- Employees apply for leave (full or half day) and always see their balance per leave type.
- **People Success approves or rejects**, as today. PST also handles long or emergency leave for employees on probation.
- Employees can cancel a request. Everyone involved is notified.
- HR sets up leave types and their rules. The leave year runs January to December.
- Holidays: fixed, optional and floating. A team view shows who's away, without leave reasons.

### Expenses
- Employees submit a claim with receipts, including vehicle expenses, and track its status.
- Finance approves, rejects or sends it back, then sends approved claims for payout outside the platform.
- Only the claimant and Finance see a claim.

### Reports in use today
- Leave Balance, Leave Balance Summary and Vehicle Expenses, available only to the roles that need them.

### Platform
- **Sign-in** with the Incubyte Microsoft account only.
- **Role-based access:** each person sees only what their role needs, and menus show only what they can use.
- **Audit trail:** every change, and every view of sensitive data, is recorded.
- **Notifications** by email and in the portal.
- **Data move** from Frappe (see below).

## Release 2: Performance and lifecycle

### Feedback and 360 reviews
- Anyone can give a colleague feedback at any time. Prompts can always be dismissed.
- HR runs 360 review cycles: self, peer and manager reviews, with deadlines and reminders.
- Feedback and reviews are seen only by the people involved and HR.

### PIP
- HR and the manager set a plan with goals, check-ins and an outcome. The employee acknowledges it.
- Only the employee, their manager and HR can see a PIP. It never appears anywhere else.

### Onboarding and Offboarding
- Shared checklists for joiners and leavers, with tasks for PST, TA, HR, the manager and the buddy.
- Leavers lose access straight away.

### Letters
- Employees request standard letters (such as employment verification) and download them. Some need HR approval first.

## Release 3: Organisation and communication

### Policies and Announcements
- One home for policies and announcements. Employees acknowledge key policies.
- Each item goes out at once to the portal, by email, and to the company-wide Teams channel **under the publisher's own name**, with a record of when and where it was sent.

### Teams & Pods *(working name)*
- Each person's pod, its lead and members, with a clear "my pod" view.

### Assignments
- Kept as a module. **Its purpose is confirmed in the Stage 2 interviews** before it's designed.

## Later
- Wider reports and dashboards.

## Quality standards

| Area | Standard |
|---|---|
| **Ease of use** | Common tasks take a few clear steps, on laptop or phone browser |
| **Speed** | Common screens load in under 2 seconds |
| **Security** | Access checked on every request. Sensitive data protected. Independent security review before launch. |
| **Privacy** | See the [access and privacy principles](../stage-2-discovery/04-keep-fix-add-drop.md#access-and-privacy-principles). Data hosted in India. |
| **Reliability** | Daily backups. No lost or duplicated leave or expense records. |

## Data to move

People records, leave balances and all leave history, past expense claims with receipts, and pods with their members. Holidays are set up fresh. Details: [Stage 2 data plan](../stage-2-discovery/03-data-to-move.md).

## Out of scope

Payroll, accounting, recruitment, bench planning, timesheets, and mobile or Teams apps (posting to a Teams channel *is* included).

## Main risks

| Risk | How we reduce it |
|---|---|
| Leave rules turn out more complex than expected | Confirm the rules with PST and HR before design |
| Frappe data is messy | Trial move early, with checks signed off before switching |
| Someone sees data beyond their role | Access checked on every request, tested for every role, reviewed before launch |
| The new tool isn't clearly easier | Test each task with real employees before launch |

## Open questions

1. Should managers take part in leave approval, or only be informed?
2. Is Finance the only approver for expenses?
3. What is Assignments used for?
4. Is the pod lead the same person as the manager? What should "Teams & Pods" be called?
5. Should feedback be named or anonymous?
6. Can PIPs start without HR approval?

[ ] Reviewed
