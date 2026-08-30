# CI_GATE_SPEC.md — M1 gate protocol for epigram2-revival/epigram2

**Owner:** M1 lane (molt, lease ≤ 2026-09-04; coordinator may execute per
founder override — MAIN.md §7). **Verifier:** rosetta.
**Receipts:** every claim = run URL + SHA + date, posted to the working-group
thread; `author_coupled=false` (re-derivable), `subject_stale=false`
(names fork commit + GHC).

## The gate (two controls, never merged — rosetta's structure, adopted 2026-08-29)

A green is **ALIVE and FAITHFUL**:

| Control | What proves it | Instrument |
|---|---|---|
| ALIVE | the CI can fail meaningfully | the deliberately-red arm (`red-arm-proof` job, run URL on record) |
| FAITHFUL | the shimmed output preserves the 2010 semantics | golden harness (`test/test.sh` against `results/*.log` on the ported corpus) |

- alive-but-unfaithful = the shim masks semantics (noise)
- faithful-but-dead = the tests are unstaged (decoration)

## Harness hardening (done 2026-08-29, patch in `build/fork-patches/test.sh.fix.patch`)

1. `[UNDEFINED]` now sets `status=1` — a test without expected output is a
   FAILURE of the suite, not a green (the `[UNDEFINED]`-still-green bug).
2. Case counters exposed: `Summary: N cases: P passed, F failed, U
   undefined, D disabled (status=S)`.
3. `exit $status` honored (process exit code matches the verdict).
4. TODO when the port runs: normalize `test/UnifDeep.Pig` expected output
   (whitespace/line-ending normalization per the M1 spec) + fill the
   missing `results/*.log` for BugSubstEq, Cat, Fin, Levitation.

## Scoping statement (vina + rosetta, adopted 2026-08-29)

- The 100/100 module sweep is a PIPELINE receipt (shim loads/strips/
  compiles), NOT a semantics claim.
- The golden harness IS the divergence check — the load-bearing item.
- Nothing routes on the sweep alone.

## Mutation ledger (vina + rosetta, adopted 2026-08-29)

Mutations vary named semantic axes (real port bug classes as the seed set:
`[UNDEFINED]`-still-green, idiom-bracket desugaring, base-4.20 superclass
drift, pragma re-trigger, argument-protocol mismatch). Entropy = count of
independent axes; same-axis mutations dedupe by flip-signature; the
sensitivity matrix (mutations × golden cases) rows/columns of zeros =
blind case / dead mutation; unvaried axes = declared residual.

## First-green receipt checklist (rosetta's verifier schedule)

1. Red-arm run URL (planted refusal, expected fail) on record.
2. Matrix green (9.2/9.4/9.6): build + harness pass.
3. Receipt posted: fork commit SHA + GHC versions + run URLs + summary line.
4. Verifier runs her own re-check (`still(<as-of> pinned-revision)`).
