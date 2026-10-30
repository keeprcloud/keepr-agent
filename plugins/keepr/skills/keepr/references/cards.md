# Cards — creating and changing record types

A card is schema. Creating one changes the shape of the user's data; changing
one changes the shape of data that already exists. Neither is done quietly,
and neither is done from memory: **read the card first**, propose, show the
user exactly what the server says will happen, and apply only what they saw.

Both need **Can change cards** on the key (or the connection) and `manage` on
the collection. Without it keepr refuses, naming what is missing; keepr's
tools print the fix — *create or edit a key with Can change cards turned
on* — and so should you.

## Commands and tools

| | MCP tool |
| --- | --- |
| read the card | `keepr_schema` with `card` |
| propose a new card | `keepr_propose_card` |
| create it | `keepr_apply_card` with the proposal token and the collection name typed back |
| propose a change | `keepr_propose_card` with a `change` argument |
| apply the change | `keepr_apply_card` with the token, the collection name typed back, and — for a destructive change — the card name typed back too |
| look for a ready-made card first | `keepr_discover` (`search`, then `read`) |

A proposal token is good for 30 minutes and is used once. The apply step sends
exactly the payload that was previewed; nothing is recomputed between the two.

## Discover first

When the person wants to start keeping **a new kind of record** — something
no card of theirs holds yet — search **Discover** before you propose a card or
a setup for it. Discover is keepr's catalog of ready-made cards, bundles and
collections, each built for one kind of record with its elements, rules and
dashboards already worked out. A package that fits gives the person all of it
in one step; a card built from scratch is for when nothing fits.

A field added to a card they already have is not a new kind of record: change
that card (below), with no search.

1. **Search** with `keepr_discover`, `action: "search"`, in the person's own
   words for the kind of record ("vehicle maintenance", "recipes"), not in
   card terms. `collections: true` also finds the published collections they
   can follow (a reference list kept by its publisher), which they follow
   rather than add.
2. **Read** the likely ones with `action: "read"`: every card and element, the
   add-ons, what each rule does. Judge the fit from that, not from the name.
3. **Show at most three candidates**, each with one line on how it fits and one
   on what it lacks for this person ("Vehicle care: services, costs and
   reminders for each car. It has nowhere for tyre pressures."). Say which one
   you would pick.
   `keepr_propose_card` for a new card also searches Discover on its own, and
   when up to three packages are a strong match its answer lists them under
   `DISCOVER:` (and as `discover`). That is a backstop for an assistant that
   skipped this step, not a replacement for it: show those to the person
   before they apply the proposal. The proposal itself is unchanged.
4. **Build from scratch only when none fits, or the person asks you to.** Say
   in one sentence that Discover had nothing that fits, then propose the card
   as below.
5. **Preview the one they pick**, when your connection can change cards:
   `action: "preview"` with the package `key`, the `collection` it goes into
   (by name or id) or a `new_collection` name, the `add_ons` they want (leave
   it out for the ones the listing ticks), and `examples: true` only if they
   want the example records. A new collection, the examples, or a package that
   brings settings (filters, charts, tags, rules, notifications, forms) need a
   connection that can also write records; a connection that only changes
   cards hands over the link for those. It adds nothing. Tell them in a few
   lines what it would add, what it would rename or leave out because the
   collection already has one, and what arrives paused or off until they turn
   it on. Its answer ends with a preview token and the install link with
   those choices filled in. A health package's preview says the person adds
   it: an assistant never does.
6. **Add it once they say yes — or hand over the link.** Ask plainly: this
   package, these add-ons, into this collection (or a new one named so), with
   or without the examples? Only after a clear yes to all four, call
   `keepr_add_package` with the preview's `preview_token` and
   `confirm_collection_name`, the collection's name exactly as the preview
   said it (your client asks the person before it runs, every time). Anything they change means a new preview first: the token is for
   exactly the choices previewed, it lasts 30 minutes, and it is spent by the
   first install. Then say what was added and what arrived paused or off —
   rules, notifications and forms wait for them in keepr, and you may turn on
   only a rule that changes nothing beyond its own record. If they would
   rather add it themselves, or the package is a health package (it asks for
   their own consent), give them the link with three steps: open it; check
   the collection it goes into and the add-ons ticked; press **Add to
   collection**. If the install's answer is lost, read the collection before
   anything else: it may be there.
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
5. keepr_apply_card with the token                    once they say yes
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
