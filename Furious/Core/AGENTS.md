# Embedded runtime guidance

Inherit the root and package guides. Consult Interface for runtime contracts and Service for connection ownership.
This scope owns reusable embedded execution machinery and application tun2socks; connection policy remains outside it.

- `Core` supplies shared multiprocessing runtime machinery, bounded output transport, and application tun2socks. External
  Core owns its separate direct `subprocess.Popen`; neither layer owns controller, repository, UI, or protocol policy.
- A launch spec describes prepared child construction, never semantic connection readiness. Serialization and launch
  arguments are prepared before execution starts; constructors may create owned timers/queues that still need
  disposal if execution never starts. The service observes endpoints/process survival and commits later.
- `CoreRuntime` execution state, typed terminal exit, and readiness are separate contracts. A process becoming alive is
  not proof that its proxy/TUN endpoint is ready, while a readiness timeout must not overwrite an already observed typed
  exit.
- A runtime owns and reaps its exact child, process handle, monitor/drain timers, queues, callbacks, and feeder resources.
  Stop is bounded, escalates only that child when needed, closes handles, and is safe after partial start or repetition.
- Process-backed runtimes monitor and reap their own child, interpret a raw exit exactly once, and publish one typed exit
  event. `isRunning()` is a passive execution-liveness query and must not consume or dispatch lifecycle events.
- Child targets never touch Qt widgets. Output transport is non-blocking and bounded in message size, pending volume, and
  per-turn drain work; draining continues independently of Log-page visibility and backs off only when idle.
- Parentless timers are acceptable only with a durable runtime owner and explicit disposal. Leaving the manager pool
  must not leave timers, callbacks, queues, or process handles alive. Stopping execution is not QObject destruction:
  disposal must also release monitors and output infrastructure, including for a runtime that was never started.
- Verify invalid target/serialization, failed spawn, early exit, readiness compatibility, burst output
  bounds/backoff, normal and forced stop, repeated disposal, and absence of residual children, handles, timers,
  queues, or callbacks. Start with `tests/test_runtime_lifecycle.py` and `tests/test_connection_startup_async.py`;
  output/process stress lives in the tiers documented by `tests/README.md`. Review output admission and draining
  together when changing backpressure.
