# Module Map and High-Level Requirements

*Stage 3 · For leadership review · Draft*

The modules of the new people platform, what each one does, and when it ships. The [PRD](02-prd.md) adds roles, quality standards, risks and open questions.

## Module map

```
┌───────────────────────────────────────────────────────────────────┐
│                 INCUBYTE PEOPLE PLATFORM (web app)                │
├───────────────────────────────────────────────────────────────────┤
│  RELEASE 1: CORE                                                  │
│    Employee Profile    Leaves    Expenses    Reports in use today │
├───────────────────────────────────────────────────────────────────┤
│  RELEASE 2: PERFORMANCE AND LIFECYCLE                             │
│    Feedback & 360    PIP    Onboarding & Offboarding    Letters   │
├───────────────────────────────────────────────────────────────────┤
│  RELEASE 3: ORGANISATION AND COMMUNICATION                        │
│    Policies & Announcements    Teams & Pods    Assignments        │
├───────────────────────────────────────────────────────────────────┤
│  PLATFORM (from Release 1, shared by every module)                │
│    Microsoft sign-in · Role-based access · Audit trail ·          │
│    Notifications (portal, email, Teams) · Data move from Frappe   │
└───────────────────────────────────────────────────────────────────┘
```

## Modules

| Module | Release | What it does |
|---|---|---|
| **Employee Profile** | 1 | One record per person. Employees update their own details, and HR maintains the rest. |
| **Leaves** | 1 | Apply, see balances, and see who's away. People Success approves. |
| **Expenses** | 1 | Claim with receipts (including vehicle expenses). Finance approves and sends for payout. |
| **Reports in use today** | 1 | Leave Balance, Leave Balance Summary and Vehicle Expenses |
| **Feedback & 360 reviews** | 2 | Feedback anytime, plus HR-run 360 review cycles |
| **PIP** | 2 | A confidential, structured improvement plan |
| **Onboarding & Offboarding** | 2 | Shared checklists for joiners and leavers |
| **Letters** | 2 | Self-service standard letters |
| **Policies & Announcements** | 3 | One home for policies and news, sent to the portal, email and Teams under the publisher's name |
| **Teams & Pods** *(working name)* | 3 | Each person's pod, its lead and members |
| **Assignments** | 3 | Kept. Purpose to be confirmed in Stage 2 interviews. |
| **Wider reports and dashboards** | Later | Beyond the reports used today |

## Platform requirements

- **Sign-in** with the Incubyte Microsoft account only.
- **Role-based access:** people see only what their role needs. Private details, expenses and performance records stay with the people involved and HR.
- **Audit trail** of every change and every view of sensitive data.
- **Notifications** by email and in the portal. Announcements also go to Teams.
- **Data move** from Frappe, checked before switching over.

## Not included

Payroll, accounting, recruitment, bench planning, timesheets, and mobile or Teams apps.

[ ] Reviewed
