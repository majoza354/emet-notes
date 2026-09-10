# The Engine — One Loop

The design of the EMET engine as ruled on 2026-09-10, in the voice session and at the
keyboard. Law 10 framing: this is the structure intended, not the structure as built. It says
what the engine is, not how the current one is torn down to reach it; that is the plan's job.
Written to be read aloud.

## What the engine is

The engine is a small set of fixed readers and one writer. It moves rows and never looks at
them. Every behaviour is a row: what counts as new, how a value folds, where a landing goes,
what a value becomes on the way, which marketplaces a lot qualifies for. The only code is the
executors, and there is no executor left that judges.

The unit is the gear: one schema, six tables, the same six everywhere. entity, eav, boms,
a_types, avs, bom_types. Because the shape never changes, the engine never knows which gear it
is in; it asks the rows.

Every entity row carries `tenant_uuid`, the uuid of the tenant's root entity in its usr gear.
It is stamped at routing and it is a column, never an aspect, because the tenant wall is a
WHERE clause on every read. On every tenant-scoped row it is not null, by convention, not by
schema: the scribe stamps it on every tenant-scoped birth and pre-flight refuses a
tenant-scoped candidate without one. A gear's own rows, its passports, api calls, fields,
vocabulary, belong to the gear and carry none.

Two things have two names. The strip, form, item, lot, allocation, is ops. What a
marketplace holds is a listing, the allocation's twin. A listing's parent is the tenant root;
an allocation's is the lot; a lot's is the item; an item's is the form; a line's is the twin of
its order. Each gear states its own parentage from what it knows locally.

## The two stations

Format lives outside harry, at two symmetrical points, both keyed by the same dialect word
the gear puts on the call.

- **The courier**, outbound: renders the standard package into the dialect the call names,
  flat file, XML, JSON, and puts it on the wire.
- **Reception**, inbound, for a reply or a webhook: parses the dialect back into the standard
  package and hands it to routing.

Inside harry there is only the standard package. A passport reads that and nothing else. The
gatehouse, before reception, authenticates at the border and stamps the waybill; it never reads
a payload.

## The loop

One shape, all the way round. A package starts it; every consequence re-enters it.

1. **Routing.** The package arrives with its waybill. Routing resolves the tenant from the
   credential and stamps `tenant_uuid`; the route rows say which gear it belongs to. The
   package is deposited whole in that gear as an arrival. That deposit is the one write
   outside the drain: nothing received is ever lost (Law 1).

2. **Unwrap.** The passport says what the package is and where each line goes: which table,
   which entity type, which aspect. Every line becomes a candidate row.

3. **is_new, the pre-flight.** Every candidate row on every one of the six tables gets its
   own table's check. Cheap, deterministic, no judgement:
   - entity: (tenant, e_type, native id), or our own uuid coming home
   - eav: the fold (see is_diff)
   - boms: (edge type, parent, child)
   - a_types: (e_type, label)
   - avs: canonical value
   - bom_types: label

   An entity not found is a birth and is queued. An edge or a vocabulary row already present
   trips and nothing happens. A known entity carries on with its facts.

4. **Queue.** The candidate joins the package's one job. One package, one job; is_new and
   everything after it run per row inside that job. This is a rule, not an optimisation:
   without it a hundred-line sweep is a hundred jobs.

   Jobs are ordered by the package's own clock, the time the marketplace reported, never
   by arrival (the quantity ruling of 2026-09-08: a fact beats a preference). Jobs for one
   tenant and one marketplace run one at a time in that order. That is what makes pre-flight
   sufficient: a push and a sweep carrying the same new listing are two packages on one
   stream, the earlier clock mints, the later finds it and binds. There is no mint race to
   lock against, because there is no concurrency on an identity.

5. **Fold.** The standing of that aspect on that entity, read from the stream. The fold is
   what the pair rule reads: a transform that turns an absolute into a delta needs the
   standing it is measured against. A birth has no fold and skips 5 and 6.

6. **Pair rule.** The cross-table, one row per (origin SEA, destination SEA), with an
   ordered list of verbs, because one movement is often several: elect the destination,
   transform the value against the fold, match the birth, bind or mint. The verbs:
   - **route**: which destination gear, by election
   - **xfm**: through a named transform row, by id, never by name, reading the fold
   - **drop**: this pair never moves
   - **match**: for a birth only, test whether the destination already exists by the
     destination's identity rule (a SKU across marketplaces, a picture, a code), reading
     the foreign id to recognise, never to key on. Found: bind with an edge instead of
     minting. Not found: mint. This is the one place the loop looks backward, and it is the
     hard no to a duplicate mint.
   - **pass**: the value lands as it is

   A pair with no row does not move. The executor never chooses a verb; a missing row is
   silence, and silence is no movement.

7. **is_diff.** Does the transformed value move the destination's standing. No: it is
   dropped; a confirmation is not a value, and the clock of the confirmation belongs to the
   arrival, or to a seen aspect whose is_new word is `always`. Yes: it is an event. Ten then
   eight: the fold says ten, the xfm makes minus two, is_diff says it moves, minus two lands.

8. **Compliance.** Before the write: may this row land. The standing NOs on the entity, the
   allocation, or the qualification edge hold the row in the ledger, visibly, until a landing
   clears it (Law 8). A NO is never edited away.

9. **Write.** The scribe writes the row, draws the edge the row names, writes the face. A
   birth takes its parent from the word on the birth row: `root`, `strip`, or
   `twin_of_parent`. Nothing is transported from the source. The attempt is stamped: one job,
   what landed, what held its standing. A write that did not happen is a run that did not
   pass, visible, stoppable, re-runnable from the package.

10. **Alert.** The hopper fires on the write, on all six tables, carrying the folded standing.
   The machine is downstream of the trigger by construction; it is never told.

11. **Consequences.** The machine reads what this landing owes, keyed by the origin SEA:
    a landing in another gear, an edge, a NO, an outgoing call, a birth. Every consequence is
    a candidate row about what should exist next, and every one re-enters the loop at step 3.
    A landing that owes nothing is the common case, and silence is the right answer.

    A job is done when its own candidate rows are written or dropped. Every consequence is
    its own job, stamped with the run it came from, so the chain reads back to the package
    without the package's job staying open through calls and replies. A cycle cannot form:
    the pair rule is origin to destination, and a pair whose origin and destination are the
    same aspect is a row nobody writes.

12. **The exit.** A consequence that is an outgoing call composes the standard package from
    the rows; the courier renders and sends; the reply returns through reception to step 1.

There is no inbound and no outbound, no push path and no sweep path, no twin logic separate
from birth. Direction was an accident of which side arrived first.

## The vocabulary

An a_type carries four words:

- `folds`: how the stream reduces to a standing (last, sum, ranked, …)
- `is_new`: `identity` for an entity, `label` for an aspect, `fold` for a value (the default),
  `always` for a stream aspect where every observation is an event
- `lands`: where a landing goes, one entry per destination
- `arrives`: which path in a parcel routed to this passport the SEA sits at

The cross-table carries the fifth word for each pair: the ordered verbs, and the xfm row each names.

A passport carries `becomes`, `at`, `id_field`, `many`, `ord_field`, `identity_at`, and the
parent word. Its cue rows are the identity rules the match verb reads.

## Election

For every tenant, every lot gets one allocation per qualified marketplace, and the edge
`lot_has_allocation` is what says so. The marketplace listing is the allocation's twin.

Qualification is division × marketplace, one edge per pair in the tenant's gear. A lot's
allocations are the consequences of that edge: born with the lot, appended when a marketplace
comes on line. Turning a marketplace off is a NO on the qualification edge; a refusal for one
lot is a NO on its allocation. Nothing is deleted, and no edge goes missing to say no.

The boms table is the join table. Lots × marketplaces, at most ten marketplaces per tenant,
read by the walk everything else uses.

An allocation's identity is (tenant, lot, marketplace): the lot is its parent, the
marketplace gear's uuid is its native_id, the tenant is on the row. Pre-flight for a
strip-born entity is (tenant, parent, e_type, native_id), one index lookup, so a second
allocation for the same lot and marketplace trips and is never minted. Today an allocation's
native_id is a random uuid and 56 lot-and-marketplace pairs carry two; that is the shape being
torn down.

## Land everything

Every field that arrives lands in the gear it arrived in, whether or not anyone wants it yet.
An aspect with no home is minted as an empty a_type: label, entity type, dtype, folds null,
no lands. When someone wants it, the words are filled in and the hopper replays what already
landed. Backfill is a lands row; it costs nothing.

The five thousand null-fold a_types on harry today are this shape working.

The hopper stands on all six tables in every gear, the three administrative gears included,
and the cross-table decides what a write owes. A write to a_types owes one thing, the
replay: land what already landed under the new word, from the clock the lands row names;
a lands row with no clock replays nothing. A write to avs or bom_types owes nothing. No
pair rule points a gear's vocabulary back at itself. Scoping is rows, never a list of
tables the trigger skips.

## What holds the loop

- **Yes unless no.** Nothing waits for permission. A NO row is the only brake, and it is
  cleared by a landing, never by an edit.
- **Append only.** A correction is a new row. A duplicate that pre-flight missed is recorded
  and later bound, never merged.
- **The tenant wall is on the read**, at the grant, on `tenant_uuid`. A cross-tenant leak
  needs a write that never exists. The write contract is that every reference in a
  candidate row was obtained by a read the wall allowed: pre-flight resolves an edge's
  ends, a fact's entity and a birth's parent as the job's tenant, and a uuid the tenant
  cannot see resolves to nothing and the candidate is refused before the write. The scribe
  runs as the job's tenant and the policies carry WITH CHECK as the backstop; the gear's
  own rows are written as the gear, never inside a tenant's job.
- **The ledger is accountable.** Every write happened inside a run, and the run row says so.

## The executors that remain

- the door: deposit the package, stamp the tenant
- pre-flight: six checks, one per table
- the fold: reduce
- the scribe: write, edge, face
- the hopper: the trigger on six tables
- the machine's lookup: read the cross-table for a landing and emit candidate rows
- compose: build the standard package for an outgoing call

Plus, outside harry, the courier and reception, one job each.

Everything else that exists today, the recognition loop inside resolve, the cue walk inside
is_new, the election read, the rank and xfm and via logic inside land, the response
processors, is either a row in this design or gone.

## Ruled

- The machine is **the_machine** (Mike, 2026-09-10).
- **No family square.** family_uuid and item_uuid on ops_com.entity (family_uuid on ops_cat)
  are torn down under phase 6; the strip walk is three indexed joins and the parent word
  makes the strip explicit. A column holding what the parent chain already says is a second
  home for one fact (Mike, 2026-09-10).

## Open

- **The replay bound.** A lands row added to a hot aspect replays what already landed; how far
  back is a row, and its shape is not yet said.
- **The dead routine rows** in the registry that still say gear.alloc: read by nothing,
  brought into spec when touched.
