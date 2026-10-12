# Changelog

## 1.0.0 - 2026-10-12

First public release of the GePatch Movie fork, based on GePatch v0.19.1.

- MPEG AVC/VME colour-conversion hooks select normal PSP presentation for video.
- Native frame copying is suppressed while converted video is active.
- Native GE-list rewriting resumes after one second of decoder inactivity.
- Gameplay can return to native rendering after fresh geometry is submitted.
- Bounded transition diagnostics retain the `movie-test1` prefix.
- Restored sources reproduce the user-tested first-build binary byte-for-byte.

This release intentionally excludes the later experimental synchronization and
list-tracking changes that caused a reported hardware startup regression.
Internal PSP module metadata and the binary have not been changed for cosmetic
versioning. Known concurrency, stalled-list and compatibility limits remain;
see README.md before installing.
