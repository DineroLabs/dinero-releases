# Dinero Releases

Official binary releases for Dinero (DNR) - Real Money for Free People.

## v0.3.0-stable

Major daemon and wallet hardening release with sync profiles, CLI parser fix, and export seed support.

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

### What's new in v0.3.0

- **wallet.exportseed RPC** — new endpoint returns HD mnemonic phrase for cross-device restore
- **dinero-qt Export Seed fix** — "Export Seed for Mobile" button now works correctly
- **Sync profile policy** — platform-aware sync profiles (mac_fullblock, ios_utreexo) with capability gating
- **Mining context RPC gating** — mining RPCs hard-gated by sync profile capabilities
- **CLI arg parser fix** — space-separated args (`--datadir /path`) now parsed correctly; `--listen`, `--rpc`, `--server` support optional-value semantics
- **Wallet hardening** — per-address balance computation, defensive seed recovery, empty-result failsafe on getnewaddress
- **NodeCore FFI** — utreexo_stateless wired from config, platform-default sync profiles

### What was in v0.2.0

- Encrypted-at-rest wallet storage (AES-256-GCM)
- KDF parameter persistence and version-dispatched seed decryption
- Legacy plaintext load blocked (migration required)
- FFI security hardening (global mnemonic removed, ScopeExit RAII cleanup)

### What was in v0.1.0

- BIP86 Taproot-only wallet (m/86'/1447'/0'/0/*)
- BIP39 12-word mnemonic seed backup
- SHA256d Proof-of-Work consensus
- PSBT support (hardware wallet compatible)
- Canonical PBKDF2-HMAC-SHA512 key derivation

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
