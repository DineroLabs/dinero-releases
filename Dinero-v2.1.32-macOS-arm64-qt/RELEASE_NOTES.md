# Dinero v2.1.32 — Shielded Pool Mainnet Activation

**Release date:** 2026-04-27 (signing); notarisation pending.

## Headline

Shielded pool goes live on mainnet at **block 8650**. Until that
height the existing transparent + P2MR + Vault flows behave exactly
as they did in v2.1.31; once the chain crosses 8650, shielded RPCs
go from `shielded_not_active` to functional and the new 🛡 Shielded
tab in dinero-qt becomes operational.

## What's new (user-visible)

- **🛡 Shielded tab** in dinero-qt with:
  - Status banner (active / parked).
  - Shielded balance + tree size + confirmed-note count.
  - Receive address (account 0, j-bumper, copy button) with
    persistent address book across restarts.
  - Shield (transparent → shielded) form: amount in DIN + fee.
  - Send shielded form: rdins/tdins/dins recipient + DIN/una
    amount mirroring + 512-byte UTF-8 memo + fee.
  - Unshield (shielded → transparent) form.
  - Notes table: leaf_index, value, state, commitment hex.
  - Activity log of submitted txids.
  - Auto-refresh on tip advance.

## What's new (under the hood)

- **Path C consensus** — Pedersen value commitments + BIP340 Schnorr
  binding signature + `pedersen_verify_tally` for cross-bundle
  supply integrity. Generator V derived from
  `"DIN/v7/shielded/cv/V/v1"`.
- **Phase 5 derivation** — Sapling-shape sk → ask/nsk → ak/nk → ivk
  → diversifier → P_d → pk_d → bech32m address. BIP32 path
  `m/99'/1448'/account'`. dins / tdins / rdins HRPs.
- **Encrypted notes** — secp256k1 ECDH with `epk = esk · P_d`,
  HKDF-SHA256 key schedule, ChaCha20-Poly1305 AEAD over the
  563-byte plaintext. Receiver scans every shielded output's
  encrypted_note against accounts 0..3 ivks via
  `TryDecryptNoteForViewer`.
- **Daemon RPCs**:
  - `wallet.shield`
  - `wallet.unshield`
  - `wallet.transfer` (self / multi-spend / addressed-with-memo)
  - `wallet.shieldedbalance`
  - `wallet.listshielded`
  - `wallet.getshieldedaddress`
- **Test surface** — 25/25 ctests green: shielded validation, v0.3.0
  wire vectors, Phase 5 key + address derivation, ECDH/AEAD round
  trip, A→B addressed transfer, multi-account receive scan, §7.1
  HRP enforcement, restart-equivalence and reorg-disconnect
  fixtures, e2e RPC ctests for shield, unshield, transfer (multi /
  addressed / addressed-detect).

## Compose

- **Daemon:** `Dinero-Coin/p2p-fix @ 93f09f115`
- **Qt UI:** `dinero-qt/qt-main @ c62e02a` (tagged v2.1.32)

## Activation

`chainparams_impl.cpp:78` — mainnet `shielded_activation_height = 8650`.

Tip at decision time was 8602; +48 blocks ≈ ~8 hours at typical
~10-min mainnet cadence. Rationale, risk inventory, and operator
recovery procedures live in
`Dinero-Coin:docs/specs/shielded_activation_plan.md`.

Testnet stays at `UINT32_MAX` (parked). Regtest stays at 0 (active).

## Notarisation

This release is signed (Developer ID `Mirsad Hajdarevic JXJS6ZA5FJ`)
but **not yet notarised**. To finish the release before public
distribution:

```sh
xcrun notarytool submit Dinero-Qt.app \
    --apple-id <your-apple-id> \
    --team-id JXJS6ZA5FJ \
    --password <app-specific-password> \
    --wait
xcrun stapler staple Dinero-Qt.app

# Re-zip after stapling (the ticket modifies the bundle):
ditto -ck --keepParent Dinero-Qt.app Dinero-v2.1.32-macOS-arm64-qt.zip
```

The signed app bundle is in this directory. After notarisation +
stapler, replace this RELEASE_NOTES.md "Notarisation" section with
"Notarised: <date>" and zip the bundle.

## Linux server build (separate)

The Linux x86_64 / arm64 server `dinerod` builds aren't packaged
here. Server fleet (LA / VA / MO / CN) builds from source via:

```sh
cd /root/Dinero-Coin
git fetch origin p2p-fix && git checkout p2p-fix && git pull origin p2p-fix
cmake --build build --target dinerod -j$(nproc)
cp build/dinerod build/dinerod.backup.pre-93f09f115
systemctl stop dinerod && sleep 3 && systemctl start dinerod
```

Run sequentially (LA → VA → MO → CN), keeping ≥3 nodes serving
during each restart.
