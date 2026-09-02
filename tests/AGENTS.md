# Furious test guidance

Inherit repository-wide rules from the root `AGENTS.md`. This scope specializes isolation, evidence, resource ownership,
and test-tier selection; test convenience never weakens a production invariant.

## Isolation is a product invariant

- Tests must not affect a running Furious instance, production settings/data, desktop windows, tray, system proxy, DNS,
  routing, TUN, startup registration, interfaces, unrelated processes, or external services. Use `tests/support.py`, an
  offscreen Qt platform set before Qt import, a temporary INI settings identity, temporary files, and fully mocked host
  and network boundaries.
- Own exact child processes, threads, timers, replies, sockets, and handles created by a test. All waits are bounded and
  cleanup targets only those resources. Normal suites require neither network access nor installed proxy cores.
- Tests may exercise real Qt event loops, subprocesses, and compiled probes when that boundary is the defect, but use a
  hermetic child, temporary settings, disabled singleton/tray/restoration, and mocked host mutation.
- Import order is part of isolation: select the offscreen Qt platform and temporary settings identity before importing
  modules that can create Qt/application globals. A late patch is not equivalent to preventing the side effect.

## Test the contract

- Assert public semantic behavior and architectural invariants, not private coordinates, incidental call order, or one
  implementation's cache. Cover success, invalid input, timeout/cancel, stale/partial completion, rollback, cleanup, and
  compatible persisted input where applicable.
- For staged changes, fail immediately before commit and prove live plus persisted state is unchanged. Test a
  post-commit side-effect failure separately. Keep persisted-profile assertions distinct from runtime-copy output.
- Use stable profile/subscription identities in reconciliation and async tests. Exercise supersession, removal/reorder,
  duplicate endpoints, bounded scheduling, and unrelated-work preservation rather than relying on row positions.
- Qt behavior involving focus, selection, proxy mapping, shortcuts, queued delivery, geometry, animation, or destruction
  uses real widgets and `QTest`. Localized-text tests choose an explicit language inside `isolatedSettings()`.
- Prefer exact state, signal counts, destroyed signals, weak references, registry/child counts, thread/process/handle
  ownership, and final exit status. RSS/handle trends and repeated lifecycle batches belong in stress tiers;
  `gc.collect()` is diagnostic at batch boundaries, never a production fix or per-cycle requirement.

## Tiers and maintenance

- Run the narrow module first, then the affected tier documented in `tests/README.md`. The release-confidence tier is
  explicitly opt-in with `FURIOUS_VERY_HEAVY_TESTS=1`; packaged/manual smoke work uses disposable environments.
- Benchmarks report scale and latency but are not correctness gates. Keep deterministic scale assertions in normal or
  stress tests and avoid machine-dependent elapsed-time thresholds unless the test is explicitly diagnostic.
- Update `tests/README.md` when coverage ownership, modules, commands, tiers, opt-ins, or environment requirements change.
  The final unittest status and process exit code are authoritative even when negative paths intentionally log errors.
- Review new tests for production-state mutation, live network dependence, process-name cleanup, unbounded waits, shared
  mutable fixtures, order dependence, timing-only assertions, and storage assertions where runtime output is the contract.
