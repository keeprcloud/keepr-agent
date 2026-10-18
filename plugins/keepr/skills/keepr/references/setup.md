# Setup — arranging a collection and the rules it runs on

A **setup** is everything about a collection that is not its records: how its
cards look in a list (tile layouts), which columns its tables show, how its
item form is grouped, its printable pages, its saved filters and quick adds,
the **related lists** each item shows (an epic's open work items), its tags, the **rules** that run on its records (automations), its
notifications, and its charts and dashboards. One setup can make new cards
too, and arrange the ones the collection already has.

keepr previews the whole setup before anything changes, and applies it **all
or nothing**. The preview is what the person agrees to — so read it to them.

## When to use it

| the person wants… | use |
| --- | --- |
| a new card, or a field added to one card | `keepr_propose_card` (`references/cards.md`) |
| a list, table, form or page laid out differently | a setup |
| filters or quick adds for everyone | a setup |
| a named list on each item of the items that link to it — an epic's "Open work items" | a setup (related lists) |
| "when X happens, do Y" — stamp a date, notify me, update linked records, make a task every week | a setup (automations) |
| a notification for the collection | a setup (notifications) |
| new cards **and** their layouts, filters and rules together | a setup — one preview, one apply |
| to see what rules run, pause one, or see who changed something | `keepr_automations`, `keepr_set_automation_state`, `keepr_history` (below) |

## Commands and tools

| | MCP tool |
| --- | --- |
| read the collection's cards and its rules | `keepr_schema` |
| read each card's layouts (what a layout entry would replace) | `keepr_schema` with `include_layouts: true` |
| propose a setup | `keepr_propose_setup` |
| apply it | `keepr_apply_setup` with the token and the collection name typed back |
| list rules, a rule's runs | `keepr_automations` (`list`, `runs`) |
| pause one, or turn one on | `keepr_set_automation_state` (`pause`, `turn_on`) |
| who changed what | `keepr_history` |

The apply sends exactly the setup that was previewed, and **checks the plan has
not moved**: if something was added or renamed in the collection since the
preview (so an "adds" would now be a "changes"), nothing is applied and you
propose again. A tool token lasts 30 minutes and is used once.

## The setup document

It is keepr's own format — the same one `references/cards.md` calls a card
blueprint — so there is nothing to translate. Every section is optional:

```jsonc
{
  "cards": [ { "localId": "bug", "name": "Bug", "elements": [ … ] } ],   // new cards (references/cards.md)
  "collection": {
    "cardLayouts":       [ … ],   // tile, table, page, form
    "savedFilters":      [ { "name": "Open bugs", "query": "card = bug and status != done" } ],
    "quickAddTemplates": [ { "name": "New bug", "cardRef": { "ref": "bug" },
                             "defaults": { "status": { "kind": "static", "value": "open" } } } ],
    "relatedLists":      [ … ],   // named lists of linked items on each item of a card — below
    "tags":              [ … ],   // new tags, each optionally applied by a rule — which always arrives paused,
                                  // and the person turns it on in keepr › Settings › Tags
    "automations":       [ … ],   // rules — below
    "notifications":     [ … ],   // the collection's notifications — below
    "charts": [ … ], "dashboards": [ … ], "overview": { "dashboardRef": "…" }
  }
}
```

**New cards** are written as `references/cards.md` teaches — an element's
`isTitle`, `required`, `choices` and other options beside its name, a lookup as
`"lookupCard": "<key>"` — or in keepr's own shape with them under `options`;
keepr's tools put them in place before anything is sent.

**Naming a card.** `{ "ref": "bug" }` is a card this setup makes (its
`localId`); `{ "key": "task" }` is one the collection has (from `schema`);
`{ "globalKey": "person" }` a global card. A rule's `card_id` takes the same.

**Changing what is there.** An entry that matches something the collection
has — a tile or table or form at its tier, or a page layout, filter, quick add,
rule, chart or dashboard by `id` or exact name (a notification by `key`) —
**changes** it rather than adding a second. A dashboard's tile names one of the
setup's charts by `chartRef` (its `localId`) or a chart the collection already
shows by `chartId`. The preview marks every entry `adds`, `CHANGES` (with
what changes, before and after) or `unchanged`; an unchanged one is not
written. An entry says the whole of what it sets: give every key you mean to
keep. A filter keepr stores names an element by its id (`#k7f3q2xa`,
`kql.md`): `keepr_propose_setup` names each one at the end of its preview. Tell the person the element, not the id.

**A filter or condition names each element by its `#id`** from `schema`. A
name works only where keepr reads it on one card — a rule's condition on the
rule's card, as below, or a `card =` beside it (`card = bug` for the card this
setup makes, which has no ids yet). Where more than one element answers to a
name, the preview refuses it, naming each by its label and card; take the
`#id` from `schema` (an element of a sub-collection's card is in that
sub-collection's `schema`, not this one's). If the person has not said which
one they mean, ask them, never pick, and propose again with the `#id` — or, for
any of those cards, the condition once per id (`kql.md`, "When keepr asks
which element").

## Layouts

Each entry: `{ "kind": "tile" | "table" | "form" | "page", "cardRef": …,
"scope": "collection" | "card", "body": … }`. On a card the collection owns,
`scope: "card"` sets it wherever the card is used; by default the layout
belongs to this collection, and `scope: "card"` on a card the collection does
not own is refused, not moved. A form layout is card-tier
only, on a card the collection owns. A page layout also has a `name`.

**Tile** — the card as it appears in the item list. A grid of **6 columns and at
most 3 rows**: say `"cols": 6`, and each row's spans add up to 6 at most; each
element appears once:

```json
{ "kind": "tile", "cardRef": { "key": "task" },
  "body": { "cols": 6,
            "rows": [ [ { "element": "title", "span": 4 }, { "element": "status", "span": 2 } ],
                      [ { "element": "due", "span": 3 }, { "element": "owner", "span": 3 } ] ] } }
```

Every type needs room to be read, and a span narrower than that is refused,
never quietly widened:

| element type | span at least |
| --- | --- |
| short text, email, url, location, a looked-up record | 4 |
| long text, rich text | 4 — alone in the body at 6, it is the tile's paragraph |
| everything else (numbers, dates, choices, a person, a phone, a file's photo…) | 2 |

A tile body with **no `cols`** is read in thirds — the three-column grid tiles
had before — and keepr doubles every span as it writes it (`"span": 1` becomes
2 of 6). So a body written for three columns still works, but always send
`"cols": 6` with spans in sixths: that is how keepr stores every tile once
the owner's migration has run (a setup's preview draws each width out of six,
`title (4/6)`), and an odd width (3 of 6 is half the tile) has no spelling in
thirds. Spans in sixths sent without `cols` are read as thirds and doubled.

The tile's header — its badge, date, Open and ⋯ — is keepr's; the body is
what you arrange. Zero rows is a tile of identity and date alone.

**Table** — the item table's columns, in order: `{ "columns": [ { "element":
"title" }, { "system": "updatedAt" }, … ] }`. System columns: `title`,
`primaryDate`, `updatedAt`, `createdAt`, `sourceCreatedAt`, `tags`, `owner`.

**Form** — the item form's order, groups and field widths. A group's `elements`
holds **entries**: an element's name, or `{ "element": name, "span": …,
"newRow": true }` once the person says how wide a field is or where a row
starts:

```json example
{ "groups": [ { "id": "g1", "title": "Basics",
                "elements": [ "title", { "element": "status", "span": 4 },
                              { "element": "owner", "span": 4, "newRow": true } ] },
              { "id": "g2", "title": "Dates", "elements": ["due"], "collapsed": true } ] }
```

`span` is how many of the form's columns the field takes, or `"auto"`; a bare
name is `auto` with no new row, which is also what every layout saved before
widths existed means. `newRow: true` starts the field on a fresh row. Write an
entry as an object only for a field the person asked to size or to start a row:
the rest stay bare names, so each keeps the width its own kind of data suits.
Do not work out a width from what you know of the data. How wide a field may be
depends on what it holds, and keepr decides: its preview refuses a width the
field cannot take, naming the field and the range, and the next proposal uses
that range. keepr also keeps an entry in one spelling, so a field sent with
`"span": "auto"` may come back as the bare name; it is the same thing. The
preview draws each field with what it sets, `phone (span 6)` or `dob (span 4,
new row)`, and a form that changes only a width shows it under "now:" and
"becomes:". Read those lines to the person; do not describe widths from memory.
A form never hides an element (a required one hidden is a record nobody can
save).

**Page** — paper: a record per sheet (a face sheet, a label) or the records a
query selects, with blocks placed in the sheet's units. Pages are designed
best in keepr's page editor; propose one only from an existing page's body the
person shows you, or for a change the person names precisely (a new name, a
size). The preview shows a page by its subject, size and number of blocks.

## Related lists

A **related list** is a named list, on every item of a card, of the items that
link to that item: an epic's **Open work items** are the work items whose Epic
lookup holds that epic, narrowed to the ones not done. The item view shows a
card's lists after **Related items**, in one switcher, each with its count.
`keepr_schema` lists each card's lists — name, id, the card whose items it
shows, the lookup it goes through, its filter — and which ones a parent card
defines.

```jsonc example
"relatedLists": [
  { "cardRef": { "key": "epic" }, "name": "Open work items",
    "sourceCardRef": { "key": "work-item" }, "via": "epic", "filter": "status != done" },
  { "cardRef": { "key": "epic" }, "name": "All work items",
    "sourceCardRef": { "key": "work-item" }, "via": "epic" },
  { "cardRef": { "key": "procedure" }, "name": "Steps",
    "sourceCardRef": { "key": "step" }, "via": "procedure",
    "sort": { "element": "step_number", "direction": "asc" } }
]
```

- `cardRef` — the card the list is shown on: one this setup makes, or one this
  collection **owns**. A card it uses but does not own (a parent collection's,
  a global one) has its lists changed where it is defined; keepr refuses it here.
- `sourceCardRef` — the card whose items it lists: one of this collection's,
  one of a sub-collection's, or the card itself (sub-tasks). Its child cards
  come too: a list of work items includes bugs when Bug extends Work item.
- `via` — the lookup on that card that holds the item, by name or `#id`. Leave
  it out for any lookup there that points at this card.
- `filter` — written as a person types it, over the listed card's elements;
  keepr stores it by id. `hidden: true` keeps the list without showing it.
- `sort` — the order the list shows its items in: an element of the listed
  card, by name or `#id`, and `direction` `asc` (the default) or `desc`. Leave
  it out for the default order, newest first. Rich text, a file or a repeating
  group has no order, and keepr refuses it. Items that tie keep the default
  order among themselves.

An entry matches one of that card's own lists by its `id` (from the schema) or
its name, and **changes** it; otherwise it is added after the lists already
there. It says the whole list: leave `via`, `filter` or `sort` out and that part goes.
A list a parent card defines is changed on that card. **A setup never removes
a list** — hide it, and the person removes it in keepr (the card's Display tab,
Related lists). keepr says, in the preview, when a name is taken, a lookup does
not point here, or a card would hold more lists than it allows.

## Rules (automations)

A rule says **when** (its trigger) and **what** (its actions):

```jsonc
// Done stamps the date — runs at once
{ "name": "Done stamps the date",
  "trigger": { "type": "item-event", "card_id": { "key": "work-item" }, "events": ["updated"],
               "condition": { "kql": "status = done" } },
  "actions": [ { "type": "set-elements", "elements": { "done-on": "{{today}}" } } ] }

// A release ships its items — changes OTHER records, so it arrives paused
{ "name": "A release ships its items",
  "trigger": { "type": "item-event", "card_id": { "key": "release" }, "events": ["updated"],
               "condition": { "kql": "status = released" } },
  "actions": [ { "type": "update-items",
                 "scope": { "via": "lookup", "card_id": { "key": "work-item" }, "linkElementName": "release" },
                 "set": { "status": "deployed" } } ] }

// Tell me when work waits on me — notifies, so it arrives paused
{ "name": "Tell me when work waits on me",
  "trigger": { "type": "item-event", "card_id": { "key": "work-item" }, "events": ["updated"],
               "condition": { "kql": "status = needs-input or status = in-review" } },
  "actions": [ { "type": "notify", "channels": ["inApp"], "audience": { "type": "role", "minRole": "manage" },
                 "title": "Waiting on you" } ] }

// Nothing started this week — runs on a schedule, so it arrives paused
{ "name": "Nothing started this week", "kind": "expected-item",
  "trigger": { "type": "schedule", "schedule": { "every": 1, "unit": "day", "time": "09:00", "timezone": "America/Los_Angeles" } },
  "check": { "card_id": { "key": "work-item" }, "window": { "type": "rolling", "minutes": 10080 },
             "condition": { "kql": "status = in-progress" } },
  "actions": [ { "type": "notify", "channels": ["inApp"], "audience": { "type": "role", "minRole": "manage" },
                 "title": "No work started this week" } ] }
```

Kinds: `rule` (the default), `threshold` (a value crossing a bound),
`expected-item` (a record that should have been **added** by a time: it
notifies when no record of its card, matching its condition, was created in
the window — it does not notice a record that stopped changing) and
`generate-items` (make a record per source record, on a schedule). Never send
`enabled` — keepr decides it.

**How a rule arrives.** A rule that only sets values on the record that set it
off **runs as soon as it is applied**. A rule that **notifies anyone, changes
other records, creates records, or runs on a schedule arrives PAUSED**, and so
does every notification — whoever applies the setup. It stays paused, marked
as waiting for the person, until **they** turn it on in keepr. keepr refuses an
assistant or an API key that tries, and that is the design. Changing what a running rule of that kind does — who it tells, what it
says, which records it changes — pauses it again; a rename does not.

**Reading the preview aloud.** For each rule the preview gives keepr's own
sentence for what it does, how many records it would touch today, who a
notification would reach and what it would say, its next runs, and whether it
runs at once or arrives paused, with why. Tell the person all of it, in those
words. Then, for every rule that arrives paused, say exactly:

> Only you can turn this on, in keepr › Settings › Automations.

(For a notification: **in keepr › Settings › Notifications.** For an automatic
tag's rule: **in keepr › Settings › Tags.**)

**Never promise that a paused rule will run**, or that it is "set up and
working". It is set up; it is not on. If the person says "turn it on", call
`keepr_set_automation_state` with `turn_on` once — keepr answers that it is theirs to do,
and you say the sentence above. Do not retry, and do not look for another way.

A rule naming a card the setup makes cannot be previewed until the card
exists: its step says so, and it is judged when the setup is applied (a refusal
then leaves nothing behind).

## Notifications

A collection notification is a definition everyone in the collection may
receive: `{ "key": "work-waiting", "name": "Work is waiting", "severity":
"info", "category": "items", "trigger": { … }, "audience": { … }, "channels":
[], "template": { "title": "…" } }`. **Always send `channels`**: `[]` is the
inbox only, and leaving it out means email — it leaves keepr. A key keepr's own notifications use is refused —
pick another. Every notification arrives paused.

## Reading and switching rules

`keepr_automations` lists a collection's rules and
notifications: what each does, whether it is **ON**, **PAUSED**, or **WAITING**
for the person, and who made it — a person, an API key, an assistant. It reads
a rule's recent runs, and changes nothing. `keepr_set_automation_state`
**pauses** one by id or exact name. Pausing is
always open; turning on is the person's when keepr says so.

`keepr_history` reads who changed what and when — an
item's, a card's, or the collection's. It reads only.

## What is not here

**Deleting, sharing, undoing and keys are the person's, in keepr — never from
here.** No tool deletes a card, a rule, a filter, a layout or a record; none
shares a collection or sends an invitation or share link; none reverts a
change or undoes an import; none makes or changes API keys. A rule that is no
longer wanted is **paused** here, and deleted by the person in keepr. Say so
plainly when asked, and point them to the place.
