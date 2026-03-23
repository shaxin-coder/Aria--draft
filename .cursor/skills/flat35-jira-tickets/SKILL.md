---
name: flat35-jira-tickets
description: >-
  Create and manage Jira tickets (Bug, Epic, Story, Task, Internal Test, Stress Test, QA) for the
  FLAT35 project. Use when the user wants to create a bug report, epic, story,
  task, or any Jira issue for FLAT35, or mentions creating tickets, filing bugs,
  planning work, or linking issues for FLAT35.
---

# FLAT35 Jira Ticket Creation

## Project Info

| Project | Key | Usage |
|---------|-----|-------|
| CNTD QA | `CNTDQA` | QA request tickets, Epics, Internal Test, Stress Test |
| Japan Point Platform | `JPP` | Bug tickets |

- **Jira Instance**: jira.rakuten-it.com

## Overall Workflow for FLAT35 QA

The typical FLAT35 QA ticket lifecycle:

```
1. CNTDQA ticket (QA or Stress Test) ← QA request from dev team
       ↓
2. CNTDQA Epic ← created to track all QA work, linked to original CNTDQA ticket
       ↓
3. CNTDQA Internal Test tickets ← created under the Epic for individual QA tasks
       ↓
4. JPP Bug tickets ← filed when bugs are found, linked back to original CNTDQA ticket
```

## Workflow Steps

1. Determine the ticket type the user wants
2. Gather required information based on ticket type (ask the user if missing)
3. Create the ticket in the correct project (`CNTDQA` or `JPP`)
4. Create links between related tickets
5. Return the created ticket key and URL to the user

---

## Ticket Types and Detailed Instructions

### 1. CNTDQA QA Request Ticket (type: QA)

**Project**: `CNTDQA`  
**Issue Type**: `QA`  
**Purpose**: Functional Testing request from dev team

| Field | Required | Notes |
|-------|----------|-------|
| summary | Yes | e.g. `[Point Bitcoin] FT - Feature Name` |
| description | Yes | Use the QA Request Template below |
| priority | No | Default: Major |
| assignee | No | QA member who will handle |
| labels | No | e.g. `["pss-qa"]` |

**QA Request Description Template:**

```
|*Requesting Type*|Functional Testing|
|*Requesting Team*|PSS-QA|
|*Environment*|STG|
|*Contact Person*|<contact_person>|
|*Request Information*|------------------------------------
{*}Description{*}:
<description_of_request>
------------------------------------
{*}Project Plan{*}:
 * {*}Requirements/Spec Duration{*}: ~
 * {*}Dev Duration{*}: ~
 * {*}Dev Resource Count{*}: DEV
 * {*}Dev PIC{*}: <dev_pic>
 * {*}SRE PIC{*}: <sre_pic>
 * {*}QA Proposal Duration{*}: <start_date>~<end_date>
 * {*}QA Proposal Resource{*}: QA
 * {*}Release Duration{*}: <release_date>
------------------------------------
{*}Requirements{*}:
 * {*}PDM PIC{*}: <pdm_pic>
 * {*}Confluence Page{*}: <confluence_link>
 * {*}High Level Request/Scope{*}: <scope_description>|
|{*}Related Jira Tickets{*}{*}{{*}}| |
```

**Example:**

```
jira_create_issue(
  project_key="CNTDQA",
  summary="[Point Bitcoin] FT - Unscheduled Recovery Batch Improvement",
  issue_type="QA",
  description="<use template above>",
  additional_fields={"priority": {"name": "Major"}, "labels": ["pss-qa"]}
)
```

### 2. CNTDQA Stress Test Request Ticket (type: Stress Test)

**Project**: `CNTDQA`  
**Issue Type**: `Stress Test`  
**Purpose**: Performance Testing request from dev team

Same template as QA Request but with `Requesting Type` = `Performance Testing`.

### 3. CNTDQA Epic (type: Epic)

**Project**: `CNTDQA`  
**Issue Type**: `Epic`  
**Purpose**: Track all QA work related to a CNTDQA request ticket

| Field | Required | Notes |
|-------|----------|-------|
| summary | Yes | Format: `Epic for CNTDQA_XXXX - <original ticket title>` |
| description | No | Confluence project URL |
| priority | No | Default: Major |
| assignee | No | QA lead |
| labels | Yes | Copy from context + always include `QA` |

**IMPORTANT**: After creating the Epic, you MUST:
1. Link it to the original CNTDQA ticket using `Relates` link type
2. The title format uses underscore: `Epic for CNTDQA_XXXX` (not hyphen)

**Example:**

```
# Step 1: Create Epic
jira_create_issue(
  project_key="CNTDQA",
  summary="Epic for CNTDQA_4836 - [Point Bitcoin] FT - Unscheduled Recovery Batch Improvement",
  issue_type="Epic",
  description="Project url:\n\nhttps://confluence.rakuten-it.com/confluence/...",
  additional_fields={
    "priority": {"name": "Major"},
    "labels": ["BSG", "BankService", "Batch", "Bitcoin", "QA"]
  }
)

# Step 2: Link Epic to original CNTDQA ticket
jira_create_issue_link(
  link_type="Relates",
  inward_issue_key="CNTDQA-4842",   # the new Epic
  outward_issue_key="CNTDQA-4836"    # the original QA/Stress Test ticket
)
```

### 4. CNTDQA Internal Test (type: Internal Test)

**Project**: `CNTDQA`  
**Issue Type**: `Internal Test`  
**Purpose**: QA member's individual tasks (Test Design, Test Execution, Review, etc.)

| Field | Required | Notes |
|-------|----------|-------|
| summary | Yes | Format: `[Group][Component][Task] -CNTDQA-XXXX:<type> - <original title>` |
| description | No | Test point confluence link, etc. |
| priority | No | Default: Major |
| assignee | Yes | QA member doing the work |
| labels | Yes | Same labels as the Epic |
| Epic Link | Yes | Link to the CNTDQA Epic via `customfield_12810` |
| time tracking | No | Original estimate via `timetracking` field |

**Summary format examples:**
- `[BSG][Batch][Test Design] -CNTDQA-4836:FT - [Point Bitcoin] FT - Unscheduled Recovery Batch Improvement`
- `[BSG][Batch][Test Execution] -CNTDQA-4836:FT - [Point Bitcoin] FT - Unscheduled Recovery Batch Improvement`

**Example:**

```
jira_create_issue(
  project_key="CNTDQA",
  summary="[BSG][Batch][Test Design] -CNTDQA-4836:FT - [Point Bitcoin] FT - Unscheduled Recovery Batch Improvement",
  issue_type="Internal Test",
  assignee="yang.r.liu@rakuten.com",
  description="Test Point: \n\nhttps://confluence.rakuten-it.com/confluence/...",
  additional_fields={
    "priority": {"name": "Major"},
    "labels": ["BSG", "BankService", "Batch", "Bitcoin", "QA"],
    "customfield_12810": "CNTDQA-4842",
    "timetracking": {"originalEstimate": "3h 45m"}
  }
)
```

### 5. JPP Bug Ticket (type: Bug)

**Project**: `JPP`  
**Issue Type**: `Bug`  
**Purpose**: Report bugs found during QA testing

| Field | Required | Default | Notes |
|-------|----------|---------|-------|
| summary | Yes | - | Format: `[CNTDQA-XXXX][flat35][API/UI/Batch]bug description` |
| description | Yes | - | Use Bug Template below |
| priority | Yes | `Major` | Default to Major |
| assignee | No | - | Developer responsible (ask user) |
| QA Assignee | Yes | - | `customfield_35101`: QA member's username |
| labels | Yes | - | MUST include: `flat35`, `QA`, and component label (`API`/`UI`/`Batch`) |
| Why occurred? | Yes | `Other` | `customfield_13609`: `{"value": "Other"}` |
| Category | Yes | `BU-Requirement` | `customfield_16900`: `{"value": "BU-Requirement"}` |
| components | No | - | e.g. `"Front - PointBitCoin"` |

**Summary format**: `[CNTDQA-XXXX][flat35][API/UI/Batch]bug description`
- `CNTDQA-XXXX` = the original CNTDQA request ticket number
- `API/UI/Batch` = choose based on the component where the bug was found

**IMPORTANT**: After creating the Bug, you MUST link it to the original CNTDQA ticket:

```
jira_create_issue_link(
  link_type="Relates",
  inward_issue_key="JPP-XXXXX",     # the new bug
  outward_issue_key="CNTDQA-XXXX"   # the original QA/Stress Test ticket
)
```

**Bug Description Template:**

```
■Precondition 　

<precondition, e.g. Prepare Golden rank user>

■Test Environment

<environment, e.g. STG2>

■ReproduceStep:

Step1. XX

Step2. XX

Step3. XX

 

■Expected result： <expected> Screenshot

 

■Actual result: <actual> Screenshot

 

■Test Data/Account: <test_data>
```

**Example:**

```
# Step 1: Create Bug
jira_create_issue(
  project_key="JPP",
  summary="[CNTDQA-4836][flat35][Batch]Recovery batch fetches tx_time within 5 mins",
  issue_type="Bug",
  description="■Precondition\n\nExisting user with status=processing\n\n■Test Environment\n\nStg2\n\n■ReproduceStep:\n\nStep1. Prepared existing user with status=processing & tx_time=2026-01-26 01:02:00.000 utc\n\nStep2. Run batch at 2026-01-26, 01:03:24 utc\n\n■Expected result：\n\nThe record should NOT be fetched by batch since tx_time is within 5 minutes.\n\n■Actual result:\n\nThe record was processed by batch.\n\n■Test Data/Account: easyId:702384741",
  additional_fields={
    "priority": {"name": "Major"},
    "labels": ["flat35", "QA", "Batch"],
    "customfield_35101": {"name": "yue.a.zhou"},
    "customfield_13609": {"value": "Other"},
    "customfield_16900": {"value": "BU-Requirement"}
  }
)

# Step 2: Link Bug to original CNTDQA ticket
jira_create_issue_link(
  link_type="Relates",
  inward_issue_key="JPP-40327",
  outward_issue_key="CNTDQA-4836"
)
```

---

## Quick Reference: What to Ask the User

### When creating a CNTDQA QA/Stress Test ticket:
- What is the feature/scope?
- Contact person, Dev PIC, SRE PIC, PDM PIC?
- QA duration and release date?
- Confluence page link?

### When creating a CNTDQA Epic:
- Which CNTDQA ticket is it for? (to derive title and link)
- Confluence project URL?
- Labels (group, service, component)?

### When creating Internal Test tickets:
- Which Epic does it belong to?
- What task? (Test Design / Test Execution / Review / etc.)
- Assignee (QA member)?
- Time estimate?

### When creating a JPP Bug:
- Which CNTDQA ticket is it related to?
- Component: API, UI, or Batch?
- Bug description (steps, expected, actual)?
- QA Assignee (QA member's username)?
- Assignee (developer)? (optional)
- Test environment?

---

## Linking Issues

### Link Types Used in FLAT35

| Type | Usage |
|------|-------|
| Relates | Link Epic ↔ CNTDQA ticket, Link Bug ↔ CNTDQA ticket |
| Cloners | Clone Internal Test tickets (e.g. Test Design → Test Execution) |

```
jira_create_issue_link(
  link_type="Relates",
  inward_issue_key="CNTDQA-4842",
  outward_issue_key="CNTDQA-4836"
)
```

## Custom Fields Reference

| Field | ID | Usage |
|-------|----|-------|
| Epic Link | `customfield_12810` | `{"customfield_12810": "CNTDQA-4842"}` |
| Epic Name | `customfield_12811` | Auto-set when creating Epic |
| QA Assignee | `customfield_35101` | `{"customfield_35101": {"name": "username"}}` |
| Why occurred? | `customfield_13609` | `{"customfield_13609": {"value": "Other"}}` |
| Category | `customfield_16900` | `{"customfield_16900": {"value": "BU-Requirement"}}` |
| Story Points | `customfield_13200` | `{"customfield_13200": 5}` |
| Sprint | `customfield_10301` | Managed via board |

## Pre-configured CNTDQA Testing Tickets

These CNTDQA tickets have already been created for FLAT35 testing. Use them when linking Epics, Internal Test tickets, or Bug tickets.

| Testing Type | Ticket Key | URL |
|--------------|------------|-----|
| Operation Testing | `CNTDQA-5115` | https://jira.rakuten-it.com/jira/browse/CNTDQA-5115 |
| Performance Testing | `CNTDQA-5116` | https://jira.rakuten-it.com/jira/browse/CNTDQA-5116 |
| System Testing | `CNTDQA-5117` | https://jira.rakuten-it.com/jira/browse/CNTDQA-5117 |
| Integration Testing | `CNTDQA-5118` | https://jira.rakuten-it.com/jira/browse/CNTDQA-5118 |

When the user mentions one of these testing types, use the corresponding ticket key for linking (e.g., creating an Epic for Performance Testing should link to `CNTDQA-5116`).

---

## Tips

- Always confirm the ticket type with the user if ambiguous
- For CNTDQA tickets, the project key is `CNTDQA`, NOT `FLAT35`
- For Bug tickets, the project key is `JPP`, NOT `FLAT35` or `CNTDQA`
- Bug title MUST follow the format: `[CNTDQA-XXXX][flat35][API/UI/Batch]description`
- Bug labels MUST always include `flat35` and `QA`, plus the component (`API`/`UI`/`Batch`)
- When creating an Epic, always link it back to the original CNTDQA ticket with `Relates`
- When creating a Bug, always link it back to the original CNTDQA ticket with `Relates`
- Priority defaults to `Major` for all ticket types unless specified otherwise
- `Why occurred?` defaults to `Other`, `Category` defaults to `BU-Requirement` for bugs
- Use `jira_get_transitions` to check available status transitions before moving tickets
- Use `jira_search` with JQL `project = CNTDQA` or `project = JPP` to find existing tickets

## Batch Creation Workflow

When the user wants to set up a full QA cycle for a CNTDQA ticket, follow this order:

1. **Create the CNTDQA Epic** → link to original CNTDQA ticket
2. **Create Internal Test tickets** (Test Design, Test Execution, etc.) → link to Epic
3. **Create Bug tickets in JPP** (as bugs are found) → link to original CNTDQA ticket
