> Source: the Engine artifact of 2026-09-10, converted to markdown for the chat side. Figures are as measured on harry that day. No tenant rows, no credentials.

# The Engine

Everything EMET has built for the engine, as it stands on harry on the tenth of September, 2026. Written to be read aloud. The tables carry the exact figures; the prose carries the meaning.

  * 1\. What the engine is
  * 2\. The gear
  * 3\. SEA and the fold
  * 4\. The drain
  * 5\. Landing
  * 6\. Quantity, price, location
  * 7\. The match
  * 8\. Tenancy and the door
  * 9\. The cache that was torn down
  * 10\. The postal side
  * 11\. Live now, and owed
  * 12\. Glossary



## 1.What the engine is, in one breath

The engine is a small set of fixed functions that read rows and do what the rows say. Every behaviour in EMET is a row. The only code is the executors, and every executor is cleared with Mike before it exists. That is Law Seven, rows not codes, and it is the reason the engine can serve a marketplace it has never heard of without a line being written.

The unit the engine works on is the gear. A gear is one schema holding six tables and nothing else. The same six tables appear in every marketplace gear, every division gear, every tenant gear, every service gear. Because the shape never changes, the engine never has to know which gear it is in. It asks the rows.

The Ten Laws are the engine's physics. First contact mints or rejects. Ops holds only four kinds of value. Every entity has a parent, and the parent is a uuid. Ids stay home and uuids travel. Settings resolve from base to gear to tenant. Rows are only ever appended. Behaviour is rows. Silence is yes. A tenant's schema is private by construction. And we are still in beta, so a wrong shape is torn down, never patched.

On harry today the engine is thirty nine functions across two schemas. Thirteen live in `ten_engine`, the engine proper. Twenty six live in `mailroom`, the postal and door side. The third schema, `ten_routines`, holds the queue and the attempt ledger and no functions at all.

| The executors on harry, measured 2026-09-10. Purpose is from the function's own comment where one exists, otherwise from its body. Function | Arguments | Purpose |
|---|---|---|
| --- | --- | --- |
| `ten_engine.receive` | node jsonb, recipe jsonb, parent uuid, space uuid | Unwraps a package by its recipe: is the entity new, is the value new, is it a change; stores the leftovers as unclaimed. Body, no comment. |
| `ten_engine.is_new` | gear, etype, cues text[], node jsonb, space uuid | Recognition by the cues the recipe named, through the index on space, e_type and native_id. No cue means every arrival is new. The gear's own space is derived, never passed, since 1461. |
| `ten_engine.recipe` | gear, passport uuid | Builds the unwrapping recipe for a passport from its rows. Body, no comment. |
| `ten_engine.sea` | gear, etype, label | Finds the one a_type row for a gear, entity type and label. The only lawful way a write finds an aspect. |
| `ten_engine.standing` | gear, entity, label | The newest value of an aspect on an entity, as text. |
| `ten_engine.to_queue` | gear, entity, etype, birth bool, label, from | The only writer of new work. Folds into one job per entity. |
| `ten_engine.trg_hopper` | trigger | Fires on entity, eav and boms of every hopper gear and calls to_queue. The birth is the first fact. |
| `ten_engine.drain` | cap int | The one consumer. Takes jobs with skip locked, becomes the tenant, dispatches receive, compose, go or land, and writes one attempt row per try. |
| `ten_engine.land` | gear, entity, aspects text[], from | The one THEN. Walks each aspect's lands entries: an endpoint entry queues a compose, a birth entry calls resolve, an entity entry crosses the fold through via shaped by how. |
| `ten_engine.resolve` | gear, entity, uuid, etype, twin_passport, parent, cue_gear, cue_entity | The resolver, once called twin. One entity is born or recognised beside another from a passport's becomes, id_field, edge, twin_uuid and cue rows. Called by land on a birth entry. |
| `ten_engine.compose` | gear, entity, apicall uuid, aspects text[] | Builds an outgoing package from the apicall's parameter rows, filling only what landed. Body, no comment. |
| `ten_engine.go` | ctx jsonb | The reply hop. Body, no comment; the four outcomes memory records it as reply only since 1350. |
| `ten_engine.gear_of` | uuid | Which gear holds this uuid. Loops over every registered gear with a dynamic query. |
| `mailroom.reduce` | gear, entity, label | The current value folded the way the aspect declares: last, ranked, sum, union, standing_no. The reducer is a row, never code. Runs as the caller. |
| `mailroom.fact` | gear, entity, label; or uuid, label | The newest value of one aspect, vocabulary first, else text. Not a fold over a stream. |
| `mailroom.strip_fact` | uuids uuid[], label, max_hops int | R-059: reads an aspect from the one level of the family strip that carries it, walking up by parentage across gears. Names no gear, no aspect, no entity type. |
| `mailroom.ladvar` | name, scope | One setting folded base to gear to tenant, newest wins. In a tenant session a null scope means the session's own tenant. |
| `mailroom.ladvar_mine` | name | Did this session choose a value for this setting. No scope argument, so it cannot be pointed at another tenant. |
| `mailroom.ask` | op, args jsonb | The terminal door's read verb. Resolves the op to a call row in emetweb, refuses anything not folded to read, runs it as the tenant's wall role. |
| `mailroom.tenant_session` | schema | The role a session becomes to read one tenant, and the tenant gear's uuid. Fails closed. |
| `mailroom.session_tenant` | none | Which tenant this session is, read from the role setting. |
| `mailroom.my_tenant` | role | The schema name behind a wall role. |
| `mailroom.upr_signin` | schema, email, sub | The sign-in door. Resolves a person by Cognito sub, or by address at first contact and mints the sub then. |
| `mailroom.upr_of` | schema, person_id | What a person holds. The definition of a grant lookup, keyed on our own id. |
| `mailroom.upr_grants` | schema, email | What a verified address holds in one tenant, one row per role. Security definer because the door has no privilege on a usr schema. |
| `mailroom.receive` | env jsonb | The postal door on the database side: a reply to a call we made, or an arrival, is stamped and routed. Body, no comment. |
| `mailroom.send` | actor, endpoint, pkg, caller | Records an outgoing call as an envelope for the valet to carry. Body, no comment. |
| `mailroom.send_mine` | endpoint, pkg, caller | send with the actor taken from the session. |
| `mailroom.uncarried` / `carried` | cap int; outgoingcall id | The valet's two calls through the wall: what is waiting to be carried, and mark one carried. |
| `mailroom.pallet` | oc, sha | Reborn in 1280 for replies over the courier's size threshold. Body, no comment. |
| `mailroom.picture` | gear, entity | One alloc's picture as a read. Mints nothing. |
| `mailroom.clock_tenants` | endpoint uuid | Which tenants a clock fires for on an endpoint. Body, no comment. |
| `mailroom.gear_of` / `parent_of` | uuid | Thin wrappers over ten_engine.gear_of and a parent lookup. |
| `mailroom.trg_route` | trigger | Before insert on central_routing: matches a route row by source and four words. No tenant dimension. |
| `mailroom.trg_wake` / `trg_wake_reply` | trigger | Wake the engine on an arrival, and on a reply row. |
  
## 2.The gear

A gear is six tables. The entity table holds the things: each row has an integer id for use inside the gear, a uuid for use across gears, an entity type, a parent uuid, a space uuid naming the tenant it belongs to, and a native id, which is the foreign name the outside world gave it. The eav table holds the facts: one row per observation of one aspect on one entity, appended and never edited. The a_types table holds the aspects themselves, the vocabulary of what can be said. The avs table holds the enumerable values an aspect may take. The boms table holds edges between entities, and the bom_types table names the kinds of edge.

A gear is born as a schema with those six tables, walls and grants, and a gear entity in the registry. The registry is itself a gear, called `ten_registries`, and today it holds twenty three gear entities. The most recent birth was the division family on the ninth of September: five schemas at once, born empty, from one file at Mike's keyboard.

Three kinds of root anchor everything. A tenant gear has a usr root. A marketplace gear has a marketplace root. A division gear has a division root. Law Three says every entity except those roots is born with a parent, and the parent reference is always a uuid. A chain that does not end at a root should never have existed. Law Four says a column ending in id holds an integer that resolves inside its own gear, and a column ending in uuid holds a value that resolves outside it. A uuid that stays home is as much a breach as an id that travels.

On harry the gears range from empty to very large. The Woo marketplace gear holds a hundred and fifty four thousand entities and nearly two million facts. The ops commerce gear holds a hundred and forty five thousand entities: forty six thousand lots, seventy two thousand allocations, sixteen thousand order lines and ten thousand orders. The five new division gears hold nothing yet, except the mint template, which holds twenty four aspects and three hundred and sixty six vocabulary values and no content.

| Every gear schema on harry with its six-table counts, measured 2026-09-10. Armed means an a_type with a lands or arrives entry. Prompts is the count of a_types carrying an ai_prompt; a dash means the column is absent in that gear. Gear | Entities | Facts | A-types | Avs | Boms | Bom types | Armed | Lands | Arrives | Prompts | Folds null |
|---|---|---|---|---|---|---|---|---|---|---|---|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| div_ | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| div_amgirl | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| div_lego | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| div_militaria2 | 88,019 | 518,350 | 113 | 15,524 | 4,056 | 13 | 0 | 0 | 0 | 52 | 110 |
| div_next | 0 | 0 | 24 | 366 | 0 | 5 | 0 | 0 | 0 | 0 | 0 |
| div_sff | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| mp_biblio | 56 | 338 | 58 | 23 | 3 | 3 | 26 | 23 | 3 | 0 | 0 |
| mp_ebay | 51,926 | 688,708 | 451 | 593 | 34,381 | 12 | 33 | 9 | 29 | 0 | 442 |
| mp_ebay_ebca | 309 | 2,034 | 34 | 78 | 308 | 2 | 0 | 0 | 0 | – | 34 |
| mp_ebay_ebuk | 87 | 1,398 | 38 | 129 | 86 | 2 | 0 | 0 | 0 | – | 38 |
| mp_ebay_ebus | 308 | 2,295 | 33 | 76 | 307 | 2 | 0 | 0 | 0 | – | 33 |
| mp_wooc | 154,174 | 1,981,648 | 4,197 | 45 | 137,931 | 9 | 43 | 9 | 39 | 0 | 4,192 |
| ops_cat | 33,602 | 16 | 46 | 15 | 24,997 | 9 | 0 | 0 | 0 | 0 | 45 |
| ops_com | 145,723 | 467,183 | 87 | 77 | 239,040 | 13 | 5 | 5 | 0 | 0 | 79 |
| ops_media | 182,336 | 750,183 | 64 | 49 | 92,465 | 7 | 2 | 1 | 2 | 0 | 62 |
| svc_anthropic | 33 | 165 | 27 | 14 | 26 | 1 | 0 | 0 | 0 | – | 27 |
| svc_enrich | 21 | 42 | 49 | 0 | 17 | 3 | 0 | 0 | 0 | – | 49 |
| ten_registries | 133 | 428 | 23 | 118 | 23 | 3 | 0 | 0 | 0 | – | 21 |
| ten_routines | 62 | 289 | 24 | 9 | 0 | 1 | 0 | 0 | 0 | – | 24 |
| usr_ | 6 | 18 | 25 | 2 | 0 | 1 | 0 | 0 | 0 | – | 17 |
| usr_milanttor | 31 | 132 | 102 | 10 | 13 | 1 | 0 | 0 | 0 | – | 98 |
| usr_torbri | 25 | 87 | 239 | 64 | 6 | 5 | 0 | 0 | 0 | – | 233 |
  
Four other schemas match the gear pattern by name but are not six-table gears: `ten_engine` and `ten_lens` hold no tables, `ten_postal` holds four, and `ops_reference` holds nine. The hopper trigger, which is what makes a gear part of the engine, stands on twenty one gears as sixty three triggers, one each on entity, eav and boms.

The edges a gear may draw are its bom types. In the ops commerce gear they read like the spine of the business: form has item, item has lot, lot has allocation, bin has lot, alloc has order line, order has line, order has customer, has component, is a, has field, delivers, and has twin. The marketplace gears add their own: apicall has param, speaks, contains, and the eBay category and aspect edges.

Bom type labels on harry, measured 2026-09-10. Gear| Labels, by id  
---|---  
ops_com| 1 form_has_item · 2 item_has_lot · 3 lot_has_allocation · 4 bin_has_lot · 5 has_component · 6 requires · 19 alloc_has_order_line · 22 is_a · 25 order_has_line · 26 order_has_customer · 29 has_field · 30 delivers · 53 has_twin  
mp_ebay| 1 apicall_has_param · 2 order_has_line · 9 contract_has_slot · 10 is_a · 11 contains · 39 has_field · 40 delivers · 135 speaks · 142 ebay_cat_child · 143 ebay_binds_aspect · 144 ebay_has_aspect · 167 has_twin  
  
## 3.SEA: the aspect rows and the fold

SEA stands for Schema, Entity type, Aspect. It is the address of every fact in EMET. The Schema is the gear. The Entity type is what kind of thing the fact is about. The Aspect is the word. A quantity on an allocation and a quantity on a lot are two different facts with two different rows, because bundles and holds and currencies make them mean different things. Mike's line for it was that the engine is just IFTTT: if this arrives, then that happens, and both halves are written on the aspect's own row.

Each a_type row carries a handful of words. The label is the aspect's name. The e_type is which entity type it belongs to, and a null e_type means the gear-wide row. The dtype says the shape of the value: text, uuid, decimal, timestamptz, int, bool, or verbatim. The folds word says how a stream of values becomes one current value. The arrives entries are the IF: which passport and which path in an arriving package this aspect is read from. The lands entries are the THEN: where the fact goes next, whether to an endpoint, to a far entity through a chain of edges, or to a birth. The ai_prompt is how an enrichment model would be asked for the value.

The SEA key is enforced as a unique index on the a_types table: the pair of e_type, with null read as empty, and label. So a label may have several rows, one per entity type, and a write must find its row through the function `sea` with all three words. Finding an a_type by label alone was the double-write bug that held the split back until S4c.

Vocabulary lives in the avs table. When an aspect is enumerable, a fact stores the av id, not the text. Law Two says enumerable values are vocabulary references and named things live in gears, so the ops gears are kept strictly free of text. The registry row that names which gears keep that strictly is the shomer row.

The fold is `mailroom.reduce`. It reads the folds word on the aspect and folds the stream accordingly: last takes the newest value, sum adds every delta, union joins every distinct value, ranked takes the top rung, and standing_no is a NO that stands until a release aspect clears it. Nothing on harry today reads ai_prompt. Fifty two a_types in the militaria division carry one, and no function looks at them.

A finding worth carrying: the folds word is null on five thousand five hundred and four of the five thousand six hundred and thirty four a_types on harry. The reducer coalesces a null fold to last, so every one of them behaves correctly, but the declaration is missing. That is a Law Five gap, not a Law Six one, and it is why the migration pre-flight now requires every new a_type to state dtype and folds.

| The folds and dtype words across every gear's a_types, measured 2026-09-10. Word | Count | Word | Count |
|---|---|---|---|
| --- | --- | --- | --- |
| folds null | 5,504 | dtype null | 5,413 |
| folds last | 106 | dtype text | 74 |
| folds union | 17 | dtype uuid | 48 |
| folds ranked | 3 | dtype decimal | 37 |
| folds standing_no | 3 | dtype timestamptz | 26 |
| folds sum | 1 | dtype int | 25 |
| | dtype bool| 10  
| | dtype verbatim| 1  
  
### The family strip and R-059

A physical thing in EMET is described on four levels, which we call the family strip: the form, the item, the lot, and the allocation. The form is what does not vary by variant. The item is the variant. The lot is one physical batch a tenant holds. The allocation is that lot offered on one marketplace. Ruling R-059, given by Mike on the ninth of September, says every aspect lands on exactly one level of the strip and is read from there by a join up the strip, never copied down. Object type lands on the form and stays there; a lot reads its form's object type by one join. The word kind died as a name. Object type stays as the fact, item type stays as the top level, and the same rule applies to category and theme when they come up.

The reader for that rule is `mailroom.strip_fact`, migration 1463, live since the ninth of September. It takes a set of uuids and a label and walks up by parentage across gears, returning the value from the first level that carries it. It was cleared as an executor because rows could not do the job: the rows-only walk over forty six thousand lots took two hundred and ninety three seconds against a door timeout of fifteen seconds, and strip_fact does it in under four. The cause was in gear_of, which loops over every registered gear per hop per row.

## 4.The drain

Work enters the engine through the hopper. When a row lands on the entity, eav or boms table of a hopper gear, the trigger `trg_hopper` calls `to_queue`, which writes one job into `ten_routines.run_queue`. A birth on the entity table is the first fact, so it queues as a birth. Jobs for the same entity merge into one post. The engine does not post about itself: the registry, the routines schema and the mailroom carry no hopper.

The queue has exactly one consumer, `drain`. It takes up to a cap of jobs in sequence order with select for update skip locked, so several drains can run at once without touching the same job. The status column on a queue row is only the claim: queued becomes taken, and that is the only update the drain ever makes. Everything else about what happened is appended to `run_attempt`, one row per try.

For each job the drain becomes the tenant the job carries, sets the configuration key emet.tenant for that transaction, and dispatches by executor name. A receive job unwraps an arrival. A compose job builds an outgoing package. A go job handles a reply. A post job is a landing, and it calls `land` with the aspects that arrived, birth first. When the job ends, the tenant key is cleared. The rename from post to land is owed as a row and has not been made; the queue still says post.

There are four outcomes and a fifth holding word. Pass means the executor returned. Fail is a verdict the executor itself returns, never an error class; the attempt records the verdict. Strike is an exception: it is recorded and the same job goes to the back of the queue as a new row in the same lineage. Quarantine is the Nth strike, where N is the ladvar queue failures to quarantine, three at base; the job stops and waits to be looked at. Held is an attempt row on a job that stays queued and untaken. The outcome vocabulary is rows in the routines gear, and the drain raises if any of the five words is missing.

Law Eight, yes unless no, is what makes this safe to leave running. A thing that exists but is unqualified is inert. A job proceeds unless a standing NO objects, and a NO is written by an observation, never a switch. The compliance hold in the drain matches a job against executor, passport, actor, entity and space; the 2026-09-02 review found it had held nothing in four hundred and forty thousand jobs and could not, and the fifteen dead NOs were revoked. Silence means yes.

The queue and the attempt ledger, measured 2026-09-10. The queue was truncated 2026-08-26; the oldest row is 2026-08-27, the newest 2026-09-09 19:52 UTC. Measure| Count  
---|---  
run_queue rows| 509,969  
status done, legacy before 1333| 440,837  
status taken, the resting state since 1333| 69,128  
status failed, legacy| 4  
status queued| 0  
executor post| 488,169  
executor go| 13,553  
executor receive| 7,716  
executor compose| 531  
run_attempt rows| 509,969  
outcome pass| 508,607  
outcome fail| 1,358  
outcome strike| 4  
outcome quarantine| 0  
outcome held| 0  
last twenty four hours: pass| 3,857  
last twenty four hours: any other outcome| 0  
  
The PLQ stream is the quantity and price part of that flow. PLQ stands for price, location, quantity. Since 1379 it rises from a marketplace line to the lot, folds at the lot, and fans out to every allocation. Section six says what the words are.

## 5.Landing

A lands entry is a small object on an a_type row. It says `to`, the uuid of the far thing, which is either an endpoint or a passport. It may say `as`, the aspect name at the far end, `via`, a list of edge labels to walk, `how`, the name of a ladvar that shapes the value, and `op` with `says`, a test on the aspect's own standing that gates the entry. A birth entry says `to` and `passport` and, for a directed edge, `edge`.

A passport is an entity of type etype that describes how a package becomes entities. Its words are avs on the passport: `becomes`, the entity type born; `at`, the dotted path in the package where the nodes sit; `id_field`, the key when there are no cues; `edge`, the bom type that binds the born thing to its source, has_twin by default or a directed one like lot_has_allocation; `twin_uuid`, the next passport in the chain; and `parent`. Under the passport sit child rows: entities of type cue, each naming an aspect to recognise by, in order; entities of type edge, each three words, label, of and by, for entity-to-entity facts found by a cue; and mapping rows for the fields.

`resolve` is the birth executor. Given a source entity and a twin passport it finds the twin's gear, reads becomes, id_field, edge and twin_uuid, works out the parent in the twin's gear, tries the standard identity first, then runs the cues in order, and mints if nothing recognised. Then it draws the edge. For has_twin it writes the edge in both gears. For a directed edge it writes one row in the twin's gear with the twin as parent and the source as child. Then it births the twin with its face from the passport's has_field rows, and follows twin_uuid to resolve the next link with the same cue source.

`land` is the fact executor. For each aspect handed to it, birth first, it reads the lands entries. If an entry has an op, it tests the aspect's standing and writes a breadcrumb when it skips. An endpoint entry queues a compose job carrying that aspect, and since 1359 a statement to an endpoint is always due: the cascade tells every marketplace including the source, and the ring is stopped by arithmetic rather than by remembering where a value came from. A birth entry calls resolve once per target. An entity entry folds the value with reduce, resolves how as a ladvar at the entity's own space, walks via to the far ends, and writes there only when the value differs from what the far end already holds. An unchanged value is not a landing.

The door conversion is the how word `minus_fold`. Ops may not hold a level, so a level arriving from outside becomes a change: the change equals the level minus what the destination believes, measured per far entity inside the write loop. At a birth the destination believes nothing, so the whole level becomes the opening delta. The ladvar `how:opening` carries minus_fold and `only_when_unknown`, which is a standing NO that keeps the conversion armed only where the destination believes nothing. Two other how rows exist today: `how:sold` with scale minus one, and `how:qty`, which is empty and means identity. The when and emit pair that land also supports has never been used by any row.

The twin is how identity crosses gears. A marketplace allocation and its ops allocation are two entities joined by has_twin, and the ops allocation is joined to its lot by lot_has_allocation. On harry the eBay gear holds twenty eight thousand seven hundred and twenty six has_twin edges, the Woo gear a hundred and thirty six thousand six hundred and forty five, and the ops gear a hundred and fifteen thousand six hundred and twenty eight lot_has_allocation edges. Mike's ruling of the tenth of September closes the question of what propagates: twins carry identity and lands carry facts. If X is a thing, it is a twin. If X is a fact about a thing, it is a landing. If the answer is copy it, the answer is wrong.

Live lands entries in three gears, measured 2026-09-10. Uuids shortened. Gear · aspect| Entries  
---|---  
mp_wooc · alloc · qty_is| to endpoint d446ce8d as stock_quantity; to passport 3b1e1eda as qty_chg, how how:opening, via has_twin then lot_has_allocation  
mp_wooc · alloc · birth| two entries to passport 8ebe0d1b with edge lot_has_allocation  
mp_wooc · price_is| to 8ebe0d1b as price_is via has_twin  
ops_com · alloc · price_is| to 3b1e1eda via lot_has_allocation, rank mp:rank, rank_by has_twin  
ops_com · qty_is| to dcd658ae and to 2a7f11cc as qty_is, how how:qty, via has_twin  
ops_com · lot · qty_is, folds sum| to 8ebe0d1b as qty_is, how how:qty, via lot_has_allocation  
ops_com · alloc · birth| to 3b1e1eda, passport 8ebe0d1b  
  
## 6.Quantity, price, location

There are two quantity words and one price word. `qty_is` is a level, what is true now; on an allocation it folds last, and on the lot it folds sum over its own changes. `qty_chg` is a change, an event, what a level is folded from. `price_is` is a level and folds last; there is no price change word, because a price is replaced, not accumulated. Mike's line was sum, replace, replace. These went live in 1379 through 1385 on the fourth of September.

Location is not on the allocation at all. MilAntTor has one location for everything, so location is a ladvar at tenant scope, the broadest scope at which it is true. An allocation that genuinely sits elsewhere gets a row and wins. The birth mints two values, not three.

The sources of truth are split by home. The lot's quantity is the ops fold. The allocation's quantity is the marketplace's own echo, the number the marketplace last confirmed. The tenant sees both through views. Ops never asserts an allocation number, and since 1380 a marketplace can no longer overwrite a lot. Mike's quantity spec of the eighth of September makes quantity a live fold, the newest level plus every newer change, compared and landed at every hop, changes up and levels down, with no opening dial.

Three hazards keep the door conversion unarmed beyond births. The race: a sweep observed before a sale but arriving after it would erase the sale, and guarding it needs the sweep's own clock. The second voice: two marketplaces correcting each other forever, now that the ranked echo is gone and nothing replaced it. The concurrent double open: the drain skips locked rows, so two allocations on one anchorless lot can both read nothing and both open it. Until those are rows, the only_when_unknown NO stands.

Two measurements from the ninth of September bear on this. Every one of the hundred and twenty two eBay quantity writes was an echo of eBay's own number, and none of the twenty eight thousand qty_is rows was ever written by a drain run. And ninety nine negative qty_is rows stand, sixty two of them in ops. The question of which path may say a quantity to a marketplace, the ops fold or the marketplace's own last reply, is recorded as unruled.

## 7.The match

Recognition is claim, then confirm. A claim says who I say I am: either EMET's own identity in the marketplace's slot, which is the standard and settles it, or a code such as a sku, which claims a lot. Neither is trusted alone. A claim must survive two confirmations: the picture, whether the candidate's hero image is in my gallery, and the title, whether it agrees. Claim plus picture plus title means identical, bind. Claim plus picture with a different title means the same form, its own item, and the title goes to the parser for the variant. No claim, picture agrees, and titles agree up to a trailing remainder means the same form and a new item and lot beneath it. Picture agrees but titles diverge inside and both continue means a stock photo on unrelated things: reject the picture, mint separately. Ambiguous goes to a human, never guessed.

In the engine `is_new` is the recognition call. It takes the cues the recipe named and looks each up through the index on space, e_type and native_id. No cue declared means every arrival is new; the recipe said so, not the engine. Since 1461 the gear's own configuration space is derived from the gear rather than passed, so no caller can name a space and reach another tenant.

The live defect here is the parentless lot mint. Migration 1288 moved the ops alloc-to-lot passport's id_field from uuid to sku on the twenty eighth of August; its partner 1289, which would have keyed every lot on its sku, was claimed sixty two seconds later and is still not deployed. No lot carries a sku, so the cue can never match, and every arrival on that passport mints instead of recognising. On harry today four thousand and twenty nine lots are parented straight to the MilAntTor root with no item and no form, and three thousand and fifty one items hold two ops lots. The fix is named as one ruling, rows plus the drain line, and is not dispatched.

## 8.Tenancy and the door

The terminal door is the Lambda `emet_terminal_door`. It has four gates. Gate one is deployment scope: the door serves one tenant prefix, and an op outside it is refused. Gate two is the registry: the op must name a call row in the emetweb gear, and the allowlist is those rows. Gate three is who: a Cognito token is verified against the pool's keys, which are rows, and resolved through `upr_signin` to a grant in that tenant's own rows; enforcement is the auth_mode row. Gate four is become the tenant: the door calls `tenant_session`, which answers a role name and a gear uuid or nothing, sets emet.tenant to that uuid, and runs SET LOCAL ROLE so the session dies with the transaction. Then it calls `ask`, which runs as that role. A statement timeout of fifteen seconds is set per request.

Four tenant wall roles exist on harry, each named from a gear uuid. The six base roles, Owner, Inventory, Fulfillment, Support, Sysop and Debug, are entities in the base tenant gear, and a grant in a tenant gear points at them by uuid; nothing is copied. Holding a role today means the door knows you; rights finer than that do not exist yet.

Law Nine promises that a tenant's schema holds one tenant's private data, that nothing pushes data in, that wides are views over the fold, and that the tenant wall is enforced at the view grant. What is built: the wall role, the space uuid on every row, the emet.tenant key, the drain becoming the tenant per job, and the hardening of `ladvar` so a null scope means the session's own tenant. What is not built: the views. There are zero views in any usr schema on harry today. The wide views the ancient model had went with it, so the wall is absent at the view layer rather than breached. The operator read, a named base read for what the system says regardless of tenant, is also not built; a tenant session cannot read base at all, since tenant roles hold no usage on the registry.

Settings are ladvars, Levers and Dials variables. They are entities of type ladvar in the registry gear, sixty six entities carrying forty four distinct names, and they resolve base to gear to tenant through `mailroom.ladvar`. The names read like the system's control panel: the clocks for each sweep, the Cognito pool, the rank of marketplaces, the valet's carry cap, the queue's failure threshold, the three how words, the engine vocabulary, the gap thresholds, the theme and skin. A separate table `mailroom.var` holds a hundred and twenty six rows, and ladvar does not read it.

Ladvar names on harry, measured 2026-09-10. Sixty six entities, forty four distinct names. Names  
---  
auth_mode · chip:edge · chip:ink · clock:biblio_listing_drain · clock:biblio_order_sweep · clock:biblio_upload_poll · clock:ebay_listing_events · clock:ebay_listing_sweep · clock:ebay_order_sweep · clock:woo_order_sweep · clock:woo_product_sweep · cognito:client_id · cognito:domain · cognito:jwks · cognito:pool_id · elect:mp_ebay · engine:lens_label_aspects · engine:vocabulary · gap:alloc · gap:alloc_thresholds · gap:lot · gap:lot_thresholds · gap:wire · gap:wire_thresholds · how:opening · how:qty · how:sold · match_bits_exact · match_bits_review · mp:rank · orders:open · orders:qty_since · queue:failures_to_quarantine · rail:window · roster:page · routes:axes · scale:surface · skin:surface · sync:cascade · sync:stream · sync:sweep · tenant_resolver:ebay · theme:surface · valet:carry_cap  
  
## 9.The cache that was torn down

Until the ninth of September the tenant panel read from two cached tables in the ops gear, lot_state and order_state, kept by a function called refresh that folded the tail of the stream past a mark. The mark last moved on the twenty eighth of August. Refresh took seventy two seconds against the door's fifteen second timeout, so the fold never committed, the door rolled it back, and the panel told a tenant that month-to-date sales were zero for eleven days. The tile for object type read the item, where the aspect never was, so it had said one hundred percent unclassified since it shipped.

Law Nine names the fault in its own words: if a usr surface can be stale, it was pushed. A cached verdict is a second home for a derivable fact. Mike's ruling was that the new quantity takes care of it, and the cache goes under Law Ten. Four files did it in one chain from his keyboard at sixteen thirty three UTC on the ninth: 1469 tore out the committed proof fixtures that were serving a real tenant a fake theme, 1433 dropped refresh, order_state, the fold mark and five registry rows, 1440 rewrote the panel body to derive object type up the strip and deleted the kind ladvar, and 1464 dropped lot_state. 1463, the strip reader, had gone first at fifteen fifty. On harry today lot_state, order_state and refresh do not exist, and the panel reads seventy three object types where it read unclassified.

What replaced it is strip_fact and a live fold. The rows-only walk measured two hundred and ninety three seconds; strip_fact measures three point nine. Forty two thousand lots live-fold in one pass in under a second. The Lambda's fold block was removed in the same move and deployed on the ninth with a rollback version kept; Mike's signed-in read proved zero refresh lines in the log.

The cache teardown, from the ledger and the engine lead's measurements. Measure| Value  
---|---  
rows-only walk of obj_kind over 46,940 lots| 292.8 s and 313.4 s, two runs  
strip_fact over the same| 3.86 s cold, 3.90 s warm  
door statement timeout| 15,000 ms  
refresh, the old fold| 72 s  
live quantity fold, every lot, one pass| 731 to 904 ms  
lot_state rows dropped| 42,911  
order_state rows dropped| 10,168  
chain deployed| 1469 → 1433 → 1440 → 1464, one transaction, 2026-09-09 16:33:39 UTC  
  
## 10.The postal side, as the engine sees it

Outside harry there are three Lambdas the engine treats as a contract. The gatehouse is the public door: it validates what arrives and stamps it. The courier is the hand that makes calls to marketplaces and carries replies back. Reception is inside the private network and deposits every arrival whole into `mailroom.central_routing`, where the route trigger names the passport and the engine's receive job unwraps it. A hundred and seven thousand arrivals stand there today, nine hundred and seven of them in the last day. Outbound, the drain's compose job builds a package, `send` records it as an envelope, and the valet, a baton door in reception, carries it to the courier; sixty thousand outgoing calls are recorded.

There are two kinds of inbound. A push is the marketplace telling us something happened: an order confirmation, a listing event. A sweep is us asking on a clock: get seller list, get orders, list products. Mike's rule is that pushes drive and everything else verifies. The sweeps are not per tenant; everything is the app, and reception takes care of tenancy before routing. Push health is therefore a comparison, not a clock: a push lane shows red the moment a sweep lands a fact on an allocation that no push had already reported, with the allocation and the fact as evidence. Green is the sweep confirming only what pushes already said. No hours row, no watch and worry multiples for pushes. The oversell watcher runs beside all this every ten minutes with no database and writes through the courier; its kill switch is one Lambda variable.

## 11.What is live right now, and what is owed

Live: the six-table gear on twenty one hopper gears, the drain with its four outcomes, SEA rows with the strip reader, the PLQ words, the door with four gates, the cache gone. One thousand three hundred and twenty two migrations are deployed of one thousand four hundred and fourteen ledger rows; the newest to land was 1466 at sixteen thirty six UTC on the ninth, which tore down the militaria field map under R-059.

The outbound chain is the thing in front of us. Mike asked for a lot dropped in to land in the marketplaces that qualify, staged as rows on the existing birth executor plus an election ladvar. The seeder staged it under rollback and the chain fires: one ops allocation and one marketplace allocation per lot, no executor touched, zero jobs queued. Two blockers stop it, both the engine desk's. Blocker one: `resolve` writes the directed edge with the twin as parent, unconditionally. Inbound that is right, because the allocation is the source and the lot is the twin. Outbound the roles swap and the row comes out as parent alloc, child lot, against a hundred and fifteen thousand live rows that read parent lot. There is no word to reverse it. Blocker two: there is no election. The lands array is fixed on one a_type row resolved by gear and entity type alone, and the birth branch of `land` reads no ladvar, no rank and no scope, so a MilAntTor lot births into TorBri's Biblio gear. No row can decline that birth. The election ladvar `elect:mp_ebay` exists as a row today; the birth branch does not read it. Mike's word waiting is clear, for the two executor lines.

1451 and 1452 are the api call observer. 1451 would land a call's status as rows from the Ack inside a reply, and it has been refused twice as rows because the reply must resolve to the apicall entity and the only mechanism that can, identity_at, is fenced to the tenant space; 1461 widened `is_new` but not `receive`, so a third small engine line is named. 1452 retires four timestamp aspects that cannot be landed and need not be, since created_at already is the clock; it is held on Mike because it deletes rows.

The lawcheck brief: Law Seven's probe A scans a schema called ten_registry that does not exist, so it has reported clean on every run without comparing the blessed executor list to a single live function. Ninety of the hundred and twenty eight baseline keys name the dead schema, and the other thirty eight name schemas the probe never looks at. The brief is written and the fix is not.

The union retraction word: a union fold joins every distinct value ever appended, with no supersede and no release term, while standing_no right beside it does have a release. So a value that enters a union can never leave it. Seventeen aspects fold as union today. Naming the word is a ruling owed to Mike, not a desk's choice.

The folds-null finding is above: five thousand five hundred and four a_types fold by the reducer's default and not by a row that says so.

The sixteen militaria words: when the division template was seeded in 1467, the object type vocabulary was harvested from militaria's hundred and two words down to seventy nine, and sixteen of those are militaria-flavoured: battledress, bayonet, cap badge, collar badge, death penny, gas masks, iron cross, magic lantern slide, nasa patch, powder flasks, rank slides and epaulettes, shoulder title, sniper tape, trench art, victory medal, webbing. They stay unless Mike strikes them, because a vocabulary is a menu, not a claim.

The division family: D1, migration 1459, is live from Mike's keyboard and bore six schemas: div_ as the reference base, div_next as the mint template, and div_sff, div_lego and div_amgirl as divisions, all empty, with the old enum torn down and the broken gaps view dropped. D2, 1467, is live and seeded div_next with twenty four aspects, three hundred and sixty six values and five bom types. D3, 1470, is the mint: the three divisions born as copies of div_next's vocabulary, with a proof that no a_type or av uuid is shared between gears; it is on its seventh cut with the mashgiach and Astra and not yet deployed. D4 moves the sixteen thousand concepts and ninety seven relations of central_div into div_ as entities, and D4b drops central_div. D5 is each division's rows beyond the baseline, its segment addresses, and the enrichment arming as ladvars. D6 is the first book, end to end, traced through the rows afterwards.

| The migrations named in this section, from the ledger on 2026-09-10. Id | What | State |
|---|---|---|
| --- | --- | --- |
| 1289 | the catch-up: lots re-keyed on the tenant code | claimed 2026-08-28, never deployed |
| 1428 | the listing doorbell asks GetItem | KOSHER, held on Mike |
| 1429 | land speaks at every exit | live |
| 1433 | the cache torn down: refresh, order_state, fold mark | live |
| 1440 | kind dies as a name; object type read up the strip | live |
| 1451 | the api call observer as rows | held; needs a third engine line |
| 1452 | retire the four timestamp a_types | held on Mike, row delete |
| 1459 | D1, the division family born | live |
| 1461 | receive stamps apicall; is_new admits the gear's own space | live |
| 1463 | the strip reader, mailroom.strip_fact | live |
| 1464 | drop lot_state | live |
| 1466 | the militaria field map torn down | live |
| 1467 | D2, div_next seeded | live |
| 1469 | the committed proof fixtures torn out | live |
| 1470 | D3, the mint | in review, not deployed |
  
## 12.Glossary

a_type
    One row in a gear's a_types table: an aspect, addressed by entity type and label, carrying dtype, folds, arrives, lands and ai_prompt.
alloc
    An allocation: one lot offered on one marketplace. It has a marketplace face and an ops face, joined by has_twin.
arrives
    The IF on an a_type: which passport and which path in an arriving package the aspect is read from.
av
    One vocabulary value an aspect may take. A fact stores the av id, not the text.
bom
    An edge between two entities, typed by a bom_type such as has_twin or lot_has_allocation.
cue
    A child row under a passport naming an aspect to recognise an entity by, in order.
drain
    The one consumer of the queue. Takes jobs, becomes the tenant, dispatches the executor, appends the attempt.
edge
    The bom that binds a born thing to its source; has_twin by default or a directed one named on the passport.
executor
    One of the fixed functions the drain dispatches. The only code in EMET, and each is cleared with Mike.
fold
    Reducing a stream of appended values to one current value, the way the aspect's folds word declares.
gear
    One schema of six tables: entity, eav, a_types, avs, boms, bom_types. Every gear has the same shape.
hopper
    The trigger on a gear's three tables that turns every landed row into a queue job.
ladvar
    A Levers and Dials variable: one named setting, resolved base to gear to tenant, most specific wins.
lands
    The THEN on an a_type: where the fact goes next, to an endpoint, a far entity through via, or a birth.
lot
    One physical batch a tenant holds. Its quantity is the ops fold. It sits under an item, which sits under a form.
passport
    An etype entity describing how a package becomes entities: becomes, at, id_field, edge, twin_uuid, plus cue and edge children.
PLQ
    Price, location, quantity: the three live values on an allocation, first class since 1379.
reduce
    The function that folds. It reads the folds word on the row and never decides for itself.
SEA
    Schema, Entity type, Aspect: the address of every fact, and the unique key on a_types.
strip
    The family strip: form, item, lot, allocation. An aspect lands on one level and is read up the strip.
twin
    The same thing's face in another gear, joined by a has_twin edge. Twins carry identity; lands carry facts.
usr, mp, div
    The three kinds of root: a tenant, a marketplace, a division. Every parent chain ends at one of them.
wide
    A view over the fold that shows an entity as one row. Law Nine's promise; not built on harry today.

As of the tenth of September, 2026. Sources: the Ten Laws in docs/EMET-ARCH-LAWS.md; the memory files project_engine_lead.md, project_plq_verbs.md, project_qty_roles.md, project_ops_qty_hub.md, project_the_match_sequence.md, project_twins_carry_identity_lands_carry_facts.md, project_push_health_is_a_comparison.md, project_seeder.md, project_edmund.md, project_the_four_outcomes.md, project_the_new_engine.md, project_sea_split_s4c.md, project_the_comm_redesign.md, project_base_roles_are_grantable_without_code.md and project_the_walk_and_the_valet.md; ruling R-059 in docs/rulings.md; the division plan docs/superpowers/plans/2026-09-08-division-family-stood-up.md; the lawcheck brief at commit 0d2b2af5; the outbound proofs at commit 071af483; migration 1467's header on branch seeder/d2; the terminal door handler in aws/resources/lambdas/emet_terminal_door; and read-only queries on harry through scripts/rds.sh on the tenth. Every count in a table came from a query run that day, except the cache-chain timings, which are the engine lead's measurements of the ninth as recorded in memory. Where memory and harry disagreed, harry is stated: the folds-null count is five thousand five hundred and four across every gear today against the four thousand seven hundred and seventy five the engine lead measured over four gears; the ladvar count is sixty six entities and forty four names against memory's sixty seven; the parentless lot count of four thousand and twenty nine agrees.
