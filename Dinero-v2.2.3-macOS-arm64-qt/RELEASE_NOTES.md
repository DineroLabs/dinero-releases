# Dinero v2.2.3 macOS arm64 Qt

Release date: 2026-04-30

This macOS Apple Silicon release ships the post-incident daemon hardening and
the refreshed Qt wallet privacy UX. It embeds the latest daemon, CLI, miners,
Stratum helpers, solo miner, and SV2 miners inside the signed and notarized
`Dinero-Qt.app`.

## Included refs

| Component | Ref | Notes |
|---|---:|---|
| Daemon / CLI / miners | `5a6cc0799` | ConnectTip durability checks decode undo, bind UTXO batch, and preserve hardened reorg recovery |
| Qt wallet | `8557108` | v2.2.3 bundle metadata plus public/private wallet balance and send UX |
| Solo miner | `71ee61e` | embedded solo miner CLI |
| Stratum V1 server | `2fcfe40` | localhost bind support |
| SV2 miners | `91fa22b` | current release CPU/GPU SV2 miners |

## What's new since v2.2.2

- **Qt app reports `2.2.3`** - bundle version, `Info.plist`, and release
  identity manifest are all refreshed.
- **Reindex shielded validation fixed** - reindex and live ConnectTip now share
  the same shielded validation context source of truth, including tx sighash.
- **Reorg durability hardened** - ConnectTip verifies disconnect material before
  publishing the active tip, with undo decoding and UTXO-batch binding checks.
- **Missing-undo recovery broadened** - disconnect recovery can regenerate undo
  for transparent and shielded block shapes that would previously wedge reorgs.
- **Utreexo delta atomicity** - Utreexo disconnect sidecar data is committed in
  the canonical tip batch instead of a later side write.
- **Wallet balance UX clarified** - the Balance panel now separates public
  transparent Taproot, public P2MR quantum-safe, shielded private, pending, and
  mining balances.
- **Send UX reflects privacy intent** - Send now offers public send, private
  spend, convert-to-public, and contracts as explicit modes.
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
    "version": "2.2.3",
    "commit": "8557108f4e947727a8118368a935181caa0eff51"
  },
  "expected_repo_heads": {
    "dinero": "5a6cc07994a9a63aa0da76a354e6e396cf5a3e8b"
  }
}
```
