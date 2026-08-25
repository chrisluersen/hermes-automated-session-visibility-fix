# Hermes Automated-Session Visibility Fix

A private patch kit for a Hermes Agent session-history improvement. It keeps automated sessions in the canonical session store while hiding them from default human-history views, provides explicit retrieval, and renders consistent lifecycle/source labels.

## Behavior

- Default human history excludes `cron`, `kanban`, and `tool` sessions.
- Explicit source filters continue to expose automated sessions.
- Stored source, profile ownership, routing, archive, pin, and activity metadata are preserved.
- CLI, gateway, desktop/profile APIs, search, sidebar, browse, and pinned listings receive consistent display labels:
  - `[open]`
  - `[closed]`
  - `[automated]`
- Closed-session reopening is transaction-safe, clears terminal lifecycle fields, and uses deterministic collision-safe titles.

## Exact candidate

- Source commit: `379a2e92899fee80102e73b336db5879af682ec0`
- Patch baseline (parent): `3a42f99b200297ff10519b0aecf758084180021d`
- Changed files: 10
- Patch: `patches/hermes-automated-session-visibility-fix.patch`

The baseline is a local Hermes Agent integration point rather than an upstream release tag. Do not force-apply this patch to another revision. Personal Hermes should inspect and port/cherry-pick the bounded change onto its chosen current upstream baseline, then rerun verification.

## Apply safely

```bash
git checkout <reviewed-personal-hermes-baseline>
git apply --check patches/hermes-automated-session-visibility-fix.patch
git apply patches/hermes-automated-session-visibility-fix.patch
```

If `git apply --check` fails, stop and port the change deliberately rather than using a three-way or force apply.

## Verification evidence

The committed candidate passed:

```text
143 passed, 1 pytest import-rewrite warning
Python compilation passed
git diff --check passed
static security scan found no matches
```

The warning was pytest's already-imported `anyio` assertion-rewrite warning, not a test failure.

Recommended verification after porting:

```bash
pytest -q \
  tests/hermes_cli/test_session_listing.py \
  tests/hermes_cli/test_dashboard_admin_endpoints.py \
  tests/hermes_cli/test_profiles_sidebar_scope.py \
  tests/hermes_cli/test_profiles_sidebar_cache.py \
  tests/hermes_cli/test_web_server_session_search.py \
  tests/hermes_cli/test_sessions_pin.py \
  tests/agent/test_api_content_sidecar.py \
  tests/test_background_review_session_isolation.py \
  tests/state/test_session_turn_lease.py
```

## Scope and privacy

- Private personal patch kit only.
- No Nous Research branch, pull request, issue, comment, or notification is created by this repository.
- No organization-specific content, endpoints, credentials, runtime transcripts, local absolute paths, or team artifacts are included.
- The live source installation and its dirty follow-up changes are not included.
- This repository does not authorize upstream publication.

## License

The patch modifies MIT-licensed Hermes Agent source. The upstream MIT license is included as `LICENSE`.
