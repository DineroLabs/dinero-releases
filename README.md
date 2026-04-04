# Dinero Releases

Official binary releases for Dinero (DIN) - Real Money for Free People.

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
