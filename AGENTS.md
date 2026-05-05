# AGENTS.md — smarttube-player / MediaServiceCore submodule

Every AI agent working in this submodule MUST comply with the canonical
[ATMOSphere Constitution](../../../../docs/guides/ATMOSPHERE_CONSTITUTION.md)
in the parent repo, and with the [smarttube-player AGENTS.md](../AGENTS.md).

This is the YouTube backend module for SmartTube. It carries its own
nested `SharedModules/` submodule. Pointer cascade order (Constitution
§3): `MediaServiceCore/SharedModules` content commit → MediaServiceCore
captures pointer + commits + pushes → smarttube-player captures both
nested pointers + commits + pushes → Android-15 parent captures
smarttube-player pointer + commits + pushes via `commit_all.sh`.

## Non-negotiable summary

1. **Commit + push via `bash scripts/commit_all.sh "…"` from the parent
   repo root** for the final pointer cascade. For source changes inside
   nested submodules: commit deepest-first, push to every remote at each
   level, fast-forward pull in any other working tree of the same repo,
   THEN allow the parent's `commit_all.sh` to capture pointer advances.
2. **YouTube backend behaviour MUST validate as user-visible playback /
   search / subscription on the flashed device** — not just unit tests
   in isolation. §11.4 covenant binds this submodule equally.
3. **Tags cascade**: every main-repo tag mirrors on this submodule at
   its current HEAD across every remote it publishes to.

Non-compliance is a blocker regardless of context.

---

## MANDATORY HOST-SESSION SAFETY (Constitution §12)

This submodule inherits parent Constitution §12 and §12.1 in full. No
script here may suspend / hibernate / log-out the user session, and any
operation expected to exceed ~4 GB RSS MUST be wrapped in `bounded_run`
from `scripts/lib/host_session_safety.sh` (parent repo). Cross-reference:
parent `docs/guides/ATMOSPHERE_CONSTITUTION.md` §12.

## MANDATORY ANTI-BLUFF VALIDATION (Constitution §8.1 + §11)

Tests must validate user-visible behaviour, not just metadata. PASS / FAIL
/ SKIP must be mechanically distinguishable. Every parent-repo gate that
depends on this submodule MUST have a paired mutation in
`scripts/testing/meta_test_false_positive_proof.sh`. The bar is not
"tests pass" but "users can use the feature."

## MANDATORY ABSOLUTE DATA SAFETY — ZERO RISK (Constitution §9)

EVERY destructive repository operation MUST follow parent Constitution §9.
Hardlinked backup is so cheap (zero disk, <2 s) that there is NEVER an
excuse to skip it. Force-push requires explicit per-session authorization
AND a green post-op gate.

## MANDATORY ANTI-BLUFF COVENANT — END-USER QUALITY GUARANTEE (User mandate, 2026-04-28)

**Forensic anchor — direct user mandate (verbatim):**

> "We had been in position that all tests do execute with success and all Challenges as well, but in reality the most of the features does not work and can't be used! This MUST NOT be the case and execution of tests and Challenges MUST guarantee the quality, the completion and full usability by end users of the product!"

This is the historical origin of the project's anti-bluff covenant.
Every test, every Challenge, every gate, every mutation pair exists
to make the failure mode (PASS on broken-for-end-user feature)
mechanically impossible.

**Operative rule:** the bar for shipping is **not** "tests pass"
but **"users can use the feature."** Every PASS in this codebase
MUST carry positive evidence captured during execution that the
feature works for the end user. Metadata-only PASS, configuration-
only PASS, "absence-of-error" PASS, and grep-based PASS without
runtime evidence are all critical defects regardless of how green
the summary line looks.

**Tests AND Challenges (HelixQA) are bound equally** — a Challenge
that scores PASS on a non-functional feature is the same class of
defect as a unit test that does. Both must produce positive end-
user evidence; both are subject to the §8.1 five-constraint rule
and §11 captured-evidence requirement.

**Canonical authority:** parent
[`docs/guides/ATMOSPHERE_CONSTITUTION.md`](../../../../docs/guides/ATMOSPHERE_CONSTITUTION.md)
§8.1 + §11 + §11.3 (the "no bluff" CLAUDE.md / AGENTS.md mandate) +
**§11.4 (this end-user-quality-guarantee forensic anchor — propagation
requirement enforced by pre-build gate `CM-COVENANT-PROPAGATION`)**.

**§11.4.1 extension (Phase 33, 2026-05-05) — FAIL-bluffs equally
forbidden.** A test that crashes for a script-internal reason
(undefined variable under `set -u`, regex error, malformed assertion,
missing argument) and produces a FAIL exit code is just as misleading
as a PASS-bluff. Both let real defects ship undetected. Per parent
[Constitution §11.4.1](../../../../docs/guides/ATMOSPHERE_CONSTITUTION.md#114-end-user-quality-guarantee--forensic-anchor-user-mandate-2026-04-28),
every test MUST fail ONLY for genuine product defects — script-bug
failures must be fixed at the source layer (helper library, shared
lib, test source), not patched in individual call sites.

Non-compliance is a release blocker regardless of context.

## MANDATORY §12.6 MEMORY-BUDGET CEILING — 60% MAXIMUM (User mandate, 2026-04-30)

**Forensic anchor — direct user mandate (verbatim):**

> "We had to restart this session 3rd time in a row! The system of
> the host stays with no RAM memory for some reason! First make sure
> that whatever we do through our procedures related to this project
> MUST NOT use more than 60% of total system memory! All processes
> MUST be able to function normally!"

**The mandate.** Project procedures MUST NOT use more than **60%
of total system RAM** (`HOST_SAFETY_MAX_MEM_PCT`). The remaining
40% is reserved for the operator's other workloads so the host can
keep serving them while project work proceeds.

Three consecutive session-loss SIGKILLs on 2026-04-30 during 1.1.5-dev
established this; full forensic + library API in parent
[`docs/guides/ATMOSPHERE_CONSTITUTION.md`](../../../../docs/guides/ATMOSPHERE_CONSTITUTION.md)
§12.6.

**No escape hatch.** §12.6 has NO operator-facing override flag.
The cap exists for the operator's own protection; bypassing it is
the bluff the §11.4 covenant specifically prohibits.

Non-compliance is a release blocker regardless of context.
