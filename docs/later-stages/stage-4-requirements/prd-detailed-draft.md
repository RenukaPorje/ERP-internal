# Detailed PRD Draft (Stage 4 reference)

> **Stage 4 reference only, not part of the leadership review.** This is the earlier detailed draft (21 slices with acceptance criteria). It predates the Stage 1–2 decisions: phased releases, People Success approving leave, and moving past expenses and pods. It will be refreshed in Stage 4 from the [Stage 3 PRD](../../stage-3-product-definition/02-prd.md).


_Date: 2026-11-04 · Status: Draft for review, updated with stakeholder answers and the expanded core scope (expenses, feedback, PIP, assignments, pods) · Risk: HIGH (sign-in, personal data, performance data, data migration)_
_Next milestone: architecture and design ready **next week**._

> **How to read this document.** Sections 1–6 explain why we are building this and what success looks like. Sections 7–9 describe what gets built, in order, with checklists that define "done". Sections 10–16 cover quality standards, structure, migration, risks, decisions and open questions.
>
> Anything marked **[Assumption]** is a reasonable default we picked because the discussions didn't settle it. Each one is repeated in Open Questions (section 15). Please confirm or correct them before the related work starts.

---

## 1. Summary

Incubyte runs its HR work on an in-house tool built on Frappe Cloud. People find it hard to use, some features are missing or wrong, it has security problems, and the interface is poor. We will **replace it with a new, purpose-built web app** that we design from scratch.

The platform covers the **core of how Incubyte runs its people**: in the stakeholders' words, "profile, leaves, expenses, feedback, PIP, assignments, team/pod info". The first release includes secure sign-in with Microsoft accounts, employee profiles, leave and holidays, onboarding and offboarding, expense claims, peer feedback and review cycles, performance improvement plans (PIPs), client/project assignments, and pods. Our existing HR data moves over from Frappe. HR reporting comes in **v1.5**.

Some things stay in other systems. Payroll calculation, accounting (general ledger, tax, invoicing) and full resourcing (bench, roll-off, staffing search) are out of scope. Approved expense claims are **exported** to the separate finance/payroll system, which pays them out.

### Modules at a glance

| Module | What it does | Phase |
|---|---|---|
| **Identity & Access** | Microsoft sign-in, roles, who-can-see-whom | v1 |
| **Audit Log** | Tamper-proof record of every change and every view of sensitive or confidential data | v1 |
| **People** (core) | Employee records, org chart, employment history, documents, protected sensitive fields | v1 |
| **Leave & Holidays** | Leave types and policies, balances, requests and approvals, holiday calendars | v1 |
| **Onboarding / Offboarding** | Joiner and exit checklists with tasks for People Success, TA, HR and managers/buddies | v1 |
| **Teams (Pods)** | Pods, their members and pod leads | v1 |
| **Assignments** | Client and project reference list, and who is assigned to which project, when, and at what allocation | v1 |
| **Expenses** | Expense claims with receipts, Finance approval, export of approved claims to the finance/payroll system | v1 |
| **Feedback & Reviews** | Anytime peer feedback, plus HR-run 360 review cycles (self, manager and peer reviews) | v1 |
| **PIP** | Strictly confidential performance improvement plans with goals, check-ins and outcomes | v1 |
| **Notifications** | Email (Microsoft 365) and in-app alerts | v1 |
| **Frappe Importer** | One-time, re-runnable migration with reconciliation | v1 |
| **Reporting** | Headcount, attrition, leave usage, onboarding progress, plus summary views for the new modules | v1.5 |

## 2. Why (Background)

- **The current tool is failing on four fronts:** usability, missing or incorrect features, security issues, and a poor interface.
- **We're replacing it, not repairing it.** The brainstorm looked at six options: building on Frappe HR, buying a SaaS HR system, a chat-first tool and others. We chose a **clean-slate rebuild**. Building on Frappe would carry over the same limits. Frappe's generic "one form per database table" screens are the likely cause of much of today's UX pain.
- **This is the core people platform, not a full ERP.** It covers what every Incubyte employee touches day to day: their profile, leave, expenses, feedback, assignments and pod. Payroll calculation and accounting stay in separate systems, which removes the most heavily regulated and complex layers.
- **Security can't wait.** We hold salary, ID and bank details from day one, and now also performance data (reviews and PIPs) that is just as sensitive to the people involved. Access control, field-level protection and audit logging must be in the first release.

## 3. Problem Statement

Incubyte's employees, managers, HR and Finance rely on an internal tool that is clunky, unreliable and not secure enough for the personal data it holds. Everyday tasks like requesting leave, claiming an expense, giving feedback or onboarding a new joiner take more effort than they should, and some (reviews, PIPs, pod and project information) live in scattered places. Sensitive data such as salaries, ID numbers, bank details and performance plans isn't protected or tracked well enough. We need one secure, workflow-first platform for core people work that we can trust.

## 4. Goals and Non-Goals

### Goals
1. **Trustworthy security from day one.** Everyone signs in with their Incubyte Microsoft account. People see only what their role allows. Every change, and every view of sensitive or confidential data, is recorded.
2. **Workflow-first experience.** Common tasks (request leave, approve leave, submit an expense, give feedback, complete a review, onboard or offboard someone) each take a few clear steps, on desktop or phone browser.
3. **Correct people data and rules.** Leave balances, holidays, employee records, pods and assignments are accurate, and employees no longer need HR to explain them.
4. **Confidential performance processes.** Reviews and PIPs are visible only to the people who must see them, and every view is traceable.
5. **Clean hand-off to finance.** Approved expense claims reach the finance/payroll system in a format it can use, with no re-typing.
6. **Clean exit from Frappe.** All needed data moves across and is checked, and the old tool can be switched off.
7. **Room to grow.** Firm module boundaries let us add HR reporting (v1.5) and future modules without a rewrite.

### Non-Goals
- Not a general-purpose ERP. No general ledger, tax, invoicing, billing, or client rates and margins.
- No payroll. **(Confirmed)** No payroll calculation and no payroll export. The only outbound data to finance is the approved-expense export.
- No expense payout tracking. The platform stops at "approved and exported"; finance pays out in its own system.
- No full resourcing: no bench or roll-off views, staffing search, skills matching, utilisation or billing. Assignments are a record of who works where, not a staffing tool.
- No link between review results and compensation in this scope.
- Not a product for sale. It's for Incubyte's internal use only.
- No Microsoft Teams app, Slack app or native mobile app. Phone access is through the responsive web app.
- No AI "ask-anything" assistant.
- Rollout strategy isn't decided in this document (see Open Questions).

## 5. Who (Users, Personas and Roles)

### Personas

| Persona | Who they are | What they need most |
|---|---|---|
| **Employee** | Every Incubyte team member | See and update their own profile, request leave, see balances and holidays, submit expenses, give and receive feedback, complete reviews, see their pod and assignments, complete onboarding tasks |
| **Manager** | Anyone with direct reports (the reporting manager) | Approve or reject leave, see their team's leave calendar, write and finalise reviews for direct reports, run PIPs with HR, own onboarding/offboarding tasks for their reports |
| **Pod Lead** | The person leading a pod. May or may not be the reporting manager **[Assumption: separate role]** | See pod members' work profiles, assignments and leave calendar, give feedback |
| **HR** | People-operations team | Maintain employee records, configure leave policies and holidays, run onboarding and exits, run review cycles, run PIPs, maintain pods, clients, projects and assignments, view sensitive data when needed |
| **Finance** | Finance team | Configure expense categories and limits, approve or reject expense claims, export approved claims to the finance/payroll system |
| **People Success** | People Success team | Own onboarding and offboarding checklists and their tasks, track joiner and exit progress |
| **TA (Talent Acquisition)** | Recruiters handing over a hire | Complete pre-joining onboarding tasks, such as offer details and joining date handover |
| **Buddy** | Colleague assigned to a new joiner | Complete buddy tasks (team intro, first-week support) |
| **Admin** | Platform owner(s) | Manage roles and access, system settings, run the Frappe import, review the audit log |
| **IT** *(to confirm)* | IT team | Complete access and asset tasks (laptop, account setup/removal) if they own them |

### Roles and scopes

Access combines a **role** (what you can do) with a **scope** (whose data you can do it to).

| Role | Default scope | Sensitive fields (salary, ID numbers, bank details) |
|---|---|---|
| Employee | Own record | Own record only, masked by default, full value on request (audited) |
| Manager | Own record plus direct and indirect reports | **Never visible** for reports **(Confirmed)** |
| Pod Lead | Own record plus current pod members (work profile, assignments, leave calendar) **[Assumption]** | **Never visible** |
| HR | Everyone | Visible on request (audited) |
| Finance | Expense claims of everyone, plus directory-level details of claimants | **Never visible** unless also given the HR role |
| People Success / TA / Buddy | Own record, plus the checklists and tasks assigned to them | **Never visible** |
| Admin | Everyone, for settings and roles | **Never visible** unless also given the HR role **(Confirmed)**. This keeps system admin separate from HR data access. |

One person can hold several roles. A manager is also an employee, and a pod lead may also be a manager, for example.

### Who sees performance and expense data

| Data | Who can see it | Who never sees it |
|---|---|---|
| **Peer feedback** | Recipient, giver and HR. The recipient's manager only if the recipient shares a specific item. **[Assumption]** | Pod lead, skip-level managers, Admin, Finance, other colleagues |
| **Review cycle content** (self, peer and manager reviews) | The employee (self-review and finalised manager review), each peer reviewer (only their own peer review), the direct manager, HR **[Assumption]** | Pod lead, skip-level managers, Admin, Finance, other colleagues |
| **PIP** | The employee, their current direct manager, HR **(Confirmed)** | Pod lead, skip-level managers, Admin, Finance, peers. Its existence is not shown anywhere else. |
| **Expense claims and receipts** | The claimant and Finance **[Assumption: managers and HR don't see claims unless also Finance]** | Everyone else |
| **Pod and assignments** | The employee, their manager line, their pod lead, HR. Pod name is shown in the directory. **[Assumption]** | Directory users don't see client or project names |

### Scale
- **(Confirmed)** Fewer than 300 employees, all in India. Performance targets in section 10 are sized for up to 1,000 employees to leave room for growth.
- **(Confirmed)** Everyone who needs access, including contractors and interns, has an Incubyte Microsoft 365 account.

## 6. Success Criteria and Metrics

### Outcomes
- Employees and managers do their everyday people tasks themselves, without asking HR or Finance for help.
- HR trusts the data. Leave balances, employee records, pods and assignments match the agreed policy and source documents.
- Finance receives approved expenses ready to pay, with no re-typing and no duplicate payouts.
- Review cycles run on time inside the platform, without spreadsheets or email chains.
- Confidential performance data (reviews, PIPs) never reaches someone who shouldn't see it.
- Security findings against the old tool are closed, with no new high-severity findings at launch.
- Frappe is switched off for HR.

### Proposed metrics (targets to confirm)

| Metric | Target | How we measure |
|---|---|---|
| Sign-in through Microsoft SSO | 100% of users. No local passwords exist. | Identity settings plus audit log |
| Time to submit a leave request | Under 1 minute, median | Product analytics, usability test |
| Leave balance accuracy after migration | 100% match with the HR-signed-off reconciliation sheet | Migration reconciliation report |
| Leave-related questions to HR | 50% reduction within 3 months of launch | HR ticket/email count before and after (baseline needed) |
| Employee satisfaction with the tool | +30 points versus a baseline survey of the old tool | Short in-app survey (baseline needed) |
| Sensitive-data and PIP views with an audit entry | 100% | Automated audit checks |
| High or critical security findings at launch | 0 open | Independent security review / penetration test |
| Onboarding checklist completed by day 1 | 90% of new joiners | Onboarding module data |
| Time from expense submission to Finance decision | Under 5 working days, median **[Assumption]** | Expenses module data |
| Approved claims rejected by the finance/payroll system on import | 0 | Finance feedback per export |
| Review cycle completion by the final deadline | 90% of participants finalised and acknowledged **[Assumption]** | Review cycle dashboard |
| Confidentiality incidents for feedback, reviews or PIPs | 0 | Audit log review, incident reports |

## 7. Hypotheses

Each hypothesis brings a capability into scope or rules it out. Each must be confirmed or rejected before or during the related slice.

- **H1 — Updated:** Payroll calculation and accounting are separate systems and fully out of scope. The only outbound finance data is the approved-expense export (H9). No payroll export.
- **H2 — Updated:** The reporting manager approves leave. **Some leave types also need HR approval** (a second step). HR configures which types. *Which types need HR is an open question.*
- **H3:** Leave policy can be described with a small set of settings per leave type: yearly entitlement, how it builds up, carry-forward cap, whether half-days are allowed, and whether the balance can go below zero. *How leave accrues is still to be confirmed with HR. If the rules don't fit these settings, we need a more flexible rules engine.*
- **H4:** Holiday calendars differ by **location**, and each employee has exactly one location that decides their holidays.
- **H5 — Confirmed:** Everyone has an Incubyte Microsoft 365 account. SSO is the only sign-in method.
- **H6:** Frappe data is clean enough to import automatically, with only a short list of records needing manual fixes. *If rejected, add a data-cleanup step before cutover.*
- **H7 — Confirmed:** Only the employee and HR see sensitive data. Managers, Admins, pod leads and Finance never do.
- **H8 — Confirmed:** Data is hosted in an India region, and retention follows legal requirements (exact periods to be confirmed with legal).
- **H9 [Assumption]:** Expense claims need **Finance approval only**, with no manager step. *If rejected, add a manager approval step before Finance, reusing the leave approval pattern.*
- **H10 [Assumption]:** Peer feedback is named (not anonymous) and visible only to the recipient, the giver and HR. The manager sees an item only if the recipient shares it. *If anonymity is needed, the giver's name must be hidden from the recipient but kept for HR.*
- **H11 [Assumption]:** Pod lead is a separate role from reporting manager. A pod lead sees pod members' work profiles, assignments and leave calendar, and can give feedback, but never sees reviews, PIPs or sensitive data, and doesn't approve leave. *If rejected (pod lead = manager), the pod-lead scope collapses into the manager scope.*
- **H12 [Assumption]:** Each employee belongs to at most one pod at a time.
- **H13 — Confirmed:** A PIP is strictly confidential to HR, the employee's direct manager and the employee. Every view is audited, and it is handled like sensitive data.
- **H14 [Assumption]:** There is no historical expense, feedback, review or PIP data in Frappe that must be migrated. These modules start empty. Pods, clients, projects and current assignments are entered by HR or loaded from a spreadsheet at launch.

## 8. Scope by Phase (Milestone Map)

Each slice below works end to end and could be released on its own. Slices are ordered so that each one builds on the ones before it. Slice 1 is the **walking skeleton**: the thinnest complete path through the system.

Slices 1–12 are the HR foundation. Slices 13–21 add the newly confirmed core modules. They depend on Slices 1–4 (sign-in, profiles, records, sensitive-data protection) and Slice 12 (audit viewer, notifications), but not on each other except where stated.

---

### v1 — Part A: HR foundation

#### Slice 1: Sign in and see my own profile (walking skeleton)
*Covers: Identity & Access, People, Audit Log, a minimal Frappe import. Proves the full path from Microsoft sign-in to an audited view of real employee data.*

- [ ] User can sign in with their Incubyte Microsoft account (Entra ID single sign-on)
- [ ] After sign-in, the user lands on their own profile with name, work email, designation, department, location, reporting manager and joining date
- [ ] Profile data for active employees is loaded from Frappe by the importer (basic fields only in this slice)
- [ ] Shows a "you don't have access yet, contact HR" page when a valid Microsoft account has no matching employee record
- [ ] Sign-in is refused for Microsoft accounts outside the Incubyte organisation
- [ ] Sign-in is refused for an employee whose record is marked as exited
- [ ] User is signed out after 30 minutes of inactivity **[Assumption]**, and must sign in again to continue
- [ ] User can sign out, after which pressing the browser back button doesn't show protected pages
- [ ] Every successful sign-in, failed sign-in and sign-out is recorded in the audit log with user, time and outcome
- [ ] An Employee who changes the web address to another employee's profile sees "not found", and the attempt is recorded in the audit log
- [ ] There is no username/password sign-in option anywhere in the app

#### Slice 2: Find colleagues (directory and org chart)
*Covers: People, Identity & Access (scopes).*

- [ ] Any signed-in user can search the directory by name and see work email, designation, department, location and manager
- [ ] The directory never shows personal contact details, salary, ID numbers or bank details, for any role
- [ ] User can open an org chart showing an employee's manager and direct reports
- [ ] Search returns "no results" with a clear message when nothing matches
- [ ] Exited employees don't appear in the directory
- [ ] A Manager can open the full profile of anyone in their reporting line, direct or indirect
- [ ] A Manager who tries to open the full profile of someone outside their reporting line is refused, and the attempt is audited

#### Slice 3: HR maintains employee records
*Covers: People, Audit Log.*

- [ ] HR can create a new employee record with the required fields: name, work email, joining date, designation, department, location, reporting manager, employment type
- [ ] Shows an error when a required field is missing, and names the field
- [ ] Shows an error when the work email already belongs to another employee
- [ ] Shows an error when the reporting manager chosen would create a loop (A reports to B, B reports to A)
- [ ] HR can change designation, department, location or manager with an effective date, and the previous value is kept in employment history
- [ ] Employee can update their own personal contact details and emergency contacts, but can't change job details
- [ ] Every change is recorded in the audit log with who, when, which field, old value and new value
- [ ] When two HR users edit the same record at the same time, the second save is blocked with "this record was changed by someone else, please reload", so no change is silently lost
- [ ] Employee or Manager attempts to use HR-only actions are refused and audited
- [ ] Creating an employee record raises an "employee joined" event that later slices react to (onboarding, notifications)

#### Slice 4: Protect sensitive data (salary, ID numbers, bank details)
*Covers: People (sensitive fields), Identity & Access, Audit Log.*

- [ ] HR can record salary, government ID numbers (such as PAN and Aadhaar) and bank details on an employee record
- [ ] Sensitive values are masked on screen by default (for example, only the last 4 digits shown)
- [ ] HR can reveal a full sensitive value with a deliberate "show" action, and each reveal is recorded in the audit log with who, whose record, which field and when
- [ ] Employee can reveal their own sensitive values, and each reveal is audited
- [ ] Managers never see sensitive fields for their reports, either masked or unmasked
- [ ] Admin doesn't see sensitive fields unless also given the HR role
- [ ] Sensitive values never appear in notifications, emails, exports or error messages
- [ ] Sensitive values are stored encrypted, so a database copy alone doesn't expose them
- [ ] Shows an error when an ID or bank number doesn't match the expected format (for example, wrong length for an IFSC code)
- [ ] A request for a sensitive field by a user without permission returns nothing for that field and is audited as a denied access

#### Slice 5: Holiday calendars by location
*Covers: Leave & Holidays.*

- [ ] HR can create a holiday calendar for a location and calendar year (Jan–Dec), with dated named holidays
- [ ] HR can mark each holiday as **fixed** (everyone off) or **optional**
- [ ] HR can set how many optional holidays each employee may take in the year
- [ ] Employee can choose optional holidays from their location's list, up to the limit
- [ ] Shows an error when an employee tries to take more optional holidays than the limit
- [ ] HR can give each employee a set number of **floating holidays** that they may take on any working day
- [ ] Employee sees the holiday list for their own location for the current and next year
- [ ] Shows an error when HR adds two holidays on the same date in the same calendar
- [ ] Changing an employee's location switches the holidays they see from the effective date
- [ ] Employee, Manager and Admin can't edit holiday calendars

#### Slice 6: Request and approve leave
*Covers: Leave & Holidays, Notifications (email via Microsoft 365, in-app).*

- [ ] Employee can request leave by choosing leave type, start date, end date and an optional reason
- [ ] The request shows how many working days it uses, skipping weekends and the employee's location holidays
- [ ] Employee can request a half-day where the leave type allows it **[Assumption, H3]**
- [ ] Shows an error when the request overlaps an existing pending or approved leave
- [ ] Shows an error when the end date is before the start date
- [ ] Shows an error when the request is more than the available balance and the leave type doesn't allow negative balance
- [ ] The reporting manager gets an email and an in-app notification for the new request
- [ ] Manager can approve or reject a request, with an optional comment (comment required for rejection **[Assumption]**)
- [ ] For leave types that HR has marked as "needs HR approval", the request goes to HR after the manager approves, and is approved only once HR approves
- [ ] Employee sees which approval step their request is waiting on
- [ ] Employee gets an email and in-app notification when their request is approved or rejected
- [ ] Employee can cancel a pending request, or an approved one whose start date hasn't passed
- [ ] When the manager is absent or has left, HR can approve or reject on their behalf, and the audit log records that HR acted
- [ ] Employee can't approve their own leave, even if they are their own manager in the data
- [ ] If two approvers act on the same request at the same moment, only the first decision counts and the second sees "already decided"
- [ ] If the email service is unavailable, the in-app notification still appears and the email is retried later

#### Slice 7: Leave balances, accrual and carry-forward
*Covers: Leave & Holidays.*

- [ ] HR can set up a leave type with name, yearly entitlement, accrual frequency (monthly or yearly), carry-forward cap, half-day allowed (yes/no), negative balance allowed (yes/no) and needs HR approval (yes/no)
- [ ] v1 leave types: Earned/Privilege, Casual, Sick, Birthday leave, Bereavement, Unpaid (loss of pay), plus "other" types HR can add. Fixed, optional and floating holidays are handled in Slice 5.
- [ ] Employee can take Birthday leave only within the window HR sets around their birthday **[Assumption: same month]**
- [ ] Employee sees, for each leave type, entitled, accrued, used, pending and available days
- [ ] The leave year runs January to December
- [ ] Accrual adds the right amount on the configured schedule **[Open question: accrual rules per leave type]**
- [ ] New joiners accrue pro-rata from their joining date
- [ ] At year end, unused balance up to the carry-forward cap moves to the new year, and the rest lapses
- [ ] HR can make a manual balance adjustment, with a required reason, and it is audited
- [ ] Approving leave reduces the available balance once, even if the approval is submitted twice
- [ ] Cancelling approved leave returns the days to the balance
- [ ] If the year-end or monthly balance run stops halfway, re-running it gives the same result as running it once, with no double credit
- [ ] Employee can see a dated history of every change to their balance and its reason

#### Slice 8: Team leave calendar
*Covers: Leave & Holidays.*

- [ ] Manager sees a calendar of approved and pending leave for everyone in their reporting line
- [ ] Employee sees approved leave for their direct teammates (same manager), but not the leave type or reason **[Assumption]**
- [ ] HR sees a calendar for any department or location
- [ ] The calendar marks holidays for each person's location
- [ ] The calendar shows no leave from people outside the viewer's scope

#### Slice 9: Onboarding a new joiner
*Covers: Onboarding/Offboarding, Notifications, People events.*

- [ ] People Success can create onboarding checklist templates with tasks, each assigned to a role (People Success, TA, HR, Manager, Buddy, the new joiner, and IT if confirmed) and due a set number of days before or after joining
- [ ] TA sees pre-joining tasks for their hires before the joining date
- [ ] People Success can assign a buddy to a new joiner, and the buddy gets their tasks
- [ ] Creating a new employee record starts an onboarding checklist from the template chosen by HR
- [ ] Each task owner gets a notification and sees their tasks in a "My tasks" list
- [ ] The new joiner can upload requested documents (for example, ID proof, education certificates) in their onboarding tasks
- [ ] Uploaded documents are visible only to the joiner and HR
- [ ] Shows an error when an uploaded file is too large or of an unsupported type
- [ ] HR sees each joiner's onboarding progress with overdue tasks highlighted
- [ ] A task owner gets a reminder when a task is overdue
- [ ] Asset handover tasks record which asset (for example, a laptop serial number) was given to the employee
- [ ] If the joining date changes, task due dates move with it

#### Slice 10: Offboarding (exit workflow)
*Covers: Onboarding/Offboarding, People, Identity & Access.*

- [ ] HR can start an exit for an employee with resignation date, last working day and exit reason
- [ ] Starting an exit creates an offboarding checklist (for example, asset return, knowledge handover, access removal), owned by People Success
- [ ] Access removal for Microsoft 365 and other systems is a tracked task, not automatic **[Assumption, to confirm: recommended option]**
- [ ] Assets recorded at onboarding appear on the offboarding checklist for return
- [ ] Pending leave requests after the last working day are cancelled automatically, and the employee is notified
- [ ] After the last working day, the employee can no longer sign in to the platform
- [ ] Direct reports of the exiting employee are flagged to HR for reassignment before the exit is completed
- [ ] HR can't complete an exit while mandatory checklist tasks are still open
- [ ] The exited employee's record and history are kept (not deleted) and are visible only to HR

#### Slice 11: Full Frappe migration with reconciliation
*Covers: Integration (Frappe importer). Expands on the basic import in Slice 1.*

- [ ] Admin can run the importer to bring across employees (active and exited), org structure, employment history, sensitive fields, leave types, leave balances, leave history and holiday calendars from Frappe
- [ ] The importer produces a reconciliation report showing, per data type, records found in Frappe, imported, skipped and failed, with reasons
- [ ] Records that fail checks (for example, a missing work email or an unknown manager) are listed for HR to fix, and don't block the rest
- [ ] Running the importer again updates existing records instead of creating duplicates
- [ ] If an import stops halfway, it can be re-run safely with no partial or duplicate data
- [ ] Imported leave balances are shown next to Frappe's values for HR to sign off, and differences are highlighted
- [ ] Every imported record is marked with its source and import time in the audit log
- [ ] Sensitive fields are encrypted as they are imported and never written to logs or the reconciliation report in plain form
- [ ] Only Admin can run the importer

#### Slice 12: Audit log viewer and in-app notification centre
*Covers: Audit Log, Notifications.*

- [ ] Admin can search the audit log by person acted on, person acting, action type and date range
- [ ] Admin can filter the audit log to show only sensitive-data views
- [ ] No one, including Admin, can edit or delete audit log entries
- [ ] Viewing the audit log is itself audited
- [ ] User sees an in-app notification bell with unread count, and can mark notifications as read
- [ ] Clicking a notification takes the user to the related item (for example, the leave request)

---

### v1 — Part B: Core people modules (pods, assignments, expenses, feedback, PIP)

#### Slice 13: Pods and pod leads
*Covers: Teams, Identity & Access (pod-lead scope), Leave (calendar view), Audit Log. Risk: MODERATE.*

- [ ] HR can create a pod with a name, a pod lead and one or more members
- [ ] Shows an error when the pod name is already used by another pod
- [ ] Shows an error when the chosen pod lead is not an active employee
- [ ] Shows an error, naming the current pod, when HR adds an employee who is already in another pod **[Assumption: one pod per person, H12]**
- [ ] HR can move a member to another pod, change the pod lead, or close a pod, and each change is audited
- [ ] Employee sees their pod name and pod lead on their profile
- [ ] Any signed-in user sees a colleague's pod name in the directory
- [ ] Pod lead sees each current pod member's work profile (directory details plus current assignments)
- [ ] Pod lead sees approved and pending leave for pod members on the team calendar, without leave type or reason **[Assumption]**
- [ ] Pod lead never sees pod members' sensitive fields, personal contact details, peer feedback, reviews or PIPs
- [ ] Pod lead who opens the profile of someone outside their pod (and outside any other scope they hold) sees "not found", and the attempt is audited
- [ ] When a member leaves the pod, the pod lead loses access to that person's work profile and leave from that moment
- [ ] Exited employees are removed from their pod automatically after their last working day
- [ ] Only HR can create or change pods **[Assumption]**. Attempts by others are refused and audited.

#### Slice 14: Clients, projects and assignments
*Covers: Assignments, People, Audit Log. Risk: MODERATE.*

- [ ] HR can create a client record with a name
- [ ] HR can create a project under a client with a name, start date, optional end date and status (active or closed)
- [ ] Shows an error when a project name already exists under the same client
- [ ] Client and project records have no fields for rates, billing or margins
- [ ] HR can assign an employee to an active project with a start date, an optional end date and an allocation percentage
- [ ] Shows an error when the assignment end date is before its start date
- [ ] Shows an error when the allocation is not between 1% and 100%
- [ ] Shows an error when the employee's total allocation on any day would go above 100% **[Assumption]**
- [ ] Shows an error when assigning someone to a closed project, or for dates outside the project's dates
- [ ] Employee sees their current and past assignments (client, project, dates, allocation) on their profile
- [ ] Manager sees assignments for their reporting line, pod lead for their pod members, and HR for everyone
- [ ] A user who requests assignments for someone outside their scope gets nothing back, and the attempt is audited
- [ ] When an employee exits, their open assignments end automatically on their last working day
- [ ] Every assignment change is recorded in the audit log with who, when, old value and new value

#### Slice 15: Submit an expense claim
*Covers: Expenses, Files, Notifications, Identity & Access (Finance role). Risk: MODERATE.*

- [ ] Finance can set up expense categories (for example, travel, meals, internet), each with an optional limit per item in INR **[Assumption: limits per item]**
- [ ] Employee can create a claim with one or more items, each with a date, category, amount in INR, description and at least one receipt
- [ ] All amounts are in INR only; there is no currency choice **(Confirmed)**
- [ ] Employee can save a claim as a draft and submit it later
- [ ] Shows an error when an item has no receipt attached
- [ ] Shows an error when an amount is zero or negative
- [ ] Shows an error when an expense date is in the future
- [ ] Shows an error when a receipt file is larger than 10 MB or is not a PDF, JPG or PNG **[Assumption]**
- [ ] Shows an error when the exact same receipt file is already attached to another claim
- [ ] An item over its category limit can be submitted only with a written justification, and is flagged "over limit" for Finance **[Assumption]**
- [ ] An item with the same date, amount and category as another of the employee's claims is flagged "possible duplicate" for Finance
- [ ] Employee sees each claim's status: Draft, Submitted, Approved, Rejected or Exported
- [ ] Employee can withdraw a submitted claim until Finance decides on it
- [ ] Finance gets an in-app and email notification for each submitted claim, without receipts attached
- [ ] Receipts and claims are visible only to the claimant and Finance. Anyone else gets "not found", and the attempt is audited.

#### Slice 16: Approve and export expense claims
*Covers: Expenses, Notifications, Audit Log. Risk: HIGH (money leaving the company).*

- [ ] Finance sees a queue of submitted claims, with "over limit" and "possible duplicate" flags shown
- [ ] Finance can approve a claim **[Assumption: Finance-only approval, H9]**
- [ ] Finance can reject a claim, and a reason is required
- [ ] Employee gets an in-app and email notification when their claim is approved or rejected
- [ ] A Finance member can't approve or reject their own claim. Another Finance member must decide it.
- [ ] If two Finance members act on the same claim at the same moment, only the first decision counts, and the second sees "already decided"
- [ ] Approved, rejected and exported claims can't be edited by anyone
- [ ] Finance can export all approved claims that haven't been exported yet as one file **[Assumption: CSV; format to confirm with the finance/payroll system]**
- [ ] Each export row includes employee ID, employee name, claim reference, item date, category, amount and approval date
- [ ] The export never includes bank details or any other sensitive field
- [ ] Exported claims are marked Exported with the export date, and are not included in the next export
- [ ] If an export fails partway, no claim is marked Exported, and Finance can run it again
- [ ] Finance can download a previous export file again, and each download is audited
- [ ] Finance can't see salary, ID numbers, bank details, feedback, reviews or PIPs unless they also hold the HR role
- [ ] Every approval, rejection and export is recorded in the audit log

#### Slice 17: Peer feedback (anytime)
*Covers: Feedback, Notifications, Audit Log. Risk: HIGH (confidentiality).*

- [ ] Any employee can write feedback for any active colleague at any time
- [ ] Shows an error when the feedback text is empty
- [ ] Shows an error when an employee tries to give feedback to themselves
- [ ] Exited employees can't be chosen as recipients
- [ ] Feedback shows the giver's name to the recipient **[Assumption: not anonymous in v1, H10]**
- [ ] The recipient gets an in-app and email notification that doesn't include the feedback text
- [ ] The recipient sees all feedback they have received, newest first
- [ ] The giver sees all feedback they have given
- [ ] Feedback can't be edited or deleted after it is sent **[Assumption]**
- [ ] HR can see feedback for any employee, and each HR view is audited
- [ ] The recipient's manager doesn't see feedback unless the recipient shares a specific item with them **[Assumption, H10]**
- [ ] The recipient can share a specific item with their manager, and can stop sharing it later
- [ ] Pod leads, skip-level managers, Admin, Finance and other colleagues never see someone else's feedback. A direct link returns "not found", and the attempt is audited.
- [ ] The recipient can report an item as inappropriate, which alerts HR
- [ ] HR can hide a reported item with a required reason. The item is then no longer shown to the recipient or the giver, and the action is audited.

#### Slice 18: Review cycles: set-up and peer nominations
*Covers: Feedback (review cycles), Notifications, Onboarding "My tasks" list, Audit Log. Risk: HIGH.*

- [ ] HR can create a review cycle with a name, the review period, the participants (everyone, or chosen departments or locations) and deadlines for nominations, self-reviews, peer reviews and manager reviews
- [ ] Shows an error when the deadlines are not in order (nominations, then self and peer reviews, then manager reviews) or are in the past
- [ ] HR can set the questions for self, peer and manager reviews as free-text questions **[Assumption: ratings wait for a decision on the rating scale]**
- [ ] Launching a cycle notifies each participant and their manager
- [ ] Employees who join after launch are not included unless HR adds them
- [ ] A participant can nominate between 2 and 5 peers to review them **[Assumption]**
- [ ] Shows an error when a participant nominates themselves, their own manager, an exited employee, or more than the maximum
- [ ] The participant's manager can add or remove nominees until the nomination deadline **[Assumption]**
- [ ] Each confirmed peer reviewer gets a review task in "My tasks" and a notification
- [ ] A peer reviewer can decline with a reason, and the participant and manager are notified so they can nominate someone else
- [ ] People with pending review tasks get reminders 3 days and 1 day before each deadline **[Assumption]**
- [ ] HR sees a progress view for each participant, with overdue items highlighted
- [ ] HR can extend a deadline, and everyone affected is notified
- [ ] A participant who exits during the cycle is removed from it, and their pending review tasks are cancelled
- [ ] Only HR can create, change or launch review cycles. Attempts by others are refused and audited.

#### Slice 19: Review cycles: writing, finalising and acknowledging reviews
*Covers: Feedback (review cycles), Notifications, Audit Log. Risk: HIGH (confidentiality).*

- [ ] A participant can write their self-review, save it as a draft, and submit it
- [ ] A peer reviewer can write their peer review, save it as a draft, and submit it
- [ ] Submitted self and peer reviews can't be edited unless HR reopens them, and each reopen is audited
- [ ] The direct manager sees the participant's self-review and all submitted peer reviews
- [ ] The direct manager can write a manager review and finalise it once the self-review is submitted or its deadline has passed
- [ ] The finalised review shows which self or peer reviews were missing
- [ ] The participant sees their finalised manager review alongside their self-review
- [ ] The participant doesn't see individual peer reviews **[Assumption, pending the anonymity decision]**
- [ ] A peer reviewer sees only the review they wrote, never other peers' reviews or the manager review
- [ ] The participant can acknowledge the finalised review with an optional comment. Acknowledging means "I have seen it", not "I agree".
- [ ] A finalised review is locked. HR can reopen it with a required reason, and the reopen is audited.
- [ ] If the participant's manager changes during the cycle, the pending manager review moves to the new manager, and the previous manager loses access **[Assumption]**
- [ ] Pod leads, skip-level managers, Admin, Finance and other colleagues can't see any review content. A direct link returns "not found", and the attempt is audited.
- [ ] Every view of review content by HR or the manager is recorded in the audit log
- [ ] Notifications about reviews never include review text

#### Slice 20: Create a PIP and keep it confidential
*Covers: PIP, Identity & Access, Notifications, Audit Log. Risk: HIGH (strictly confidential, treated as sensitive data).*

- [ ] HR or an employee's direct manager can draft a PIP with goals, success criteria for each goal, start date, end date and check-in schedule
- [ ] Shows an error when a PIP has no goals, or a goal has no success criteria
- [ ] Shows an error when the end date is before the start date
- [ ] Shows an error when the employee already has an active PIP
- [ ] A manager can draft a PIP only for their own direct reports
- [ ] A drafted PIP becomes active and visible to the employee only after HR approves it **[Assumption]**
- [ ] On activation, the employee gets a notification that says only "you have an item to review", with no PIP details
- [ ] The employee can acknowledge the PIP with an optional comment. Acknowledging means "I have received it", not "I agree".
- [ ] HR sees whether and when the employee acknowledged, and the employee gets a reminder if they haven't acknowledged within 3 working days **[Assumption]**
- [ ] Only the employee, their current direct manager and HR can open a PIP
- [ ] Pod leads, skip-level managers, Admin, Finance and colleagues who try to open a PIP see "not found", and the attempt is audited as a denied access
- [ ] A PIP's existence is never shown on the profile, directory, org chart, team calendar, pod view, search results or reports
- [ ] Every view of a PIP is recorded in the audit log with who, whose PIP and when
- [ ] PIP-related audit entries are visible only to users with the HR role, not to Admin alone **[Assumption]**
- [ ] PIP content is stored encrypted and never appears in emails, logs, error messages or exports
- [ ] When the employee's manager changes, the new manager gains access, and the previous manager loses access immediately **[Assumption]**

#### Slice 21: PIP check-ins and outcome
*Covers: PIP, Notifications, Audit Log. Risk: HIGH. Builds on Slice 20.*

- [ ] The manager or HR can record a check-in with a date, progress for each goal and notes
- [ ] The employee can see each check-in and add a comment to it **[Assumption]**
- [ ] The manager gets a reminder 2 days before each scheduled check-in **[Assumption]**, and HR sees missed check-ins highlighted
- [ ] The manager can recommend an outcome: Successful, Extended or Unsuccessful
- [ ] HR records the final outcome with a summary **[Assumption: HR confirms, manager recommends]**
- [ ] Choosing "Extended" requires a new end date that is later than the current one
- [ ] The employee is notified that an outcome has been recorded, without the outcome appearing in the notification
- [ ] The employee can see the outcome and acknowledge it with an optional comment
- [ ] Once an outcome is recorded (other than Extended), the PIP is closed and becomes read-only
- [ ] If two people record an outcome at the same moment, only the first counts, and the second sees "already decided"
- [ ] If the employee exits during an active PIP, it is closed as "closed: employee exited" **[Assumption]**
- [ ] Closed PIPs stay visible only to the employee, their current direct manager and HR, and every view is audited

---

### v1.5 — Reporting (rougher, to refine later)

- Headcount by department, location, pod and employment type, at any date
- Joiners and leavers by month, and attrition rate
- Leave usage by type, team and month, plus outstanding balances (helps with year-end liability)
- Onboarding completion and overdue-task reports
- Expense totals by category and month, and time to decision (for Finance only)
- Review cycle completion rates (counts only, no review content)
- Assignment allocation by client and project (a snapshot, not a staffing tool)
- PIPs are **never** included in reports
- Report access follows the same role and scope rules (managers see only their reporting line, sensitive fields excluded)
- CSV export of reports, audited

## 9. Out of Scope

- Finance and accounting: general ledger, tax, invoicing, billing, client rates and margins **(Confirmed: separate systems)**
- Expense payout and reimbursement tracking. Approved claims are exported; the finance/payroll system pays them **(Confirmed)**
- Multi-currency expenses (INR only), corporate cards, mileage calculators and advance requests
- Payroll: calculation, payslips and payroll exports **(Confirmed: separate system)**
- Full resourcing: bench and roll-off views, staffing search, skills matching, utilisation and billing **(Confirmed)**. Assignments are a record only.
- Recruitment / applicant tracking (hiring starts in this platform at "employee record created")
- Linking review results to compensation, promotions or bonuses
- Rating scales and calibration in review cycles (pending decision, see Open Questions)
- Standalone goal-setting or OKRs (goals exist only inside a PIP)
- Learning management
- Timesheets
- Microsoft Teams, Slack or native mobile apps
- AI assistant / chat interface
- Username/password sign-in or social sign-in (Google and others)
- Approval chains beyond manager + HR for leave (for example, pod-lead or project-lead approval)
- Comp-off (time off in lieu) and sandwich-rule leave calculations **[to confirm]**
- Two-way sync back to Frappe (depends on the rollout decision)
- Self-service role management by non-Admins

## 10. Non-Functional Requirements

### Security
- Single sign-on through Microsoft Entra ID only. No local passwords. Multi-factor sign-in is enforced through our Microsoft settings.
- Access is checked on the server for every request (role + scope), not just hidden in the interface.
- Sensitive fields (salary, ID numbers, bank details) **and PIP content** are encrypted at rest. Sensitive fields are masked by default and revealed only on purpose.
- Confidential performance data (peer feedback, review content, PIPs) never appears in notifications, emails, logs, error messages or exports.
- A denied request for confidential data returns "not found", so the request itself doesn't reveal whether the data exists.
- All traffic is encrypted in transit.
- Sessions expire after inactivity **[Assumption: 30 minutes]** and are fully ended on sign-out or exit.
- Separation of duties: system administration, HR data access and Finance approval are separate roles. Nobody approves their own expense claim.
- File uploads (documents and receipts) are checked for type and size, and stored privately (no public links).
- An independent security review or penetration test is done before launch. No high or critical findings may be open at launch.
- Common web attack protections (for example, the OWASP Top 10 list) are part of the definition of done for every slice.

### Privacy and compliance
- All employees are in India, so India's Digital Personal Data Protection (DPDP) Act 2023 and its Rules apply. Obligations include a clear purpose for each data item, security safeguards, breach notification, and retention limits. *Legal to confirm the detailed obligations, including for performance data and receipts.*
- Collect only the personal data the platform actually needs (data minimisation). Each sensitive field must have a documented purpose.
- Data is hosted in an **India region** **(Confirmed)**.
- Retention of exited employees' data, expense records, reviews and PIPs follows legal requirements **(Confirmed approach; exact periods to be confirmed with legal)** and is enforced automatically.
- Personal data is never used in non-production environments unless masked.

### Audit
- Append-only audit log of every create, update and delete, every sign-in attempt, every permission denial, every sensitive-field view, every PIP view, every view of review content or feedback by HR or managers, every expense decision, every export and every import.
- Each entry records who, what, whose record, when, and old/new values (sensitive and confidential values are never stored in plain form in the log).
- Nobody can change or delete audit entries. Retention follows legal requirements **[Assumption: at least 7 years until legal confirms]**.

### Performance and reliability
- Pages load in under 2 seconds for 95% of requests on a normal office connection, at up to 1,000 employees.
- Directory search returns results in under 1 second.
- Available during business hours across our locations, with a monthly availability target of 99.5% **[Assumption]**.
- Daily backups, with a tested restore. At most 24 hours of data loss in a disaster **[Assumption]**.
- Scheduled jobs (accrual, year-end, reminders, review and PIP reminders, expense export) are safe to re-run.

### Accessibility and usability
- Meets WCAG 2.2 level AA, the international web accessibility standard (keyboard use, screen readers, colour contrast).
- Responsive: every v1 workflow can be completed on a phone browser, including photographing and uploading a receipt.
- Works in current versions of Edge, Chrome, Safari and Firefox.
- Workflow-first design: each common task is reachable in two clicks from the home screen.
- Usability-test every major workflow with real employees, managers, HR and Finance before launch.

## 11. High-Level Architecture and Modules

*(Settled in the brainstorm and the architecture document. Technology choices such as language, framework and hosting come later.)*

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
│  │  People  │  │Leave&Holidays│  │ On/Offboard  │  │ Reporting  │    │
│  │  (core)  │◄─┤              │  │ (lifecycle)  │  │   (v1.5)   │    │
│  └────▲─────┘  └──────────────┘  └──────────────┘  └────────────┘    │
│       │                                                              │
│       │  ┌────────┐ ┌─────────────┐ ┌──────────┐ ┌──────────┐ ┌────┐ │
│       │  │ Teams  │ │ Assignments │ │ Expenses │ │ Feedback │ │PIP │ │
│       │  │ (pods) │ │(clients,    │ │          │ │& Reviews │ │    │ │
│       │  │        │ │ projects)   │ │          │ │          │ │    │ │
│       │  └────────┘ └─────────────┘ └──────────┘ └──────────┘ └────┘ │
│       └──────── all modules depend on People via its public API ─────┘
│                                                                      │
│  ┌──────────── Integration ─────────────────────────────────────┐    │
│  │ Frappe Importer (one-time, re-runnable migration)            │    │
│  └──────────────────────────────────────────────────────────────┘    │
└──────────────────────────────────┬───────────────────────────────────┘
                                   │
                 ┌─────────────────▼─────────────────┐
                 │ PostgreSQL: one schema per module │
                 │ (no cross-schema joins)           │
                 └───────────────────────────────────┘
```

**In plain terms:** one application, split into separate modules with firm boundaries. Each module keeps its own data and talks to the others only through agreed entry points. "People" is the centre, and every other module refers to employees by ID. When something happens ("employee joined", "leave approved", "employee exited", "pod lead changed"), the module that owns it announces the event and other modules react. Onboarding starts when an employee joins, assignments end when an employee exits, and notifications go out when leave or an expense claim is decided. These boundaries mean any module can become a separate service later if there is a real reason to split it.

The new modules follow the same rules as the existing ones. They depend on People (and Identity for access checks) and never the other way round. The pod-lead scope used by Identity is kept up to date from Teams events, in the same way the manager scope is kept up to date from People events, so Identity never has to call Teams directly.

### Module Structure

| Module | Owns | Depends on | Phase |
|---|---|---|---|
| `identity/` | Users, roles (including Finance and the derived Pod Lead role), scopes, sessions, Entra SSO link | People (to resolve the signed-in employee); listens to Teams events for pod-lead scope | v1 |
| `audit/` | Audit entries (append-only), with PIP-related entries restricted to HR | (none) | v1 |
| `people/` | Employee, org structure, reporting lines, designations, departments, locations, employment history, documents, emergency contacts, sensitive fields (encrypted) | Audit | v1 |
| `leave/` | Leave types, policies, balances, balance ledger, requests, approvals, holiday calendars | People, Audit | v1 |
| `lifecycle/` | Checklist templates, onboarding/offboarding checklists, tasks, asset handovers | People, Audit | v1 |
| `teams/` | Pods, pod membership (dated), pod leads | People, Audit | v1 |
| `assignments/` | Client and project reference records (no billing or rates), assignments with dates and allocation % | People, Audit | v1 |
| `expenses/` | Expense categories and limits, claims, claim items, receipt references, Finance decisions, export batches | People, Files, Audit | v1 |
| `feedback/` | Peer feedback, sharing with manager, moderation; review cycles, question sets, nominations, self/peer/manager reviews, acknowledgements | People, Audit | v1 |
| `pip/` | Performance improvement plans, goals and success criteria, check-ins, outcomes, acknowledgements (encrypted, sensitive class) | People, Audit | v1 |
| `notifications/` | Notification messages, delivery status, in-app inbox | People (recipient lookup) | v1 |
| `files/` | Stored documents and receipts, and access links | Identity | v1 |
| `integration/` | Frappe import runs and reconciliation | People, Leave, Audit | v1 |
| `reporting/` | Read models for headcount, attrition, leave usage, onboarding, expense and review-completion summaries (never PIPs) | People, Leave, Lifecycle, Teams, Assignments, Expenses, Feedback (read-only) | v1.5 |

**Key events:** `EmployeeJoined`, `EmployeeUpdated`, `ManagerChanged`, `EmployeeExited`, `LeaveRequested`, `LeaveApproved`, `LeaveRejected`, `LeaveCancelled`, `TaskAssigned`, `TaskOverdue`, `PodMembershipChanged`, `PodLeadChanged`, `AssignmentChanged`, `ExpenseClaimSubmitted`, `ExpenseClaimApproved`, `ExpenseClaimRejected`, `ExpensesExported`, `FeedbackGiven`, `ReviewCycleLaunched`, `ReviewTaskAssigned`, `ReviewFinalised`, `PipActivated`, `PipCheckInDue`, `PipClosed`. Event payloads carry IDs only, never feedback text, review content, PIP details or receipt contents.

*The architecture document (`docs/later-stages/stage-6-technical-design/architecture.md`) has been updated to cover all v1 modules, the Finance and Pod Lead roles, the "confidential performance" data class and the new events.*

## 12. Migration from Frappe

- **Source:** Frappe Cloud REST API, which exposes all record types (DocTypes).
- **What moves:** employees (active and exited), org structure and reporting lines, designations/departments/locations, employment history, sensitive fields, documents, leave types, leave balances and history, and holiday calendars.
- **New modules start empty [Assumption, H14]:** no historical expense claims, peer feedback, reviews or PIPs are migrated. Pods, clients, projects and current assignments are set up by HR at launch, or loaded from a spreadsheet if HR prefers. *To confirm: does Frappe or any spreadsheet hold project, pod or past review data that must be kept?*
- **How:** a re-runnable importer (Slice 1 for basic fields, Slice 11 for everything). It produces a reconciliation report and an exceptions list for HR to fix.
- **Sign-off:** HR signs off leave balances and headcount against Frappe before the new platform becomes the source of truth.
- **Safety:** sensitive data is encrypted on import and never written in plain form to logs or reports. Imports are audited.
- **Cutover:** depends on the rollout strategy (deferred, see Open Questions). The brainstorm options were:
  - A) **HR core + strangler cutover.** The new app becomes the source of truth for people and leave, with a one-way sync to Frappe for any parts still in use there.
  - B) **Parallel run, then switch.**
  - C) **Security shell first.** Sign-in, roles and audit, plus a read-only directory imported from Frappe.
  - The brainstorm leaned towards A, with C as its first step. Slice 1 is built to fit any of the three.

## 13. Risks

| Risk | Impact | Likelihood | Mitigation |
|---|---|---|---|
| Leave policy rules are more complex than expected (sandwich rules, comp-off, location-specific rules) | Balances wrong, trust lost | Medium | Confirm policies with HR before Slice 7. Test balances against real employee examples. |
| Frappe data is messy or inconsistent | Migration delays, wrong balances | Medium–High | Early trial import in Slice 1, exceptions list, HR sign-off before cutover |
| Sensitive data exposed through a gap in access checks | Legal, trust and reputational harm | Low–Medium (high impact) | Server-side checks on every request, security test cases in every slice, penetration test before launch |
| A PIP, review or piece of feedback is seen by someone who shouldn't see it (for example, a pod lead, skip-level manager or Admin) | Serious harm to the employee and to trust in the platform | Low–Medium (very high impact) | Deny by default, "not found" on denial, a permission test for every role against every confidential item, every view audited, no content in notifications, PIP audit entries HR-only |
| Pod lead and reporting manager roles are confused, giving pod leads too much or too little access | Confidentiality gaps or blocked work | Medium | Confirm H11 before Slice 13. Keep pod-lead scope narrow until confirmed. |
| Wrong or duplicate expense payouts | Money lost, Finance distrust | Low–Medium | Duplicate-receipt detection, "possible duplicate" flags, no self-approval, each claim exported once, export failures leave nothing half-marked |
| Export file doesn't match what the finance/payroll system expects | Manual re-keying, delays | Medium | Agree the export format with Finance before Slice 16. Test with a sample file. |
| Scope grows from "assignments" into full resourcing (bench, staffing search) | Delays to v1 | Medium | Resourcing explicitly out of scope; assignments are a record only |
| Review cycles are more complex than assumed (ratings, calibration, anonymity) | Rework in Slices 18–19 | Medium | Free-text questions in v1. Settle rating scale and anonymity before Slice 18. |
| Clean-slate rebuild takes longer than expected, now with more modules | Old insecure tool runs longer | Medium–High | Vertical slices, walking skeleton first. Ship the HR foundation (Part A) before Part B if needed. Consider shipping Slices 1–2 early as a security shell. |
| DPDP compliance requirements are found late, including for performance data | Rework, possible launch delay | Medium | Early legal review. Build in data minimisation, retention and audit from the start. |
| Running old and new systems side by side causes data drift | Conflicting records | Medium (depends on rollout) | Decide rollout strategy before Slice 11. Name one source of truth per data type. |
| Low adoption if the new UX isn't clearly better | Goals missed | Low–Medium | Usability tests with real users per workflow. Measure against a baseline survey. |
| Email delivery through Microsoft 365 is throttled or misconfigured | Missed approvals and reminders | Low | In-app notifications as backup, retries, delivery status tracking |

## 14. Hypotheses Summary and Decisions Log

**Settled (from brainstorm):**
- Clean-slate rebuild, not Frappe-based and not a SaaS HR system
- Modular monolith, PostgreSQL with one schema per module, modules talk through public APIs and in-process events
- One responsive web app only. No Teams or mobile apps.
- Microsoft 365 organisation. Entra ID single sign-on.
- Security (roles + scopes, field-level protection, audit including sensitive reads) is a launch requirement

**Settled (stakeholder answers, 2026-11-04, first round):**
- Core HR in v1 (Identity & Access, Audit, People, Leave & Holidays, On/Offboarding, Notifications, Frappe Importer), with Reporting in v1.5
- No payroll calculation and no payroll export (H1)
- Fewer than 300 employees, all in India
- Everyone who needs access has a Microsoft 365 account; SSO only (H5 confirmed)
- Sensitive data visible only to the employee and HR; never to managers or Admins (H7 confirmed)
- Leave year is January–December
- Leave types: Earned/Privilege, Casual, Sick, Birthday, Bereavement, Unpaid, other; holidays are fixed, optional and floating
- Leave approval: manager approves; some leave types also need HR approval (H2 updated)
- Onboarding/offboarding owners: People Success team, TA (during onboarding), HR, Manager/Buddy
- Hosting in an India region; retention per legal requirements (H8 confirmed)
- Timeline: architecture and design needed **next week**

**Settled (stakeholder answers, 2026-11-04, scope expansion):**
- v1 scope is now the **"ERP core": profile, leaves, expenses, feedback, PIP, assignments, team/pod info**, plus onboarding/offboarding. Reporting stays in v1.5.
- **This supersedes three earlier decisions on these specific points:**
  - *"Performance out"* is replaced: peer feedback, 360 review cycles and PIPs are in v1. Compensation links, rating scales and standalone goals stay out.
  - *"Finance out"* is narrowed: expense claims and approval are in v1, with approved claims exported to the finance/payroll system. Payroll calculation, payout tracking, general ledger, tax and invoicing stay out.
  - *"Resourcing out"* is narrowed: client/project reference records, assignments (dates and allocation %) and pods are in v1. Bench, roll-off, staffing search, skills and billing stay out.
- Expenses: employee submits a claim with receipts, it is approved, and approved claims are exported for payout elsewhere. INR only. No payout tracking. A new Finance role handles claims and never sees HR sensitive data.
- Feedback: anytime peer feedback, plus HR-run 360 review cycles (self, manager and nominated peers, with HR creating the cycle, nominations, deadlines and reminders, manager finalising, and employee acknowledging).
- PIP: HR and the manager create the plan (goals, success criteria, duration, check-ins); the employee sees and acknowledges it; outcome is Successful, Extended or Unsuccessful; strictly confidential to HR, the manager and the employee; every view audited; handled like sensitive data (H13 confirmed).
- Assignments: client/project reference records with no billing or rates, and assignments of people with start/end dates and allocation %.
- Pods: pods have members and a pod lead.

**Still assumptions:** H3, H4, H6, H9, H10, H11, H12, H14, and every remaining item marked **[Assumption]**.

## 15. Open Questions

**Rollout and timeline**
1. Rollout strategy: strangler cutover, parallel run, or security shell first? *(Deliberately deferred.)*
2. Beyond the architecture and design next week, is there a target go-live date (for example, 1 January for clean leave-year balances)? Should Part B (Slices 13–21) launch with Part A or shortly after?
3. Is there any non-HR data in Frappe (for example, projects or pods) that still needs a home once Frappe is switched off?

**Leave policy**
4. Entitlement per leave type?
5. How does each leave type accrue (monthly, yearly up front, or mixed)? Probation rules, and pro-rata rules for joiners and leavers?
6. Carry-forward caps, lapse rules and leave encashment?
7. Half-days, negative balances, and sandwich rules (weekends/holidays between leave days counted as leave)?
8. Which leave types need HR approval in addition to the manager?
9. Birthday leave: what window around the birthday can it be taken in?
10. How many optional and floating holidays per employee per year?
11. Are maternity/paternity leave and comp-off needed? (Not selected, but maternity leave is statutory in India.)
12. What can teammates see in the team calendar: just "away", or the leave type too?

**Onboarding / offboarding**
13. Exact tasks for People Success, TA, HR and Manager/Buddy in each checklist?
14. Does IT own laptop and account tasks, and should they be on the checklist?
15. Should offboarding remove Microsoft 365 access automatically, or create a tracked task? *(Leaning towards a tracked task; to confirm.)*
16. Which documents must be collected from joiners, and are any legally required?

**Compliance and data**
17. Detailed DPDP obligations and deadlines (legal to confirm), including for performance data and receipts?
18. Exact retention periods for exited employees' data, documents, expense records and receipts, reviews, PIPs and audit logs?
19. Are there client contracts requiring background-verification records or specific data handling (including client names shown in assignments)?

**Success measurement**
20. Can we capture baselines now (HR ticket volume, satisfaction survey of the old tool, list of known security findings, current expense turnaround) so we can measure improvement?

**Expenses**
21. Is Finance approval enough, or must the employee's manager approve first? *(Assumed Finance-only, H9. Stakeholder: "Not sure, I think Finance only".)*
22. What file format and columns does the finance/payroll system need for the export? *(Assumed CSV with employee ID, name, claim reference, item date, category, amount and approval date.)*
23. Which expense categories are needed, and what are the policy limits for each? Are limits per item, per month or per year? *(Assumed per item.)*
24. Should over-limit items be blocked, or allowed with a justification? *(Assumed allowed with justification and flagged.)*
25. Is there a deadline for claiming (for example, within 60 or 90 days of the expense)?
26. Should managers or HR be able to see their team's claims? *(Assumed no, claimant and Finance only.)*
27. Accepted receipt types and maximum file size? *(Assumed PDF, JPG, PNG up to 10 MB.)*
28. Can someone hold the Finance role and also approve claims from people they manage? Is a second Finance approver needed above a certain amount?

**Feedback and review cycles**
29. Should peer feedback be anonymous, or always named? *(Assumed named in v1.)*
30. Who can see peer feedback: recipient, giver and HR only, or the manager too? *(Stakeholder: "Recipient, Giver, HR, not sure". Assumed manager sees an item only if the recipient shares it.)*
31. Should the employee see individual peer reviews from a review cycle, and if so, named or anonymous? *(Assumed not shown to the employee.)*
32. Is there a rating scale for reviews, and what is it? *(Assumed free text only in v1.)*
33. How many peers can or must be nominated, and who confirms the nominations? *(Assumed 2–5, manager can add or remove.)*
34. How often do review cycles run (for example, twice a year), and when do reminders go out? *(Assumed reminders 3 days and 1 day before each deadline.)*
35. Do review results link to compensation or promotions? *(Out of scope for now.)*
36. Can a giver edit or withdraw feedback after sending it? *(Assumed no.)*

**PIP**
37. Can a manager start a PIP alone, or must HR approve before the employee sees it? *(Assumed HR approval needed.)*
38. Who records the final outcome: HR, the manager, or both together? *(Assumed manager recommends, HR records.)*
39. When the manager changes during a PIP, should the previous manager keep any access? *(Assumed no.)*
40. Should Admins (without the HR role) see PIP-related audit entries? *(Assumed no, HR only.)*
41. Standard PIP durations (for example, 30, 60 or 90 days) and check-in frequency? How long are closed PIPs kept?
42. Can the employee comment on check-ins? *(Assumed yes.)*

**Pods and assignments**
43. Is the pod lead a separate role from the reporting manager, or usually the same person? *(Stakeholder: "Not sure". Assumed separate, H11.)*
44. What exactly should a pod lead see: work profile, assignments and leave calendar only, or more? Should pod leads approve anything (for example, leave)? *(Assumed no approvals.)*
45. Can an employee belong to more than one pod at a time? *(Assumed no, H12.)*
46. Who maintains pods, clients, projects and assignments: HR, a delivery or operations team, or pod leads? *(Assumed HR.)*
47. Should total allocation above 100% be blocked? *(Assumed blocked.)*
48. Can every employee see colleagues' client and project names, or only pod names? *(Assumed pod names only in the directory.)*

## 16. Revised Assessment

- **Size:** EPIC (v1 is 21 vertical slices across 13 modules: 12 for the HR foundation and 9 for the new core modules, followed by v1.5 Reporting)
- **Greenfield:** yes
- **Risk:** HIGH (sign-in and access control, sensitive personal data, confidential performance data (reviews and PIPs), money leaving the company through expense exports, data migration from Frappe)

[ ] Reviewed
