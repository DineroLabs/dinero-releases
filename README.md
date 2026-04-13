# Dinero Releases

Official binary releases for Dinero (DIN) - Real Money for Free People.

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
