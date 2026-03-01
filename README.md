# Dinero Releases

Official binary releases for Dinero (DNR) - Real Money for Free People.

## v0.8.3 - P2P Auto-Connect Fix (2026-02-28)

### dinerod
- **Fix automatic seed connection**: Nodes now reliably auto-connect to `-addnode` peers on startup without requiring manual `addnode "onetry"` RPC calls
- **Fix one-seed-per-cycle bottleneck**: Connection manager now tries ALL unconnected seeds per iteration instead of only one every 10 seconds
- **Fix duplicate connection race**: Added `connecting_peers_` guard to prevent overlapping handshakes to the same peer
- **Fix multiple `-addnode` CLI flags**: Multiple `-addnode=X -addnode=Y` flags now append correctly instead of the last one overwriting all previous
- **Deduplicate seed nodes**: Same seed from CLI + hardcoded list no longer creates duplicate entries

### dinero-qt
- **Updated embedded dinerod** with all auto-connect fixes above
- **Dark theme styling**: Receive tab now matches dark chrome theme

### Impact
All nodes were affected — seeds configured via `-addnode` or hardcoded would not auto-connect. Users had to manually run `addnode "onetry"` via RPC after every restart.

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Updated |
| **Linux** (x86_64) | Updated |

## v0.8.2 - P2P Networking Fix: Multi-Peer Deadlock (2026-02-28)

### dinerod
- **Fix peers_mutex_ deadlock**: Mutex was held during message deserialization, causing 188+ threads to pile up in futex_wait — nodes with 2+ peers would stall
- **Fix global send_mutex_ bottleneck**: One slow peer send (e.g. 260KB headers) blocked ALL other peer sends; replaced with per-socket send locks
- **Fix double-lock deadlock**: send_to_peer + send_message both acquiring same mutex caused immediate deadlock on std::mutex (non-reentrant)
- **P2PService scheduler tick loop**: Block download schedulers now tick every 5s to prevent stalls during headers-first sync
- **Getheaders storm prevention**: Only request headers on outbound connections; inbound peers initiate their own

### dinero-qt
- **Updated embedded dinerod** with all P2P fixes above

### Impact
Any node with 2+ peers was affected. Symptoms: sync stalls, blocks requested but never received, TCP receive window closure (`rwnd_limited`). Single-peer connections worked but multi-peer topologies would deadlock within minutes.

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Updated |
| **Linux** (x86_64) | Updated |

## v0.8.1 - Genesis Guard: Stale Chain Data Detection (2026-02-27)

### dinerod
- **Genesis guard**: Detects incompatible chain data from pre-chain-reset versions before full daemon init
- **Exit code 10**: Stable contract for GUI to detect genesis mismatch
- **`--wipe-stale-chain` flag**: Backs up wallet, wipes stale chain data, proceeds with fresh sync

### dinero-qt
- **Automatic stale chain detection**: Shows dialog when embedded daemon exits with genesis mismatch
- **"Wipe and Restart" button**: One-click fix — backs up wallet and re-syncs from scratch

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Updated |
| **Linux** (x86_64) | Binaries unchanged from v0.8.0 |

## v0.8.0 - Dark Chrome UI, Wallet Fixes, Mining Console (2026-02-26)

### dinero-qt
- **Dark chrome theme**: Professional dark UI with unified styling across all tabs
- **Single wallet mode**: Auto-loads "default" wallet on startup
- **Merged toolbar**: Status bar folded into single toolbar row
- **Wallet lock fix**: Lock button no longer overridden by daemon polling
- **Rescan Wallet button**: Explicit blockchain rescan with confirmation
- **wallet.load fallback**: Compat with daemons exposing `wallet.load`
- **Emergency restore hardening**: Wizard warns restore is migration/recovery only
- **BIP39 checksum bypass**: Recover wallets from older versions
- **Space Mono mining console**: Deterministic font fallback chain
- **100-char comet mining animation**

### dinerod
- **Gap-limit address derivation on restore** (20 external + 20 change)
- **Block-scanning rescan** (replaces broken forEachUTXO for Utreexo)
- **wallet.getinfo `unlocked` field** in both handlers
- **Mining setThreadCount race fix**, threads=0 auto-detect
- **mining.start**: object params, --miningthreads config, max 256

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Updated |
| **Linux** (x86_64) | Updated |

## v0.7.7 - Single-Wallet Mode, Block-Scanning Rescan, Mining Fixes (2026-02-26)

### dinero-qt
- **Single wallet mode**: auto-loads "default" wallet, hides multi-wallet selector
- **Rescan Wallet button**: explicit blockchain rescan with confirmation dialog
- **wallet.load fallback**: compatibility with daemons that expose `wallet.load` instead of `wallet.open`
- **Emergency restore hardening**: wizard warns restore is for migration/recovery only
- **BIP39 checksum bypass**: checkbox to recover wallets created by older versions with checksum bugs
- **Disconnect cleanup**: properly resets wallet name, flags, and UI on daemon disconnect
- **Mining animation**: 100-character comet trails replace single-glyph flares

### dinerod
- **Gap-limit address derivation on restore**: pre-derives 20 external + 20 change addresses so rescan discovers all historical UTXOs
- **Block-scanning rescan**: replaces broken `forEachUTXO` approach (Utreexo nodes don't store full UTXO set) with proper block-walking rescan
- **storeMasterSeed clears stale tx history**: prevents ghost transactions from previous wallet seed
- **Mining `setThreadCount` race fix**: inline clean shutdown instead of calling `stopMining()`; `threads=0` means auto-detect
- **`mining.start` RPC improvements**: supports object params `{"threads":N, "address":"..."}`, reads `--miningthreads` config, max threads bumped to 256
- **NodeCore xcframework**: deterministic production-only lib list (no more test archive symbol collisions)

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Updated in this release |
| **Linux** (x86_64) | Binaries unchanged from v0.7.6 |

## v0.7.6 - Correct Daemon Embed (2026-02-26)

This release refreshes bundled daemon binaries so GUI wallet switching uses the correct RPC-capable daemon build:

- Updated `dinerod` to include `wallet.open` (active-wallet switching)
- Updated bundled `dinero-cli` and `dinero-miner` to the same build set
- Refreshed both **macOS arm64** and **Linux x86_64** artifacts

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Updated in this release |
| **Linux** (x86_64) | Updated in this release |

## v0.7.5 - Wallet Switching + Seed Reveal UX (2026-02-26)

This release focuses on safer, clearer wallet operations in `dinero-qt` and daemon RPC:

- Added `wallet.open` RPC handler for explicit active-wallet switching
- Added wallet selector + `Load Wallet` action in the `dinero-qt` top toolbar
- Fixed seed display UX in wallet creation:
  - Seed now appears automatically after generation
  - Reveal action is click/toggle (no press-and-hold requirement)
  - Added fallback seed-field parsing for compatibility (`mnemonic`, `seed_phrase`, `seedPhrase`)
  - Prevents continuing when seed phrase is unavailable

### Chain reset requirement

No additional chain reset is required for v0.7.5.

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Updated in this release |
| **Linux** (x86_64) | Binaries unchanged from previous release |

## v0.7.4 - Mainnet Chain Reset Release (2026-02-25)

This release includes a consensus-locked premine re-mine and Utreexo v2 commitment alignment.
It refreshes `dinero-qt` with additional mining UI and cinematic polish:

- Mining-tab surrounding UI now darkens (instead of brightening) after active-mining delay
- Template and block-found markers use Dinero symbol styling (`D` with dual vertical lines)
- Block-found icon uses a yellow coin with Dinero symbol inside
- Small and large cinematic comets increased for a more alive mining output
- ~50% of single spark pulses now render as short comets
- Added an extra-long ~40-character comet approximately once per minute

### Mandatory Action: wipe all existing chain data

Do this before starting upgraded nodes, otherwise you can stay pinned to stale state.

```bash
# Linux/macOS daemon nodes
rm -rf ~/.dinero/blockchain ~/.dinero/blocks ~/.dinero/headers ~/.dinero/utreexo \
       ~/.dinero/blockchaindb ~/.dinero/mempool.db ~/.dinero/peers.db ~/.dinero/blocks.db
```

For DineroDPI, this release path expects a full reset (wallet + chain data).
Full operator runbook: `CHAIN_RESET.md`.

### Canonical chain anchors

- `genesis_hash`: `0000002ef4d930597e7978d7b3b6864fe212bef920dc95ddb9794184dbcfecad`
- `genesis_nonce`: `3762293379`
- `premine_hash` (height 1): `00000026ec31b18fca329fc77084cdfaa9c446a8097cc76061098a54c467bad4`
- `premine_nonce`: `2695050871`
- `premine_utreexo_root`: `cff69d5f4f799c9f8d024df3acb6dd1bdf0e3e7988f8789bc2a6b07d0665abfc`
- `premine_address`: `dnr1pegrzhlug8ak32yd89fu2p8e6zl9kwd8ee6z5874xdalrsr2c6xmss6h8k0`

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Updated in this release |
| **Linux** (x86_64) | Binaries unchanged from previous release |

### Binaries

- `dinerod` - Full node daemon
- `dinero-cli` - Command-line RPC client
- `dinero-miner` - CPU miner
- `dinero-qt.app` - GUI wallet with embedded miner (macOS only)

### Verification

```bash
cd mac && shasum -a 256 -c SHA256SUMS.txt
cd linux && shasum -a 256 -c SHA256SUMS.txt
```

### Post-start sanity check

After the wipe + first start:

```bash
dinero-cli getblockhash 0
dinero-cli getblockhash 1
```

Expected:

- height 0: `0000002ef4d930597e7978d7b3b6864fe212bef920dc95ddb9794184dbcfecad`
- height 1: `00000026ec31b18fca329fc77084cdfaa9c446a8097cc76061098a54c467bad4`

## Links

- Website: https://dinero-coin.com (coming soon)

---

Dinero: Real Money for Free People - Genesis Block 2025
