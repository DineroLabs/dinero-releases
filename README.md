# Dinero Releases

Official binary releases for Dinero (DNR) - Real Money for Free People.

## v0.5.0-stable — Chain Reset

**BREAKING**: New genesis block. All nodes must wipe chain data and resync.

Genesis: `0000002ef4d930597e7978d7b3b6864fe212bef920dc95ddb9794184dbcfecad`

### What changed

- **New genesis block** — remined with v2 Utreexo commitment and fixed merkle root byte order
- **New premine block** — checkpoint `00000026ec31b18fca329fc77084cdfaa9c446a8097cc76061098a54c467bad4`
- **Utreexo v2 commitment** — empty forest uses SHA256(numLeaves || roots) instead of all-zeros
- **Domain-separated hashing** — BIP340 taggedHash for Utreexo leaf computation
- **dinero-qt** — GUI wallet with embedded one-click miner
- **Legacy wallet compatibility** — HMAC-SHA512 KDF fallback for pre-v0.4.0 wallets

### Upgrade Instructions

```bash
# 1. Stop dinerod
pkill -f dinerod

# 2. Wipe old chain data (wallet seeds are NOT affected)
DATA_DIR=~/Dinero-Coin/data-main
rm -rf "$DATA_DIR/blocks" "$DATA_DIR/chainstate" "$DATA_DIR/peers.dat" \
       "$DATA_DIR/utreexo" "$DATA_DIR/banlist.dat" "$DATA_DIR/fee_estimates.dat"

# 3. Replace binaries and start
./dinerod
```

### Downloads

| Platform | Status |
|----------|--------|
| **macOS** (Apple Silicon arm64) | Available |
| **Linux** (x86_64) | Available |

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

## Quick Start

1. Download binaries for your platform
2. **macOS: remove quarantine flag before first run:**
   ```bash
   xattr -cr dinerod dinero-cli dinero-miner dinero-qt.app
   ```
3. Run `dinerod` to start the node
4. Run `dinero-qt.app` for GUI wallet (macOS)
5. Create wallet with 12-word seed phrase

## Links

- Website: https://dinero-coin.com (coming soon)

---

Dinero: Real Money for Free People - Genesis Block 2025
