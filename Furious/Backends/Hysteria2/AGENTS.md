# Hysteria 2 guidance

Inherit the root, package, and common backend guides; consult Plugins for capability contracts. This scope owns
Hysteria 2's nested upstream document, native-TUN
capability, statistics, and editor projection.

## Native document and editor projection

- The persisted Hysteria 2 client document is authoritative and is submitted to the embedded runtime. The GUI is a
  partial projection, not an Xray-shaped compiler or a general upstream-schema normalizer.
- Preserve upstream names, optional-group absence, unknown siblings, and future string values. Effective defaults such
  as `realm.ipMode` are presented without materializing them during an untouched save; editing one leaf changes only
  that leaf.
- `obfs.type` selects tagged subtype data. Unknown types remain visible and survive untouched. An explicit switch to a
  known type may remove incompatible subtype branches, but never unrelated document branches.

## TUN, statistics, and lifecycle

- Managed native TUN replaces only the runtime copy’s `tun`. Disabled management preserves any explicit `tun`,
  including malformed data for the core to reject; only absence permits application tun2socks. Linux native TUN
  requires the backend’s privilege and server-route-exclusion guarantees. Probe/download copies always remove native
  TUN. Managed preparation currently resolves server addresses synchronously; do not describe the whole native-TUN
  stage as event-driven merely because connection readiness is asynchronous.
- The statistics provider is a process-lifetime capability; the runtime captures a configured server-API target and
  sampling owns its monitor/query lifetime. API URL, client ID, and authorization secret are distinct from client
  connection credentials. Keep requests bounded, validate counters, and never log the secret or infer statistics
  from merely having a running Hysteria2 process.
- Capability presence is independent: native TUN, statistics, actions, settings, routing, and protocol editing must
  continue to work or fail through their own declared contracts rather than being inferred from the runtime type.
- Verify nested sibling/default preservation, known and unknown values, obfuscation switching, URI/document
  equality, every native/application-TUN and resolution case, probe stripping, readiness/exit cleanup, statistics
  cancellation, and repeated transient editor/settings-dialog destruction. Keep this guide synchronized with
  verified upstream schema changes rather than treating current field lists as permanent. Start with
  `tests/test_hysteria2_compatibility.py`, `tests/test_native_tun_semantics.py`, and
  `tests/test_backend_editor_contract.py`.
