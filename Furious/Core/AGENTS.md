# Embedded runtime guidance

Inherit the root, package, interface, and service rules. This scope owns reusable embedded execution machinery and
application tun2socks, while connection policy remains outside it.

- `Core` supplies shared multiprocessing runtime machinery, bounded output transport, and application tun2socks. External
  Core owns its separate direct `subprocess.Popen`; neither layer owns controller, repository, UI, or protocol policy.
- A launch spec describes only validated child construction, never semantic connection readiness. Runtime preparation
  completes before construction; the asynchronous connection transaction observes endpoints/process survival and
  commits later. Keep any synchronous waiting isolated as an explicit compatibility path.
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
  must not leave timers, callbacks, queues, or process handles alive.
- Verify invalid target/serialization, failed spawn, early exit, readiness compatibility, burst output bounds/backoff,
  normal and forced stop, repeated disposal, and absence of residual children, handles, timers, queues, or callbacks.
