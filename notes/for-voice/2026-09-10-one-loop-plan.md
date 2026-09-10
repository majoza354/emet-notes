# Bringing the engine to the One Loop — the plan

Source: the design, `docs/superpowers/specs/2026-09-10-the-engine-one-loop-design.md`, as
ruled by Mike 2026-09-10, and phase zero's counts,
`docs/superpowers/briefs/2026-09-10-one-loop-phase0-measure.md`. Law 10 throughout: the old
shape is torn down, not patched, and the teardown is not a cost.

Every phase is: engine lead briefs → seeder writes rows (mashgiach, the pipe on
mashgiach + Edmund) → an executor change is one named unit cleared by Mike → tracer proves
it on harry with one lot → Astra reads every executor change. DDL is Mike's keyboard.

Mike's "engage", 2026-09-10, starts the plan at phase 1.

## Phase 0 — Measure — DONE 2026-09-10

The page of counts is on main. What it sized: phases 1–2 small; 3 the real build, with no
`held` precedent in 510,722 attempts; 4 is 63 triggers; 5 is nine how words; 6 is the one
four-figure teardown. Two things it found that the design has since ruled: the family square
is a self-reference (torn down in 6); 56 lot×marketplace pairs carry two allocations (the
allocation identity in 2 makes that impossible).

## Phase 1 — The parent word on the birth row (closes blocker one)

Rows: a vocabulary of three words, `root | strip | twin_of_parent`, and one word on every
passport's birth entry. listing → root; allocation → strip (lot); lot → strip (item);
item → strip (form); line → twin_of_parent.

Executor: one line in `ten_engine.resolve()`: read the word, transport nothing.

Proof: a lot dropped into TorBri births its marketplace listing under the tenant root, and an
inbound listing still births its allocation under the lot. Twinning writes `has_twin`, never a
parent; birth writes a parent, never an edge.

## Phase 2 — Election as edges; the allocation's identity (closes blocker two)

Rows: the qualification edge, division × marketplace, one per pair in the tenant's gear
(`div_sells_on_mp`), with its bom_type. The consequence on a lot's birth: one allocation per
qualified marketplace, identity (tenant, lot, marketplace): the lot as parent, the
marketplace gear's uuid as native_id. Pre-flight for a strip-born entity becomes
(tenant, parent, e_type, native_id).

Executor: the birth branch reads the qualification edges and emits one allocation candidate
per marketplace; the recognition index for strip-born entities gains the parent. One unit.

Proof: a lot in TorBri births exactly its elected allocations and their listings;
MilAntTor's eBay is never touched; a second run of the same package mints nothing.

Phases 1 and 2 together are the Outbound Birth rewritten, and the first thing built.

## Phase 3 — Pre-flight, the pair rule, is_diff, the scribe (the real build)

The loop's steps 3 to 9 as executors, replacing resolve, is_new and land as they stand:

- **pre-flight**: six checks, one per table; `is_new` as a word on the a_type
  (`identity | label | fold | always`)
- **the pair rule**: the cross-table, one row per (origin SEA, destination SEA), an ordered
  verb list (`route | xfm | drop | match | pass`), a missing row is no movement; the_machine's
  lookup reads it and emits candidate rows
- **is_diff**: after the fold and the xfm, against the destination's standing
- **compliance**: standing NOs on the entity, the allocation, the qualification edge; a held
  row stays in the ledger until a landing clears it
- **the scribe**: write, edge, face; parent from the word; tenant stamped; run as the job's
  tenant; every reference resolved through the wall at pre-flight
- **one package, one job**, ordered by the marketplace's reported clock, serial per tenant
  and marketplace; a job is done when its own rows are written; consequences are their own
  jobs stamped with their origin run

This is the largest executor change in the plan and gets the full three-agree. The cue rows
under each passport become the identity rules the `match` verb reads.

## Phase 4 — The hopper on all six tables, everywhere

DDL: triggers on a_types, avs and bom_types in the 21 gears, and on all six in mailroom,
ten_routines and ten_registries. Mike's keyboard. Rows: the hopper posts the folded standing;
a lands row replays from the clock it names, no clock replays nothing; no pair rule points a
gear's vocabulary at itself.

## Phase 5 — The cross-table takes the how words

Rows only: the nine `how` words on lands entries (eight a_types, four gears) become pair
rows with ordered verbs; `elect:` on mp_ebay 510 becomes a `route` verb reading phase 2's
edge. The `how` slot is retired.

## Phase 6 — The ops teardown (Law 10, Mike's keyboard)

- allocations re-minted under their lots with identity (tenant, lot, marketplace): 72,495
  rows; 199 with no lot surfaced for Mike; the 56 doubled pairs collapse to one each
- the 15 lines parented across the wall re-minted under the twin of their order, after
  Mike's mint-or-reparent ruling on the five eBay orders with no ops twin
- the 43,366 `lot_has_allocation` edges whose child is not an ops allocation: measured and
  ruled before the re-mint
- `family_uuid` and `item_uuid` dropped from ops_com.entity and ops_cat.entity
- tenant_uuid stamped on the tenant-scoped rows that carry none: 60,941 mailroom calls and
  1,466 pallets, 372 ops_media images, 64 allocations

## Phase 7 — Land everything

Rows: an aspect with no home is minted as an empty a_type at unwrap (label, entity type,
dtype; folds null, no lands); receive() stops striking on a missing a_type. Backfill is then a
lands row and the hopper's replay.

## Order

1 and 2 now, together. 3 after they are proved on one lot. 4 and 5 after 3. 6 and 7 when 5
stands. Each phase ends with one lot walked by the tracer and nothing else in flight.

## Mike's, along the way

- the five eBay orders with no ops twin under the 15 cross-wall lines: mint or re-parent
  (phase 6)
- every DDL: phases 4 and 6
- every executor unit: 1, 2, 3
