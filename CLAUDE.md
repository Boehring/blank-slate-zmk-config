# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

ZMK firmware config for the LP Galaxy Blank Slate keyboard (ortholinear 4×12, all 1u). Builds via GitHub Actions — no local ZMK toolchain needed.

## Build

Firmware builds run on push/PR via `.github/workflows/build.yml`, which delegates to `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3`.

Two artifacts are produced (defined in `build.yaml`):
- `lpgalaxy_blank_slate` — standard build
- `lpgalaxy_blank_slate-studio` — ZMK Studio build (adds `studio-rpc-usb-uart` snippet and `CONFIG_ZMK_STUDIO=y`)

To trigger a build without a code change: use GitHub Actions → workflow_dispatch.

## Key Files

- `config/lpgalaxy_blank_slate.keymap` — all keymap logic (layers, behaviors, macros)
- `config/lpgalaxy_blank_slate.conf` — Kconfig flags (currently only `CONFIG_ZMK_PM_SOFT_OFF=y`)
- `config/west.yml` — ZMK version pinned to `v0.3` from `zmkfirmware/zmk` + `petejohanson/blank-slate-zmk-module`
- `build.yaml` — GitHub Actions matrix

## Keymap Architecture

4 layers defined in `lpgalaxy_blank_slate.keymap`:

| Layer | ID | Access |
|---|---|---|
| Base | 0 | default |
| Nav | `NAV_L` (1) | `mo NAV_L` on thumb |
| Num | `NUM_L` (2) | `mo NUM_L` on thumb |
| Sym | `SYM_L` (3) | `mo SYM_L` on thumb |

**Behaviors defined:**
- `hm` (homerow_mods): balanced hold-tap, 280ms tapping-term, 175ms quick-tap, 150ms require-prior-idle. GACS order (LGUI/LALT/LCTRL/LSHFT on left; RSHFT/RCTRL/RALT/RGUI on right).
- `shifty` (tap-dance): single tap = LSHFT, double tap = caps_word
- `thumbs_up` (macro): types `+:+1:\n`
- `&lt` override: quick_tap_ms = 200

## Soft-Off Support

Builds against petejohanson's ZMK fork that includes soft-off (PR not yet merged to ZMK main). To switch to ZMK `main`, update `config/west.yml` (see README) and comment out `CONFIG_ZMK_PM_SOFT_OFF=y` in `config/lpgalaxy_blank_slate.conf`.

## Keymap Editor Compatibility

Keymap uses pure devicetree syntax (no `#include`-based ZMK Studio config file) to stay compatible with the online ZMK keymap editor.
