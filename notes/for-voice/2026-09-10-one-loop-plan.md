# Bringing the engine to the One Loop — the plan

Source: `notes/from-voice/2026-09-10-one-loop.md` (the voice session of 2026-09-10) and the
door's answers #111517, #114007, #114819, #115603. Law 10 throughout: the old shape is
torn down, not patched. Propose-only until Mike says go on each phase.

Every phase is: engine lead briefs → seeder writes rows (mashgiach, the pipe on
mashgiach + Edmund) → an executor line, if any, is one named change cleared by Mike →
tracer proves it on harry with one lot → Astra reads every executor change.

## Phase 0 — Measure (tracer, read-only, one brief)

Numbers the later phases are sized by, none of them known today:

- rung-one misses per gear: mints against binds, from the attempt ledger
- lands entries carrying a `how` word (what Phase 5 re-mints)
- the hopper's coverage: which three of six tables it stands on, and the queue rate each
- compliance rows: expected 0; the drain outcomes it has ever produced
- ops allocations with no lot (198 seen), ops lines parented across the wall (15 seen)
- family_uuid / item_uuid population (the half-built square)

Output: one page of counts. No rows written.

## Phase 1 — The parent word on the birth row (closes blocker one)

Rows: a vocabulary of three words, `root | strip | twin_of_parent`, and one word on every
passport's birth entry (the cue rows under the target passport). Listing → root,
allocation → strip (lot), lot → strip (item), item → strip (form), line → twin_of_parent.

Executor: one line in `ten_engine.resolve()` — read the word, transport nothing. Mike clears.

Proof: a lot dropped into TorBri births its marketplace twin with the tenant root as parent,
and an inbound listing still births its ops side under the lot. Twinning writes `has_twin`,
never a parent; birth writes a parent, never an edge.

## Phase 2 — The birth branch reads the election (closes blocker two)

Rows: the election ladvar (division × marketplace, up the strip to the form) as the tenant's
connection rows; a consequence may be a set an executor returns.

Executor: the birth branch reads the election before it births. One line. Mike clears.

Proof: a lot in TorBri births only into TorBri's elected gears; MilAntTor's eBay is never
touched (the test Mike named: don't turn on MilAntTor flooding eBay).

Phases 1 and 2 are the Outbound Birth brief, rewritten to this model, and they are the
first thing built.

## Phase 3 — The ladder out of resolve; qualify then mint

Rows: `is_new` becomes a word on the a_type beside `folds` and `lands`; the cue rows
(identity, code, picture) run as machine jobs, not an executor loop inside resolve.

Executor: `resolve()` is rewritten as a scribe — write the row, draw the edge the row names,
write the face. Compliance gates the job before any write it would make, births included;
rung one's miss is a NO at the gate; the held run is the holding pen, in the ledger; the
release is an observation landing (the machine's reply as a package), never an edit.

Rule written into the design before a line: **one package, one job**; is_new and compliance
run per row inside it. Without this the loop is an order of magnitude slower.

This is the largest executor change of the plan and gets the full three-agree.

## Phase 4 — The hopper on all six tables; the fold before the alert

DDL: triggers on the three vocabulary tables (aspect, value, edge type). Mike's keyboard.
Rows: the hopper posts the folded standing, not the raw row; an unchanged fold is no landing.

## Phase 5 — Compliance as the router; the how word moves to the pair

Rows only: two lookups. By source (this gear, entity type, aspect → what it owes) and by
pair (source → destination: the transform and whether allowed). The `how` word leaves the
lands entry and lives on the pair. Measured in Phase 0; every lands entry carrying a how is
re-minted. Compliance's first rule is a NO writer, not a gate (Law 8).

## Phase 6 — The ops allocation under its lot (Law 10 teardown)

The ops allocation's parent moves from the tenant root to the lot: ~72,000 rows re-minted,
198 with no lot surfaced for Mike, 15 cross-wall lines re-minted under the twin of their
order. family_uuid / item_uuid: teardown or the square's seed, ruled first. Mike's keyboard.

## Phase 7 — Land everything; the replay bound

Rows: an aspect with no home is minted as an empty a_type (label, entity type, dtype; folds
null, no lands) at unwrap. The replay bound is a row: how far back a new lands entry replays.
Backfill is then the hopper doing its job.

## Order

0 now. 1 and 2 next, together, as the Outbound Birth rewritten. 3 after 1–2 are proved on
one lot. 4 and 5 after 3. 6 and 7 when 5 stands. Each phase ends with one lot walked by the
tracer and nothing else in flight.

## Open, and whose

- the name for the machine (Mike)
- the replay bound's shape (engine lead proposes, Mike rules)
- family_uuid / item_uuid: teardown or seed (Mike)
- the courier's flat_file renderer (the Biblio lane; independent of this plan; one executor
  line for Mike's clear)
