# GePatch Movie 1.0.0

First public release of an experimental cutscene-aware GePatch fork for
PS Vita / Adrenaline 7, based on TheOfficialFloW's GePatch v0.19.1.

This is the exact first-build binary reported working for both video cutscenes
and gameplay after rollback on a user's Vita. Restored source reproduces it
byte-for-byte. Later experimental builds that caused a startup regression are
not included.

Download `ge_patch_movie.prx`, or the ZIP with the licence and instructions.
Install in `ux0:pspemu/seplugins/`, disable the original GePatch entry, enable
`ms0:/seplugins/ge_patch_movie.prx 1`, and fully restart Adrenaline. Enable GAME
plugins and disable Force high memory layout. See the README for full setup
and rollback instructions.

Videos retain their source resolution. This release does not guarantee all-game
compatibility: concurrency, stalled/reused display lists and unhooked codecs can
still produce incorrect graphics, hangs or crashes. Back up saves before use.

Binary size: 14,566 bytes.
Binary SHA-256: `cd7e0df1ff42cb09b4a1f4747df98a778a497f5bde2e83b84ded3a969af5dbef`.
Release numbering is `1.0.0`; internal module metadata and `movie-test1` logs
remain unchanged to preserve the tested binary.

Original GePatch/Adrenaline credit: TheOfficialFloW and upstream contributors.
Upstream GPL v2 and PSPSDK notices are included in LICENSE.TXT. Corresponding
source accompanies this release. No game images or Sony firmware are distributed.
