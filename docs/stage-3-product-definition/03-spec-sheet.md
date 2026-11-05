# Spec Sheet: Incubyte People Platform

*Stage 3 · One-page summary · Draft*

The platform's key facts at a glance. It's a summary of the [PRD](02-prd.md) and adds nothing new. If they ever differ, the PRD wins.

## At a glance

| | |
|---|---|
| **What it is** | Incubyte's internal web platform for everyday people work |
| **Replaces** | The in-house HR tool on Frappe Cloud |
| **Users** | Fewer than 300 employees, all in India |
| **Sign-in** | Incubyte Microsoft account only |
| **Access** | Role-based. People see only what their role needs. |
| **Works on** | Laptop and phone browsers |
| **Hosting** | Data hosted in India |
| **Releases** | 3, starting with Profile, Leaves and Expenses |

## Modules

| Release | Module | Key capability |
|---|---|---|
| 1 | Employee Profile | Self-service personal details. HR maintains the rest. |
| 1 | Leaves | Apply, see balances, see who's away. People Success approves. |
| 1 | Expenses | Claim with receipts, including vehicle expenses. Finance approves and exports. |
| 1 | Reports | Leave Balance, Leave Balance Summary, Vehicle Expenses |
| 2 | Feedback & 360 reviews | Feedback anytime, plus HR-run review cycles |
| 2 | PIP | Confidential plan: employee, manager and HR only |
| 2 | Onboarding & Offboarding | Shared checklists. Leavers lose access straight away. |
| 2 | Letters | Self-service standard letters |
| 3 | Policies & Announcements | Sent to the portal, email and Teams under the publisher's name |
| 3 | Teams & Pods | "My pod" view, with lead and members |
| 3 | Assignments | Kept. Purpose to be confirmed. |
| Later | Wider reports | Dashboards beyond today's reports |

## Connects to

| System | For |
|---|---|
| Microsoft Entra ID | Sign-in |
| Outlook email | Notifications and announcements |
| Microsoft Teams | Posting announcements to the company-wide channel |
| Frappe (one-time) | Moving existing data across |
| Finance's payout process | Receiving approved expense claims |

## Standards

| Area | Target |
|---|---|
| Speed | Common screens load in under 2 seconds |
| Security | Access checked on every request. Independent security review before launch. |
| Audit | Every change and every view of sensitive data recorded |
| Reliability | Daily backups. No lost or duplicated leave or expense records. |

## Not included

Payroll, accounting, recruitment, bench planning, timesheets, mobile apps, Teams apps.

[ ] Reviewed
