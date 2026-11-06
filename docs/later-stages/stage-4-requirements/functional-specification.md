# Functional Specification

This specification sets out the detailed, testable behaviour of the People Platform, slice by slice. Each slice can be built and released on its own.

For each module's purpose, who uses it and who sees what, see the Product Requirements Document (PRD). For goals and scope, see Vision and Scope. Items marked **[Assumption]** are working defaults until the PRD's open questions are answered.

## Release 1: Core

### Slice 1: Sign in with Microsoft and see my profile
**Goal:** Anyone at Incubyte signs in with their Microsoft account and lands on their own profile.

- [ ] Users sign in with their Incubyte Microsoft account (single sign-on). There is no username and password option anywhere.
- [ ] Sign-in is refused for Microsoft accounts outside Incubyte, and for employees marked as exited. The only exception is joiners before Day 1 (see Slice 16).
- [ ] A valid Microsoft account with no matching employee record sees a "you don't have access yet, contact HR" page
- [ ] Users are signed out after 30 minutes of inactivity **[Assumption]**. After signing out, the browser's back button doesn't show protected pages.

### Slice 2: Roles and menus
**Goal:** Each person sees only what their role needs, and nothing more.

- [ ] Admin can give or remove a role (HR, PST, Manager, Pod Lead, Finance, TA, Buddy, Admin), and each change is recorded
- [ ] Menus and buttons show only what the person can use
- [ ] Access is checked on the server for every request. Opening something outside your role, for example by changing the web address, shows "not found" and is recorded.
- [ ] Admin never sees personal details, leave reasons, claims, feedback, reviews or PIPs unless they also hold the HR role
- [ ] A role change takes effect the next time the person opens a page

### Slice 3: Employee Profile
**Goal:** One trusted record per person: employees keep their own details current, and HR maintains the rest.

**Profile and directory**
- [ ] After sign-in, the user lands on their own profile, showing name, work email, role, department, location, manager, pod lead and joining date
- [ ] Any signed-in user can search the directory by name and see work email, role, department, location and manager
- [ ] The directory never shows personal contact details, salary, ID numbers or bank details, for any role
- [ ] Users can open an org chart showing a person's manager and direct reports. Exited employees don't appear in the directory.
- [ ] A manager can open the full work profile of anyone in their reporting line, direct or indirect. Opening someone outside it shows "not found", and the attempt is recorded.
- [ ] Employees can update their personal contact details and emergency contacts, but not their job details

**HR records**
- [ ] HR can create a record with the required fields: name, work email, joining date, role, department, location, manager, pod lead and employment type
- [ ] The platform names the missing field when a required detail is missing, and rejects a work email that already belongs to someone else
- [ ] The platform rejects a manager choice that would create a reporting loop (A reports to B, and B reports to A)
- [ ] HR can change role, department, location, manager or pod lead with an effective date, and the previous value is kept in the employment history
- [ ] If two HR users edit the same record at once, the second save is blocked with a prompt to reload, so no change is silently lost
- [ ] Every change records who made it, when, which field, and the old and new values

**Sensitive details**
- [ ] HR can record salary, government ID numbers (such as PAN and Aadhaar) and bank details. They are masked on screen by default, for example showing only the last 4 digits.
- [ ] HR and the employee can reveal a full sensitive value with a deliberate "show" action, and every reveal is recorded with who, whose record, which field and when
- [ ] Managers, pod leads, TA, Finance and Admin never see sensitive values, masked or unmasked, unless they also hold the HR role
- [ ] Sensitive values are stored encrypted and never appear in notifications, emails, exports or error messages
- [ ] The platform rejects an ID or bank number in the wrong format, such as an IFSC code of the wrong length

### Slice 4: Apply for leave and see my leave record
**Goal:** Employees apply for leave in a few steps, with the team's situation in front of them.

- [ ] Employees can request leave by choosing a type, start date, end date and an optional reason
- [ ] The request shows how many working days it uses, excluding weekends and holidays
- [ ] Employees can request a half day **[Assumption: allowed for every type]**
- [ ] The platform rejects a request that overlaps existing pending or approved leave, or whose end date is before its start date
- [ ] Confirmed employees have no leave limit. Every employee sees the leave they have taken this year by type, and a dated history.
- [ ] Employees on probation see a reminder when a request is longer than 2 days at a stretch or is their second leave that month. Employees serving notice see a reminder that leave may extend their notice period. Neither reminder blocks the request.
- [ ] Employees can cancel a pending request, or an approved one that hasn't started. Everyone involved is notified.

### Slice 5: Lead, PST and HR approve leave
**Goal:** The pod lead approves first, PST gives the final decision, and special cases go to HR.

- [ ] The employee's pod lead receives an email and a portal notification for each new request, and approves or rejects it. A rejection requires a reason. **[Assumption: to confirm in interviews]**
- [ ] Once the pod lead approves, the request goes to PST for the final decision. PST sees the employee's leave history and whether they are on probation or serving notice.
- [ ] Requests for leave during probation (including long or emergency leave), maternity leave, paternity leave and possibly leave of absence also go to HR, who decides them **[Assumption: HR decides after PST]**
- [ ] The employee sees which step their request is waiting on, and is notified by email and in the portal when it is approved or rejected
- [ ] Leave counts only once it is fully approved
- [ ] When a request is approved, the employee is reminded to set Out of Office in Outlook and Teams
- [ ] If an approver is absent or has left, HR can decide on their behalf, and the record shows that HR acted
- [ ] Nobody can approve their own leave, even if the data shows them as their own lead
- [ ] If two approvers act on the same request at the same moment, only the first decision counts, and the second sees "already decided"

### Slice 6: Leave setup, holidays and who's away
**Goal:** HR sets the rules once, and anyone can see who's away.

- [ ] HR can set up leave types (including maternity and paternity), the probation and notice rules, and which cases go to HR
- [ ] HR maintains the holiday calendar for the current and next year, marking each holiday as fixed or optional, and sets how many optional and floating holidays each employee may take
- [ ] Employees choose optional and floating holidays up to the limit. The platform rejects a choice over the limit, or two holidays on the same date.
- [ ] A "who's away" view shows approved leave for colleagues in the same department, without the leave type or reason. Pod leads see their members' approved and pending leave.
- [ ] Only the employee, their own pod lead, PST and HR see a leave request and its reason. Other employees and other pod leads see only who is away.
- [ ] In Release 1, PST checks team availability and black-out dates manually. Teams & Pods automates both in Release 3.

### Slice 7: Claim an expense
**Goal:** Employees submit expense claims with receipts and always know where each claim stands.

- [ ] Employees can create a claim with one or more items, each with a date, category, amount in INR, description and at least one receipt
- [ ] Each item uses one of the categories: Conferences, Home office setup, Learning and development, Micromeet, OKR meet, Software tooling, Term insurance, Trainings (in person), Work in person (WIP) or Others. Vehicle expenses are claimed under Others and marked as vehicle.
- [ ] Employees can save a claim as a draft and submit it later
- [ ] The platform rejects an item with no receipt, an amount of zero or less, or a future date
- [ ] The platform rejects a receipt larger than 10 MB or not in PDF, JPG or PNG format **[Assumption]**, and a receipt file already attached to another claim
- [ ] An item over its category limit can be submitted only with a written justification, and is flagged "over limit" for Finance **[Assumption]**
- [ ] An item with the same date, amount and category as another of the employee's claims is flagged "possible duplicate" for Finance
- [ ] Employees see each claim's status: Draft, Submitted, Returned, Approved, Rejected or Exported. They can withdraw a submitted claim until Finance decides.
- [ ] A returned claim can be corrected and resubmitted by the employee

### Slice 8: Finance reviews and exports claims
**Goal:** Finance decides each claim and hands approved ones over for payout, with each claim paid only once.

**Finance review**
- [ ] Finance can set an optional limit for each category **[Assumption: limit per item]**
- [ ] Finance receives a portal and email notification for each submitted claim, without the receipts attached
- [ ] Finance sees a queue of submitted claims with their flags, and can approve a claim, reject it with a reason, or return it for changes **[Assumption: Finance is the only approver]**
- [ ] The employee is notified by email and in the portal when their claim is approved, rejected or returned
- [ ] A Finance member can't decide their own claim; another Finance member must
- [ ] If two Finance members act on the same claim at once, only the first decision counts
- [ ] Approved, rejected and exported claims can't be edited by anyone

**Export and privacy**
- [ ] Finance can export all approved claims not yet exported as one file **[Assumption: CSV, format to confirm with Finance]**, with employee ID, name, claim reference, item date, category, amount and approval date
- [ ] Exported claims are marked Exported with the export date and are left out of the next export. If an export fails partway, no claim is marked Exported.
- [ ] The export never includes bank details or any other sensitive field
- [ ] Only the claimant and Finance can see claims and receipts. Anyone else sees "not found", and the attempt is recorded.
- [ ] Finance can't see salary, ID numbers, bank details, feedback, reviews or PIPs unless they also hold the HR role

### Slice 9: Reports in use today
**Goal:** The three reports people use today, shown only to the people who need them.

- [ ] HR and PST can view the Leave Balance report: leave each employee has taken by type, and what remains for types with a limit, such as optional and floating holidays
- [ ] HR and PST can view the Leave Balance Summary report: the same, totalled across the company
- [ ] Finance can view the Vehicle Expenses report for a chosen date range
- [ ] People without access don't see these reports in their menu
- [ ] Each report can be downloaded as a spreadsheet, and every download is recorded **[Assumption]**

### Slice 10: Notifications
**Goal:** People hear about anything they need to act on, in the portal and by email.

- [ ] Every event that needs someone's action sends a portal notification and an email to the people involved
- [ ] Users see a notification bell with an unread count, and can mark notifications as read
- [ ] Clicking a notification opens the related item, such as the leave request
- [ ] Emails contain only a short summary and a link. They never include leave reasons, claim details, feedback, review text or PIP details.
- [ ] If an email fails to send, the portal notification still appears and the email is retried later

### Slice 11: Audit trail
**Goal:** Every change, and every look at sensitive data, can be traced.

- [ ] Every sign-in, failed sign-in and sign-out is recorded with the user, time and outcome
- [ ] Every change records who made it, on whose record, what changed and when
- [ ] Every view of sensitive data, feedback, review content or PIPs by someone other than the owner is recorded
- [ ] HR can search the trail by the person acted on, the person acting, the action type and a date range, and filter to sensitive-data views only **[Assumption: HR, not Admin]**
- [ ] Nobody, including Admin and HR, can edit or delete an entry. Viewing the trail is itself recorded.
- [ ] PIP-related entries are visible only to HR
- [ ] Sensitive values are never stored in plain form in the trail

### Slice 12: Move data from Frappe
**Goal:** Release 1 data arrives complete and checked, and is signed off before we switch over.

- [ ] Admin can run the move to bring across employees (active and exited), org structure and reporting lines, employment history, sensitive fields, leave types, all leave records and history, and past expense claims with receipts
- [ ] Open expense claims are closed in Frappe before the move. Holidays are set up fresh.
- [ ] The move produces a check report showing, for each type of data, what was found in Frappe, moved, skipped or failed, with reasons
- [ ] Records that fail checks, such as a missing work email or an unknown manager, are listed for HR to fix and don't block the rest
- [ ] Running the move again updates existing records instead of creating duplicates. If it stops halfway, it can be re-run safely.
- [ ] Moved leave records appear next to Frappe's values, with differences highlighted, for HR to sign off. Finance signs off the expense data.
- [ ] Every moved record is marked with its source and time in the audit trail
- [ ] Sensitive fields are encrypted as they are moved and never written to logs or the check report in plain form
- [ ] Only Admin can run the move

## Release 2: Performance and lifecycle

### Slice 13: Feedback anytime
**Goal:** Anyone can give a colleague feedback at any time, and it stays private.

- [ ] Any employee can write feedback for any active colleague at any time. The platform rejects empty feedback and feedback to oneself.
- [ ] Feedback shows the giver's name to the recipient **[Assumption: named, not anonymous]**
- [ ] The recipient gets a notification that doesn't include the feedback text, and sees all feedback received, newest first. The giver sees all feedback they have given.
- [ ] Feedback can't be edited or deleted after it is sent **[Assumption]**
- [ ] The recipient can share a specific item with their manager, and stop sharing it later. Otherwise the manager doesn't see it.
- [ ] HR can see feedback for any employee, and each HR view is recorded
- [ ] Pod leads, skip-level managers, Admin, Finance and colleagues never see someone else's feedback. A direct link shows "not found", and the attempt is recorded.
- [ ] The recipient can report an item as inappropriate. HR can hide it with a required reason, and the action is recorded.
- [ ] Feedback prompts can always be dismissed

### Slice 14: 360 review cycles
**Goal:** HR runs 360 reviews with self, peer and manager input, on time.

**Review cycles: set-up and nominations**
- [ ] HR can create a review cycle with a name, the review period, the participants (everyone, or chosen departments), and deadlines for nominations, self-reviews, peer reviews and manager reviews
- [ ] The platform rejects deadlines that are out of order or in the past
- [ ] HR sets free-text questions for self, peer and manager reviews **[Assumption: ratings wait for a decision on a rating scale]**
- [ ] Launching a cycle notifies each participant and their manager. People who join after launch are included only if HR adds them.
- [ ] Each participant nominates 2 to 5 peers **[Assumption]**. The platform rejects nominating oneself, one's own manager or an exited employee.
- [ ] The participant's manager can add or remove nominees until the nomination deadline **[Assumption]**
- [ ] Each confirmed reviewer gets a task and a notification, and can decline with a reason so someone else can be nominated
- [ ] People with pending review tasks get reminders 3 days and 1 day before each deadline **[Assumption]**
- [ ] HR sees each participant's progress, with overdue items highlighted, and can extend a deadline. Everyone affected is notified.
- [ ] A participant who leaves during the cycle is removed from it, and their pending tasks are cancelled

**Review cycles: writing and finalising**
- [ ] Participants and peer reviewers can save their reviews as drafts and submit them. Submitted reviews can be edited only if HR reopens them, and each reopen is recorded.
- [ ] The direct manager sees the self-review and all submitted peer reviews, then writes and finalises the manager review once the self-review is in or its deadline has passed
- [ ] The finalised review shows which self or peer reviews were missing
- [ ] The participant sees their finalised manager review alongside their self-review, but not individual peer reviews **[Assumption: pending the anonymity decision]**
- [ ] A peer reviewer sees only the review they wrote
- [ ] The participant acknowledges the finalised review, with an optional comment. Acknowledging means "I have seen it", not "I agree".
- [ ] A finalised review is locked. HR can reopen it with a required reason, and the reopen is recorded.
- [ ] If the participant's manager changes during the cycle, the pending manager review moves to the new manager, and the previous manager loses access **[Assumption]**
- [ ] Every view of review content by HR or the manager is recorded. Notifications never include review text.

### Slice 15: PIP
**Goal:** A structured, strictly confidential plan to help someone improve.

**Creating the plan**
- [ ] HR or the employee's direct manager can draft a PIP with goals, success criteria for each goal, a start date, an end date and a check-in schedule
- [ ] The platform rejects a PIP with no goals, a goal with no success criteria, an end date before the start date, or a second active PIP for the same employee
- [ ] A manager can draft a PIP only for their own direct reports
- [ ] A drafted PIP becomes active and visible to the employee only after HR approves it **[Assumption]**
- [ ] On activation, the employee is notified only that they have an item to review, with no PIP details
- [ ] The employee acknowledges the PIP, with an optional comment. Acknowledging means "I have received it", not "I agree". HR sees whether and when they acknowledged, and the employee is reminded after 3 working days **[Assumption]**.
- [ ] When the employee's manager changes, the new manager gains access and the previous manager loses it immediately **[Assumption]**

**Check-ins and outcome**
- [ ] The manager or HR records each check-in, with progress against each goal and notes. The employee can see each check-in and comment on it.
- [ ] The manager is reminded 2 days before each check-in **[Assumption]**, and HR sees missed check-ins highlighted
- [ ] The manager recommends an outcome, and HR records the final outcome with a summary: Successful, Extended or Unsuccessful **[Assumption: HR confirms]**. Extending requires a later end date.
- [ ] The employee is notified that an outcome is recorded, without the outcome in the notification, and can acknowledge it
- [ ] Once an outcome other than Extended is recorded, the PIP is closed and read-only. If the employee leaves during an active PIP, it is closed as "employee exited" **[Assumption]**.

**Confidentiality**
- [ ] Only the employee, their current direct manager and HR can open a PIP. Anyone else sees "not found", and the attempt is recorded.
- [ ] A PIP never appears on profiles, in the directory, org chart, calendars, pod views, search results or reports
- [ ] Every view of a PIP is recorded, and those records are visible only to HR
- [ ] PIP content is stored encrypted and never appears in emails, logs, error messages or exports

### Slice 16: Onboarding
**Goal:** TA runs a shared checklist so every joiner has a smooth start, without seeing their sensitive documents.

**Checklist**
- [ ] TA can create checklist templates with tasks assigned to a role (TA, HR, the manager, the joiner, and IT if confirmed), each due a set number of days before or after joining
- [ ] Creating a new employee record starts an onboarding checklist from the template TA chooses
- [ ] Each task owner is notified and sees their tasks in a "My tasks" list, and is reminded when a task is overdue
- [ ] If the joining date changes, task due dates move with it
- [ ] Asset handover tasks record which asset was given, such as a laptop serial number

**What TA sees**
- [ ] TA sees every onboarding task, each joiner's progress with overdue tasks highlighted, and whether each document is submitted, verified or missing
- [ ] TA sees the joiner's name, role, team, joining date, work location, personal contact details until the work account is active, offer status, background check status (clear or flagged), and laptop, ID card and access requests
- [ ] TA can't open the contents of ID documents, bank details, salary after the offer stage, medical or health declarations, insurance nominees, the full background check report, or emergency contacts. Only the joiner and HR can.
- [ ] TA's access to a joiner ends when their checklist is closed. If TA needs a sensitive document, HR provides it, and the access is recorded.

**Joiner and buddy**
- [ ] Before Day 1, joiners sign in with their personal email and see only the onboarding module: their own tasks and document uploads. Opening anything else shows "not found". From Day 1 they sign in with their Incubyte Microsoft account, and personal-email access ends. **[Assumption: sign-in method and onboarding-only access to confirm with IT]**
- [ ] Joiners upload requested documents, such as ID proof and education certificates, in their tasks. The platform rejects files that are too large or of an unsupported type.
- [ ] Once the joiner has started, TA assigns a buddy, schedules a bi-weekly 1:1 between them, and gives the buddy a few tasks. The buddy sees only those tasks.

### Slice 17: Offboarding
**Goal:** Every leaver gets a clean, complete exit and loses access straight away.

- [ ] HR can start an exit with the resignation date, last working day and exit reason
- [ ] Starting an exit creates an offboarding checklist owned by PST, covering asset return, knowledge handover and access removal
- [ ] Removing Microsoft 365 and other access is a tracked task, not automatic **[Assumption]**
- [ ] Assets recorded at onboarding appear on the checklist for return
- [ ] Pending leave after the last working day is cancelled automatically, and the employee is notified
- [ ] Direct reports of the leaver are flagged to HR for reassignment before the exit is completed
- [ ] HR can't complete an exit while mandatory tasks are still open
- [ ] After the last working day, the leaver can't sign in. Their record and history are kept, and only HR can see them.

### Slice 18: Letters
**Goal:** Employees get standard letters without waiting on HR.

- [ ] Employees can request a standard letter, such as employment verification or address proof, and download it straight away
- [ ] Letters that HR marks as needing approval are created only after HR approves, and the employee is notified
- [ ] The platform names the missing detail when a letter can't be created, such as no address on record
- [ ] Every letter has a reference number, and employees see all the letters they have received
- [ ] Only the employee and HR can see an employee's letters

### Slice 19: Past performance records from Frappe
**Goal:** Past feedback, reviews and PIPs are kept safe without spreading further.

- [ ] Past feedback, reviews and PIPs from Frappe are kept as a read-only archive **[Assumption: archive, not move. HR to decide.]**
- [ ] Only HR can open the archive
- [ ] Each archived item shows where it came from and its original date

## Release 3: Organisation and communication

### Slice 20: Policies
**Goal:** One place to find every company policy, with key policies acknowledged.

- [ ] HR can publish a policy, and later publish a new version while earlier versions stay in its history
- [ ] Employees can find every current policy in one place
- [ ] HR can mark a policy as one employees must acknowledge. HR sees who has acknowledged each version, and when.
- [ ] Reminders to acknowledge can be dismissed and return weekly until the policy is acknowledged **[Assumption]**

### Slice 21: Announcements and broadcast
**Goal:** Every policy and announcement reaches people at once, under the publisher's name, with a record.

- [ ] A publisher can post an announcement with a title and message, to everyone or to chosen departments or pods, with optional pinning and an expiry date **[Assumption: HR and anyone Admin gives publisher rights]**
- [ ] Publishing a policy or announcement sends it at once to the portal, by email and to the company-wide Teams channel. The email and Teams post appear under the publisher's own name.
- [ ] Each item keeps a broadcast record: who published it, when, and where it went, with delivery times
- [ ] If one channel fails, the others still go out, and the publisher sees the failure and can resend

### Slice 22: Teams & Pods *(working name)*
**Goal:** Everyone sees their pod, its lead and its members.

**Pods**
- [ ] HR can create a pod with a name, a pod lead and members. The platform rejects a duplicate pod name or a lead who isn't an active employee.
- [ ] Each employee belongs to one pod at a time **[Assumption]**. Adding someone already in another pod shows an error naming that pod.
- [ ] HR can move a member, change the lead or close a pod, and each change is recorded as history
- [ ] Employees see their pod name and lead on their profile, and colleagues see pod names in the directory
- [ ] Pods and their members move over from Frappe, with a check report for HR
- [ ] Only HR can create or change pods **[Assumption]**

**Pod lead view**
- [ ] Pod leads see their members' work profiles and their approved and pending leave, including the leave type and reason
- [ ] Pod leads never see members' sensitive details, personal contact details, feedback, reviews or PIPs. Opening someone outside their pod shows "not found", and the attempt is recorded.
- [ ] When a member leaves the pod, the lead loses access to their profile and leave from that moment. Leavers are removed from their pod automatically.

**Leave checks**
- [ ] Each pod member has a role (Dev, QA or BQA), and each pod can mark its black-out dates
- [ ] When applying for leave, employees see their pod's black-out dates and who in the pod is already away
- [ ] PST is warned when approving leave of more than one day would leave fewer than half of a role in the pod available. Floating holidays are not checked.

### Slice 23: Assignments *(pending Stage 2 interviews)*
**Goal:** Keep the Assignments module. Its purpose will be confirmed in the 16 Nov interviews before it's designed.

- [ ] *(Pending)* Employee can see their own current assignments
- [ ] *(Pending)* Current assignments move over from Frappe once the purpose is confirmed
- [ ] *(Pending)* Assignments are visible only to the roles agreed in the interviews

[ ] Reviewed
