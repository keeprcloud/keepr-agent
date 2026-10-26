# Reading — finding and reporting what is there

"What's in my collection?", "how many books did I finish this year?", "look up
the Northwind contact", "which trucks are due for service?". The discipline
here has the same weight as the write rules in `adding.md`: reading cannot
break a record, but a wrong answer about a record is a wrong answer about the
user's own data, said with confidence.

Read the collection's `schema` first (SKILL.md). It tells you the element
names a query can use, which element is the title, what the card's primary
date is, and what a choice's values are called. A query written from memory
of what the collection "probably" has compiles to a silent no-match, not an
error — you would report "none" for a question that had an answer.

## Commands and tools

| MCP tool | what it answers |
| --- | --- |
| `keepr_collections` | which collections this key can see, and its role in each |
| `keepr_schema` | the cards and elements — read this first |
| `keepr_get_items` with `collection` and `q` | a page of items, count first |
| `keepr_get_items` with `ids` (or `item_id`) | one or more items in full (keepr caps the ids a call resolves) |
| `keepr_get_items` with `item_id` and `comments: true` | an item's comments, oldest first; `comment_id` for one with its history |
| `keepr_combined_lists` | the person's saved combined lists and the ones keepr suggests; with a list or a match, its sources and the elements to filter it by |
| `keepr_get_items` with `combined_list` or `match` | the rows of one kind of record across every collection the person has, each saying which collection it is in |
| `keepr_search` | free-text substring search across collections, cards, items and tags — tags by name and items used as tags by title; `types` narrows it (`tags` for tags alone) |
| `keepr_chart` | a total, count, average, earliest/latest or share, by month or by category — worked out by keepr over every item; or a saved chart. `charts.md` |

`q` is KQL — `references/kql.md` has the grammar and worked examples. It is
the difference between paging a collection into context and asking the server
the actual question. `search` is not a query: it is a case-insensitive
substring match on names and text values, for "is there anything called…".

When a name in `q` could mean more than one element — two cards each with a
`status` — keepr refuses the query and lists each element with its `#id`,
label and card. If the person has not said which one they mean, ask them;
never pick one. Then send `q` again with that `#id` (`kql.md`, "When keepr
asks which element").

## Everything in a collection, as one file

"Give me all of it", "back up my collection", "I'm leaving — export
everything": that is not a page of items, and paging `items` past its cap is
the wrong tool. keepr makes the whole collection into **one zip** — every item
the person can read with every value, the card definitions, every attachment
and file as it was uploaded, and if asked the change history and the
sub-collections — in the background, and keeps it for a week.

| MCP tool | what it does |
| --- | --- |
| — | starts the export (or picks up the one already being made), waits for it and saves the zip |
| — | saves an export keepr already made |
| — | the person: the collection's ⋯ menu › **Export everything…** in keepr, which also emails them when it is ready |

Through the connector there is no tool for it: tell the person to choose
**Export everything…** in the collection's ⋯ menu, or to run the command above.
What is in the zip is exactly what this sign-in reads — a key confined to some
collections, or one that may not see an element, exports less than the person
would in keepr. Say so when it matters. `manifest.json` in the zip lists the
counts and a checksum of every part, and names any file left out and why;
`README.txt` explains the layout.

## One kind of record across every collection

Some questions name a kind of record, not a collection: "what recipes do I
have?", "which vehicles are due for service?", "what help articles mention
refunds?". The records may be in the person's own collection, one shared with
them and a published one they follow, and a read of one collection answers
only a part. A **combined list** reads one kind of record across all of them:
every collection the person lists, own, shared or followed, each row saying
which collection it lives in. keepr finds the sources and lines their elements
up; what the person may read is unchanged.

Use one when the question names a kind of record and `keepr_collections` shows
the same kind of card in two or more collections, or when a search turns the
same thing up in several. Do not use one for a question about a named
collection, for totals (`keepr_chart`, one collection at a time), or to write:
a combined list is read-only, so a change goes to the collection a row names.

```
keepr_combined_lists  {}                                  # saved lists (id, name) and suggestions (each with a match)
keepr_combined_lists  { combined_list: "<id or name>" }   # its sources, and the elements it can be filtered by
keepr_get_items       { combined_list: "<id or name>", search: "…", limit: 25 }
keepr_get_items       { match: <a suggestion's match>, q: "…", sort_field: "…", exclude: [{ collection: "<id>" }] }
keepr_get_items       { match: <…>, mode: "count" }
```

- **Find the list first.** Ask `keepr_combined_lists` before reading, and
  prefer a list the person saved (it is their own choice of what belongs in it)
  over a suggestion. Pass a suggestion's `match` exactly as given; never make
  one up from ids you guessed. If nothing is saved or suggested, say that
  keepr has no combined list for it yet and read the collections one by one.
- **A saved list brings its own filter and sort.** To ask something else of
  the same records, take the list's `match` from `keepr_combined_lists` and add
  your own `q`. `search` (plain words) and `limit` work with a saved list.
- **Name elements as keepr lists them.** `keepr_combined_lists` with a list or
  a match names each element it can be filtered or sorted by, and how many of
  the sources have it. Use that name in `q`; if it means more than one element,
  ask the person which (`kql.md`, "When keepr asks which element").
- **Say where each row is from.** Every row names its collection: report
  "3 from your recipes, 2 from your sister's" rather than one blended count,
  and open or change a row in the collection it names.
- **Page by cursor, until it ends.** The last page's `nextCursor` goes back
  with the same arguments; there is no `skip`, and a cursor is lost if the
  arguments change. A "that's all" is only true when `nextCursor` is empty.
- **Read the source lines.** A source marked `PARTIAL` was not read in time:
  its rows may be missing, so say the list may be incomplete. A source that
  lacks an element your filter names matches nothing there: say that source
  was not searched on it, not that it has no such records. A count marked
  "at least" is a floor.
- **A key limited to some collections reads fewer sources**, and the answer
  says so. It cannot list or look up saved lists by name (keepr keeps those to
  the person signed in); it can read one by the id the person gives, and a
  suggestion's match. Report the answer as covering only the collections the
  key may read, never as everything the person has.
- When keepr says it does not offer combined lists yet, the person's keepr is
  older than this skill: say so, and read the collections one at a time.

## An item's comments

On a card that takes comments (`keepr_schema` says so), each item has a
journal of dated comments by the people who work on it. Read it when the
question is about what was said or decided on an item, or before you add to
it (`adding.md`):

```
keepr_get_items  { item_id: "<24-hex>", comments: true }                 # oldest first; skip / limit page them
keepr_get_items  { item_id: "<24-hex>", comment_id: "<24-hex>" }         # one comment, with every earlier version of its text
```

Each line says who wrote it and when, whether an assistant added it for its
person, and whether it was edited. A note from before comments has no author:
say so rather than guessing one. A deleted comment shows as a placeholder only
where the card keeps deleted comments visible; never report what one said
unless its own read gives the text. Quote a comment as its author's words, and
keep the answer's "more" line in mind: a long journal is paged.

## A repeating group on an item

An item whose card has a repeating group carries its entries, in order, under
the group's name in the **structured** result of `keepr_get_items` (the text
lists titles only, as always): `{ id, elements: { <column>: value } }` for an
entry, `{ id, section: "<title>" }` for a heading, and an empty value when the
item holds none. Report entries from there, not from the title, and keep each
entry's `id` if you will change one afterwards (`adding.md`).

- A column the key may not read is left out of every entry; do not say the
  entry has no value there.
- To find items by what their entries hold, or by how many they hold, use
  `has (…)`, `is empty` and `.count` (`kql.md`): "recipes with more than 2 of
  butter" is one question on one entry, not a count you work out from a page.
- A list can be sorted by how many entries a group holds with `sort_field`
  naming the group.
- Totals and counts **over entries** come from `keepr_chart` (`charts.md`).

## The rules

**Never present a page as the whole.** `items` answers `N of TOTAL items in
"Collection" matching Q` on its first line for a reason: say all three parts
to the user — how many you are showing, how many there are, and what filter
produced them. "Here are your books" is wrong when it is 25 of 340. "Here are
the 25 most recent of your 340 books" is right. If they asked for everything
and there is more than a page, say how many there are and ask how they want
it cut, rather than paging.

**Narrow before you page.** A page is small by default (`keepr_get_items` says
how small) and the API has a ceiling on it (`keepr_schema`, under WHAT KEEPR
ENFORCES). Never walk a whole collection into context page by page to answer a
question a query would answer. "How many are open?" is the query
`status = open` and reading the count line, not fetching every item and
counting. "The newest five" is a limit of 5 (the default sort is each card's primary date,
newest first). If a question really needs every item — "list every author" —
say how many items that is before you start, and stop at a page if the user
did not ask for the walk.

**A short `get` is "not readable by this key", never "deleted".** The API
omits an item it cannot show — wrong id, another collection, a card the key
cannot read, outside the key's allowlist — and says nothing about which. `get` counts
them: `2 of 3 requested — 1 not readable by this key`. Report exactly that
phrase. Do not say the item is gone, was deleted, or does not exist; you do
not know that, and the user may be looking straight at it in the web app.

**A 404 on a collection means it is not this key's.** Wrong id, or outside
the key's allowlist. Say so and stop; do not go looking through other
collections for one with a similar name.

**Name items by their display title and give the link.** Every item has a
title (the card's title element; `displayValue` on the wire) and a short web
address made from its `code`:

```
https://keepr.cloud/i/<code>
```

The MCP tools hand it to you as `url`.
Give that link exactly as it comes — never build one from ids. (An item keepr
has not given a code yet has `url: null`; its long address,
`https://keepr.cloud/collections/<collectionId>/items/<itemId>`, still works.) Use the title when you talk about a
record and give the link when the user might want to open it. Never refer to
an item by its id alone, and never invent a title for an untitled item — say
it is untitled and give the id.

**Report numbers as numbers.** Quote the count the server gave. Do not total,
average or otherwise compute over element values unless the user asked; when
they did, **ask keepr** — `keepr_chart` works the total, the average or the
breakdown out over every item and says what it left out (`charts.md`) — rather
than adding up a page. Say which items went into it — "the 12 items matching
`status = paid and paid-on > -30d`; 2 had no amount and were left out". A
number without its filter is a number nobody can check.

**Keep their values as they are.** A date is reported as stored (`2026-03-14`,
not "mid-March"); a measurement in the unit it was entered in; a choice by
its value or label as the schema names it. Round or convert only when asked,
and say so.

**Tags are the person's labels — report them by name.** `keepr_get_items`
returns each item's tags; filter with
`tags = <name>` (`kql.md`), which also matches the tags beneath it. A tag "by a
rule" was applied automatically. **My tags** are the person's own private
tags: only they see them, so mention them to them and nobody else. A tag shown
as "no longer available" is an item used as a tag that this key can no longer
read — say that, don't guess what it was.

**Don't answer what you didn't fetch.** If a question needs an element the
schema does not have, say the collection does not track that. If it needs a
join across two cards, `kql.md` shows the dot-walk (`resident.move-in-date`)
and facet (`@person = …`) forms; if neither reaches it, say so rather than
fetching both sides and matching them by eye.

## Reporting

A good read answer has four parts, in this order:

1. **The scope**: which collection, which filter, how many matched, how many
   you are showing.
2. **The records**: title, the two or three values that answer the question,
   the link when useful. Not every element of every item.
3. **What was left out**: items not readable by this key, values that were
   empty, a filter that matched nothing.
4. **How to see more**: the next page, or the narrower query you would run.

```
12 of 12 expenses in "Household" matching merchant ~ "blue bottle" and spent-on > -90d
  2026-03-14  Blue Bottle Coffee   $18.50   https://keepr.cloud/i/7k3qx9m
  …
Two of them have no receipt attached.
```
