# Spec: Incubyte People Platform

*Spec sheet and functional spec · Draft · Pending approval of Stages 1–3*

Part 1 is the spec sheet: the platform's key facts on one page. Part 2 is the functional spec: what each module must do, release by release. Each slice is something people can use on its own, with a one-line goal and a short checklist that defines "done". Items marked **[Assumption]** are defaults we chose; each one comes up again under Open questions.

# Part 1: Spec sheet

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

## Standards (apply to every slice)

| Area | Target |
|---|---|
| Access | Checked on every request. Anyone opening something outside their role sees "not found". Menus show only what that person can use. |
| Ease of use and speed | Common tasks take a few clear steps on laptop or phone. Common screens load in under 2 seconds. |
| Security and audit | Every change and every view of sensitive data recorded. Independent security review before launch. |
| Reliability | Backed up daily. No leave or expense record is lost or duplicated. |

# Part 2: Functional spec

## Who uses it

| Role | In short |
|---|---|
| **Employee** | Profile, leave, expenses, feedback, letters, policies |
| **People Success Team (PST)** | Approves leave. Runs onboarding and offboarding. |
| **HR** | Employee records, leave rules, reviews, PIPs, policies, letters |
| **Manager** | Kept informed about their team's leave. Reviews their team. Runs PIPs with HR. |
| **Pod Lead** | Sees their pod's members and availability |
| **Finance** | Approves expense claims and exports them for payout |
| **TA / Buddy** | Complete their onboarding tasks |
| **Admin** | Manages roles and settings. Never sees personal data unless also HR. |

---

## Release 1: Core

### Slice 1: Sign in with Microsoft and see my profile
**Goal:** Anyone at Incubyte signs in with their Microsoft account and lands on their own profile.

- [ ] User signs in with their Incubyte Microsoft account. There is no other way to sign in.
- [ ] Sign-in is refused for Microsoft accounts outside Incubyte
- [ ] Shows a "no access yet, contact HR" page when a valid Incubyte account has no employee record
- [ ] After sign-in, the user sees their own profile and a home page
- [ ] A leaver can no longer sign in once HR marks them as exited

### Slice 2: Roles and menus
**Goal:** Each person sees only what their role needs, and nothing more.

- [ ] Admin can give or remove a role (HR, PST, Manager, Pod Lead, Finance, TA, Buddy, Admin)
- [ ] Menus show only the modules and actions the person's roles allow
- [ ] A role change takes effect the next time the person opens a page
- [ ] Admin who isn't also HR can't see any personal details, leave reasons or expense claims
- [ ] An employee who changes the web address to open someone else's record sees "not found"

### Slice 3: Employee Profile
**Goal:** One trusted record per person: employees keep their own details current, and HR maintains the rest.

- [ ] Employee can update their own personal and contact details
- [ ] Employee can't change their role, department, location, manager or joining date
- [ ] HR can create and update an employee's role, department, location, manager and joining date
- [ ] Shows an error naming the field when a required detail is missing
- [ ] Colleagues see only a basic directory entry: name, role, department, location, manager and work email
- [ ] Personal and sensitive details are visible only to the employee and HR

### Slice 4: Apply for leave and see my balance
**Goal:** Employees apply for leave in a few steps and always know their balance.

- [ ] Employee can apply for full-day or half-day leave of any type: fixed, optional or floating holiday, birthday, earned/privilege, casual, sick, unpaid, bereavement or other
- [ ] The request shows how many working days it uses, skipping weekends and holidays
- [ ] Employee always sees their balance for each leave type: available, used and pending
- [ ] Shows an error when the request overlaps leave they already have
- [ ] Shows an error when the request is more than the available balance, except for unpaid leave
- [ ] Employee can cancel a request, and the days go back to their balance

### Slice 5: People Success approves leave
**Goal:** PST approves or rejects leave as they do today, and everyone involved stays informed.

- [ ] PST sees all pending leave requests and can approve or reject each one, giving a reason when rejecting
- [ ] PST can approve long or emergency leave for an employee on probation even if their balance doesn't cover it **[Assumption]**
- [ ] The employee is notified of the decision and sees their updated balance
- [ ] The employee's manager is told about the request and its outcome but can't approve or reject it **[Assumption]**
- [ ] The manager never sees the leave reason. Only the employee, PST and HR see it.
- [ ] Everyone involved is notified when a request is cancelled

### Slice 6: Leave setup, holidays and who's away
**Goal:** HR sets the leave rules once, and anyone can see who's away.

- [ ] HR can set up leave types and their rules, such as yearly entitlement and how much carries forward
- [ ] HR can set up fixed, optional and floating holidays for the current year and the next
- [ ] Employee can choose optional and floating holidays up to the limit HR sets
- [ ] When the January–December leave year ends, balances roll over following each type's rules
- [ ] Employees can see who in their department is away on a given day, but not the leave type or reason **[Assumption: grouped by department until pods arrive in Release 3]**

### Slice 7: Claim an expense
**Goal:** Employees submit expense claims with receipts and always know where each claim stands.

- [ ] Employee can create a claim with one or more items, each with a date, category, amount and receipt
- [ ] Employee can claim vehicle expenses as one of the categories
- [ ] Shows an error when an item has no receipt, or an amount of zero or less
- [ ] Employee sees each claim's status: Draft, Submitted, Returned, Approved, Rejected or Exported
- [ ] Employee can fix a returned claim and submit it again
- [ ] Only the claimant and Finance can see a claim and its receipts

### Slice 8: Finance reviews and exports claims
**Goal:** Finance decides each claim and hands approved ones over for payout, with each claim paid only once.

- [ ] Finance can approve, reject or send back a claim, giving a reason when rejecting or sending back
- [ ] Finance can't decide their own claim **[Assumption: Finance is the only approver]**
- [ ] The claimant is notified of every decision
- [ ] Finance can export all approved claims that haven't been exported yet as one file for payout outside the platform
- [ ] A claim is never included in more than one export

### Slice 9: Reports in use today
**Goal:** The three reports people use today, shown only to the people who need them.

- [ ] HR and PST can view the Leave Balance report: each employee's balance per leave type
- [ ] HR and PST can view the Leave Balance Summary report: balances totalled across the company
- [ ] Finance can view the Vehicle Expenses report for a chosen date range
- [ ] People without access don't see these reports in their menu
- [ ] Each report can be downloaded as a spreadsheet **[Assumption]**

### Slice 10: Notifications
**Goal:** People hear about anything they need to act on, in the portal and by email.

- [ ] Every leave and expense event in Slices 5–8 sends a portal notification and an email to the people involved
- [ ] User sees unread notifications in the portal and can mark them as read
- [ ] Clicking a notification opens the related item
- [ ] Emails contain only a short summary and a link, never leave reasons or claim details
- [ ] If an email fails to send, the portal notification still appears and the email is tried again

### Slice 11: Audit trail
**Goal:** Every change, and every look at sensitive data, can be traced.

- [ ] Every change records who made it, on whose record, what changed and when
- [ ] Every view of personal or sensitive details by someone other than the owner is recorded
- [ ] HR can search the audit trail by person, record and date range **[Assumption: HR only, not Admin]**
- [ ] No one, including Admin and HR, can edit or delete an audit entry

### Slice 12: Move data from Frappe
**Goal:** Release 1 data arrives complete and checked, and is signed off before we switch over.

- [ ] People records move over, including past employees
- [ ] Leave balances and all leave history move over
- [ ] Past expense claims and their receipts move over
- [ ] The move stops and lists any expense claim still open in Frappe, so it can be closed there first
- [ ] Each run produces a check report: records found, moved and failed, with the reason for each failure
- [ ] Moved leave balances appear next to Frappe's values, with differences highlighted, for HR to sign off **[Assumption: HR signs off people and leave data, Finance signs off expenses]**

---

## Release 2: Performance and lifecycle

### Slice 13: Feedback anytime
**Goal:** Anyone can give a colleague feedback at any time, and it stays private.

- [ ] Employee can give feedback to any current colleague, with their name shown **[Assumption: named]**
- [ ] Shows an error when the feedback is empty or the employee picks themselves
- [ ] Recipient sees all feedback they've received
- [ ] Feedback prompts can always be dismissed
- [ ] Only the giver, the recipient and HR can see a piece of feedback

### Slice 14: 360 review cycles
**Goal:** HR runs 360 reviews with self, peer and manager input, on time.

- [ ] HR can launch a review cycle with participants, peer reviewers and deadlines
- [ ] Each participant writes a self-review, each chosen peer writes a peer review, and the manager writes the final review
- [ ] People with pending reviews get reminders before each deadline
- [ ] HR sees who has completed each review in a cycle
- [ ] The employee sees their finished review and can acknowledge it
- [ ] Review content is visible only to the people involved and HR

### Slice 15: PIP
**Goal:** A structured, strictly confidential plan to help someone improve.

- [ ] HR and the manager can set up a PIP with goals, check-in dates and an end date
- [ ] The employee sees the PIP only after HR approves it **[Assumption]**
- [ ] The employee can acknowledge the PIP
- [ ] The manager records progress at each check-in, and the outcome at the end
- [ ] Shows an error when the employee already has an active PIP
- [ ] Only the employee, their manager and HR can see a PIP. It never shows on profiles, the directory, search, reports or notifications to anyone else.

### Slice 16: Onboarding
**Goal:** Every joiner gets the same well-run first days, with shared tasks everyone can see.

- [ ] HR or PST can start an onboarding checklist for a joiner from a standard template
- [ ] The checklist gives tasks to PST, TA, HR, the manager and the buddy
- [ ] Each person sees their own tasks in one place and can mark them done
- [ ] Task owners get a reminder when a task is overdue
- [ ] HR and PST see each joiner's progress

### Slice 17: Offboarding
**Goal:** Every leaver gets a clean, complete exit and loses access straight away.

- [ ] HR or PST can start an exit with a last working day, which creates an offboarding checklist
- [ ] The checklist gives tasks to PST, HR, the manager and IT
- [ ] HR can't complete an exit while required tasks are still open
- [ ] The leaver loses access at the end of their last working day **[Assumption]**
- [ ] After they leave, their record is kept and only HR can see it

### Slice 18: Letters
**Goal:** Employees get standard letters without waiting on HR.

- [ ] Employee can request a standard letter, such as employment verification, and download it straight away
- [ ] Letters that HR marks "needs approval" are created only after HR approves, and the employee is notified
- [ ] Shows an error naming the missing detail when a letter can't be created, such as no address on record
- [ ] Employee sees all the letters they've received
- [ ] Only the employee and HR can see an employee's letters

### Slice 19: Past performance records from Frappe
**Goal:** Past feedback, reviews and PIPs are kept safe without spreading further.

- [ ] Past feedback, reviews and PIPs from Frappe are kept as a read-only archive **[Assumption: archive, not move. HR to decide.]**
- [ ] Only HR can open the archive
- [ ] Each archived item shows where it came from and its original date

---

## Release 3: Organisation and communication

### Slice 20: Policies
**Goal:** One place to find every company policy, with key policies acknowledged.

- [ ] HR can publish a policy, and later publish a new version while earlier versions stay in its history
- [ ] Employees can find every current policy in one place
- [ ] HR can mark a policy as one that employees must acknowledge
- [ ] Employee can acknowledge a key policy. HR sees who has acknowledged each version, and when.
- [ ] Reminders to acknowledge can be dismissed and come back weekly until the policy is acknowledged **[Assumption]**

### Slice 21: Announcements and broadcast
**Goal:** Every policy and announcement reaches people at once, under the publisher's name, with a record.

- [ ] A publisher can post an announcement with a title and message **[Assumption: HR and anyone Admin gives "publisher" rights]**
- [ ] Publishing a policy or announcement sends it at once to the portal, by email and to the company-wide Teams channel
- [ ] The Teams post and the email appear under the publisher's own name
- [ ] Each item keeps a broadcast record: who published it, when, and where it went
- [ ] If one channel fails, the others still go out, and the publisher sees the failure and can resend

### Slice 22: Teams & Pods *(working name)*
**Goal:** Everyone sees their pod, its lead and its members.

- [ ] Employee sees "my pod": its name, lead and members
- [ ] HR can create a pod, choose its lead and add or remove members **[Assumption: one pod per person]**
- [ ] The pod lead sees who in their pod is away, without leave type or reason
- [ ] Leavers are removed from their pod automatically
- [ ] Pods and their members move over from Frappe, with a check report for HR

### Slice 23: Assignments *(pending Stage 2 interviews)*
**Goal:** Keep the Assignments module. Its purpose will be confirmed in the 16 Nov interviews before it's designed.

- [ ] *(Pending)* Employee can see their own current assignments
- [ ] *(Pending)* Current assignments move over from Frappe once the purpose is confirmed
- [ ] *(Pending)* Assignments are visible only to the roles agreed in the interviews

---

## Later

Wider reports and dashboards, beyond the three reports used today. They'll get their own spec once Releases 1–3 are under way.

## Out of scope

Payroll, accounting, recruitment, bench planning, timesheets, and mobile or Teams apps. Posting to the company-wide Teams channel *is* in scope.

## Open questions

Each one shows the default this spec uses until it's answered.

1. **Leave approval:** should managers approve leave, or only be informed? *Default: informed only, and they don't see leave reasons.*
2. **Leave rules:** what are the entitlement and carry-forward rules for each type? What counts as "long" leave for someone on probation? *Default: PST can approve long or emergency leave for someone on probation even without enough balance.*
3. **Expenses:** is Finance the only approver? How are vehicle expenses worked out? *Default: Finance only, and Finance can't decide their own claims.*
4. **Feedback:** named or anonymous? *Default: named.*
5. **PIP:** can a PIP start without HR approval? *Default: the employee sees it only after HR approves.*
6. **Past performance records:** move them or archive them? *Default: an archive only HR can open.*
7. **Teams & Pods:** what should it be called? Is the pod lead the same person as the manager? Can someone be in more than one pod? *Default: one pod per person. Until Release 3, the "who's away" view groups people by department.*
8. **Assignments:** what is it used for, and by whom? *Slice 23 stays on hold until the 16 Nov interviews.*
9. **Other defaults to confirm:** HR signs off moved people and leave data, and Finance signs off expenses. Only HR searches the audit trail. Reports can be downloaded. Leavers lose access at the end of their last working day. Policy reminders come back weekly. HR and named publishers can post announcements.

[ ] Reviewed
