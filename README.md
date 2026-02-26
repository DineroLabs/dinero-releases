# Dinero Releases

Official binary releases for Dinero (DNR) - Real Money for Free People.

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
