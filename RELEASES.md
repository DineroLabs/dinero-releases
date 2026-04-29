# Dinero Releases

Official binary releases for Dinero (DIN) - Real Money for Free People.

## v2.2.2 - Shielded Wallet + Hardware Wallet Release Polish (2026-04-29)

This macOS Apple Silicon release refreshes the signed Qt wallet after the final
shielded-wallet, hardware-wallet, Rust bridge, and mobile framework cleanup.
It embeds the latest daemon and helper binaries, keeps the post-recovery
atomic chainstate build, and updates the app metadata to `2.2.2`.

### What's new

- **Qt app rebuilt as `2.2.2`** - reports Qt commit `9b2f27a` and bundle
  version `2.2.2`
- **Daemon refreshed to `aa97c1882`** - keeps the atomic shielded/chainstate
  persistence hardening from v2.2.1 plus the latest v7 coin-type docs and
  shielded txid bundle-commit fix
- **Shielded wallet UX tightened** - receive/balance views stay available for
  viewing while locked, confirmed and pending note counts are separated, and
  sending still requires passphrase confirmation
- **Hardware wallet PSBT flow polished** - user-facing text says "Partially
  Signed Dinero Transaction", 1447 defaults are retired, styling matches the app
  chrome, and QR signing shows a decoded transaction summary before QR display
- **Rust bridge and mobile refs aligned** - Rust Tier-3 bridge commit
  `db34fea6f` carries v7 chain params; DineroDPI commit `21be2c1` includes the
  refreshed embedded consensus frameworks
- **Notarized macOS app** - every embedded Mach-O binary/framework/plugin is
  signed inside-out with Developer ID, hardened runtime, and secure timestamp;
  the app is notarized and stapled

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.2.2-macOS-arm64-qt.zip` | Latest |

### Verification

The extracted Qt app passes:

```text
spctl -a -t exec -vv Dinero-Qt.app
accepted
source=Notarized Developer ID
origin=Developer ID Application: Mirsad Hajdarevic (JXJS6ZA5FJ)
```

Commit: `aa97c1882` (daemon/CLI/miners) / `9b2f27a` (qt) / `db34fea6f` (rust bridge) / `21be2c1` (DPI) / `2fcfe40` (stratum) / `91fa22b` (sv2 miners)

---

## v2.2.1 - Atomic Chainstate Recovery + Qt Wallet Refresh (2026-04-29)

This macOS Apple Silicon release is the post-recovery mainnet build. It ships
the atomic chainstate persistence hardening, canonical reindex recovery for the
April 29 Utreexo forest-root incident, and a Qt wallet bundle rebuilt as
`2.2.1` with the current daemon embedded.

### What's new

- **Qt app rebuilt as `2.2.1`** - reports Qt commit `0380dd9a` and bundle
  version `2.2.1`
- **Daemon refreshed to `85652d00b`** - includes atomic shielded/chainstate
  persistence hardening and the reindex recovery path used for the recovered
  mainnet fleet
- **Retired derivation paths rejected** - wallet paths/descriptors using
  `coin_type 1447` now fail explicitly; v7 wallets use `coin_type 1448` only
- **Shielded consensus storage aligned** - mutable chainstate pointers,
  Utreexo/shielded markers, nullifiers, and crash journal rows are covered by
  the ChainDB-backed atomic persistence path
- **Notarized macOS app** - every embedded Mach-O binary/framework/plugin is
  signed inside-out with Developer ID, hardened runtime, and secure timestamp;
  the app is notarized and stapled

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.2.1-macOS-arm64-qt.zip` | Latest |

### Verification

The extracted Qt app passes:

```text
spctl -a -t exec -vv Dinero-Qt.app
accepted
source=Notarized Developer ID
origin=Developer ID Application: Mirsad Hajdarevic (JXJS6ZA5FJ)
```

Commit: `85652d00b` (daemon/CLI/miners) / `0380dd9a` (qt)

---

## v2.1.25 - Qt Wallet Polish + Settings/Backup Rewire (2026-04-25)

This macOS Apple Silicon refresh rolls up the final Qt polish pass after
v2.1.24: the overview dashboard, wallet receive/seed guidance, advanced-tab
cleanup, settings backup/runtime wiring, and stale balance RPC fix are all in
the signed and notarized app.

### What's new

- **Qt app rebuilt as `2.1.25`** - reports Qt commit `1b601eac` and bundle
  version `2.1.25`
- **Overview dashboard polished** - compact network/system cards, professional
  peer labels, supply wired to the economics RPC, and no N/A latency/version
  noise
- **Wallet UX refreshed** - seed/mobile restore guidance now documents Taproot
  `m/86'/1448'` and quantum-safe P2MR `m/88'/1448'`; wallet controls live in
  the Wallet tab
- **Mining UI cleanup retained** - SV2 CPU/GPU backend switching, cinematic
  pool output, hashrate/share parsing, and solo/runtime status fixes remain
  bundled
- **Settings/backup rewired** - Settings shows the real per-user daemon data
  directory, hides bundled daemon AppTranslocation paths, and backs up current
  wallet/chain data locations
- **Stale balance RPC removed** - Qt no longer calls unsupported
  `wallet.gettotalbalance`; the status strip stays clean using
  `wallet.getbalance`
- **Notarized macOS app** - every embedded Mach-O binary/framework/plugin is
  signed inside-out with Developer ID, hardened runtime, and secure timestamp;
  the app is notarized and stapled

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.25-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.25-macOS-arm64-qt.zip` | New |

### Verification

The extracted Qt app passes:

```text
spctl -a -t exec -vv Dinero-Qt.app
accepted
source=Notarized Developer ID
origin=Developer ID Application: Mirsad Hajdarevic (JXJS6ZA5FJ)
```

Commit: `11405965` (daemon/CLI/miners) / `1b601eac` (qt) / `608bf83e` (sv2 miners) / `2fcfe408` (stratum)

---

## v2.1.24 - Bundled SV2 Miner Path Fix + Notarized macOS Refresh (2026-04-24)

This macOS Apple Silicon refresh keeps the final SV2 pool mining UI from
v2.1.23 and fixes the release-app path behavior. Downloaded Qt apps now prefer
the SV2 CPU/GPU miners bundled inside their own `Dinero-Qt.app` before any
saved local setting, so a maintainer/dev path like `/Users/.../src/dinero-qt`
cannot leak into normal user installs.

### What's new

- **Qt app rebuilt as `2.1.24`** - reports Qt commit `df386c14` and includes
  the bundled SV2 miner discovery fix
- **Release-safe SV2 miner lookup** - `dinero-sv2-miner` and
  `dinero-sv2-gpu-miner` are resolved from `Contents/MacOS` /
  `Contents/Resources` first, then saved/manual paths are considered only if
  the bundled miner is missing
- **Stale SV2 settings cleaned up** - when the bundled miner exists, old saved
  SV2 miner paths are cleared so backend switching stays portable across Macs
- **v2.1.23 polish retained** - backend switching, live SV2 hashrate/share
  counters, parsed pool output, and the solo-style matrix/hash/comet background
  remain intact
- **Notarized macOS app** - every embedded Mach-O binary/framework/plugin is
  signed inside-out with Developer ID, hardened runtime, and secure timestamp;
  the app is notarized and stapled

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.24-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.24-macOS-arm64-qt.zip` | New |

### Verification

The extracted Qt app passes:

```text
spctl -a -t exec -vv Dinero-Qt.app
accepted
source=Notarized Developer ID
origin=Developer ID Application: Mirsad Hajdarevic (JXJS6ZA5FJ)
```

Commit: `11405965` (daemon/CLI/miners) / `df386c14` (qt) / `608bf83e` (sv2 miners) / `2fcfe408` (stratum)

---

## v2.1.23 - SV2 Pool UI Final Polish + Notarized macOS Refresh (2026-04-24)

This macOS Apple Silicon refresh rebuilds the Qt wallet from the final SV2 pool
miner polish pass. It keeps the live SV2-JD mining path from v2.1.22, then fixes
the last UX/runtime edges: GPU/CPU backend switching now selects the matching
SV2 miner binary automatically, stale saved miner paths are ignored, SV2 output
uses the same cinematic hash/comet treatment as solo mining, and the mining
runtime label now populates as `Run:` instead of sitting at `Up: -`.

### What's new

- **Qt app rebuilt as `2.1.23`** - reports Qt commit `0b68d920` and includes
  the SV2 runtime UI fixes from `9ca7647`
- **SV2 backend picker fixed** - switching between CPU and GPU loads
  `dinero-sv2-miner` or `dinero-sv2-gpu-miner` automatically and rejects the
  wrong binary when browsing manually
- **SV2 output polished** - pool mining output renders parsed SV2 events,
  live hashrate, in-place share totals, and the matrix/hash/comet background
  without changing the solo miner background behavior
- **Runtime status repaired** - the mining row shows a populated `Run:` elapsed
  timer for process miners instead of the old empty `Up:` field
- **Bundle hygiene retained** - SV2 CPU/GPU miners are bundled in both
  `Contents/MacOS` and `Contents/Resources`, and pool miners are no longer
  stopped by daemon reconnect/disconnect UI paths
- **Notarized macOS app** - every embedded Mach-O binary/framework/plugin is
  signed inside-out with Developer ID, hardened runtime, and secure timestamp;
  the app is notarized and stapled

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.23-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.23-macOS-arm64-qt.zip` | New |

### Verification

The extracted Qt app passes:

```text
spctl -a -t exec -vv Dinero-Qt.app
accepted
source=Notarized Developer ID
origin=Developer ID Application: Mirsad Hajdarevic (JXJS6ZA5FJ)
```

Commit: `11405965` (daemon/CLI/miners) / `0b68d920` (qt) / `608bf83e` (sv2 miners) / `2fcfe408` (stratum)

---

## v2.1.18 - Local Stratum Launcher + P2MR FFI Refresh (2026-04-22)

This macOS arm64 refresh rebuilds Dinero after the Qt wallet gained a
one-click local Stratum server launcher and the app-linked NodeCore layer
picked up local P2MR wallet primitives for DineroDPI. The packaged core
binaries are refreshed to current `p2p-fix`, the Qt wallet reports `2.1.18`,
and the release now ships the standalone `dinero-stratum` server alongside
the existing Stratum worker.

### What's new

- **Qt app rebuilt as `2.1.18`** - reports Qt commit `37fb19c` and embeds
  refreshed core binaries plus the local Stratum server
- **Local Stratum launcher added** - Pool mining mode can start/stop a
  localhost `dinero-stratum` server, bind it to `127.0.0.1:3333`, and set the
  pool endpoint automatically
- **Stratum server bind hardening** - `dinero-stratum` commit `2fcfe40` adds
  `--stratumhost` / `--bind` so the Qt-launched server can stay local-only
- **Core binaries refreshed to `0ecb8577a`** - packaged `dinerod`, CLI, CPU
  miner, GPU miner, and Stratum worker report current `p2p-fix`; this is a
  build-identity refresh, not a daemon consensus/RPC behavior change
- **Standalone `dinero-stratum` now included** - both macOS core and Qt
  packages include the Stratum server binary
- **Release gates completed** - Qt tests passed, checksums verified, Developer
  ID code-sign verification passed; app is not notarized

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.18-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.18-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.18` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `0ecb8577a` (daemon/CLI/miners/worker) / `37fb19c` (qt) / `71ee61e` (solo miner) / `2fcfe40` (stratum)

---

## v2.1.16 - Mining Longpoll + Undo Safety Refresh (2026-04-20)

This macOS arm64 refresh rebuilds Dinero after the latest mining-side daemon and
solo-miner improvements landed on `p2p-fix`. The packaged core binaries now
include server-side `getblocktemplate` long-polling, stale-job interruption on
the mining side, safer handling around missing undo rewrites, and the new
first-seen tie policy for same-work branches. The Qt wallet is rebuilt as
`2.1.16` so the bundle metadata, embedded daemon, and release identity all
point at the current branch tip.

### What's new

- **Core binaries refreshed to `de4e8140a`** — the packaged `dinerod`,
  `dinero-cli`, CPU miner, GPU miner, and Stratum worker now include the latest
  long-polling mining path, stale-work interruption, shutdown wakeups for
  waiting miners, and the same-work tie handling refresh
- **Qt app rebuilt as `2.1.16`** — the packaged app reports Qt commit
  `d00ef4f` and embeds the refreshed core binaries from the current `p2p-fix`
  head
- **Bundled solo miner refreshed to `71ee61e`** — the packaged standalone solo
  miner now follows the new client-side longpoll loop and matches the refreshed
  daemon behavior
- **Quick multi-miner smoke completed** — the refreshed Mac and Dell mining
  paths ran concurrently without re-entering the earlier `missing undo data`
  safe-mode wedge during the observation window
- **Automatic recovery remains conservative** — this release still keeps the
  automatic chainstate recovery fuse disarmed so the policy decision stays
  separate from the correctness rollout
- **macOS bundle remains sanitized** — the shipped Qt app still passes local
  `codesign --verify --deep --strict` and remains free of the stray Qt PDF /
  SVG / virtual-keyboard baggage

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.16-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.16-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.16` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `de4e8140a` (daemon/CLI/miners/worker) / `d00ef4f` (qt) / `71ee61e` (solo miner) / `64e1a7c` (stratum)

---

## v2.1.15 - Canonical Recovery + Shielded Restart Hardening (2026-04-19)

This macOS arm64 refresh rebuilds Dinero after the latest canonical-state and
shielded restart-recovery hardening landed in the daemon. The packaged core
binaries now include the restart-safe fixes across connect/reindex/disconnect/
reconsider, interrupted reindex-promotion recovery, and the shielded
reindex/restart consistency work. The Qt wallet is rebuilt as `2.1.15` so the
bundle metadata, embedded daemon, and release identity all point at the new
core head.

### What's new

- **Core binaries refreshed to `eff8632be`** — the packaged `dinerod`,
  `dinero-cli`, CPU miner, GPU miner, and Stratum worker now include the
  canonical-state recovery audit fixes, interrupted reindex-promotion recovery,
  and the latest shielded restart/reindex hardening
- **Qt app rebuilt as `2.1.15`** — the packaged app reports Qt commit
  `01faaff` and embeds the refreshed core binaries from the current `p2p-fix`
  head
- **Shielded recovery coverage expanded** — the shipped daemon now includes the
  new shielded tip-marker, reindex rebuild, and daemon-level crash/restart
  recovery fixes that were added alongside the dedicated recovery gates
- **Automatic recovery remains conservative** — this release ships the recovery
  correctness work while intentionally leaving the automatic chainstate recovery
  fuse disarmed, so the policy decision stays separate from the correctness
  rollout
- **Bundled solo miner retained** — the packaged standalone solo miner remains
  `2808786`, aligned with the current v7 genesis checks
- **macOS bundle remains sanitized** — the shipped Qt app still passes local
  `codesign --verify --deep --strict` and remains free of the stray Qt PDF /
  SVG / virtual-keyboard baggage

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.15-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.15-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is ad-hoc signed in this local rebuild and passes local
`codesign --verify` checks. It is not notarized in this repository refresh, so
first-open warnings may still appear on another Mac.

### Scope note

This `v2.1.15` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `eff8632be` (daemon/CLI/miners/worker) / `01faaff` (qt) / `2808786` (solo miner) / `64e1a7c` (stratum)

---

## v2.1.14 - Mining Console Polish (2026-04-18)

This macOS arm64 refresh rebuilds Dinero after the mining console in the Qt
wallet was visually softened to better match the rest of the interface. The
animated matrix output now uses a cool light-grey palette instead of dominant
green, and the temporary block-found card no longer draws the decorative
`o-o-o` rails around its content.

### What's new

- **Qt app rebuilt as `2.1.14`** — the packaged app now includes Qt commit
  `66a8eca`, which polishes the Mining tab output styling without changing the
  underlying mining workflow
- **Mining matrix palette softened** — the scrolling console field now uses
  neutral dark greys and silver-grey glyphs/highlights, so the output sits more
  naturally inside the existing wallet chrome
- **Block-found cards cleaned up** — the temporary decorative `o-o-o` border
  lines and side rails are removed, leaving just the useful status text
- **Core binaries retained at `5aea67246`** — the packaged `dinerod`,
  `dinero-cli`, CPU miner, GPU miner, and Stratum worker still match the latest
  current v7 mining build
- **Bundled solo miner retained** — the packaged standalone solo miner remains
  `2808786`, aligned with the current v7 genesis checks
- **macOS bundle remains sanitized** — the shipped Qt app still passes local
  `codesign --verify --deep --strict` and remains free of the stray Qt PDF /
  SVG / virtual-keyboard baggage

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.14-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.14-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is ad-hoc signed in this local rebuild and passes local
`codesign --verify` checks. It is not notarized in this repository refresh, so
first-open warnings may still appear on another Mac.

### Scope note

This `v2.1.14` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `5aea67246` (daemon/CLI/miners/worker) / `66a8eca` (qt) / `2808786` (solo miner) / `64e1a7c` (stratum)

---

## v2.1.13 - Unified Receive Filters + P2MR Coinbase Update (2026-04-18)

This macOS arm64 refresh rebuilds Dinero after the Qt wallet Receive tab was
collapsed into a single filtered address table and the daemon/miner side picked
up the latest v7 mining update. The packaged core binaries now match current
`p2p-fix`, the Qt app advances to `2.1.13`, and the macOS bundle continues to
ship without the stray Qt PDF / SVG / virtual-keyboard baggage.

### What's new

- **Core binaries refreshed to `5aea67246`** — the packaged `dinerod`,
  `dinero-cli`, CPU miner, GPU miner, and Stratum worker now include the latest
  v7 mining-side update, including accepting P2MR as a coinbase destination
- **Qt app rebuilt as `2.1.13`** — the packaged app reports Qt commit
  `d82d43f` and includes the new unified Receive-tab filter model:
  `All / Taproot / Quantum-Safe (P2MR)`
- **Receive tab simplified** — the Qt wallet now uses a single address table,
  removes the redundant P2MR preview/copy row, and correctly surfaces legacy
  P2MR rows by both explicit `type` and address prefix
- **Balance panel cleaned up** — the old confidential/private rows are gone, so
  the wallet now presents a simpler `Transparent / Pending / Mining /
  Quantum-safe` breakdown
- **Bundled solo miner retained** — the packaged standalone solo miner remains
  `2808786`, aligned with the current v7 genesis checks
- **Qt deploy sanitization retained** — the shipped macOS app still passes
  `codesign --verify --deep --strict` and contains no `QtPdf.framework`,
  `libqpdf.dylib`, `QtSvg.framework`, or virtual-keyboard plugin baggage

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.13-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.13-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is ad-hoc signed in this local rebuild and passes local
`codesign --verify` checks. It is not notarized in this repository refresh, so
first-open warnings may still appear on another Mac.

### Scope note

This `v2.1.13` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `5aea67246` (daemon/CLI/miners/worker) / `d82d43f` (qt) / `2808786` (solo miner) / `64e1a7c` (stratum)

---

## v2.1.12 - P2MR Receive Fix + Qt Deploy Sanitize (2026-04-18)

This macOS arm64 refresh rebuilds Dinero after the P2MR unified-address listing
fix landed in the daemon and the Qt/macOS deploy path was hardened to strip the
unused Qt PDF / SVG / virtual-keyboard plugin baggage before packaging.

### What's new

- **Core binaries refreshed to `fab28f376`** — the packaged `dinerod`,
  `dinero-cli`, CPU miner, GPU miner, and Stratum worker now include the
  daemon-side `wallet.listaddresses` fix that surfaces stored P2MR addresses in
  the unified address list
- **Qt app rebuilt as `2.1.12`** — the packaged app reports Qt commit
  `2724b5b` and bundles the refreshed daemon/miners from the current core build
- **macOS deploy flow hardened** — the Qt build now sanitizes Homebrew’s plugin
  scan before `macdeployqt`, preventing the `QtPdf` rpath failure that was
  interrupting release rebuilds
- **Final app bundle stays clean** — the shipped macOS app contains no
  `QtPdf.framework`, `libqpdf.dylib`, `QtSvg.framework`, or virtual-keyboard
  frameworks, while still passing `codesign --verify --deep --strict`
- **Bundled solo miner retained** — the packaged standalone solo miner remains
  `2808786`, aligned with the current v7 genesis checks

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.12-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.12-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is ad-hoc signed in this local rebuild and passes local
`codesign --verify` checks. It is not notarized in this repository refresh, so
first-open warnings may still appear on another Mac.

### Scope note

This `v2.1.12` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `fab28f376` (daemon/CLI/miners/worker) / `2724b5b` (qt) / `2808786` (solo miner) / `64e1a7c` (stratum)

---

## v2.1.11 - v7 Genesis Refresh + Qt Bundle Cleanup (2026-04-18)

This macOS arm64 refresh rebuilds Dinero after the v7 genesis pin, miner
identity sync, and Qt-side archive cleanup landed across the active branches.
The packaged daemon and miners now align with the new v7 genesis hash, the Qt
bundle advances to `2.1.11`, and the shipped macOS app no longer carries the
unused Qt PDF / SVG / Virtual Keyboard baggage in the final bundle.

### What's new

- **Core binaries refreshed to `e5995949a`** — the packaged `dinerod`,
  `dinero-cli`, CPU miner, GPU miner, and Stratum worker now match the current
  v7 genesis-pinned core build
- **Qt app rebuilt as `2.1.11`** — the packaged app reports Qt commit
  `cee3b96` with refreshed embedded release metadata
- **v7 cleanup now ships end-to-end** — archived ring / CT UI references and
  dead privacy RPC client methods are removed from the bundle, keeping the app
  aligned with the post-excision codebase
- **Qt PDF baggage is absent from the final app** — the shipped macOS bundle
  contains no `QtPdf.framework` or `libqpdf.dylib`, while still passing
  `codesign --verify --deep --strict`
- **Bundled solo miner refreshed** — the packaged standalone solo miner is now
  `2808786`, aligned with the v7 genesis checks

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.11-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.11-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.11` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `e5995949a` (daemon/CLI/miners/worker) / `cee3b96` (qt) / `2808786` (solo miner) / `64e1a7c` (stratum)

---

## v2.1.10 - macOS Qt Refresh + Signing Hardening (2026-04-17)

This macOS arm64 refresh republishes Dinero with the latest pushed Qt fixes and
a hardened macOS signing path. Core binaries remain on `d77a719`, while the Qt
bundle advances to `2.1.10` and now ships the cleaned-up v7-facing wallet UI
and a corrected release-signing flow that survives strict macOS validation.

### What's new

- **Qt app rebuilt as `2.1.10`** — the packaged app now reports Qt commit
  `f5e41ab` with refreshed embedded release metadata
- **Mac release signing hardened** — the Qt release-signing flow was tightened
  so the shipped app bundle passes `codesign --verify --deep --strict` after
  packaging, avoiding the invalid-page crashes seen from stale nested
  signatures
- **v7 UI cleanup landed across the wallet** — the Receive tab now uses
  `Quantum-Safe (P2MR)` wording, the Send and Contracts tabs drop legacy ring /
  CT references, and the wallet and history views are cleaner and easier to
  scan
- **Explorer and scrolling fixes included** — the Qt app includes the latest
  overview, wallet, and explorer usability fixes from `qt-main`
- **Bundled daemon and miners remain current** — the Qt bundle still embeds
  `dinerod d77a719e0`, the packaged solo miner stays on `fa755af`, and the
  Stratum worker remains bundled for macOS

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.10-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.10-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.10` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `d77a719` (daemon/CLI/miners/worker) / `f5e41ab` (qt) / `fa755af` (solo miner) / `560b170` (stratum)

---

## v2.1.9 - macOS Arm64 Rebuild Refresh (2026-04-17)

This macOS arm64 refresh rebuilds Dinero from the latest pushed core and Qt
heads after a heavy development pass. The live `v5` behavior from `v2.1.8`
stays intact, but the packaged binaries, embedded release identities, and the
Qt bundle itself are refreshed to current source revisions and republished as
`2.1.9`.

### What's new

- **Mac bundle refreshed to current core** — the standalone binaries and Qt app
  now embed `dinerod d77a719e0`
- **Qt app rebuilt as `2.1.9`** — the packaged app reports the current Qt repo
  commit and refreshed build metadata
- **Freeze fork protections from `v2.1.8` remain in place** — the activation
  gate logic, template filtering, and freeze-aware Qt behavior are retained in
  this rebuild
- **Developer ID signing refreshed for the Qt bundle** — the local macOS app
  bundle passes `codesign --verify --deep --strict`
- **Bundled miner and worker paths retained** — GPU miner, CPU miner, Stratum
  worker, and the clean committed solo miner remain packaged for macOS

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.9-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.9-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.9` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `d77a719` (daemon/CLI/miners/worker) / `ce193c7` (qt) / `fa755af` (solo miner)

---

## v2.1.8 - Freeze Fork Activation Safeguards (2026-04-16)

This macOS arm64 refresh rebuilds Dinero after the v5 freeze fork activation
work landed in core and Qt. The daemon now enforces the activation gates at
block `4000`, filters frozen pre-activation private transactions out of
post-activation block templates so the chain keeps advancing, and the Qt app
surfaces a countdown banner before activation and blocks private send/shield
actions once the freeze is live.

### What's new

- **Freeze fork is armed for mainnet** — the daemon now advertises activation
  height `4000`, the live gate state, and the remaining blocks through
  `privacy.getstatus`
- **Chain-halt risk removed at activation** — stale CT / ring transactions
  admitted before block `4000` are excluded from post-activation block
  templates instead of poisoning every new candidate block
- **Qt warns before freeze and blocks private actions after** — the Overview
  tab shows a countdown before activation and a red frozen-state banner after,
  and private wallet actions are rejected cleanly in pre-flight
- **Mac bundle refreshed to current core** — the standalone binaries and Qt app
  now embed `dinerod 130eefb0c`
- **Existing bundled miner/worker paths retained** — GPU miner, CPU miner,
  Stratum worker, and the clean committed solo miner remain packaged for macOS

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.8-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.8-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.8` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `130eefb0c` (daemon/CLI/miners/worker) / `ff5e15c` (qt) / `566e37b` (solo miner) / `560b170` (stratum)

---

## v2.1.7 - Recovery + Version Alignment Refresh (2026-04-14)

This macOS arm64 refresh rebuilds Dinero after the daemon-side missing-undo
auto-recovery hardening and the build metadata refresh fix landed in core. The
Qt bundle is rebuilt as `2.1.7`, and the packaged app now reports the current
core and Qt commits correctly in its embedded release identity manifest.

### What's new

- **Automatic recovery from missing-undo wedges** — the daemon now marks the
  exact LA/Mac-style chainstate wedge and recovers via chainstate rebuild on the
  next start instead of silently staying stuck
- **Rebuild metadata is trustworthy again** — normal rebuilds now refresh the
  embedded Git commit metadata, so `dinerod --version` and shipped manifests no
  longer lag behind the actual source revision
- **Mac bundle refreshed to current core** — the standalone binaries and the Qt
  app now embed `dinerod 0a19430b5`
- **Previous 2.1.6 ZK determinism fix retained** — the cross-architecture
  Spartan/Hyrax verification fixes remain in this refresh
- **Qt bundle stays trimmed** — the final macOS app still excludes unused Qt
  PDF, SVG, and Virtual Keyboard baggage

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.7-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.7-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.7` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `0a19430b5` (daemon/CLI/miners/worker) / `0ca6bc9` (qt) / `566e37b` (solo miner) / `560b170` (stratum)

---

## v2.1.6 - Cross-Architecture ZK Determinism Fix (2026-04-14)

This macOS arm64 release refresh rebuilds Dinero after the Spartan/Hyrax
verifier drift between x86_64 and Apple Silicon was fixed in core. The
refreshed daemon now uses deterministic integer math and deterministic function
argument evaluation in the affected ZK path, eliminating the cross-architecture
ring-covenant verification mismatch that let server nodes accept a covenant
transaction while the Mac rejected the same proof.

### What's new

- **Cross-architecture Spartan verification fixed** — Hyrax no longer relies on
  platform-sensitive floating-point sqrt behavior in the affected path
- **Deterministic ZK evaluation order enforced** — the `ec_add_complete`
  argument-evaluation root cause is now fixed so ARM64 and x86_64 derive the
  same proof result
- **Recent consensus hardening retained** — this refresh still carries the CT
  output index rebuild, durable `invalidateblock` persistence, preserved failed
  validation flags on relay re-accept, and restart branch-metadata recovery
- **Qt bundle remains trimmed** — the final macOS app still excludes unused Qt
  PDF, SVG, and Virtual Keyboard baggage

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.6-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.6-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.6` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `ea10c5044` (daemon/CLI/miners/worker) / `edb227a` (qt) / `566e37b` (solo miner) / `560b170` (stratum)

---

## v2.1.5 - Restart Recovery + Consensus Persistence Hardening (2026-04-14)

This release refreshes the macOS arm64 Dinero artifacts after the post-fork
consensus hardening pass and the Mac restart/bootstrap sync issue were fixed in
core. The refreshed daemon now restores persisted branch metadata for stored
better blocks after restart, so nodes no longer get stranded below tip because
restart import forgot their `BLOCK_VALID_CHAIN` candidacy. This release also
ships the CtOutputIndexDB startup rebuild, durable `invalidateblock`
persistence, preserved failed-validation flags on relay re-accept, and a
leaner Qt bundle that permanently strips unused Qt PDF/SVG/VirtualKeyboard
framework baggage from the final app.

### What's new

- **Restart bootstrap repaired** — stored non-active branch blocks above tip
  now rehydrate their persisted metadata on startup, fixing the Mac-style
  stuck-behind-tip recovery path
- **CT output index self-heals on startup** — CtOutputIndexDB is rebuilt on
  startup so cross-node CT pool drift does not survive restarts
- **Invalidations now survive restart** — `invalidateblock` state is persisted
  durably instead of being forgotten after daemon restart
- **Failed-validation flags are preserved** — relay re-accept no longer clears
  invalidation state and accidentally resurrects known-bad blocks
- **Qt app bundle trimmed** — the final macOS app no longer ships unused Qt
  PDF, SVG, or Virtual Keyboard baggage

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.5-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.5-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.5` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `d0543ba59` (daemon/CLI/miners/worker) / `b1028f0` (qt) / `566e37b` (solo miner) / `560b170` (stratum)

---

## v2.1.4 - Archival Read Crash Fix (2026-04-13)

This release refreshes the macOS arm64 Dinero artifacts after the daemon
startup crash on missing archival flatfiles was fixed in block storage. Under
the wrong recovery shape, `dinerod` could abort before RPC came up when the
block download scheduler touched a missing archival body path. The refreshed
core now fails that read safely as `NotFound`, letting the daemon stay up and
recover cleanly instead of crashing the app backend at launch.

### What's new

- **Missing archival files now fail safely** — block and undo flatfile reads
  no longer throw through the scheduler path when a referenced archival file is
  absent
- **Qt app startup is more resilient** — the embedded daemon can now stay up on
  real datadirs that previously crashed before RPC initialization
- **Prior safety work is retained** — this refresh keeps the ring-covenant
  mining fix, datadir dual-writer guard, real `--rpc-readonly` protection, and
  safer app-scoped daemon shutdown path
- **macOS artifacts rebuilt from current heads** — standalone binaries and the
  signed Qt app bundle are refreshed together, still bundling the dedicated
  Stratum worker and solo miner paths

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.4-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.4-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.4` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `4574de3ae` (daemon/CLI/miners/worker) / `4b2c27a` (qt) / `566e37b` (solo miner) / `560b170` (stratum)

---

## v2.1.3 - Ring-Covenant Mining Fix + Safety Refresh (2026-04-13)

This release refreshes the macOS arm64 Dinero artifacts after the confidential
Utreexo leaf hashing mismatch was fixed in consensus and block assembly. That
bug could cause `ComputeUtreexoRootPure` remove failures and block
ring-covenant transactions from being mined. The refreshed core also carries
the recent datadir dual-writer guard and startup consistency repairs for
KeyImageDB, and the Qt bundle keeps the safer app-scoped daemon shutdown path.

### What's new

- **Ring-covenant mining repaired** — confidential leaves now normalize to the
  consensus-visible zero amount in the Utreexo path, so pure root validation
  and mined block assembly agree for confidential outputs
- **KeyImageDB startup recovery** — the daemon now rebuilds or reconciles
  KeyImageDB from ChainDB when startup detects inconsistency instead of leaving
  the private-lane state partially stale
- **Datadir dual-writer protection** — the daemon now takes an actual datadir
  guard and PID file before touching chain data, making accidental second
  writers fail fast instead of silently corrupting the node
- **Qt shutdown safety retained** — the macOS Qt bundle still scopes daemon
  shutdown to the local wallet node instead of killing unrelated `dinerod`
  processes on the machine
- **macOS artifacts rebuilt from current heads** — standalone binaries and the
  signed Qt app bundle are refreshed together, still bundling the dedicated
  Stratum worker and solo miner paths

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.3-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.3-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.3` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `54057cfd4` (daemon/CLI/miners/worker) / `a19c2f4` (qt) / `566e37b` (solo miner) / `560b170` (stratum)

---

## v2.1.2 - Self-Loop Guard + Fleet Sync Recovery (2026-04-13)

This release refreshes the macOS arm64 Dinero artifacts after the fleet-wide
self-loop sync failure was traced, fixed, and rebuilt into the core daemon.
Nodes now refuse to dial their own advertised interfaces both at peer-source
load time and at outbound connect time, and the refreshed Qt bundle embeds the
same repaired core binaries.

### What's new

- **Outbound self-loop protection** — self IPs are filtered from hardcoded
  seeds, anchors, DNS results, persisted peers, addrman gossip, and direct
  outbound dials
- **Sandbox-safe interface detection** — local interface discovery now falls
  back cleanly when `getifaddrs()` is blocked under the systemd address-family
  sandbox used on fleet nodes
- **Fleet recovery hardening** — the repaired core also carries the recent RPC
  concurrency/body-size fixes that helped private-lane bootstrap and local
  mining stay stable under heavier traffic
- **macOS artifacts rebuilt from current heads** — standalone binaries and the
  signed Qt app bundle are refreshed together, still bundling the dedicated
  Stratum worker and solo miner paths

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.2-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.2-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.2` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `2b5657560` (daemon/CLI/miners/worker) / `88c65db` (qt) / `998acf0` (solo miner) / `560b170` (stratum)

---

## v2.1.1 - Pool Worker + Stratum Soak Hardening (2026-04-12)

This release refreshes the macOS arm64 Dinero artifacts after the dedicated
Stratum worker landed, the pool/restart soak path was hardened, and the Qt
bundle was updated to package the new worker cleanly alongside the existing
daemon, CLI, CPU miner, GPU miner, and solo miner.

### What's new

- **Dedicated `dinero-stratum-worker` binary** — pool mining now has a real
  bundled Stratum client worker instead of relying on unfinished Qt scaffolding
- **4-worker pool soak coverage** — vardiff on/off, Stratum restart, daemon
  restart, and explicit mixed-address `--payout` flows were exercised and fixed
- **Restart recovery hardening** — Stratum now reloads RPC cookies after daemon
  restarts, refreshes stale templates faster, and serializes socket writes more
  safely under reconnect pressure
- **Worker stale-job fixes** — the Stratum worker drops pause jobs and clears
  stale block-candidate work after accepted submissions
- **Qt pool mode now targets the real worker** — `Dinero-Qt` pool mining uses
  the new bundled `dinero-stratum-worker`
- **Qt packaging cleanup** — unused `QtPdf` bundle dependencies are stripped so
  the app bundle packages cleanly without the stale PDF framework drag-in

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.1-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.1-macOS-arm64-qt.zip` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This `v2.1.1` refresh currently ships new **macOS arm64** artifacts. Linux and
Windows artifacts are unchanged in this release pass.

Commit: `2255d7822` (daemon/CLI/miners/worker) / `123b48a` (qt) / `beadf9c` (solo miner) / `560b170` (stratum)

---

## v2.1.0 - V5 Genesis Reset + Archival Flatfile Release (2026-04-12)

This release resets Dinero onto the April 11, 2026 v5 genesis and ships the
first macOS arm64 and Linux x86_64 releases built on the new
flatfile-authoritative archival model. Historical block bodies are now treated
like Bitcoin Core-style release artifacts: canonical in `blk*.dat` / `rev*.dat`,
auditable, and required for full replay.

### What's new

- **v5 genesis reset** — fresh fair-launch chain with genesis hash
  `0000002d6b0abbf955fbf81faa4df1d0349a91c22d92ed9dd31cb4d79390b3d2`
- **Flatfile-authoritative archival storage** — accepted blocks and undo data
  now live in flatfiles first; startup refuses missing-body coverage instead of
  silently masking holes
- **Qt bundle version normalized to `2.1.0`** — app metadata, release identity,
  and packaging names now agree
- **Linux Qt AppImage added** — Linux x86_64 now ships as a self-contained
  `Dinero-Qt` AppImage with the daemon, CLI, CPU miner, GPU miner, and solo
  miner bundled alongside the GUI entrypoint
- **Embedded tools refreshed from current heads** — the macOS Qt app now embeds:
  - `dinerod`
  - `dinero-cli`
  - `dinero-miner`
  - `dinero-gpu-miner`
  - `dinero-solo-miner`
- **Linux artifacts hotfixed after chain-identity sync** — the Linux standalone
  tarball and Qt AppImage now embed the v5 genesis-aware `dinero-solo-miner`,
  and the Linux Qt `release-identity.json` records the refreshed solo-miner head
- **Clean bundle packaging** — stale embedded binaries are purged before copy, so
  new bundles no longer inherit old daemon/miner payloads from prior builds

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.1.0-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.1.0-macOS-arm64-qt.zip` | New |
| **Linux** (x86_64) | `Dinero-v2.1.0-linux-x86_64.tar.gz` | New |
| **Linux Qt** (x86_64) | `Dinero-v2.1.0-linux-x86_64-qt.AppImage` | New |

### Gatekeeper note

The macOS Qt bundle is Developer ID signed and passes local `codesign --verify`
checks. It is not notarized in this repository refresh, so first-open warnings
may still appear on another Mac.

### Scope note

This repository refresh now includes new **macOS arm64** and **Linux x86_64**
artifacts. Windows artifacts in this repository are unchanged pending a proper
Windows Qt release toolchain refresh.

Commit: `5f6d5de68` (daemon/CLI/miners) / `8794e3d` (qt) / `b1a58c6` (solo miner) / `626b5e3` (stratum manifest head)

---

## v2.0.3-PrivateLane - Ring index rebuild RPC + batch verifier hardening (2026-04-05)

macOS rebuild adding the `wallet.rebuildringindexes` RPC and two critical ZK batch
verifier soundness fixes from adversarial code review.

### What's new

- **`wallet.rebuildringindexes` RPC** — rebuild `ct_output_index` and `keyimages`
  databases from the active chain without stopping the daemon. Pass `apply=true`
  to atomically swap the rebuilt databases into place live.
- **ZK batch verifier soundness** — two adversarial-review findings fixed:
  - Circuit dimensions (`n`/`N`) now derived from the reconstructed verifier circuit,
    not from proof-supplied fields — closes the "self-consistency without circuit
    satisfaction" attack path.
  - Batch weight hash now covers the full serialized proof payload, preventing
    correlated two-item cancellation across a batch.
- Full macOS rebuild: `dinerod`, `dinero-qt`, `dinero-miner`, `dinero-gpu-miner`,
  `dinero-solo-miner`, `dinero-cli` all rebuilt from the same commit.

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.0.3-PrivateLane-macOS-arm64.tar.gz` | New |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.0.3-PrivateLane-macOS-arm64-qt.zip` | New |

### Gatekeeper note

Developer ID signed (`Mirsad Hajdarevic (JXJS6ZA5FJ)`). Not notarized — first
open on another Mac may show a Gatekeeper prompt. Right-click → Open to proceed.

Commit: `8f09a572c` (daemon) / `b792de850` (qt)

---

## v2.0.2-PrivateLane - macOS ring-spend + wallet path refresh (2026-04-04)

This tag refreshes the macOS Dinero release artifacts after the mainnet ring-spend
validation fixes, the confidential wallet path cleanup, and a full rebuild of the
portable macOS packaging path.

### Verified in this release

- Ring transaction round-tripping fixed in canonical serialization paths
- Mempool policy tightened so invalid ring spends no longer poison block templates
- `TransactionParser` now delegates to canonical transaction serialization logic
- Wallet-facing macOS binaries now use the canonical confidential view-key path
  `m/77'/1447'/0'/...`, with schema v25 migration rewriting legacy `144777` path
  strings on wallet open
- macOS Dinero-Qt bundle rebuilt from committed heads with embedded:
  - `dinerod` at commit `45f720dff8a4eb209acfeda961bd523ac095e548`
  - `dinero-miner` at commit `45f720dff8a4eb209acfeda961bd523ac095e548`
  - `dinero-gpu-miner` at commit `45f720dff8a4eb209acfeda961bd523ac095e548`
  - `dinero-solo-miner` at commit `c15de1f128508ce64df3fb6a74b08a9d1e06e5b5`
  - `dinero-qt` at commit `b792de850061d1236d352ce0065887eb3fa6fbd0`
- macOS release packaging now bundles non-system dylib dependencies into the app
  and standalone release folder so downloaded artifacts no longer depend on
  Homebrew runtime libraries

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `Dinero-v2.0.2-PrivateLane-macOS-arm64.tar.gz` | Updated |
| **macOS Qt** (Apple Silicon arm64) | `Dinero-v2.0.2-PrivateLane-macOS-arm64-qt.zip` | Updated |

### Gatekeeper note

These macOS artifacts are Developer ID signed and pass local `codesign --verify`
checks. They are not notarized in this repository refresh, so downloaded files may
still trigger the standard first-open Gatekeeper warning on another Mac.

### Scope note

This is a macOS refresh only. Existing Linux and Windows artifacts in this
repository are left unchanged.

Commit: `45f720dff` / `b792de850`

## v1.0.1-Utreexo - macOS Utreexo patch refresh (2026-03-21)

This tag refreshes the macOS Dinero bundle after the `v1.0.0-Utreexo` milestone.
It keeps the validated Utreexo architecture and updates the downloadable artifacts
to match the current approved desktop build.

### Verified in this release

- Switchable multi-wallet desktop flow with safer wallet unload/switch behavior
- Utreexo bridge diagnostics expanded into a real operator-facing proof dashboard
- Mining cinematic solved-block renderer hardened and crash-trimmed
- Mining solved-block card now ships with the approved single chain frame styling
- macOS Dinero-Qt bundle rebuilt with embedded:
  - `dinerod` at commit `0924ee6c5700fc82d66f15cc80b48527a359d306`
  - `dinero-miner` at commit `0924ee6c5700fc82d66f15cc80b48527a359d306`
  - `dinero-gpu-miner` at commit `0924ee6c5700fc82d66f15cc80b48527a359d306`
  - `dinero-qt` at commit `097f6dc3860385fc049745571c321f408e87a8f7`

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `dinero-v1.0.1-Utreexo-macos-arm64.tar.gz` | Updated |
| **macOS Qt** (Apple Silicon arm64) | `dinero-v1.0.1-Utreexo-macos-arm64-qt.zip` | Updated |

### Scope note

This is a macOS patch refresh only. It supersedes the older macOS `v1.0.0-Utreexo`
bundle and leaves existing Linux and Windows artifacts unchanged.

Commit: `0924ee6c5` / `097f6dc38`

## v1.0.0-Utreexo - Validated macOS Utreexo milestone (2026-03-20)

This tag freezes the first fully validated macOS Dinero-Qt release bundle with the
new Utreexo CSN architecture and embedded updated daemon/miners.

### Verified in this release

- Archived chain history replayed to exact bridge parity
- Proof-serving path validated across historical replay
- Restart, reorg, stale-proof recovery, and live-spend sync validated
- macOS Dinero-Qt bundle rebuilt with embedded:
  - `dinerod` at commit `582a4449050513080a39e7ba2661b44d0ac2342d`
  - `dinero-miner` at commit `582a4449050513080a39e7ba2661b44d0ac2342d`
  - `dinero-gpu-miner` at commit `582a4449050513080a39e7ba2661b44d0ac2342d`
  - `dinero-qt` at commit `5663fa65b5e8b1830f385ca77fc0f9c12e0594d9`

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `dinero-v1.0.0-Utreexo-macos-arm64.tar.gz` | Updated |
| **macOS Qt** (Apple Silicon arm64) | `dinero-v1.0.0-Utreexo-macos-arm64-qt.zip` | Updated |

### Operational note

Old unverified pre-fix CSN state should be wiped and resynced unless exact tip and
stump parity has already been proven after upgrade. Parity-verified current state
does not need to be wiped.

### Scope note

This tag refreshes the validated macOS artifacts. Existing Linux and Windows
artifacts in this repository are left unchanged.

Commit: `582a44490`

## v0.9.0 - Fair Launch: No Premine, DIN Ticker (2026-03-02)

### BREAKING CHANGES — CHAIN RESET REQUIRED

This is a **full chain reset** release. All nodes must wipe chain data and start fresh.

**What changed:**
- **Premine removed**: All premine code deleted (~25,000 lines). Genesis coinbase burns 100 DIN via OP_RETURN — zero coins minted at launch
- **Ticker renamed**: DNR → DIN, RDNR → RDIN, TDNR → TDIN
- **Address prefix changed**: `dnr1p...` → `din1p...` (Bech32m HRP updated)
- **New genesis block**: Fair Launch v3 — mined fresh with no premine

### Mandatory: Wipe all chain data

```bash
# Linux/macOS daemon nodes
rm -rf ~/.dinero

# dinero-qt users: the app will auto-detect stale data and offer "Wipe and Restart"
```

### New genesis anchors

- `genesis_hash`: `000000127a16de2416e3d5ee104436e1cc7806bb47927bac266497d726acc29a`
- `genesis_nonce`: `276919913`
- `genesis_difficulty`: `0x1d31ffce` (50x easier than Bitcoin)
- `genesis_timestamp`: `1772496000` (2026-03-03 00:00:00 UTC)
- `genesis_merkle_root`: `0f7d1982fb9c5ae07428dfa0a4acfa6fb540fcc967ea61904c503e248e6c6a41`
- `motto`: "Dinero: Real Money For Free People"
- **No premine** — coinbase burned via OP_RETURN, empty UTXO set at genesis

### dinerod
- All premine code removed (subsidy, treasury, governance)
- Ticker DIN with 8 decimal places (1 DIN = 100,000,000 unas)
- Address HRP: `din` (mainnet), `tdin` (testnet), `rdin` (regtest)
- Fair Launch v3 genesis block
- OpenSSL link fix for test_block_storage_roundtrip
- Hardcoded utreexo root to prevent platform divergence (GCC vs Clang)

### dinero-qt
- Updated embedded dinerod with all changes above
- Addresses now display as `din1p...`

### Downloads

| Platform | File | Status |
|----------|------|--------|
| **macOS** (Apple Silicon arm64) | `dinero-v0.9.0-macos-arm64.tar.gz` | Updated |
| **Linux** (x86_64) | `dinero-v0.9.0-linux-x86_64.tar.gz` | Updated |
| **Windows** (x86_64) | `dinero-v0.9.0-windows-x86_64.zip` | Pending |

### Binaries included

- `dinerod` — Full node daemon
- `dinero-cli` — Command-line RPC client
- `dinero-miner` — CPU miner
- `dinero-gpu-miner` — GPU miner (macOS Metal)
- `dinero-qt.app` — GUI wallet with embedded miner (macOS only)

### Verification

```bash
cd mac && shasum -a 256 -c SHA256SUMS.txt
cd linux && shasum -a 256 -c SHA256SUMS.txt
```

### Post-start sanity check

After wipe + first start:

```bash
dinero-cli getblockhash 0
# Expected: 000000127a16de2416e3d5ee104436e1cc7806bb47927bac266497d726acc29a
```

Commit: `f1c5a10aa`

---

## Previous Releases

<details>
<summary>v0.8.7 and earlier</summary>

### v0.8.7 - Block Height Fix (2026-03-03)
- Peer heights update on block receipt
- Live peer height tracking in getpeerinfo RPC
- Wallet sync RPC (getreorginfo, getsyncstatus, getslowreason)
- GPU miner fixes

### v0.8.5 - Metal GPU Mining (2026-03-03)
- Apple Silicon GPUs (M1/M2/M3/M4) mine SHA-256d natively via Metal compute shaders
- Multi-backend GPU architecture: CUDA, Metal, OpenCL

### v0.8.4 - P2P Peer Discovery (2026-03-03)
- DNS seed resolution, peer address exchange, persistent peer storage

### v0.8.3 - P2P Auto-Connect Fix (2026-02-28)
- Automatic seed connection, duplicate connection race fix

### v0.8.2 - P2P Multi-Peer Deadlock Fix (2026-02-28)
- peers_mutex_ deadlock, global send_mutex_ bottleneck, double-lock deadlock

### v0.8.1 - Genesis Guard (2026-02-27)
- Stale chain data detection, exit code 10, --wipe-stale-chain flag

### v0.8.0 - Dark Chrome UI (2026-02-26)
- Professional dark theme, wallet fixes, mining console

</details>

## Links

- Website: https://dinero-coin.com (coming soon)

---

Dinero: Real Money for Free People — Genesis Block 2026
