<div align="center">

<img src="https://github.com/DineroLabs.png?size=140" width="120" alt="Dinero" />

# Dinero · Official Releases

**Real Money For Free People.**
Post-quantum, utreexo-native, fair-launched. Mobile-mineable.

[![Latest release](https://img.shields.io/github/v/release/DineroLabs/dinero-releases?label=release&color=0066CC)](https://github.com/DineroLabs/dinero-releases/releases/latest)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Website](https://img.shields.io/badge/website-dinero--coin.com-FFD700)](https://dinero-coin.com)

</div>

---

## Choose By Platform

GitHub shows all files in one asset list, so use this table first.

| Platform / Need | Download | Release Page | Status |
|---|---|---|---|
| **macOS Apple Silicon** | `Dinero-v2.2.5-macOS-arm64-qt.zip` | [`v2.2.5`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5) | Stable signed/notarized Qt wallet |
| **Linux Ubuntu 24.04+ operators** | `dinero-core_2.2.5-3_amd64.deb` | [`v2.2.5-rc3`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5-rc3) | Core `.deb` release candidate |
| **Windows desktop users (recommended)** | ⬇ **[`Dinero-Setup-2.2.5-rc7.1.exe`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.5-rc7.1/Dinero-Setup-2.2.5-rc7.1.exe)** | [`v2.2.5-rc7.1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5-rc7.1) | One-click NSIS installer (24.45 MB, LZMA-solid). Installs to `C:\Program Files\Dinero\`, Start Menu shortcut, Add/Remove entry. Wallet datadir at `%APPDATA%\Dinero` is never touched by uninstall. |
| **Windows desktop power users / portable** | ⬇ [`dinero-qt-2.2.5-rc7.1-windows-win64-preview.zip`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.5-rc7.1/dinero-qt-2.2.5-rc7.1-windows-win64-preview.zip) | [`v2.2.5-rc7.1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5-rc7.1) | Portable zip — extract and double-click `dinero-qt.exe`. Same binaries as the installer, useful for testers and sandboxed environments. |
| **Windows server operators** | ⬇ [`dinero-core-2.2.5-rc7.1-windows-win64-preview.zip`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.5-rc7.1/dinero-core-2.2.5-rc7.1-windows-win64-preview.zip) | [`v2.2.5-rc7.1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5-rc7.1) | Daemon + CLI + service install/uninstall scripts. No Core installer by design (operators want manual control). |
| **Fast-sync / node bootstrap** | `utxo-snapshot-13000.dat` + manifest | [`v2.2.5-rc3`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5-rc3) | Operator snapshot asset |

## What The Names Mean

| Prefix | Meaning |
|---|---|
| `Dinero-...macOS...qt.zip` | macOS desktop wallet app |
| `dinero-core_...amd64.deb` | Linux full-node package for operators |
| `dinero-qt-...windows-win64...zip` | Windows desktop wallet bundle (mirrors macOS `Dinero-Qt.app`) |
| `dinero-core-...windows-win64...zip` | Windows operator-only daemon/CLI bundle (no GUI) |
| `dinero-miner-...win64...zip` | Miner-only preview, not the wallet |
| `utxo-snapshot-...` | AssumeUTXO / Utreexo bootstrap data, not an app |
| `SHA256SUMS.asc` | GPG signature for release checksums |
| `dinero-core-release.asc` | Public GPG release signing key |

## Release Channels

| Channel | Meaning |
|---|---|
| Stable release | Best choice for ordinary users |
| RC / pre-release | Operator or preview build; read the notes before using |
| Snapshot asset | Bootstrap data for Core; not an app |

## Download

👉 **[Get the latest release](https://github.com/DineroLabs/dinero-releases/releases/latest)** — signed and Apple-notarized.

| Platform | Bundle | What's inside |
|---|---|---|
| **macOS (Apple Silicon, arm64)** | `Dinero-v2.2.5-macOS-arm64-qt.zip` | Signed and notarized `Dinero-Qt.app` with all helpers embedded |
| **Linux (Ubuntu 24.04+, amd64)** | `dinero-core_<VERSION>_amd64.deb` | Packaged-service full node — `dinerod`, `dinero-cli`, `dinero-backup`, systemd unit, signed `SHA256SUMS.asc` |
| **Windows desktop installer (64-bit, preview)** | `Dinero-Setup-2.2.5-rc7.1.exe` | Recommended Windows desktop download. NSIS installer, ~24 MB. Per-machine install to `C:\Program Files\Dinero\`, Start Menu shortcut, Add/Remove Programs entry. Uninstall preserves your wallet at `%APPDATA%\Dinero`. See the [`v2.2.5-rc7.1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5-rc7.1) page. |
| **Windows desktop portable (64-bit, preview)** | `dinero-qt-2.2.5-rc7.1-windows-win64-preview.zip` | Same binaries as the installer, packaged as a portable zip. Extracts to `Dinero-Qt/`; double-click `dinero-qt.exe`. Useful for testers and sandboxed environments. |
| **Windows server operator (64-bit, preview)** | `dinero-core-2.2.5-rc7.1-windows-win64-preview.zip` | Daemon + CLI only, no GUI — `dinerod.exe`, `dinero-cli.exe`, runtime DLLs, plus `install-service.ps1` / `uninstall-service.ps1` for Windows service registration. Canonical Windows-aware datadir. Extracts to `Dinero-Core/`. ~16 MB. See the [`v2.2.5-rc7.1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5-rc7.1) page. |

## Verify what you downloaded

The Qt app is **notarized by Apple** and the ticket is stapled to the bundle. Confirm before launching:

```bash
spctl -a -t exec -vv Dinero-Qt.app
# Expected:
#   accepted
#   source=Notarized Developer ID
#   origin=Developer ID Application: Mirsad Hajdarevic (JXJS6ZA5FJ)
```

The CLI tarball ships a `SHA256SUMS.txt` file. Verify each binary:

```bash
shasum -a 256 -c SHA256SUMS.txt
```

## Quick start

### Run the wallet
1. Download the `-qt.zip`, extract, double-click `Dinero-Qt.app`.
2. Click **Create Wallet**, save your seed somewhere durable.
3. Go to the **Receive** tab → pick **Taproot** or **Quantum-Safe (P2MR)** → click **New Address**.

### Mine to your wallet
In `Dinero-Qt.app` → **Mining** tab:
- Pick **"Pool Mining (SV2 / Job Declaration)"** mode
- Select **CPU** or **GPU (Metal)** backend (GPU runs ~500 MH/s on M4 Max)
- Click **Use Wallet** to fill the payout address
- Click **Start Mining**

Block rewards land in your wallet directly via consensus — the pool can't redirect them. Coinbase outputs require **100 confirmations** before they become spendable (standard Bitcoin-family rule).

### Run a node (Linux, packaged-service)

The `.deb` ships a full FHS-compliant install — dedicated `dinero`
system user, datadir at `/var/lib/dinero/`, systemd unit, journald
rotation. After install the daemon **auto-discovers the network**
via the four hardcoded DNS seeds (`seed1-4.dinero-coin.com`); no
manual peer config is needed.

```bash
# 1. Prerequisites for the verifier
sudo apt-get update && sudo apt-get install -y binutils python3 wget gpg

# 2. Pull the four release artifacts. Replace TAG + DEB with the
#    values from the latest release page. Today (May 2026):
#      TAG=v2.2.5-rc3 ; DEB=dinero-core_2.2.5-3_amd64.deb
TAG=v2.2.5-rc3
DEB=dinero-core_2.2.5-3_amd64.deb
BASE=https://github.com/DineroLabs/dinero-releases/releases/download/$TAG
for f in $DEB dinero-core-release.asc SHA256SUMS SHA256SUMS.asc; do
    wget "$BASE/$f"
done

# 3. Verify signature against the published fingerprint
gpg --import dinero-core-release.asc
gpg --fingerprint "Dinero Core Release Signing"
# MUST match: 4ED3 65CE 6604 B722 D281  EC77 3A61 4979 B8A4 8C02

gpg --verify SHA256SUMS.asc SHA256SUMS
sha256sum -c SHA256SUMS

# 4. Install + start
sudo dpkg -i $DEB
sudo systemctl enable --now dinero

# 5. Confirm health and connectivity
sudo -u dinero dinero-cli -datadir=/var/lib/dinero health
sudo -u dinero dinero-cli -datadir=/var/lib/dinero getconnectioncount
```

Within ~60 seconds, `getconnectioncount` should return `≥3` and
`health` should print `OK`. If `getconnectioncount` returns `0`, your
firewall is blocking outbound to port `20999`.

For shell access from your normal user account:
```bash
sudo usermod -a -G dinero $USER   # one-time, log out and back in
dinero-cli health
```

### Run a node (macOS, bundled daemon)
```bash
# Use the bundled daemon, with your own datadir:
./Dinero-Qt.app/Contents/MacOS/dinerod --datadir ~/.dinero
```

## What is Dinero?

A cryptocurrency designed for the post-quantum era and for users who don't trust pools, third-party custodians, or premined allocations.

| | |
|---|---|
| **Post-quantum coinbase** | Both Taproot (`din1p…`) and **P2MR** (`din1r…`, hash-based, quantum-safe) addresses are first-class consensus targets. |
| **Utreexo from genesis** | Full UTXO state lives in a 1-KB accumulator — light nodes verify the chain end-to-end without trust. |
| **Mobile mining** | Stratum V2 + Job Declaration over Noise NX. Phones own their coinbase outputs. |
| **No premine** | Every coin in circulation was mined under the same rules every miner faces today. |
| **Fair-launched** | Genesis 2026-04-17 00:00:00 UTC. nBits `0x1d31ffce`. Hash `0000001c…b76f`. |

## Network endpoints

- **Mainnet RPC**: `20998`
- **Mainnet P2P**: `20999`
- **Reference SV2 pool**: `172.93.160.131:4444`
  - Pinned Noise pubkey: `17fc0efc6f937f3dd070b9d79da8b387f05a68598ecfd646db65735be5477f5f`
- **Reference V1 stratum**: `127.0.0.1:3333` (run your own; the GUI can launch it for you)

## Project structure

| Repo | Role |
|---|---|
| **[dinero-releases](https://github.com/DineroLabs/dinero-releases)** | You are here — signed binaries |
| [dinero-qt](https://github.com/DineroLabs/dinero-qt) | Cross-platform desktop wallet |
| [DineroDPI](https://github.com/DineroLabs/DineroDPI) | iOS wallet + phone-resident miner |
| [dinero-sv2](https://github.com/DineroLabs/dinero-sv2) | Stratum V2 protocol + CPU/GPU pool miners |
| [dinero-stratum](https://github.com/DineroLabs/dinero-stratum) | Legacy Stratum V1 server (ASIC compatibility) |
| [dinero-explorer](https://github.com/DineroLabs/dinero-explorer) | Web block explorer |

> **Daemon source (`dinerod`) and bridge server are kept private** while the chain is stabilizing. Both are reproducibly signed in the binary releases above; treat the signatures as the trust anchor.

## Release history

Full, detailed per-release notes are preserved in [RELEASES.md](RELEASES.md), and every tag has its own page under [Releases](https://github.com/DineroLabs/dinero-releases/releases).

## Issues, security, contributing

- **Bug reports / feature requests**: [open an issue](https://github.com/DineroLabs/dinero-releases/issues) on the relevant repo.
- **Security disclosures**: see [SECURITY.md](SECURITY.md). Please do not file public issues for vulnerabilities.
- **Pull requests**: see [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. See [LICENSE](LICENSE).

---

<div align="center">
<sub>Dinero · Real Money For Free People · <a href="https://dinero-coin.com">dinero-coin.com</a> · <a href="https://github.com/DineroLabs">github.com/DineroLabs</a></sub>
</div>
