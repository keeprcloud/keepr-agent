# Changelog

Each release lists the two versions it carries: the skill's (which is also the
plugin's) and the server's.

## 2.42.0 — 2026-10-25

Skill 2.42.0, keepr-mcp 0.41.0.

- Your assistant can attach a folder of files to items it just imported, even though the import and the folder run in different places: keepr's local file tools read the items from keepr and match each file, or its subfolder, to the external id of an item written under the import's source_system, so a folder of R-1042.pdf and R-1043.pdf lands on the right records without a map of ids. The connector's import result now names the collection and source_system to pass. A file that matches no item, or whose name differs only in case, is left unmatched rather than guessed, and a person who can only read a collection is told so before anything is read.

## 2.41.0 — 2026-10-24

Skill 2.41.0, keepr-mcp 0.40.0. Also the first release of 2.40.0, 2.39.0 and 2.38.0, below.

- Your assistant reads keepr's limits from keepr itself instead of from the skill. keepr_schema now lists what keepr enforces: how long a query or a text search may be, how many conditions and dot-walk hops a filter may have, how many entries a list of dates holds, and, for the kinds of element on the card in front of it, which totals, averages and counts a chart can work out, with a chart's caps. When keepr changes one of these, the assistant knows at once, with no new skill. Also in this release, unpublished until now: torque is a measure, so a measurement element can hold a tightening force in foot-pounds, inch-pounds or newton-meters (85 ft-lb, 100 in-lb, 120 Nm).

## 2.40.0 — 2026-10-24

Skill 2.40.0, keepr-mcp 0.39.0.

- A big read no longer overruns what your assistant can take in at once: when a page of items or a chart would pass about 150,000 characters, your assistant is given the items or rows that fit, is told how many were left out, and is told how to read the rest (skip, a smaller limit or a filter). A single item too large for one answer is shown with its largest values replaced by a note saying where to open it.

## 2.39.0 — 2026-10-24

Skill 2.39.0, keepr-mcp 0.38.0.

- The connector answers Claude and ChatGPT when their requests carry an Origin header: one from https://claude.ai, https://claude.com or https://chatgpt.com is now signed in as usual instead of being refused, so an assistant that sends one can connect. Any other Origin is still refused. Also in this release, unpublished until now: torque is a measure. A measurement element can hold a tightening or turning force in foot-pounds, inch-pounds or newton-meters, and your assistant reads and writes it in any of those units, converting between them (a torque never converts to energy). A filter written with the middle dot in a unit, such as 90 ft·lb, is quoted.

## 2.38.0 — 2026-10-24

Skill 2.38.0, keepr-mcp 0.37.0.

- Torque is a measure: a measurement element can hold a tightening or turning force in foot-pounds, inch-pounds or newton-meters, and your assistant reads and writes it as 85 ft-lb, 85 lb-ft, 100 in-lb or 120 Nm, in any of its units, converting between them (a torque never converts to energy). A filter written with the middle dot in a unit, such as 90 ft·lb, is quoted. The connector's contract now lists the torque units and their symbols (contract 014a447c0608).

## 2.37.0 — 2026-10-22

Skill 2.37.0, keepr-mcp 0.36.0. Also the first release of 2.36.0, 2.35.0 and 2.33.3, below.

- Your assistant can take everything out of a collection: keepr.py export-all makes one zip of every item, card definition and file the sign-in can read, with no item cap and the change history and sub-collections if asked, waits for keepr to finish it and saves it; the connector's keepr_get_items says that everything in a collection is one Export everything in keepr. Your assistant knows how large a file may be before it uploads one: keepr_schema says the most one file may be in a collection and each file element's real limit, and the skill's guide reads the same from the schema. A file over the limit is refused with file_too_large naming the limit, and an upload past the collection owner's storage is refused with plan_limit (nothing is refused for storage yet: every account has room). Your assistant knows a date holds one value: it never sets a date element to hold several dates, and keeps a list of dates in a repeating group with a date column (a span is two date elements, a start and an end). Also in this release, unpublished until now: your assistant can use the units a measurement element names for itself, such as a smidgen or a baker's dozen; it can fill in a measurement that allows several measures; it can show what a total over a repeating group's entries is made of; the skill and the connector know keepr's plan_limit row code; a total on a card can read a column across the repeating group entries that name its item; your assistant can read one kind of record across all your collections; new cards get an icon and a color from keepr's catalog; assistants read an item's comments and add one for you; tiles are laid out in six columns; and a write refused because the API is older than your data is no longer retried.

## 2.36.0 — 2026-10-22

Skill 2.36.0, keepr-mcp 0.35.0.

- Your assistant can use the units a measurement element names for itself, such as a smidgen, a dollop or a baker's dozen: keepr_schema lists each one with its id and names, the assistant writes "2 smidgens" or { value, unit } with the id or a name, and filters compare only within that unit. It can add own units to a card, rename one by keeping its id, and when a unit that items hold is removed it asks the person whether to move those values to another unit or clear them. keepr also counts in pinch, dash, handful, clove, slice, can, jar, stick, bunch, sprig, head, bale, flake, crate, carton, sack, tray, skein and piece. Also in this release, unpublished until now: your assistant can fill in a measurement that allows several measures, such as a recipe's amount that is 2 cups on one ingredient, 250 g on the next and 3 eggs on another. keepr_schema says which measures an element allows and lists the units of each, and the assistant writes each value in its own unit; a unit of none of them is refused with the element named; your assistant can show what a total over a repeating group's entries is made of; the skill and the connector know keepr's plan_limit row code; a total on a card can read a column across the repeating group entries that name its item; your assistant can read one kind of record across all your collections; new cards get an icon and a color from keepr's catalog; assistants read an item's comments and add one for you; tiles are laid out in six columns; and a write refused because the API is older than your data is no longer retried.

## 2.35.0 — 2026-10-22

Skill 2.35.0, keepr-mcp 0.34.0.

- Your assistant can fill in a measurement that allows several measures, such as a recipe's amount that is 2 cups on one ingredient, 250 g on the next and 3 eggs on another. keepr_schema says which measures an element allows and lists the units of each, and the assistant writes each value in its own unit; a unit of none of them is refused with the element named. Also in this release, unpublished until now: your assistant can show what a total over a repeating group's entries is made of; the skill and the connector know keepr's plan_limit row code; a total on a card can read a column across the repeating group entries that name its item; your assistant can read one kind of record across all your collections; new cards get an icon and a color from keepr's catalog; assistants read an item's comments and add one for you; tiles are laid out in six columns; and a write refused because the API is older than your data is no longer retried.

## 2.33.3 — 2026-10-22

Skill 2.33.3, keepr-mcp 0.32.1.

- Your assistant can show what a total over a repeating group's entries is made of. The skill's card guide says how to read a total's source records, that the X-Driven-Entry-Count header gives how many entries those records hold (only records you can read), and that a latest lists its records winner first. Also in this release, unpublished until now: the skill and the connector know keepr's plan_limit row code; a total on the card that a repeating group's entries name; reading one kind of record across all your collections; new cards get an icon and a color from keepr's catalog; assistants read an item's comments and add one for you; tiles are laid out in six columns; and a write refused because the API is older than your data is no longer retried.

## 2.33.2 — 2026-10-21

Skill 2.33.2, keepr-mcp 0.32.1. Also the first release of 2.33.1, below.

- The skill and the connector know one more row code from keepr's published contract, plan_limit, so neither reports the contract as newer than itself. Nothing sends it yet, and nothing your assistant does changes.

## 2.33.1 — 2026-10-21

Skill 2.33.1, keepr-mcp 0.32.0.

- Your assistant can set up a total on the card that a repeating group's entries name: a supply's Used from the job lines that name it, a service plan's Last done from the service records whose lines name it. The skill's card guide shows the rollup whose link is a lookup column, written group.column, and says that every entry naming the item counts once. Also in this release, unpublished until now: your assistant can read one kind of record across all your collections; new cards get an icon and a color from keepr's catalog; assistants read an item's comments and add one for you; tiles are laid out in six columns; and a write refused because the API is older than your data is no longer retried.

## 2.33.0 — 2026-10-20

Skill 2.33.0, keepr-mcp 0.32.0.

- Your assistant can read one kind of record across all your collections. A combined list gathers the same kind of record from your own collections, ones shared with you and published ones you follow, and every row says which collection it is in. The new keepr_combined_lists tool shows your saved lists and the ones keepr suggests, and what each holds; keepr_get_items reads one with combined_list or match, filtered, searched and sorted, a page at a time with no row repeated or missed. An assistant connected with access to only some collections reads only those, and says so. Also in this release, unpublished until now: new cards get an icon and a color from keepr's catalog; assistants read an item's comments and add one for you; tiles are laid out in six columns; and a write refused because the API is older than your data is no longer retried.

## 2.32.2 — 2026-10-19

Skill 2.32.2, keepr-mcp 0.31.2. Also the first release of 2.32.1, below.

- Your assistant no longer retries when keepr says its API is older than your data. A write refused with api_older_than_data is sent once and the answer is relayed at once, since the same request would be refused again until that API is updated.

## 2.32.1 — 2026-10-19

Skill 2.32.1, keepr-mcp 0.31.1.

- Tiles have one width: six columns. The setup tool and the skill's setup guide now say a tile body is cols: 6 with spans in sixths and no longer describe the old three-column reading. A setup written for three columns still applies (keepr doubles its spans), but write new ones in sixths.

## 2.32.0 — 2026-10-19

Skill 2.32.0, keepr-mcp 0.31.0. Also the first release of 2.31.0, below.

- Your assistant gives every new card an icon and a color, and never guesses an icon's name. keepr_schema with icons looks up the icon names keepr accepts by what a card is for, best first, with the 24 colors; keepr_propose_card and keepr_propose_setup give a new card that has none an icon found by its name and a color, and say which. keepr refuses an icon it does not have, naming up to five close matches, and takes a color as a hex or a color's name like emerald. Also in this release, unpublished until now: assistants read an item's comments and add one for you; private items have left keepr, and both write tools refuse an argument they do not take; tiles are laid out in six columns; a card can define a named action with inputs; and charts over a repeating group count only the entries the filter matches.

## 2.31.0 — 2026-10-18

Skill 2.31.0, keepr-mcp 0.30.0.

- Your assistant can read an item's comments and add one for you. keepr_get_items with comments: true lists an item's comments oldest first, with who wrote each, when, whether it was edited and whether an assistant added it, and comment_id reads one comment with its earlier versions. The new keepr_add_comment tool adds a comment under your name, on a card that takes comments and an item you can edit; keepr records that an assistant added it, everyone who can read the item sees it, and a retry after a lost connection never adds it twice. Only you can edit or delete a comment, in keepr. Also in this release, unpublished until now: private items have left keepr, so assistants no longer see or set whether an item is private, and both write tools refuse an argument they do not take; tiles are laid out in six columns; a card can define a named action with inputs; and charts over a repeating group count only the entries the filter matches.

## 2.30.0 — 2026-10-17

Skill 2.30.0, keepr-mcp 0.29.0.

- Private items have left keepr, so your assistant no longer sees or sets whether an item is private. keepr_ingest rows and keepr_update_item take no visibility, keepr_schema no longer lists a per-card private-items setting, and keepr.py no longer knows the private_not_allowed answer. Both write tools now refuse an argument, or a row field, they do not take, instead of quietly dropping it, so a call that still sends visibility is told. Private tags are unchanged. No new tools. Also in this release: Your assistant arranges tiles in six columns. A card's tile in the item list is now three rows of six columns, so a field can take half the tile or five sixths of it; the assistant sends cols: 6 with spans in sixths, and a tile it writes in thirds, the way tiles were before, is doubled by keepr as it is saved. A setup's preview draws each field's width out of the tile's columns (4/6). And a card can define a named action people press, with inputs they fill in when they press it (keepr_schema lists the card-actions endpoint).

## 2.29.0 — 2026-10-17

Skill 2.29.0, keepr-mcp 0.28.0.

- A chart over a repeating group's entries counts only the entries its filter's block matches. With line has (product = widget) in the filter, keepr_chart counts, totals and averages the Widget lines alone, not every line of an invoice that holds one; a block on that group under or or not is refused with a sentence saying how to fix it. No new tools or inputs.

## 2.28.0 — 2026-10-16

Skill 2.28.0, keepr-mcp 0.27.0.

- Your assistant can ask about the day, hour or month of a date inside a repeating group. A filter reads a date column's .weekday, .hour and .month inside has (…), such as dose has (given.weekday = mon and vaccine = flu) for a flu dose given on a Monday, and keepr_chart can group a group's entries by a date column's day of the week, hour of the day or month of the year, each row opening its items. No new tools or inputs.

## 2.27.0 — 2026-10-16

Skill 2.27.0, keepr-mcp 0.26.0.

- Your assistant can chart a repeating group's entries by a date of their own. When keepr_chart counts entries, the chart's date can be a date column of that group, such as each dose's due date, so "doses due in the last 12 months, by month" places every dose by its own date, compares it with the year before, and finds the latest dose. A card beneath that keeps the group under another name has its entries counted too. No new tools or inputs.

## 2.26.0 — 2026-10-16

Skill 2.26.0, keepr-mcp 0.25.0.

- Your assistant can put a file into a repeating group's file column, such as a photo for each step of a procedure. keepr_attach_file and keepr_request_upload take the group as element, plus the entry's id as entry_id and the column as column; each is checked first and refused with a sentence naming what is wrong. An entry you remove takes its files with it. A file element works as before.

## 2.25.1 — 2026-10-16

Skill 2.25.1, keepr-mcp 0.24.1.

- Your assistant hears keepr's "this took too long" answer at once. When a search or filter runs past keepr's time limit, such as a filter on a repeating group's entries with nothing narrowing the list first, keepr says what to change, and keepr_get_items, keepr_update_item and keepr.py now pass that on straight away instead of running the same slow search two more times first. Any other 503 is still tried again. No new tools or inputs.

## 2.25.0 — 2026-10-16

Skill 2.25.0, keepr-mcp 0.24.0. Also the first release of 2.24.0, 2.23.0 and 2.22.1, below.

- Your assistant can set the order a related list shows its items in. keepr_schema says each list's order, such as a procedure's steps by step number, and a setup's collection.relatedLists takes sort: { element, direction }, the element by name or #id, previewed with the rest of the list. Leave it out for the default order. No new tools.

## 2.24.0 — 2026-10-16

Skill 2.24.0, keepr-mcp 0.23.0.

- Your assistant hears when a calculation could not work out a value. When a save leaves a repeating group's calculated column blank in one entry, because it divides by zero or works out a text that is not one of its choices, keepr_update_item (a whole-list save and each entry operation) and keepr_ingest now list it by group, entry and column, such as line, entry 3, total: division by zero, so the assistant can tell the person which line to fix. A save with nothing to report prints nothing new. No new tools or inputs.

## 2.23.0 — 2026-10-16

Skill 2.23.0, keepr-mcp 0.22.0.

- Your assistant knows how large one item may grow. keepr refuses a change that would leave an item holding more than 8 MB, such as one more long entry on a list that is already very long, and writes nothing; it says how large the item would be and which element holds the most, so the assistant can suggest moving entries to another item or shortening text. An item already that large can still be made smaller. keepr_schema's contract gives the limit. No new tools or inputs.

## 2.22.1 — 2026-10-15

Skill 2.22.1, keepr-mcp 0.21.1.

- An import can link a repeating group's entries to other rows of the same import. In keepr_ingest, a lookup column inside an entry takes {"$ref": "<external id>"} as a lookup element does, so an invoice's lines can name products written earlier in the same import, and a re-run keeps every entry. A reference that names nothing fails that row and says which entry and column. No new tools or inputs.

## 2.22.0 — 2026-10-15

Skill 2.22.0, keepr-mcp 0.21.0.

- Your assistant knows repeating groups end to end. It can ask for items by what their entries hold: one entry that answers every condition (ingredient has (name = butter and amount > 2)), different entries (two has blocks), no entry, no entries, and how many (.count). It knows to read an item's entries first and send the whole list back changed, or change one entry on its own, how an import of a list re-runs without duplicating, and how to propose a repeating group on a card with its columns. It tells the person when a file cannot be a column. No new tools or inputs.

## 2.21.0 — 2026-10-15

Skill 2.21.0, keepr-mcp 0.21.0.

- Your assistant can total a repeating group's entries. keepr_chart takes entries: "line" to count each entry instead of each item, measures a column written line.amount, and groups by a column or by the item's own element, so it can answer how much of each product the invoice lines add up to, or the lines' total per customer. Without entries a chart still counts items, and grouping by a column counts each item once in each value its entries hold. The answer says whether it counted entries or items.

## 2.20.0 — 2026-10-14

Skill 2.20.0, keepr-mcp 0.20.0.

- Your assistant can set up the related lists an item shows. keepr_schema now lists each card's related lists: the named lists of linked items, such as an epic's open work items, each with the card it lists, the lookup it goes through and its filter. A setup can add one or change one, matched by its id or name: keepr_propose_setup takes collection.relatedLists, previews each list as added, changed or unchanged, and refuses what the card would refuse, such as a name already in use or a lookup that does not point at the card. A setup never removes a list. keepr.py setup takes them too. No new tools.

## 2.19.0 — 2026-10-14

Skill 2.19.0, keepr-mcp 0.19.0.

- Your assistant knows what keepr works out in a repeating group. On a card whose group has a calculated column, such as a line total of quantity times price, or an element that totals the group, such as an invoice's subtotal, keepr works the values out itself when the item is saved, so the assistant never sends them: keepr_schema marks those columns as worked out by keepr. When a save's calculations would need more work than one save allows, keepr refuses the row, says which calculation, and writes nothing. No new tools or inputs.

## 2.18.0 — 2026-10-14

Skill 2.18.0, keepr-mcp 0.18.0.

- Your assistant can bring a spreadsheet of entries back into keepr. keepr.py csv reads an .xlsx as well as a CSV, keepr's own Excel export among them: dates come back as dates, a choice's label as its value, and a unit or currency in a column's heading as that unit. On a card with a repeating group, lines that share the item's key become one item whose entries are those lines, in order, so a sheet exported one line per entry imports into another collection as the same entries. A bad line is named by its line number and column before anything is sent, and a refusal on the dry run says which line it came from. Rows that share the --id-column are now always one item (they used to be sent as rows of their own). An export copies into another collection; loaded back into the collection it came from, it adds the items again. No new tools or inputs.

## 2.17.0 — 2026-10-13

Skill 2.17.0, keepr-mcp 0.18.0.

- Your assistant can change one entry of a long list without sending the rest. On a card with a repeating group, such as the ingredients of a recipe, keepr_update_item takes entries: add an entry at the end or at a place, change only the columns that change, move one, or remove one, each named by its id. Two people changing different entries at the same moment no longer overwrite each other, and each change is one line in the item's history naming the entry. To add the same entry to many items, the dry run and its confirm token apply as for any change to many. On the command line, keepr.py entry does the same. No new tools.

## 2.16.1 — 2026-10-13

Skill 2.16.1, keepr-mcp 0.17.2.

- Your assistant can filter by what is inside a repeating group. keepr says how in its contract: ingredient has (name = butter and amount > 2) finds the items where one entry holds both, two such blocks may be answered by different entries, and ingredient is empty and ingredient.count >= 5 ask how many entries there are. keepr writes the group and its columns by id when a filter is saved. No new tools or inputs.

## 2.16.0 — 2026-10-13

Skill 2.16.0, keepr-mcp 0.17.1.

- Your assistant can search a list the way its search box does. text ~ "lost scissors" in a filter finds the items with every word somewhere in their text, case and accents aside, with a quoted phrase matched in order; the skill says what a search reads and what it leaves to a filter, and that keepr refuses one where a filter decides who sees what. An element called text is now written "text". No new tools or inputs.

## 2.15.1 — 2026-10-13

Skill 2.15.1, keepr-mcp 0.17.1.

- Your assistant knows when repeating groups are switched off. keepr now says in its contract whether repeating groups are on, and while they are off keepr_schema warns that a card change adding one would be refused, so the assistant does not propose a group keepr cannot save. A group a card already holds reads and saves as before. No new tools or inputs.

## 2.15.0 — 2026-10-13

Skill 2.15.0, keepr-mcp 0.17.0.

- Your assistant can fill in repeating groups. When a card has one, such as the ingredients of a recipe, the assistant reads an item's entries in order and writes the whole list back: an entry it keeps holds on to its id, a new one gets an id from keepr, and one it leaves out is removed. keepr_schema shows what each column takes, and keepr checks every entry the way it checks an item, naming the entry and the column when it refuses one. Re-running the same import changes nothing. No new tools or inputs.

## 2.14.0 — 2026-10-12

Skill 2.14.0, keepr-mcp 0.16.0.

- Your assistant is ready for repeating groups. When a card has one, such as the ingredients of a recipe, keepr_schema shows the group, how many entries an item may hold, and each of its columns with its type. Entries cannot be written yet, so the assistant leaves a group out of the rows it builds; keepr says what to send. No new tools or inputs.

## 2.13.0 — 2026-10-11

Skill 2.13.0, keepr-mcp 0.15.0.

- Your assistant now draws and explains the widths of a form. When it proposes a layout for a card's form, each field shows what it sets, such as phone (span 6) or dob (span 4, new row), instead of an unreadable object, and a form that changes only a width shows the difference under now and becomes. The assistant's tool and the skill both say what a field's span and its new row mean, and leave the limits to keepr: keepr checks each width against what the field holds and, when it refuses one, names the range, so the assistant proposes again with that. No new tools or inputs.

## 2.12.0 — 2026-10-09

Skill 2.12.0, keepr-mcp 0.14.0.

- Your assistant can say "me too" for you. On an item whose card has a Me too element, it can confirm it, or take your confirmation back, as you, once each, and it reads how many people have confirmed. keepr keeps that count itself, so an assistant never types it, and who confirmed stays with the people who manage the collection.

## 2.11.0 — 2026-10-06

Skill 2.11.0, keepr-mcp 0.13.0.

- Your assistant asks which element you mean. When a filter names an element more than one card has, such as a Task's status and a Bug's, keepr asks instead of guessing. Every keepr tool that sends a filter lists the elements it could mean by label, card and id, then writes the filter with the one you choose. Two cards with the same name are told apart by where they come from: Bug (global) is the shared card, Bug (Clinic) is your Clinic collection's own. Charts can name their fields by element id, and keepr_chart shows each by its label. keepr's text stays keepr's text: the tools and keepr.py show every name and message from keepr on one line, and anything you might type back, such as an element's name or a card's key, exactly as it is, with any invisible character spelled out. So a card's or a collection's name can't pass for an instruction. No new tools or inputs.

## 2.10.0 — 2026-10-06

Skill 2.10.0, keepr-mcp 0.12.0.

- The keepr plugin now brings keepr's connector (https://mcp.keepr.cloud/mcp) and, in Claude Code and Cowork on your computer, small local tools that attach files and folders from it — so it keeps itself current, and nothing in it reads a key. keepr.py signs in through keepr's page in your browser instead of a key you copy. The skill leaves every fact that changes (element types, error codes, limits) to keepr, so a copy a release behind is still right. Turning a rule on or pausing it is now its own tool, keepr_set_automation_state (keepr_automations only reads), and every tool has a title and says whether it changes anything. Fixes: keepr's local tools never save their sign-in without the shared lock, a sign-in keepr.py gave up on never reaches keepr late, and inside iCloud only iCloud Drive and each app's Documents folder can be attached.

## 2.9.1 — 2026-10-06

Skill 2.9.1, keepr-mcp 0.11.1. A security fix: update the plugin and the desktop extension.

- **The folder and file tools refuse secret places.** `keepr_attach_folder` no longer reads a hidden folder given as the folder itself (such as `~/.ssh`), and neither `keepr_attach_folder` nor `keepr_attach_file` opens anything under `~/Library` (iCloud Drive and cloud-storage drives such as Dropbox still work), `%APPDATA%` or `%LOCALAPPDATA%`, or `~/snap`. A Windows network path is refused before anything touches it, so your sign-in is never sent to another computer, and so is a path over 4,096 characters. The refusal says which place it was.

## 2.9.0 — 2026-10-06

Skill 2.9.0, keepr-mcp 0.11.0. Also the first release of 2.8.0 and 2.8.1, below.

- **Your assistant can read element ids.** keepr now names an element in a filter it stores by its permanent id, written `#k7f3q2xa`, so one card's element can't be confused with another card's element of the same name. `keepr_schema` shows each element's id beside its name. Every stored filter that `keepr_schema`, `keepr_chart`, `keepr_automations` and `keepr_propose_setup` print keeps keepr's own text, with a line beneath naming each id's element and card. The skill explains how to read them and when to write one.

## 2.8.1 — 2026-10-05

Skill 2.8.1, keepr-mcp 0.10.1.

- **A file too big to send inline is no longer a dead end.** `keepr_attach_file` names the way that works for your connector, ready to call: the file's path or `keepr_attach_folder` from the extension and the plugin, or `keepr_request_upload`'s link for you anywhere. It never suggests shrinking the file to fit.

## 2.8.0 — 2026-10-05

Skill 2.8.0, keepr-mcp 0.10.0.

- **Attach a whole folder from your computer in one step.** `keepr_attach_folder` matches each file to its record (from this session's import, a map, or a value the filenames carry, such as a receipt number), shows you the match first, then uploads the originals in the background, streamed from disk, up to 100 MB each. `keepr_attach_status` reports what arrived and what is still missing. Running the same folder again sends only what is new. These two tools are in the desktop extension and the Claude Code plugin, where the assistant can read your files.

## 2.7.2 — 2026-10-05

Skill 2.7.2, keepr-mcp 0.9.2.

- **A setup can change the charts and dashboards a collection already has.** A chart or dashboard in the setup that matches one shown to everyone (by its id, or by name) is shown as it is and as it becomes, and changed in place, so it stays pinned for everyone. A dashboard's tile can name a chart the collection already shows. If anything in the apply fails, each one is put back as it was.

## 2.7.1 — 2026-10-05

Skill 2.7.1, keepr-mcp 0.9.1.

- **New cards in a setup can be written the way the card chapter shows.** An element's options beside its name (`"isTitle": true`, `"choices": [...]`) and a lookup by card key (`"lookupCard": "release"`) are moved into keepr's shape before anything is sent. keepr refused them before.
- **A change says exactly what changes.** A changed rule or quick add lists each value that differs, as it was and as it becomes (`actions[0].title: Waiting on you → Work is waiting on you`), not only "actions". A changed tile, form or table is drawn as it is now and as it becomes.
- Plainer wording in keepr's previews: "reaches 1 person", and a reminder's window in days or hours with the condition it counts.

## 2.7.0 — 2026-10-05

Skill 2.7.0, keepr-mcp 0.9.0.

- **Your assistant can set up a collection.** Ask it to lay out how cards look in the list, which columns the table shows, how the item form is grouped, or a printable page, and to add saved filters, quick adds, tags, rules, notifications and charts, on new cards or ones you already have. `keepr_propose_setup` shows keepr's own preview: each part added, changed (with what it was and what it becomes) or already the same, a tile drawn as its rows, and each rule with what it does, how many records it would touch and who it would notify. `keepr_apply_setup` applies exactly what you saw, all at once or not at all, after you type the collection's name back.
- **Rules that act on their own wait for you.** A rule that only fills in values on the record that set it off runs at once. A rule that notifies people, changes other records, creates records or runs on a schedule arrives paused, and only you can turn it on, in keepr › Settings › Automations. Your assistant cannot, and it tells you so.
- **Read and pause rules, and see who changed what.** `keepr_automations` lists a collection's rules and notifications (on, paused or waiting for you, and who made each), reads a rule's recent runs, and pauses one. `keepr_history` reads an item's, a card's or a collection's changes. Neither deletes or undoes anything.
- **Change many items at once.** `keepr_update_item` takes up to 100 ids or a filter. It shows what would change first, with the count and a few titles, and makes the change only with the token that preview gave.
- **Ask for files your assistant cannot send.** `keepr_request_upload` lists the items waiting for files and gives you a link: open it, drop the files or pick photos, and keepr matches each to its item. `keepr_upload_status` says what has arrived and what is still missing.
- The script gains `setup`, `automations`, `history` and `request-upload`.

## 2.6.0 — 2026-10-04

Skill 2.6.0, keepr-mcp 0.8.0.

- **Short links to your records.** Every item now has a short address, such as `keepr.cloud/i/7k3qx9m`. `keepr_get_items` gives each item its `url`, and the assistant hands you that link instead of building a long one from ids. The links keep working when an item moves to another collection.
- **Filters in a card proposal are saved by id.** A filter that names a record by its title is refused, so a filter you approve keeps meaning the same thing after a rename.

## 2.5.0 — 2026-10-04

Skill 2.5.0, keepr-mcp 0.7.0.

- **Connect without a key.** In the Claude desktop extension (Cowork included) and the Claude Code plugin, you no longer need to create and paste an API key. The first time you ask for something in keepr, Claude opens keepr in your browser; sign in if you need to, choose what it may do, and click **Allow**. The connection is listed under **Connected assistants** on your keepr profile, where you can rename or disconnect it. Ask Claude to disconnect, or to connect again, at any time.
- **A key still works**, and wins when you set one. The extension's key field is now optional.

## 2.4.0 — 2026-10-04

Skill 2.4.0, keepr-mcp 0.6.0.

- **Ask for totals, not pages.** Questions like "how much did I spend on fuel by month?" or "how many orders by status this year?" are answered by keepr itself. The new `keepr_chart` tool runs a chart you saved, or a question built from your collection's fields, and answers a table: money in its currency, measurements in their unit, an Overall row, and what keepr left out. It counts only what you may export from keepr, and says how many items it left out for that reason. A read-only key is enough.
- **Filters can ask for part of a date and for who added an item.** `created.weekday in (sat, sun)`, `created.hour >= 9 and created.hour < 17`, `due.month = dec`, `created.by = me`. Days and hours are read in the collection's time zone; a plain date is its calendar day.

## 2.3.3 — 2026-10-03

Skill 2.3.3, keepr-mcp 0.5.1.

- **A new money field takes the right currency without being told.** When you ask for a card with a money field and don't name a currency, the assistant leaves it out, and keepr starts the field in the collection's currency, else your preferred currency (Preferences › Language & region), else US dollars. Name a currency and that one is used. A field that already exists keeps its currencies.

## 2.3.2 — 2026-10-03

Skill 2.3.2, keepr-mcp 0.5.1.

- **A tag name two tags share is refused in a search** (`ambiguous_tag`, listing each tag's path) instead of matching either: say which with the path or the id.
- **A choice's label works in a filter** and is read as its value; the value is still the one to write.

## 2.3.1 — 2026-10-03

Skill 2.3.1, keepr-mcp 0.5.1.

- **A refused upload says what keepr said.** When keepr refuses a file — for example on a record its card has locked, which now covers attachments too — `keepr_attach_file` reports keepr's own reason (`item_locked`: a manager can unlock it) instead of a bare error number.

## 2.3.0 — 2026-10-02

Skill 2.3.0, keepr-mcp 0.5.0.

- **File and photo fields.** A card can hold a file field (any file, or photos only; one file or a list of up to 20; a size limit). `keepr_attach_file` takes an `element` and puts the upload into that field: a single field's file is replaced (the key needs `delete` scope, and the assistant says so first), a list gains it at the end. The field's limits are read from the schema first.
- **Re-runs are safe.** An attachment already on the item is reused only when it is a loose attachment with the same name and size, never a different file that merely shares the name.
- **Rows never carry files.** Imports and item updates leave file fields as they are; a row that tries to set or clear one is refused for that field (`file_not_settable`). The schema describes each file field so the assistant knows to attach rather than type.

## 2.2.0 — 2026-10-02

Skill 2.2.0, keepr-mcp 0.4.0.

- **Tags are back.** Rows and item updates carry the collection's tags by name (or by path for a nested tag), items are read with their tags, and the schema lists the tags there are. A name that is no tag of the collection is refused (`unknown_tag`), never created.
- **Restricted tags stay with people.** A tag that decides who can see what is applied or taken off only by a collection manager in keepr; your assistant leaves it alone and tells you.
- **A card proposal may bring new tags,** each optionally applied by a rule that arrives paused for you to turn on.
- The old tag words are retired: a row that still sends them is refused (`tags_retired`) until the assistant is updated.

## 2.1.5 — 2026-10-01

Skill 2.1.5, keepr-mcp 0.3.5.

- **Fields have rules now.** Text has length limits and can require a pattern or capitals; a field can hold unique values; links follow one rule (a bare address becomes https://). A row that breaks a rule comes back with the field and the reason (`too_long`, `pattern`, `invalid_url`, `duplicate_value`, `invalid_color`).
- **Defaults are filled in by keepr.** A field left out of a row starts at its default, as it does in the form.
- **Choices can allow multiple.** Send a list, or one value with entries split by `;`. Numbers may be sent as "1,234", and as "12.5%" on a field shown as a percent.
- **New field types and options:** color, and the angle, frequency and fuel-economy measures; a card lookup may offer only some items ("strict" refuses the rest — `lookup_filtered_out`).
- **Groups built from records** also protect the fields they read through another record: such a change is left to a signed-in person in keepr.

## 2.1.4 — 2026-09-30

Skill 2.1.4, keepr-mcp 0.3.4.

- **Groups built from records are protected.** In a collection where a group's
  members come from records (everyone linked to a Staff record, say), linking
  an account on such a record — or changing a field the group's rule reads,
  in a way that could put the linked person in the group — is now left to a
  signed-in person in keepr. Your assistant gets `session_required` for that
  row, leaves the value out, and tells you to set it in keepr. Re-sending a
  record exactly as it is stored still works.
- A row refused because the person behind the key lacks the access such a
  group gives is now named as `account_link_needs_access`: a manager makes
  that change.

## 2.1.3 — 2026-09-29

Skill 2.1.3, keepr-mcp 0.3.3.

- **A big dry run tells the truth.** An import of more than 200 rows goes in
  several batches, and a dry run writes nothing — so until now, a row that
  pointed at a record from an earlier batch (a task naming its epic) came
  back refused in the dry run, then imported fine. The dry run now tells each
  batch which records the earlier ones will create, so it checks out the way
  the import will.
- A dry-run update that names a different card for a record the import would
  create is now refused as the import would refuse it, instead of reading as
  fine.

## 2.1.2 — 2026-09-28

Skill 2.1.2, keepr-mcp 0.3.2.

- **Old records keep their own dates.** When the data you import says when
  a record was made — a spreadsheet's "Created" column, an export's
  timestamp — your assistant can send it with each row. keepr shows it on the
  record ("Created in *system* on *date* · added to keepr *date*"), as a
  **Created in source** column in the table, and in Excel and PDF exports,
  instead of every imported record looking as though it was made today.
  keepr's own created date is still the day of the import.
- The date is set once, when a record is first imported. Re-sending a row
  with a different date still updates the record, and says the date was not
  changed.

## 2.1.1 — 2026-09-28

Skill 2.1.1, keepr-mcp 0.3.1.

- **An import can be taken back.** Every batch of one import now carries the
  same import id, so keepr lists the batches as one import, and someone who
  manages the collection can undo it from the collection's **Settings →
  Imports** in the web app: the items it added are deleted and the ones it
  changed are put back — except anything someone has worked on since, which
  stays. Your assistant tells you the import's id when it writes, and where to
  undo it. It cannot undo an import itself; deleting stays yours.
- The skill keeps an import's id from its dry run to its commit, and through a
  re-send of the rows a commit refused; `--import-id` names it yourself.
- Against an older keepr, the rows are sent without the id.

## 2.1.0 — 2026-09-28

Skill 2.1.0, keepr-mcp 0.3.0.

- **Several cards at once, all or nothing.** Ask for "a Task that belongs to
  an Epic, depends on other tasks, and a count of tasks on each Epic" and your
  assistant proposes the whole set as one: cards that look each other up, a
  card that looks up its own kind, counts and totals rolled up from another
  card, how each card's items are titled, the filters the collection starts
  with and the columns a card's table shows. keepr checks all of it before
  anything is made, and says what it would refuse, line by line. When you say
  yes, it is made in one step; if any part is refused, nothing is kept, and
  your assistant says so.
- **The keys you see are the keys keepr stores.** A key like `Work_Item` is
  shown as `work-item` in the proposal, which is how keepr keeps it.
- **A lookup to one of keepr's global cards adds it to the collection.** A
  Task whose owner is a Person brings the Person card into the collection; the
  proposal says so before you agree, and the key needs **Read and write** as
  well as **Can change cards**, as it does for filters.
- With the skill, the same spec file goes to keepr as one set, and an older
  keepr is still served card by card.

## 2.0.5 — 2026-09-28

Skill 2.0.5, keepr-mcp 0.2.5.

- **A card that inherits from another is created with its parent, or not at
  all.** Ask for "a Bug card that is a Work item plus a severity" and your
  assistant now finds the parent — in the same request, in the collection, or
  among keepr's global cards — creates it first, and links the child to it. A
  parent that names nothing is refused before anything is created. Before
  this, the connector could say "inheriting from Work item" and create the
  card without it.
- **Created cards report their real keys and ids.** keepr sometimes adjusts a
  key you ask for (`book` becomes `book-2` when `book` is taken); the result now
  says so, instead of showing no key at all.
- **Your assistant sees when an element becomes required.** "Acceptance
  criteria is required when status is ready" now reads as exactly that, rather
  than as optional. Numbered elements say how they are shown (`KPR-0042`) and
  that they are matched by the number.
- **Asking for items of a card by its key works.** A card key or name is
  looked up first; one that does not exist is named as such, instead of an
  answer of 0 items.
- With the skill, a spec whose parent or lookup names nothing now stops before
  its first card is created, not halfway through.

## 2.0.4 — 2026-09-26

Skill 2.0.4, keepr-mcp 0.2.4.

- **It tells you when it is out of date.** Your assistant now hears from keepr
  when a newer version is out, and tells you once, in a sentence — with the
  steps for the way you connected. A skill installed from the skill link
  updates itself (`keepr.py update`); for the plugin, Claude Code can run the
  update for you, or you can turn on auto-update once (`/plugin` →
  Marketplaces → keepr-agent → Enable auto-update). The extension and a skill
  uploaded to Claude get short steps. Every way:
  https://keepr.cloud/docs/guides/assistants/update-your-assistant
- Your keepr **API keys** list now says which assistant last used each key,
  and marks it **Out of date** when it is.
- Every release now also carries `keepr.zip` and `keepr.mcpb`, so the
  `releases/latest/download/…` links always get the newest.
- If you use Claude or ChatGPT, the **keepr connector** is the simplest way in:
  nothing to install, no key, and always current. See the README.

## 2.0.3 — 2026-09-25

Skill 2.0.3, keepr-mcp 0.2.3.

- **Sub-collections.** A keepr collection can now sit inside another one (a
  family's *Health* inside *Biewend*). `keepr_collections` says which
  collection a sub-collection belongs to, and a sub-collection's schema
  includes the cards it inherits from the collection above it. A parent's
  schema lists its sub-collections' cards under `familyCards`, each with the
  collection that holds it — a record of one of those is written **there**.
  A row sent to the parent instead fails with `card_not_allowed` and a hint
  (`card_in_sub_collection`) naming the right collection, and the skill knows
  to send it on.
- An API key limited to a collection also reaches its sub-collections.
- Nothing else changes: the same nine tools, the same key handling. A keepr
  deployment without sub-collections simply never sends a parent.

## 2.0.2 — 2026-09-25

Skill 2.0.2, keepr-mcp 0.2.2.

- **Money.** keepr has a currency element now. The skill reads and writes
  amounts like `12.50 USD` or `CA$12`, knows that an element may allow only
  some currencies, and explains the new `invalid_currency` refusal (an unknown
  code, a currency the element does not allow, or more decimals than the
  currency has) instead of guessing.
- **Deleting needs its own permission.** A keepr API key or connected
  assistant can no longer delete anything unless its key was created with
  **Can delete records** (off by default), and deletes through a key have a
  daily limit. Keys made before 24 September do not have it — re-create the key
  if you want your assistant to delete. The skill recognises the refusal
  (`insufficient_scope` with `requiredScope: "delete"`) and the daily limit
  (`delete_budget_exhausted`, never retried), and `keepr_collections` says
  whether the key in use can delete.
- The server reports its version on its health check, and carries the updated
  keepr contract.
- Nothing else changes: the same nine tools, the same key handling.

## 2.0.1 — 2026-09-23

Skill 2.0.1, keepr-mcp 0.2.1.

- Every keepr account now has a **personal collection** — the catch-all for a
  note or a to do you have not said where to put. `keepr_collections` (and the
  skill's reference) mark it with `kind: "personal"` and list it first, so an
  assistant asked to "jot this down" has an obvious home for it.
- Nothing else changes: the same nine tools, the same key handling. An older
  keepr deployment simply never sends `kind`.

## 2.0.0 — 2026-09-22

First public build. Skill 2.0.0, keepr-mcp 0.2.0.

- The `keepr` skill, renamed from `keepr-add`, now covers reading, adding and
  cards — `items`, `get`, `search`, `login`, `logout` and `change-card` join
  `keepr.py`.
- `keepr-mcp` bundled to a single file; nine tools; reads the key from
  `~/.config/keepr/credentials` when no environment variable is set, so the
  Claude Code plugin needs no configuration.
- The desktop extension (`.mcpb`), with the key kept by Claude desktop.
- This marketplace.
