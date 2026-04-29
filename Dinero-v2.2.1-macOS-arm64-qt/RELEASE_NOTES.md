# Dinero v2.2.1 macOS arm64 Qt

Release date: 2026-04-29

## Included refs

- Daemon: Dinero-Coin `85652d00b0185bbd25488d9237deca9b85534e77`
- Qt: dinero-qt `0380dd9a9e10c871cf5456ac6c7465c58b6c0e1b`

## Highlights

- Atomic shielded/chainstate persistence hardening.
- Canonical reindex recovery for the April 29 Utreexo forest-root incident.
- Explicit rejection of retired `coin_type 1447` wallet paths/descriptors; v7 uses `coin_type 1448` only.
- Dinero-Qt v2.2.1 bundle with embedded daemon and wallet UX fixes through `qt-main`.

## Operator note

Do not run pre-v2.2.1 binaries against recovered mainnet fleet datadirs. Old `coin_type 1447` wallets/descriptors must be rederived or restored with `coin_type 1448`.
