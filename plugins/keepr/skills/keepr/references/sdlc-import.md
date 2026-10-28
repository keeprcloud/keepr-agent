# Moving in from Jira or Azure DevOps

A team that tracks its work in Jira or Azure DevOps hands you the CSV the
product exports, and their history lands in a **Software delivery** collection
with its links intact and the source key kept. This is an import and an export,
never a sync: nothing here keeps the two systems in step afterwards.

`adding.md` is the loop (schema, rows, dry run, commit, report) and every rule in
it holds. This chapter is what is specific to these two files: which column goes
where, the two stages that make links resolve, and what to ask the person. Read
the collection's schema first; the element names below are the Software delivery
core's (Work item, Epic, Sprint), and a team that renamed one tells you in the schema.

Work with the person, not around them. **Never invent**: not an email, not a date,
not a severity, not a tag. Where the file lacks something keepr asks for, ask
once (the list is in "Ask before you build the rows"), say what you did with the
answer, and put it in the report.

## Which collection

The person names a Software delivery collection, or has none yet (they install
it from Discover first; you never build it). Check the schema has the cards
`work-item`, `epic` and `sprint`. A collection without them is not this one:
stop and say so.

## The two dialects

| | Jira | Azure DevOps |
| --- | --- | --- |
| The file | Filter results → Export → CSV (all fields) | A query's results → Export to CSV |
| Source key | `Issue key` (`APP-123`) | `ID` |
| `source.system` | `jira-csv` | `ado-csv` |
| Repeated headers | `Sprint`, `Labels`, the link columns repeat: read every cell of every copy | none |
| Dates | `02/Sep/26 10:00 AM` (day, month name, two-digit year; the site's setting may differ: read the file and ask if two readings fit) | `2026-09-02T10:00:00Z` |
| People | a **display name** (the file has no email) | `Name <email>` |
| State history | not in the file | `Activated Date`, `Closed Date` only |
| Links | columns for each link type | `Parent` only; the rest need a second file (below) |

Use the day the file shows. Send dates as `YYYY-MM-DD`.

**Azure DevOps links.** A plain query CSV carries `Parent` and nothing else. Predecessor and
duplicate links come from a direct-links query, which the person can export as a second
file. Ask for it; read its `Link Type`, `Source ID` and `Target ID` columns. A `Predecessor`
row says the target must finish first (the source is blocked by it); a `Duplicate Of`
row says the source duplicates the target. With no second file, import without those
links and say that in the report.

## The columns

Read the header row and map by these names. A column that is not here is not
imported: list it in the report rather than guessing. An element the file has
no column for is left out of the row (adding.md).

| Element | Jira column | Azure DevOps column |
| --- | --- | --- |
| `title` | `Summary` | `Title` |
| `external-key` | `Issue key` | `ID` |
| `number` | not imported: keepr issues its own number | not imported: keepr issues its own number |
| `type` | `Issue Type` | `Work Item Type` |
| `status` | `Status`, with `Status Category` as the fallback | `State` |
| `status-category` | not sent: keepr works it out from the status | not sent: keepr works it out from the status |
| `resolution` | `Resolution` | `Resolved Reason` |
| `priority` | `Priority` | `Priority` |
| `severity` | `Custom field (Severity)` if the file has it | `Severity` |
| `assignee` | `Assignee` | `Assigned To` |
| `reporter` | `Reporter` | `Created By` |
| `description` | `Description` | `Description` |
| `acceptance-criteria` | `Custom field (Acceptance criteria)` | `Acceptance Criteria` |
| `repro` | the `Description` of a bug | `Repro Steps` |
| `points` | `Custom field (Story point estimate)` or `Story Points` | `Story Points`, else `Effort`, else `Size` |
| `rank` | `Custom field (Rank)` | `Backlog Priority` |
| `sprint` | the last `Sprint` cell | the last part of `Iteration Path` |
| `area` | `Component/s` | `Area Path` |
| `tags` | `Labels` | `Tags` |
| `epic` | `Parent` when that issue is an Epic, or `Custom field (Epic Link)` | `Parent` when that item is an Epic |
| `parent` | `Parent` when that issue is not an Epic | `Parent` when that item is not an Epic |
| `blocked-by` | `Inward issue link (Blocks)` | `Predecessor` rows of the links file |
| `duplicate-of` | `Outward issue link (Duplicate)` | `Duplicate Of` rows of the links file |
| `blocked` | `Custom field (Flagged)` | `Blocked` |
| `due` | `Due date` | `Due Date` |
| `opened-on` | `Created` | `Created Date` |
| `started-on` | not in the file: leave it out | `Activated Date` |
| `done-on` | `Resolved` | `Closed Date`, else `Resolved Date` |
| `found-in` | `Affects Version/s` (a bug) | `Found In` (a bug) |

Notes that the table cannot carry:

- **Issue Type Epic / Work Item Type Epic** is a row of the `epic` card, not a work item:
  `title`, `status` (proposed, active, done, removed, by the same status rules),
  `owner` from the assignee, `priority`, `description` and `target` from the due date.
- **Rank.** Azure DevOps' `Backlog Priority` is a number: send it. Jira's rank is a
  sort string (`0|i00007:`), not a number: sort the rows by it and send 100, 200, 300 in
  that order. Say which you did.
- **Sprint.** A sprint is a row of the `sprint` card. The file has its name, never its
  dates or whether it is over. Ask for each name's start, end and state (planned,
  active, closed). A sprint with no answer is left off its items and named in the
  report; the items still import. An item that sat in several Jira sprints goes in the
  last; the earlier ones are named in the report as history keepr does not keep.
- **Labels and tags.** Tags are the collection's own and keepr never makes one from a
  row. Send a label only when the schema lists a tag of that name; otherwise list the
  labels in the report so the person can add the tags and ask you to run the file again.
- **Not imported**, and said so in the report: comments, attachments, links to the
  product's own pages (`external-url`, because the file does not carry the site's
  address), time tracking, and any custom field not in the table.
- `details` is kept for one thing: the original word of a type you had to map (below).

## People

Match by **email**, never by name. Azure DevOps gives it in `Assigned To`. Jira's file
has display names only: ask the person for the email of each distinct name, once, in
one list. Then find each account by its exact email (references/api.md, "Finding a
person by email"); send the account's id.

A person with no email, or an email with no keepr account, is **left unassigned**
and named in the report, with the reason. Do not try nearby spellings, do not
send a name or an email in the element, and do not make a guess from a
colleague's address. Say that anyone they invite later can be assigned by
running the same file again.

With only the connector's tools (no way to call the search), no one can be matched
this way: leave every assignee and reporter out, list the names and emails, and say
that assigning them is a step in keepr.

## Status, resolution, type and priority

**Status.** Map the product's state to one of Work item's six statuses by the
table. A state the table does not know is mapped through its **category**, and
the report names every such row with the state, the category and the status you chose.

| Status | Jira status | Azure DevOps state |
| --- | --- | --- |
| backlog | To Do, Backlog, Open | New, Proposed |
| ready | Ready, Selected for Development | Approved |
| in-progress | In Progress | Active |
| in-review | In Review, Code Review | Resolved |
| done | Done, Closed, Resolved | Closed, Done |
| removed | (by resolution, below) | Removed |

| Category | Jira `Status Category` | Azure DevOps states (the file has no category column) | Status |
| --- | --- | --- | --- |
| to-do | To Do | To Do, Open | backlog |
| in-progress | In Progress | Design, Doing, In Progress, Committed, Test | in-progress |
| done | Done | (Closed, Done) | done |

An Azure DevOps state that is in neither table: ask which category it belongs to.
Never leave it to chance.

**Resolution.** Done, Fixed, Complete, Completed → completed; Won't Do, Won't Fix,
As Designed, Deferred, Obsolete → wont-do; Duplicate → duplicate; Cannot Reproduce,
Incomplete → cannot-reproduce. A finished item with no resolution is completed;
a removed one is wont-do. A resolution you do not know: ask. **Work that ended
without being delivered (wont-do, duplicate, cannot-reproduce) is Removed, not
Done**, so it leaves the totals; say so once.

**Type.** Story, User Story, Product Backlog Item → story; Task, Sub-task, Issue → task;
Bug → bug; Spike → spike; Epic → the epic card. A type the card does not have
(Improvement, New Feature, Feature, Change Request) is **reported, not dropped**: ask
which of the four it should be, send that, and write the original word in `details`
(`Jira type: Improvement`) so it is not lost. If they would rather have the type
itself, that is a card change (`cards.md`) they decide first.

**Priority.** Jira: Highest → critical, High → high, Medium → medium, Low and Lowest → low.
Azure DevOps: 1 → critical, 2 → high, 3 → medium, 4 → low.

## Ask before you build the rows

Ask each of these once, in one message, and only for the ones the file needs.

1. **Emails** for the Jira names (above).
2. **Sprint dates and state** for each sprint name.
3. **A type you cannot map**, and **a state or resolution you do not know**.
4. **Severity of bugs.** Neither product's file has a severity keepr can use for Jira, and keepr
   asks for one on a bug past the backlog. Offer "use the priority" and wait for the answer.
5. **Whether to continue** when a gate would be filled with a stated line (next section).

## The gates apply to history

keepr asks for acceptance criteria from Ready on, severity and steps to reproduce on a bug
past the backlog, a resolution on Done or Removed, a reason on a blocked item, and the
original on a duplicate. A strict import is refused row by row when one is missing; never
switch a gate off.

When the file has no value, write a plain line that says so and report each one:
`Imported from Jira. No acceptance criteria were recorded.` (or `No steps to reproduce`),
and `Flagged in Jira` for a blocked reason. A bug's steps come from its description when
there is no separate column. Do not write anything that reads as the team's own words.

## Two stages

A link points at a row that must already exist, and some links point forward in the file
or in a circle (A blocks B, B blocks A). So the rows go in two stages, each its own
ingest (adding.md), all with `source.system` as above and the product's key as
`source.externalId`:

1. **Stage 1.** The sprints, then the epics, then every work item that is not a
   duplicate, with `epic` and `sprint` as `{"$ref": "<externalId>"}`, and **no** `parent`
   or `blocked-by`. A sprint's external id is `sprint:<name>`. Mark `blocked` here.
2. **Stage 2.** First the duplicates, whole rows, each with `duplicate-of` set to
   `{"$ref": "<key>"}` (keepr requires it when the resolution is duplicate, which is why
   they wait for the originals; if an original is itself a duplicate, write it first).
   Then one small row per item that has a `parent` or `blocked-by`, carrying those two
   elements and nothing else (an upsert merges: what is stored stays).

Each stage: dry run, read it, show the person, commit. A dry run writes nothing, so a
stage 2 dry run needs stage 1 committed (or its rows named in `wouldCreate`, api.md).
A link to a key that is not in the file is not sent: it is named in the report.

## Re-running

`external-key` holds the product's key and is unique in the collection; the source
id is the same key. Run the same file again and every row is `skipped`; a changed
file updates only what changed. A key that another source already holds (the same key
imported from the other product, or typed by hand) fails its row: tell the person, do not
force it.

## The report

After the last commit, say, in plain words and with the item titles or keys:

- how many sprints, epics and work items were created, updated and skipped, and the collection's link;
- **people left unassigned**, each with the reason (no email, or no keepr account);
- **statuses mapped through a category**, and **types mapped by an answer**;
- every **line you wrote to satisfy a gate**, by key and element;
- **labels** left out for want of a tag, **sprints** left off for want of dates, **sprint history** not kept;
- **links** left out (a key not in the file, a links file not given);
- what was **not imported** (comments, attachments, page links).

## Export, going back out

keepr's export (the table's Excel export, or the person's own export from the collection
menu) names each column by the element's label. A Jira or an Azure DevOps importer maps
a column on its own when the header is the product's field name; the others it maps by
hand, in Jira on the importer's mapping step, in Azure DevOps by the header. Nothing is
sent back to either product, and the person does the import there.

| Export header | Jira importer | Azure DevOps importer |
| --- | --- | --- |
| `Title` | pick `Summary` | maps as `Title` |
| `Type` | pick `Issue Type` | rename to `Work Item Type` |
| `Status` | maps as `Status` | rename to `State` |
| `Status category` | not imported (Jira works it out) | not imported |
| `Resolution` | maps as `Resolution` | rename to `Resolved Reason` |
| `Priority` | maps as `Priority` | values are 1 to 4 there: rename and renumber |
| `Assignee` | maps as `Assignee` | rename to `Assigned To` |
| `Reporter` | maps as `Reporter` | rename to `Created By` |
| `Description` | maps as `Description` | maps as `Description` |
| `Story points` | pick the story point field | rename to `Story Points` |
| `Sprint` | maps as `Sprint` | rename to `Iteration Path` |
| `External key` | pick the issue key, or leave it | leave it |

Where the table says rename, offer to write a copy of the exported file with only the
header row changed, and say which headers you changed. Statuses go out as keepr's six
(Backlog, Ready, In progress, In review, Done, Removed): the product's workflow has to
have, or map, each. Whether a given importer accepts a file without the rename is the
importer's rule, not keepr's; this table is by name, from each product's documentation.
