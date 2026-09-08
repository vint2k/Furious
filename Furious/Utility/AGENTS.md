# Outer process guidance

Inherit the root and package guides. This scope preserves the exact outer child-process/crash protocol and is not a
general-purpose utility bucket.

- `Utility` owns the child-side wrapper used by the outer application process and crash/exit translation. It is not a
  miscellaneous helper namespace and does not own application composition, repositories, runtimes, or UI policy.
- `AppMainProcess` owns one exact Qt application child and one small synchronized crash-log result. Do not add a
  `multiprocessing.Manager` or auxiliary child merely to communicate status, and preserve the platform’s explicit spawn
  behavior.
- Exception reporting must work before and after application construction. Signal handlers are installed after the
  application factory returns; do not claim that this wrapper handles pre-construction signals. Preserve semantic
  `ApplicationRunner.ExitCode` values, original exception/traceback context, and best-effort crash logging; a
  log-write failure never replaces the primary failure.
- The parent entry point joins only the child it created and shows the fallback Qt report only for a nonzero result.
  Never discover or terminate processes by name, and keep normal/source/packaged command-line entry points equivalent.
- Shared crash status is a synchronized Boolean plus the child's semantic exit result; diagnostic text is written to
  a file and may include retained logs plus a traceback. Do not describe the complete crash file as size-bounded by
  the Boolean channel. Redaction and crash-write failure are separate from application-exit correctness.
- Fallback presentation runs in the parent after a nonzero child result and does not rerun normal application
  startup. Preserve the original result when evolving error-reporting failures rather than adding another
  supervisor.
- Verify normal return, exception, assertion, signal, pre-application failure, crash-log failure, command dispatch,
  cross-platform spawn, exact child joining, and absence of manager servers or orphaned resources. If this process
  topology changes intentionally, rewrite this guide rather than layering another supervisor over the old one.
  `tests/test_application_process.py` and `tests/test_interface.py` are the contract anchors.
