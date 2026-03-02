# Dinero Releases

Official binary releases for Dinero (DIN) - Real Money for Free People.

## v0.9.0 - Fair Launch: No Premine, DIN Ticker (2026-03-02)

### BREAKING CHANGES — CHAIN RESET REQUIRED

This is a **full chain reset** release. All nodes must wipe chain data and start fresh.

**What changed:**
- **Premine removed**: All premine code deleted (~25,000 lines). Genesis coinbase burns 100 DIN via OP_RETURN — zero coins minted at launch
- **Ticker renamed**: DNR → DIN, RDNR → RDIN, TDNR → TDIN
- **Address prefix changed**: `dnr1p...` → `din1p...` (Bech32m HRP updated)
- **New genesis block**: Fair Launch v2 — mined fresh with no premine

### Mandatory: Wipe all chain data

```bash
# Linux/macOS daemon nodes
rm -rf ~/.dinero

# dinero-qt users: the app will auto-detect stale data and offer "Wipe and Restart"
```

### New genesis anchors

- `genesis_hash`: `0000000d20f34068ad683b98484f45c7e6e9b5c2fda061791558ee9ea371a8a0`
- `genesis_nonce`: `4038889668`
- `genesis_difficulty`: `0x1d31ffce` (50x easier than Bitcoin)
- `genesis_timestamp`: `1772323200` (2026-03-01 00:00:00 UTC)
- `genesis_merkle_root`: `0f7d1982fb9c5ae07428dfa0a4acfa6fb540fcc967ea61904c503e248e6c6a41`
- `motto`: "Dinero: Real Money For Free People"
- **No premine** — coinbase burned via OP_RETURN, empty UTXO set at genesis

### dinerod
- All premine code removed (subsidy, treasury, governance)
- Ticker DIN with 8 decimal places (1 DIN = 100,000,000 unas)
- Address HRP: `din` (mainnet), `tdin` (testnet), `rdin` (regtest)
- Fair Launch v2 genesis block
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
# Expected: 0000000d20f34068ad683b98484f45c7e6e9b5c2fda061791558ee9ea371a8a0
```

Commit: `f1c5a10aa`

---

## Previous Releases

<details>
<summary>v0.8.7 and earlier</summary>

### v0.8.7 - Block Height Fix (2026-03-01)
- Peer heights update on block receipt
- Live peer height tracking in getpeerinfo RPC
- Wallet sync RPC (getreorginfo, getsyncstatus, getslowreason)
- GPU miner fixes

### v0.8.5 - Metal GPU Mining (2026-03-01)
- Apple Silicon GPUs (M1/M2/M3/M4) mine SHA-256d natively via Metal compute shaders
- Multi-backend GPU architecture: CUDA, Metal, OpenCL

### v0.8.4 - P2P Peer Discovery (2026-03-01)
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
