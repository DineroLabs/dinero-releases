# Dinero v2.2.5 macOS arm64 Qt

Release date: 2026-05-01

This macOS Apple Silicon release ships the post-audit undo-rebuilder fix and
Mac launcher repair on top of the v2.2.4 embedded Qt bundle. It embeds the
latest daemon, CLI, miners, Stratum helpers, solo miner, and SV2 miners inside
the signed and notarized `Dinero-Qt.app`.

## Included refs

| Component | Ref | Notes |
|---|---:|---|
| Daemon / CLI / miners | `0786b9e10` | undo-rebuilder temp DB fix, regression guard, incident report, and Mac wallet-socket launch fix |
| Qt wallet | `a4006ea` | v2.2.5 bundle metadata |
| Solo miner | `71ee61e` | embedded solo miner CLI |
| Stratum V1 server | `2fcfe40` | localhost bind support |
| SV2 miners | `91fa22b` | current release CPU/GPU SV2 miners |

## What's new since v2.2.4

- **Qt app reports `2.2.5`** - bundle version, `Info.plist`, and release
  identity manifest are refreshed.
- **Undo-rebuilder hole-only mode fixed** - already-ok blocks now still
  populate the temporary UTXO DB, so historical undo holes rebuild cleanly.
- **Historical undo holes repaired across fleet** - CN, LA, VA, MO, and Mac
  repair paths validated; Mac rebuilt 247 holes with `verify_failed=0`.
- **Mac daemon launch fixed** - `--wallet-socket-port` and
  `DINERO_WALLET_SOCKET_PORT` now survive `DaemonApp::Init()`, avoiding the
  stale `50051` bind failure on macOS.
- **Regression coverage added** - the hole-only optimization now has a guard
  against skipping temp-DB mutation.
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
    "version": "2.2.5",
    "commit": "a4006ea807f46917798e0f50d5fdb78a4485e93a"
  },
  "expected_repo_heads": {
    "dinero": "0786b9e1078ac4f6f585014907be17941ed6d84b"
  }
}
```
