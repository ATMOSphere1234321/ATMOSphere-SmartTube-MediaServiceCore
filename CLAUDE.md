# CLAUDE.md — smarttube-player / MediaServiceCore

YouTube backend module for SmartTube. ATMOSphere fork at
`git@github.com:ATMOSphere1234321/ATMOSphere-SmartTube-MediaServiceCore.git`.
Has its own nested `SharedModules/`.

This module is bound by the parent ATMOSphere Constitution and parent
`smarttube-player` submodule guidelines. Read the canonical authority:

- `../../../../docs/guides/ATMOSPHERE_CONSTITUTION.md` (parent repo)
- `../CLAUDE.md` (smarttube-player submodule)

## MANDATORY ANTI-BLUFF VALIDATION (Constitution §8.1 + §11)

**This submodule inherits the parent ATMOSphere project's anti-bluff covenant.
A test that PASSes while the feature it claims to validate is unusable to an
end user is the single most damaging failure mode in this codebase.**

1. Tests MUST validate user-visible behaviour, not just metadata. YouTube
   backend changes here surface as actual SmartTube playback / search /
   subscription behaviour on the flashed device — verify those paths, not
   just the unit tests that exercise the backend in isolation.
2. PASS / FAIL / SKIP mechanically distinguishable. SKIP requires an
   explicit reason. PASS only on observed positive evidence.
3. Every parent-repo gate that depends on this submodule MUST have a paired
   mutation in `scripts/testing/meta_test_false_positive_proof.sh`.
4. The bar is not "tests pass" but "users can use the feature."

## MANDATORY COMMIT & PUSH CONSTRAINTS

1. **ONLY use `bash scripts/commit_all.sh "message"` from the PARENT REPO ROOT**
   (the Android-15 root). The script handles the nested-submodule cascade.
2. Never use `git commit` / `git push` directly here.
3. Pointer cascade per Constitution §3: source change here → committed
   here (and inside the nested SharedModules first if needed) → pushed
   to every remote → smarttube-player parent updated → captured by
   Android-15 parent.

## MANDATORY ABSOLUTE DATA SAFETY — ZERO RISK (Constitution §9)

EVERY destructive repository operation MUST follow parent Constitution §9.
