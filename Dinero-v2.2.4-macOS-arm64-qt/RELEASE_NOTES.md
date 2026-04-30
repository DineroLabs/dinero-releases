# Dinero v2.2.4 macOS arm64 Qt

Release date: 2026-04-30

This macOS Apple Silicon release ships the chainstate commit-batch wiring on top
of the post-incident daemon hardening and the refreshed Qt wallet privacy UX. It
embeds the latest daemon, CLI, miners, Stratum helpers, solo miner, and SV2
miners inside the signed and notarized `Dinero-Qt.app`.

## Included refs

| Component | Ref | Notes |
|---|---:|---|
| Daemon / CLI / miners | `9c53f169f` | ConnectTip commit contract plus crash oracles, missing-undo coverage, and safe undo metadata audit |
| Qt wallet | `2489f64` | v2.2.4 bundle metadata plus public/private wallet balance and send UX |
| Solo miner | `71ee61e` | embedded solo miner CLI |
| Stratum V1 server | `2fcfe40` | localhost bind support |
| SV2 miners | `91fa22b` | current release CPU/GPU SV2 miners |

## What's new since v2.2.3

- **Qt app reports `2.2.4`** - bundle version, `Info.plist`, and release
  identity manifest are all refreshed.
- **Chainstate commit contract wired** - ConnectTip now stages consensus-critical
  material through `ChainstateCommitBatch` before committing the active tip.
- **Incomplete commits fail early** - missing required batch fields now produce
  a clean `commit-batch-incomplete-*` failure instead of a latent reorg wedge.
- **Crash and missing-undo oracles added** - ConnectTip/DisconnectTip restart
  boundaries and user-transaction missing-undo regeneration are covered.
- **Undo metadata audit added** - historical undo metadata can be audited
  safely, with re-stamp refusal when undo material is not recoverable.
- **Active-tip publication centralized** - direct `active_tip_ = ...` writes are
  consolidated behind `PublishActiveTip(tip, reason)`.
- **Runtime durability checks retained** - undo decoding, UTXO read-back,
  Utreexo sidecar checks, and shielded reverse material validation still run
  before publishing a new tip.
- **Wallet balance UX clarified** - the Balance panel separates public
  transparent Taproot, public P2MR quantum-safe, shielded private, pending, and
  mining balances.
- **Send UX reflects privacy intent** - Send offers public send, private spend,
  convert-to-public, and contracts as explicit modes.
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
    "version": "2.2.4",
    "commit": "2489f6437c77acd1164256c936270afc9d12270f"
  },
  "expected_repo_heads": {
    "dinero": "9c53f169f788f310384e978c8372079ac42bbae4"
  }
}
```
