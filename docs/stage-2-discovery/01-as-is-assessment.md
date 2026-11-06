# As-Is Assessment

How things work today, before the new platform, and what to keep, fix or drop. Part 1 covers every module; Part 2 maps the three Release 1 tasks step by step. Unknowns are filled in through the interviews in the [discovery plan](02-discovery-plan.md). What the new platform adds is in the PRD.

## Part 1: Modules today

| Module | Today | Keep | Fix | Drop |
|---|---|---|---|---|
| **Employee Profile** | In the tool (see Map 3) | One record per person | Who can see what | |
| **Leaves** | In the tool. The pod lead approves, then PST gives the final decision; some cases go to HR *(to confirm)*. Unlimited leave after confirmation (see Map 1). | Applying in one place, pod lead then PST approval, unlimited leave after confirmation | Speed, and who can see others' leave | Unused leave types |
| **Expenses** | In the tool. Finance approves *(to confirm)*. Ten categories; vehicle expenses go under Others (see Map 2). | Claims with receipts, including vehicle expenses | Speed, and who can see others' claims | Duplicate steps |
| **Feedback** | In the tool | Giving feedback to colleagues | | |
| **360 reviews** | Unknown | | | |
| **PIP** | Unknown | | | |
| **Assignments** | In the tool, purpose unclear | Kept as a module | After the interviews | |
| **Teams & Pods** | Each person's pod is recorded in the tool | Pods | The name | |
| **Onboarding & Offboarding** | Unknown | | | |
| **Policies & Announcements** | Missing | | | |
| **Letters** | Missing | | | |
| **Reports** | Leave Balance, Leave Balance Summary, Vehicle Expenses | All three reports | Access limited by role | |
| **Across the tool** | | | Access, ease of use, design, speed | Menus people can't use |

## Part 2: As-is process maps

Feedback, reviews and onboarding are mapped after the interviews.

### Pain across all three

- Slow, hard to use, and not intuitive.
- People can see beyond their role. Personal and sensitive information and files are visible to other employees in some places.
- Poor UI and UX, reflected in low survey satisfaction.

### Map 1: Apply for and approve leave

```mermaid
%%{init: {"htmlLabels": false, "fontFamily": "Arial, Helvetica, sans-serif", "flowchart": {"htmlLabels": false, "padding": 16, "wrappingWidth": 400}}}%%
flowchart LR
    A[Employee plans ahead<br/>and tells team] --> B[Applies in the tool]
    B --> L{Pod lead reviews}
    L -->|Approve| C{PST final decision}
    L -->|Reject| E[Employee is told why]
    C -->|Some cases| H{HR decides}
    H -->|Approve| D[Employee sets<br/>Out of Office]
    H -->|Reject| E
    C -->|Approve| D
    C -->|Reject| E
```

| Rules today | |
|---|---|
| Who approves | The pod lead, then PST for the final decision *(to confirm)*. HR also decides leave during probation (including long or emergency leave), maternity leave, paternity leave, and possibly leave of absence. |
| Entitlement | Unlimited leave once confirmed |
| Limits | Probation: 1 leave a month, 2 days at a stretch. Notice: leave may extend it. |
| Checked by hand | Team quorum (50% of each role) and black-out dates |

**Specific pain:** quorum, black-out dates and probation limits are all checked outside the tool.

### Map 2: Submit and approve an expense

```mermaid
%%{init: {"htmlLabels": false, "fontFamily": "Arial, Helvetica, sans-serif", "flowchart": {"htmlLabels": false, "padding": 16, "wrappingWidth": 400}}}%%
flowchart LR
    A[Employee creates claim<br/>with receipts] --> B{Finance reviews}
    B -->|Approve| C[Paid out<br/>outside the tool]
    B -->|Send back| D[Employee fixes<br/>and resubmits]
    D --> B
    B -->|Reject| E[Employee is told why]
```

| Rules today | |
|---|---|
| Who approves | Finance *(to confirm)* |
| Categories | Conferences, Home office setup, Learning and development, Micromeet, OKR meet, Software tooling, Term insurance, Trainings (in person), Work in person (WIP), Others |
| Vehicle expenses | Claimed under Others, with their own report |
| Payout | Outside the tool |

**Specific pain:** other people's claims and receipts can be seen.

### Map 3: Update my profile

```mermaid
%%{init: {"htmlLabels": false, "fontFamily": "Arial, Helvetica, sans-serif", "flowchart": {"htmlLabels": false, "padding": 16, "wrappingWidth": 400}}}%%
flowchart LR
    A[Employee opens profile] --> B{Allowed to<br/>change it?}
    B -->|Yes| C[Edits and saves]
    B -->|No| D[Asks HR]
    D --> E[HR updates it]
```

| Rules today | |
|---|---|
| Employee changes | Personal and contact details |
| HR changes | Role, department, location, manager, joining date |

**Specific pain:** colleagues can see more than they should.

[ ] Reviewed
