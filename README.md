# GePatch Movie

An experimental cutscene-aware fork of [TheOfficialFloW's GePatch](https://github.com/TheOfficialFloW/GePatch)
for PS Vita running Adrenaline 7. Release **1.0.0** preserves the exact binary
reported working on a user's Vita: high-resolution gameplay and visible video
cutscenes. It is not a universal compatibility fix or an official GePatch release.

## What changes

The plugin intercepts successful MPEG AVC/VME colour-conversion calls. It
temporarily selects Adrenaline's normal PSP framebuffer and bypasses native GE
list rewriting for video, rather than displaying an old native-resolution frame.
After one second without a converted video frame, new geometry can restore
native gameplay presentation. Failed conversions do not trigger video mode.

Gameplay retains GePatch's 960x544 rendering where compatible. Videos keep their
original resolution; this plugin does not upscale video, replace textures,
change game images or add a second-stick mapping.

## Install

1. Fully close Adrenaline. Back up your current `game.txt`, plugins and saves.
2. Download `ge_patch_movie.prx` from [release 1.0.0](https://github.com/Startupingarage/gepatch-movie/releases/tag/1.0.0)
   and copy it to `ux0:pspemu/seplugins/` using VitaShell.
3. Edit `ux0:pspemu/seplugins/game.txt`. Disable an existing original GePatch
   entry and enable the new one; do not blindly replace unrelated entries:

   ```text
   ms0:/seplugins/ge_patch.prx 0
   ms0:/seplugins/ge_patch_movie.prx 1
   ```

4. In Adrenaline's Recovery Menu, enable GAME plugins and disable
   `Advanced -> Advanced configuration -> Force high memory layout`.
   Initially disable other PSP GAME/VSH plugins, as required by upstream
   GePatch. Vita system plugins are a separate configuration; do not remove them.
5. Disconnect VitaShell FTP and fully close/reopen Adrenaline before testing.
   Test a video, gameplay and the return from video to gameplay.

Never enable both GePatch builds together. Do not add this fork to `vsh.txt`:
this release targets games, not a higher-resolution XMB.

## Roll back

Fully close Adrenaline and set `ge_patch_movie.prx` to `0` in `game.txt`.
Leave the original GePatch at `0` for normal PSP rendering, or enable it for the
original high-resolution behaviour, which may still hide cutscenes. Restart
Adrenaline. No game files need restoration.

## Verification and limitations

The release was rebuilt with the pinned PSPDEV image below and compared
byte-for-byte with the saved, user-tested binary. They are identical.

- Size: **14,566 bytes**.
- SHA-256: `cd7e0df1ff42cb09b4a1f4747df98a778a497f5bde2e83b84ded3a969af5dbef`.
- Runtime module metadata remains `GePatchMovie` 1.1 and diagnostics retain the
  `movie-test1` prefix, deliberately unchanged to preserve the tested binary.
- The `1.0.0` version identifies this fork's public release, not upstream GePatch.

The user confirmed cutscenes and gameplay on one PCH-1008 / HENkaku 3.65 /
Adrenaline 7 setup after restoring this build. This is not independent hardware
certification or confirmation that every game works. Other game coverage has
not been established.

Known limitations retained in this first-build release:

- Only the hooked MPEG colour-conversion paths trigger video fallback. Export
  discovery alone does not prove that a game executes a patched syscall.
- The one-second timeout is a heuristic; video textures may also trigger it.
- Rendering mode can change during a stalled GE list, and reusable already
  rewritten lists are not restored. Such games may display incorrect graphics.
- Decoder/presentation state and the inherited copy command buffer are not
  synchronized across concurrent calls; certain games can hang or crash.
- A MPEG module reloaded at the same address may require restarting Adrenaline.
- Upstream issues such as black screens, missing textures, clipping and culling
  remain possible. Back up saves and use at your own risk.

An append-only diagnostic log is written to `ms0:/ge_patch.txt`
(`ux0:pspemu/ge_patch.txt` on the Vita), with up to 64 transition/hook records per
session plus a startup record. It contains local framebuffer/function addresses,
not account credentials. Review the log before sharing it.

## Build

Use PSPDEV / PSPSDK. The reproducible image used for this release is:

```text
pspdev/pspdev@sha256:54895e6f5afb71b8f4f6915ee6d5023e1e087ca7afdf2dcb1d7a694a5233e45f
```

From the repository directory on Linux:

```sh
docker run --rm -v "$PWD:/src" -w /src \
  pspdev/pspdev@sha256:54895e6f5afb71b8f4f6915ee6d5023e1e087ca7afdf2dcb1d7a694a5233e45f \
  make all
sha256sum ge_patch_movie.prx
```

`make test` additionally requires a native Linux C compiler and PSPDEV on PATH.
The eight small host scenarios use simulated PSP services and fixed memory
mappings; they exercise presentation transitions, not actual GPU execution,
physical MPEG decoding or the concurrency limitations above.

## Credits and licence

Based on GePatch v0.19.1, upstream commit
`2dd234ca1b5f07536542a57b056113107adfd671`. Original GePatch and Adrenaline work
is by TheOfficialFloW and upstream contributors; this fork is maintained by
[Startupingarage](https://github.com/Startupingarage) and is not endorsed by them.
The original README is preserved in [UPSTREAM_README.md](UPSTREAM_README.md).

The upstream GNU GPL v2 licence and PSPSDK BSD-compatible notices are retained
in [LICENSE.TXT](LICENSE.TXT). Corresponding source is published with this release.
No games, game images, Sony firmware or private device backups are included.
