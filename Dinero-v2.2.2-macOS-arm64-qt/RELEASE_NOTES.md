# Dinero v2.2.2 macOS arm64 Qt

Release date: 2026-04-29

This macOS Apple Silicon release refreshes the signed Qt wallet after the final
shielded-wallet, hardware-wallet, Rust bridge, and mobile framework cleanup.
It embeds the latest daemon and helper binaries, keeps the post-recovery
atomic chainstate build, and updates the app metadata to `2.2.2`.

## Included refs

| Component | Ref | Notes |
|---|---:|---|
| Daemon / CLI / miners | `aa97c1882` | v7 coin-type docs aligned; atomic shielded/chainstate fixes retained |
| Qt wallet | `9b2f27a` | v2.2.2 bundle metadata and latest shielded/hardware wallet UI |
| Rust Tier-3 bridge | `db34fea6f` | chain params synced with v7 daemon |
| DineroDPI | `21be2c1` | embedded consensus frameworks refreshed; receive state stays responsive |
| Stratum V1 server | `2fcfe40` | localhost bind support |
| SV2 miners | `91fa22b` | current release CPU/GPU SV2 miners |

## What's new since v2.2.1

- **Qt app reports `2.2.2`** - bundle version, `Info.plist`, and release
  identity manifest are all refreshed.
- **Shielded wallet polish** - receive/balance view remains available for
  viewing while locked, note counts distinguish confirmed vs pending notes, and
  send paths continue to require passphrase confirmation.
- **Shielded send safety** - shielded transaction IDs now commit to the shielded
  bundle, so different shielded payloads cannot share the same legacy txid hash.
- **Hardware wallet cleanup** - PSBT copy now says "Partially Signed Dinero
  Transaction", hardware wallet colors match the rest of the app chrome, 1447
  derivation defaults are retired, and QR signing shows a decoded transaction
  summary before displaying a scannable QR.
- **QR signing visual polish** - QR output uses the Dinero logo in the center
  and includes a demo placeholder that is clearly not a real transaction.
- **v7 derivation alignment** - desktop, mobile, and Rust bridge docs/headers
  all point at `coin_type 1448`; `1447` remains retired.
- **All helper binaries embedded** - daemon, CLI, CPU/GPU miners, Stratum
  worker/server, solo miner, and SV2 CPU/GPU miners are inside `Dinero-Qt.app`.

## Verification

The extracted Qt app should pass:

```text
spctl -a -t exec -vv Dinero-Qt.app
accepted
source=Notarized Developer ID
origin=Developer ID Application: Mirsad Hajdarevic (JXJS6ZA5FJ)
```

Release identity inside the app:

```json
{
  "qt": {
    "version": "2.2.2",
    "commit": "9b2f27a2c3309010b749c62efd44257f359d0b38"
  },
  "expected_repo_heads": {
    "dinero": "aa97c18827bb8d1deddcacc26e11bb1c5c32fd74"
  }
}
```
