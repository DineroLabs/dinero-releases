# Dinero Releases

Official binary releases for Dinero (DNR) - Real Money for Free People.

## v0.7.0 - Mainnet Chain Reset Release (2026-02-24)

This release includes a consensus-locked premine re-mine and Utreexo v2 commitment alignment.

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
