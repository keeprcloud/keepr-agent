# Cards — creating and changing record types

A card is schema. Creating one changes the shape of the user's data; changing
one changes the shape of data that already exists. Neither is done quietly,
and neither is done from memory: **read the card first**, propose, show the
user exactly what the server says will happen, and apply only what they saw.

Both need **Can change cards** on the key (or the connection) and `manage` on
the collection. Without it keepr refuses, naming what is missing; keepr's
tools print the fix — *create or edit a key with Can change cards turned
on* — and so should you.

**Moving items from one card to another is not this chapter.** A card change
edits what a card *is*. An item that sits on the wrong card is moved with
`keepr_change_card`, described in `references/adding.md` ("Moving items to
another card"): a dry run, the person's yes, then the move with keepr's token.

## Commands and tools

| | MCP tool |
| --- | --- |
| read the card | `keepr_schema` with `card` |
| propose a new card | `keepr_propose_card` |
| create it | `keepr_apply_card` with the proposal token, the collection name typed back, and the same `collection` and `cards` (with any `filters`, `layouts`, `tags`) you proposed |
| propose a change | `keepr_propose_card` with a `change` argument |
| apply the change | `keepr_apply_card` with the token, the collection name typed back, the same `collection` and `change` you proposed, and — for a destructive change — the card name typed back too |
| look for a ready-made bundle first | `keepr_discover` (`search`, `read`, `preview`), then `keepr_add_package` |

The proposal token is keepr's own: keepr made it with the preview, and keepr
uses it up on the apply. It is good for 30 minutes, once, for this person and
this connection, and only for the exact change that was previewed. Nothing is
kept on your side, so the apply sends **the same spec again** — unchanged — and
keepr refuses anything that differs, or a card that changed in between. When
keepr refuses the token, nothing was changed: propose again and show the user
the fresh preview.

## Discover first

When the person wants to start keeping **a new kind of record** — something
no card of theirs holds yet — search **Discover** before you propose a card or
a setup for it. Discover is keepr's catalog of ready-made **bundles**. A bundle
holds everything one kind of record needs, already worked out: its cards and
element sets, and whatever else its author put in — saved lists, quick adds,
layouts, charts, dashboards, rules, notifications, forms, tags, examples, and
sometimes its collection's own settings. One card, or every card a collection
needs. A bundle that fits gives the person all of it in one step; a card built
from scratch is for when nothing fits.

A field added to a card they already have is not a new kind of record: change
that card (below), with no search.

Three words the person reads, and you use too:

- **Add-ons**: optional parts of a bundle (a Fuel log for Vehicle care). The
  core always comes; each add-on comes when it is ticked.
- **Scenarios**: presets for a situation ("Family cars", "A small fleet"). A
  scenario ticks its own add-ons and examples. A scenario is not a second
  bundle: one bundle, several ways to start. (keepr used to call them
  *listings*; the tools still accept `listing` and say `scenario`.)
- **What it holds, by type**: every thing a bundle would add, grouped — cards,
  saved lists, charts, rules and the rest — each with a local id. The person
  can take or leave a whole type, or any one thing in it.

1. **Search** with `keepr_discover`, `action: "search"`, in the person's own
   words for the kind of record ("vehicle maintenance", "recipes"), not in
   card terms. A row that matched one scenario says so ("Family cars, a
   scenario of Vehicle care"). `collections: true` also finds the published
   collections they can follow (a reference list kept by its publisher), which
   they follow rather than add. A published collection whose cards extend a
   base card is a **catalog** of that base card's records: its row says so
   ("A catalog of Product models · 207 records"), and `base` (a base card's key,
   "product-model") lists only the catalogs of that base card. `kind` still narrows by shape (one card, one
   that brings collection settings, any other) but is going away: leave it out
   unless they ask for that shape.
2. **Read** the likely ones with `action: "read"`: every card and element, the
   add-ons, the scenarios, what each rule does, and **WHAT IT HOLDS, BY TYPE**:
   each type's count, how it arrives (made as it is; rules and notifications
   paused; forms prepared and off; examples only when ticked; setup offered
   afterwards) and each thing's local id, add-on and what it needs. Judge the
   fit from that, not from the name. `scenario` reads it as one scenario ticks
   it.
3. **Show at most three candidates**, each with one line on how it fits and one
   on what it lacks for this person ("Vehicle care: services, costs and
   reminders for each car. It has nowhere for tyre pressures."). Say which one
   you would pick, and which scenario fits them when it has scenarios.
   `keepr_propose_card` for a new card also searches Discover on its own, and
   when up to three packages are a strong match its answer lists them under
   `DISCOVER:` (and as `discover`). That is a backstop for an assistant that
   skipped this step, not a replacement for it: show those to the person
   before they apply the proposal. The proposal itself is unchanged.
4. **Build from scratch only when none fits, or the person asks you to.** Say
   in one sentence that Discover had nothing that fits, then propose the card
   as below.
5. **Preview the one they pick**, when your connection can change cards:
   `action: "preview"` with the bundle's `key`, the `collection` it goes into
   (by name or id) or a `new_collection` name, the `scenario` they chose, the
   `add_ons` they want (leave it out for the ones the scenario ticks), and
   `examples: true` only if they want the example records. You always name the
   target the person chose; keepr never picks one. It adds nothing.

   **Taking only some of it.** When the person wants less than the whole
   bundle ("not the reminders", "skip the dashboards", "only the Vehicle
   card"), send `select` with the types and local ids `read` listed:
   `{ "types": { "automation": false }, "things": { "fl3": false } }` (a
   thing overrides its type; the examples are the thing `ex`). The preview's
   **NOT ADDED** lists what the selection leaves out, and what waits because
   it reaches something left out (a list over a field that looks up a card they skipped). Something
   they keep may need something they left out — a filter needs its card, a
   dashboard its charts. keepr then refuses the preview
   (`selection_incomplete`) and lists each one: *"Due soon" needs "Vehicle"*.
   **keepr never widens a selection, and neither do you**: ask the person,
   for each line, whether to take the thing it needs too or to leave out the
   one that needs it, and preview again with that `select`.

   A new collection, the examples, or a bundle that brings settings (filters,
   charts, tags, rules, notifications, forms) need a connection that can also
   write records; a connection that only changes cards hands over the link for
   those — or previews a selection of the cards alone. A **new
   sub-collection** (a collection inside one of theirs: `new_collection` with
   `parent`) is the person's to make, signed in to keepr: keepr refuses it to
   an assistant (`sub_collection_session_only`), and the answer names the
   parent and hands over the link. Offer the parent itself, another collection,
   or a new top-level one instead. A bundle that brings its collection's own
   settings (the collection's name, colour, filter chips, whole tag list,
   Overview and the records it starts with) applies them only to a new
   collection: into one they already have, the preview says those are left
   out and the rest is added, so ask which they want when it matters. Tell
   them in a few lines what it would add, what it leaves out, what it would
   rename because the collection already has one, and what arrives paused or
   off until they turn it on. Its answer ends with a preview token and the
   install link with those choices filled in (the link carries the scenario,
   add-ons and examples, not the selection: they untick those again there). A
   health package's preview says the person adds it: an assistant never does.

   **A bundle that can use a card they already have.** Some bundles' cards
   (a Staff member, a Resident) can use a card the collection already holds
   instead of adding a second one. The preview's **CARDS YOU ALREADY HAVE**
   says, for each, what it would use now — one of their cards, the bundle's
   own card, or the card a package it needs brings — and the other cards that
   could fill it, with what each would change. keepr's own choice never
   changes a card of theirs. To use a different card that fits as it is, send
   the `BINDING to send` the preview printed for it in `bindings`, and preview
   again; `{ "use": "own" }` keeps the bundle's own card. **You never approve
   a change to their card** (an element, a value, a conversion or a list added
   to it): when the preview says `binding_needs_person`, or nothing fits
   without a change, it gives no token — say what would change (WOULD CHANGE
   YOUR CARDS) and hand over the link, where they choose the card and approve
   each change themselves.

   **A bundle that needs other packages.** The preview's **PACKAGES THIS
   NEEDS** lists the packages it builds on (added first), and each one's
   add-ons. The first preview of such a bundle is refused
   `dependencies_unconfirmed`: nothing comes unseen. Tell them which packages
   it would add and ask which add-ons of each they want (a *needed* one comes
   anyway), then preview again with `dependencies`:
   `[{ "package": "core-staff", "install": true, "add_ons": ["training"] }]`,
   one entry per package listed. One already in the collection is skipped. A
   selection is the asked bundle's alone: a package it needs comes whole.
6. **Add it once they say yes — or hand over the link.** Ask plainly: this
   bundle, this scenario, these add-ons, leaving out these things, into this
   collection (or a new one named so), with or without the examples — and,
   when the preview lists them, these packages it needs with these add-ons,
   using these cards of theirs? Only after a clear yes to all of it, call
   `keepr_add_package` with the preview's `preview_token` and
   `confirm_collection_name`, the collection's name exactly as the preview
   said it (your client asks the person before it runs, every time). The
   token carries exactly what was previewed — the scenario and the selection
   with the rest — and `keepr_add_package` takes nothing that could change it.
   Anything they change means a new preview first: the token is for exactly
   the choices previewed, it lasts 30 minutes, and it is spent by the first
   install; a card of theirs or a bundle in the collection changing in between
   means a new preview too, and keepr's refusal says so. Then say what was
   added, what was left out, and what arrived paused or off — rules,
   notifications and forms wait for them in keepr, and you may turn on only a
   rule that changes nothing beyond its own record. If they would rather add it
   themselves, or the package is a health package (it asks for their own
   consent), give them the link with three steps: open it; check the
   collection it goes into, the scenario and the add-ons ticked, and untick
   anything they leave out; press **Add to collection**. If the install's
   answer is lost, read the collection before anything else: it may be there.
7. **Extend a good fit afterwards.** What a good fit lacks becomes a card
   change once it is installed (`keepr_propose_card` with a `change`, on the
   card the package made), not a second card beside it. Read the collection's
   schema again first: the card keys are the ones keepr gave on install.

## Creating a card

`keepr_propose_card` with the card spec as `cards` writes nothing and returns
keepr's preview and a token; `keepr_apply_card` with that token creates it.

Propose first, show the user the elements and their types, and only apply
once they agree. `references/recipes.md` has worked card specs (a reading log,
an expense tracker, a linked contacts + notes pair) to start from. The data
types an element can have, and the options each takes, are keepr's: the
contract (`elementTypes`) lists every type, and keepr's
preview refuses an option it does not know, by name.

The whole spec goes to keepr as **one card blueprint** (keepr 2.1): keepr checks
it without writing anything — the proposal step prints its summary, or the
problems, one per line, each with where it is (`cards[1].key`) and keepr's
reason — and
Applying creates it **all or nothing**: parents first, then the lookups and
rollups once every card exists. If anything is refused part-way, keepr removes
what that apply had made and says so. So a card can look up **another card in
the same spec, or itself** (`"lookupCard": "task"` on the Task card — "depends
on"), and a rollup can read from one (`"sourceCard"` beside a `drivenFrom`),
none of which needs an id. A lookup to a **global card** the collection does
not have yet (`"lookupCard": "person"`) adds that card to the collection —
the proposal says so, and the key needs write as well. Keys are stored as
slugs (`Work_Item` becomes `work-item`); the proposal shows the key keepr will
use.

A spec is one card, a list of them, or `{ "cards": [...], "filters": [...],
"layouts": [...] }` — filters saved for the whole collection
(`{ "name": "Ready", "query": "card = task and status = ready" }`, the key needs
write as well) and table layouts for cards in the spec. Write a filter's cards
by key and its choices by value or label, as a person types them: keepr saves
each one by id and value once the spec's cards exist. Name an element on its
card, as there, or by its `#id` from `schema`: a name more than one element
answers to is refused with each one's `#id`, and you ask the person which
(`kql.md`, "When keepr asks which element"). A record goes in by its
id, never its title — `owner = Jane` is refused, with the matching records,
and the whole apply is undone; so is a key nothing in the
collection has. Table layouts are for cards in the spec
(`{ "card": "task", "columns": ["kpr", "title", "status"] }`). A card also
carries `icon` and `color` (below), and may carry `displayTemplate`
(`"{{kpr}} {{title}}"`), `elementSets` (set keys) and `options` (`primaryDate`…).

### Every new card gets an icon and a color

A card's icon on its color is its badge: every item of the card wears it in
lists, lookups, pickers and search. **When you create a card, always give it
both**, chosen for what the card is for, and never guess an icon's name — keepr
accepts only the icons in its own catalog, by their exact names (upper case
with underscores, like `BOOK_OPEN`).

1. **Look the icons up, once for the whole spec**, by what each card is for:
   `keepr_schema` with `icons: ["book", "wine", "dog"]` lists the names keepr
   accepts for each word, best first, and the colors. Take a name from the
   answer. When a word finds nothing, try another: the card's name, then what
   its items are.
2. **Pick the color from the same answer**: send a hue's key (`emerald`) and
   keepr stores its hex, or send a hex like `#2f9e44`. Give the cards of one
   collection different colors, so a mixed list is easy to scan.
3. **Say what you picked, card by card**, when you show the proposal: *"Book
   gets the BOOK_OPEN icon on Emerald; Wine gets WINE on Deep red."*

keepr's proposal tool fills a card the spec left without an icon or a color
(an empty one counts as left out) from the catalog itself, and says so ("ICONS
AND COLORS picked…"): read that out like your own picks, and pass `icon` /
`color` when its pick does not fit.
A name keepr does not have is refused before anything is written, with up to
five close matches — choose one of those, never another guess.

**Changing the icons of cards that exist** is a card change like any other
(`change` with `icon` and `color`, one card at a time, each previewed). Look
every new name up first and send only names the catalog listed. Then tell the
person, per card, the icon it had and the one it gets. A card that already
holds a name keepr does not know keeps saving other changes; only a new icon
is checked.

A spec may also bring **new tags** — `"tags": [{ "name": "Work" }, { "name":
"Late", "parent": "Work", "rule": { "card": "task", "where": "status = open" }
}]` (`tags` on `keepr_propose_card`). A tag with a `rule` is applied
automatically to the items of that card the rule holds for — but it arrives
**paused**: nothing is tagged until the person resumes it in keepr (Settings →
Tags), and the proposal and the apply say so, with how many items it would tag.
Say that to the user in those words. `parent` names another tag in the spec;
filters name tags by name. Only tags the user asked for; the key needs write as
well.

One card, for example:

```json example
{ "name": "Plant", "key": "plant",
  "elements": [
    { "name": "species", "label": "Species", "dataType": "text-small", "isTitle": true, "required": true },
    { "name": "room", "label": "Room", "dataType": "choice",
      "choices": [ { "value": "kitchen", "label": "Kitchen" }, { "value": "study", "label": "Study" } ] },
    { "name": "water-every", "label": "Water every", "dataType": "integer", "help": "days" },
    { "name": "last-watered", "label": "Last watered", "dataType": "date" } ] }
```

Options go beside the element's name in the spec — whatever keepr's schema
shows on an element like it (`allowMultiple` for "Allow multiple", say). An
option the element's type does not take is refused by the preview, by name;
read the refusal rather than guessing another.

**A date holds one value.** A date element, with a time or without, never
takes `allowMultiple`, and a list sent to one is refused, naming it. For several
dates on one record (doses, visits, nights away), add a repeating group with a
date column: each entry is one date, with room for what goes with it. For a
span, use two date elements, a start and an end (`rangeEnd` on the start). An
element that already holds several dates keeps them until keepr cuts each to
its earliest; leave its `allowMultiple` exactly as the schema shows it, and
never add it to another.

**A change to the shape of stored values waits for an answer.** Turning
"Allow multiple" on or off on a card whose items hold values (or another
change that would turn single values into lists, or lists into single values)
is refused until the change says what to do with them. keepr's refusal says
which answer it waits for, where it goes in the element's options, how many
items are affected, and a query that lists them. Tell the person what each
answer does to their data in those words. When the answer keeps one value and
drops the rest, ask the person before you send it; never pick it for them.

Only what the user described goes in. Do not add "useful" elements they did
not ask for; ask.

A card can **inherit** another card's elements — "a Bug is a Work item plus a
severity". Name the parent by key: `parentCardKey` on `keepr_propose_card`.
It may be a card in
the same spec (anywhere in it — keepr creates parents first), a card the
collection already has, or a global card. A parent that names nothing is
refused before anything is created — never created without its parent. An
item cannot move to another card later, so settle the parents before any rows
are written.

## A repeating group on a card

An element can be a **repeating group**: a list of entries inside each item,
every entry with its own columns (a recipe's ingredients, an invoice's lines).
Propose it like any element, with the columns and limits under `options`:

```json example
{
  "name": "Recipe",
  "elements": [
    { "name": "title", "dataType": "text-small", "options": { "isTitle": true } },
    { "name": "ingredient", "label": "Ingredient", "dataType": "repeating-group",
      "options": {
        "minEntries": 1,
        "sections": true,
        "columns": [
          { "name": "name", "label": "Name", "dataType": "text-small", "options": { "required": true } },
          { "name": "amount", "label": "Amount", "dataType": "measurement", "options": { "measure": "volume", "measures": ["volume", "mass", "count"], "defaultUnit": "cup" } },
          { "name": "note", "label": "Note", "dataType": "text-small" }
        ] } }
  ]
}
```

- **Ask the person** what the list is, which columns each entry has and what
  they hold, and whether it needs a minimum or section headings. Leave
  the maximum out unless they name one: keepr has a default and a ceiling, and
  `keepr_schema` shows what the card ends up with. Never state either from
  memory.
- **A measurement in several measures.** A recipe's amount is 2 cups on one
  line, 250 g on the next and 3 eggs on another, so the example's `amount`
  lists every measure it allows in `measures`, its own `measure` first (at most
  five). Most measurements are one measure: add `measures` only when the
  person's values really differ in kind. Such an element takes no `min`, `max`
  or display rules, and taking a measure away later asks what to do with the
  values in it.
- **Units of their own.** keepr already counts in pinch, dash, handful, clove,
  slice, can, jar, stick, bunch, sprig, head, bale, flake, crate, carton, sack,
  tray, skein and piece (and each, dozen, box, pack and more): use those first.
  For a unit keepr has no name for, a measurement element lists its own in
  `ownUnits`, each `{ "singular": "smidgen", "plural": "smidgens" }` (at most
  20; letters, spaces, hyphens and apostrophes; the plural is filled in when
  left out). keepr gives each an id (`own:smidgen`) and `keepr_schema` shows it.
  A name keepr already reads as a built-in unit is refused (`unit_name_taken`).
  Values are counted in them and never converted: 2 smidgens is never 2 dashes.
  Such an element takes no `min`, `max` or display rules.
  - **Renaming** a unit keeps its id: send the list back with each unit's `id`,
    the name changed. A unit sent without its id keeps the id of the unit with
    that name.
  - **Removing** a unit that items hold is refused (keepr's answer names the
    conversion it waits for, direction `ownUnits`) until the person says what to do: `"convertValues":
    "move"` with `"moveUnits": { "own:smidgen": "<another unit id>" }` moves
    each value to that unit keeping its number (`""` empties it), or
    `"convertValues": "clear"` empties them all. Ask the person which; never
    choose for them. The refusal lists the units still allowed (`into`).
    Taking away a measure and an own unit that both hold values is refused in
    one save: do the measures first, then the own units.
  - **An own unit belongs to its element.** Two elements' "smidgen" are one
    unit only while both call it that (a rename keeps the id): copy or
    compare an own-unit value between elements only where the names match.
- **A column is an element of its own**, taking the types and options any
  element does with a few exceptions: a column cannot be another group, a
  count of confirmations or a file, and cannot be required only when another
  element says so. keepr's preview names the one that was refused.
- **`keepr_schema` warns when repeating groups are switched off** on this
  keepr, and a proposal that adds one is then refused. Say so; do not work
  around it.
- **Changing a group.** Send the group by name with the whole `options.columns`
  list as `keepr_schema` printed it, changed: a column left out of the list is
  removed from the card (what entries hold under it stays on them, hidden, and
  comes back if a column of that name is added again). Adding a column or a
  choice needs nothing more. **Removing or renaming a column** hides what the
  entries hold under it, so tell the person; **retyping one** is refused while
  any item holds entries in the group. The preview lists each as
  `group.column`, with how many items hold entries. On a card other collections use, a change only grows.
- **A column that keepr works out** (a line total, an element that sums the
  group) is a formula on the element; the schema marks the result as never
  sent.
- **A total on the card the entries name** (a supply's *Used* from the job
  lines that name it, a service plan's *Last done* from the service records
  whose lines name it) is a rollup on that card whose link is a lookup
  **column**, written `"<group>.<column>"`:

  ```json example
  { "name": "used", "dataType": "decimal", "sourceCard": "job",
    "options": { "drivenFrom": { "linkElementName": "supplies.supply",
      "sourceElementName": "supplies.quantity", "aggregate": "sum" } } }
  ```

  Every entry that names the item counts once, so a job with two lines naming
  the supply counts both. The value is a column of the same group, or an
  element of the record that holds the entry (`"sourceElementName": "date"`
  with `"aggregate": "latest"`); a sum or an average needs a column. A
  `where` narrows the records, not the entries. keepr's preview refuses a
  group, a column or a card that does not fit, and names it.

  To show what such a total is made of, read its source records, with the
  item that shows the total and the total's name as the request's query:

  ```http example
  GET /api/items?driven_item=<the item>&driven_element=<the total>
  ```

  The rows are the RECORDS holding the entries, so their count is not the total's;
  the **`X-Driven-Entry-Count`** header says how many entries those records
  hold (only records the key may read; a `count` total less this is how many
  sit in records it cannot open). A `latest` lists its records winner first,
  in the total's own order, when no other sort is asked
  (**`X-Driven-Source-Order: latest`**): the first row holds the entry the
  value came from, unless that record is one the key cannot read.

## Changing a card

### The hazard, stated once

`PATCH /api/card-definitions/{id}` **replaces the card's element list whole.**
A payload that carries only the element you meant to touch removes every other
one, and hides their stored values on every item. So a partial list is never
sent: keepr's tools read the card's own elements, lay your change
over them, and send all of them — and then the server's **change-preview**
says, from the real stored data, what that payload would do. You do not get
to skip the preview: it is where the token comes from.

### The flow

```
1. keepr_schema with card                             read the card as it is
2. write the change                                   only what the user asked for
3. keepr_propose_card with change
     the server's diff, item counts, side effects — and a token. Nothing written.
4. show the diff to the user, in their words, destructive parts first
5. keepr_apply_card with the token AND the same change  once they say yes
     confirm_card_name too  when step 3 said DESTRUCTIVE
```

### The change spec

```json example
{
  "elements": [
    { "name": "rating", "max": 10 },
    { "name": "isbn", "label": "ISBN", "dataType": "text-small" }
  ],
  "remove": ["shelf"],
  "name": "Books"
}
```

- An element **named in `elements`** that the card already has is **updated**:
  only the keys you give move (`label`, `dataType`, any option). Everything
  else about it stays.
- A name the card does not have is **added**, and needs a `dataType`.
- An element **not mentioned is kept unchanged**. Leaving one out of the spec
  is not a removal.
- **Removal only through `remove`**, by name, and only for the card's own
  elements. An inherited element belongs to the parent card; say so and stop.
- `name`, `description`, `options` (merged), `color`, `icon` are card fields
  and pass through — an icon looked up first, as above. **`key` is refused**:
  it is the card's stable handle, and saved filters and links break when it
  changes.
- `guide` is the card's guide: a few paragraphs of markdown on how its records
  are used and why, shown to people where they work. `guideLinks` is the
  **whole** ordered list of keepr item ids the guide points at (a procedure, a
  reference), each an item the person can read — keepr says when a list is too
  long; `''` and `[]` clear
  them. A card's guide is its own: a child card inherits none. A new card gets
  its guide by a change once it exists.

### Reading the preview

The server answers with a classified diff:

| kind | what it means | destructive |
| --- | --- | --- |
| `added` | a new element; existing items have it empty | no |
| `labelChanged`, `optionsChanged`, `reordered` | display and validation changes; stored values untouched | no |
| `removed` | the element goes and **its stored values are hidden** on every item — kept, but no list, form, export, search or title shows them — until an element of the same name is added back (they all return) or a manager purges them from the card's settings in the web app. The preview reports how many items hold a value | **yes** |
| `retyped` | the data type changes. A number ↔ measurement move is a `conversion: measurement` and a number ↔ currency move a `conversion: currency` (values converted); anything else is `conversion: none` — the stored values are **reinterpreted, not converted**, and may stop matching or sorting | **yes** |

Plus `cardFields` (name, description…) and `sideEffects` the server knows
about — an account link that would be un-linked, automation rules that would
retire, rules and notifications that read a removed element and will see it
as unset (each by name), a primary date that would stop resolving. Read them
out.

### Say aloud what cannot be undone

Before asking for a yes, name the destructive parts in plain words, with the
server's numbers: *"Removing `shelf` hides the shelf on 12 books. They come
back only if `shelf` is added again, and a manager can purge them for good."* *"Changing `rating` from a rating to a number keeps
the 40 stored numbers but they are no longer a 0–10 scale."* Do not soften it,
and do not bundle it: a destructive change is its own question, separate from
the safe ones in the same spec, and the user may say yes to one and no to the
other — in which case, propose again with only what they agreed to.

### Rules

- **Read the card first.** A spec written from what the card "probably" has
  proposes the wrong thing, and the diff will say so — but the user should not
  be the one to catch it.
- **A card change is a proposal until the user has seen the diff.** Never
  apply in the same breath as proposing. The token exists so that what they
  saw and what gets applied are the same bytes.
- **Never remove an element the user did not name.** "Clean up the card" is
  a question ("which of these seven should go?"), not a removal.
- **Never retype to make a value fit.** If the data has "n/a" in a number
  element, the row is wrong, not the card.
- **Apply only what they saw.** If anything changed after the preview — they
  added a request, the token expired, the preview reported a refusal —
  propose again. An expired or used token is refused, on purpose.
- **A `wouldApply: false` preview is the server saying no** (an invalid
  option, a duplicate name, a scope problem). Nothing is stored; fix the spec
  and propose again. Do not try the PATCH directly.

### What is not here

**Deleting a card is not available from this skill or the MCP tools**, by
design: it is a soft delete only an administrator can reverse, it drops the
collection's membership settings for that card, and it retires the card's
automations. If the user wants a card gone, say that it is done from the
card's own page in the web app (the card's menu there), and leave it to them.

Renaming a card's **key**, moving a card between collections, and changing
its scope are likewise web-app operations.

A card's **layouts** (tile, table, form, page), its **related lists** (the named
lists of linked items each of its items shows) and the **rules** that run on its
records are not here either — they are a setup: `references/setup.md`. Rules are
no longer out of reach for an assistant; a rule that notifies, changes other
records or runs on a schedule arrives paused, and the person turns it on.
