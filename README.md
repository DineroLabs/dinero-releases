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

> **Windows users:** use `v2.2.6-rc1` or newer. v2.2.6-rc1 is the first
> release cut from the consolidated canonical branches and is rebuilt from
> source on top of v2.2.5-rc8. Carries the rc8 fixes plus Linux/aarch64
> daemon build fixes, miner stale-template / longpoll close, faster
> `template_refresh_ms`, wallet-switch state hygiene, and macOS datadir
> migration. Older Windows desktop downloads are intentionally
> de-emphasized.

| Platform / Need | Download | Release Page | Status |
|---|---|---|---|
| **macOS Apple Silicon** | ⬇ [`Dinero-v2.2.6-rc1-macOS-arm64-qt.zip`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/Dinero-v2.2.6-rc1-macOS-arm64-qt.zip) | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Signed/notarized Qt wallet release candidate with wallet-switch, datadir, fee-priority, embedded Core, and miner fixes |
| **Linux Ubuntu 24.04+ operators** | ⬇ [`dinero-core_2.2.6.rc1-1_amd64.deb`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/dinero-core_2.2.6.rc1-1_amd64.deb) | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Packaged-service Core `.deb` for Ubuntu 24.04+ amd64 operators |
| **Linux x86_64 desktop / portable** | ⬇ [`dinero-qt-2.2.6-rc1-linux-x86_64.tar.gz`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/dinero-qt-2.2.6-rc1-linux-x86_64.tar.gz) | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Portable Qt6 GUI wallet for Ubuntu 24.04-class x86_64 desktops. Extract and run `./dinero-qt`. |
| **Linux x86_64 server / portable** | ⬇ [`dinero-core-2.2.6-rc1-linux-x86_64.tar.gz`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/dinero-core-2.2.6-rc1-linux-x86_64.tar.gz) | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Portable daemon + CLI + stratum worker tarball from the Dell Ubuntu 24.04 x86_64 builder. |
| **Windows desktop users (recommended)** | ⬇ **[`Dinero-Setup-2.2.6-rc1.exe`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/Dinero-Setup-2.2.6-rc1.exe)** | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | One-click NSIS installer (24.37 MB, LZMA-solid). Installs to `C:\Program Files\Dinero\`, Start Menu shortcut, Add/Remove entry. Wallet datadir at `%APPDATA%\Dinero` is never touched by uninstall. |
| **Windows desktop power users / portable** | ⬇ [`dinero-qt-2.2.6-rc1-windows-win64-preview.zip`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/dinero-qt-2.2.6-rc1-windows-win64-preview.zip) | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Portable zip — extract and double-click `dinero-qt.exe`. Same binaries as the installer, useful for testers and sandboxed environments. |
| **Windows server operators** | ⬇ [`dinero-core-2.2.6-rc1-windows-win64-preview.zip`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/dinero-core-2.2.6-rc1-windows-win64-preview.zip) | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Daemon + CLI + service install/uninstall scripts. Windows datadir policy is `%APPDATA%\Dinero` only — operators on rc4–rc7.1 with `%USERPROFILE%\.dinero` data should migrate or pin via `-datadir` (see Core README). No Core installer by design (operators want manual control). |
| **Solo CPU + GPU mining (Windows, native MSVC)** | ⬇ [`dinero-solo-miner-2.2.6-rc2-windows-win64-msvc.zip`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc2/dinero-solo-miner-2.2.6-rc2-windows-win64-msvc.zip) | [`v2.2.6-rc2`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc2) | First native-MSVC solo miner build (replaces the rc1 MinGW-w64 cross-compile). Adds `--gpu` for CUDA mining (validated 739 MH/s on RTX 4060). GPU mode requires NVIDIA driver R535+ and CUDA Toolkit 12.x; CPU mode needs neither. |
| **Solo CPU mining (other platforms)** | `dinero-solo-miner-2.2.6-rc1-*` | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Wallet users should use the Mining tab. Standalone macOS arm64, Linux aarch64, and Linux x86_64 artifacts are live for power users and scripted solo miners. |
| **Solo GPU mining** | ⬇ [`dinero-gpu-miner-2.2.6-rc1-macOS-arm64.tar.gz`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/dinero-gpu-miner-2.2.6-rc1-macOS-arm64.tar.gz) / [`dinero-gpu-miner-2.2.6-rc1-windows-win64.zip`](https://github.com/DineroLabs/dinero-releases/releases/download/v2.2.6-rc1/dinero-gpu-miner-2.2.6-rc1-windows-win64.zip) | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Standalone `dinero-gpu-miner` from Core. macOS arm64 uses the Metal backend; Windows x64 uses the OpenCL backend (loads the system `OpenCL.dll` ICD; needs vendor GPU drivers — AMD/Intel/NVIDIA). It is intentionally separate from Linux operator Core packages so node operators do not get GPU/OpenCL dependencies by surprise. |
| **SV2 pool workers** | `dinero-sv2-miner-2.2.6-rc1-*` / `dinero-sv2-gpu-miner-2.2.6-rc1-*` | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | CPU and GPU SV2 pool workers are live for macOS arm64, Windows x64, Linux aarch64, and Linux x86_64. They connect to an SV2 pool and let the miner own the coinbase outputs. |
| **SV2 pool operators** | `dinero-sv2-pool-2.2.6-rc1-*` / `dinero-tp-2.2.6-rc1-*` | [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) | Reference SV2 pool server plus Template Provider. Operator software, not a wallet download. Live for macOS arm64, Windows x64, Linux aarch64, and Linux x86_64. |
| **Fast-sync / node bootstrap** | `utxo-snapshot-13000.dat` + manifest | [`v2.2.5-rc3`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.5-rc3) | Operator snapshot asset |

## What The Names Mean

| Prefix | Meaning |
|---|---|
| `Dinero-...macOS...qt.zip` | macOS desktop wallet app |
| `dinero-core_...amd64.deb` | Linux full-node package for operators |
| `dinero-qt-...windows-win64...zip` | Windows desktop wallet bundle (mirrors macOS `Dinero-Qt.app`) |
| `dinero-core-...windows-win64...zip` | Windows operator-only daemon/CLI bundle (no GUI) |
| `dinero-solo-miner-...` | Standalone solo CPU miner |
| `dinero-gpu-miner-...` | Standalone solo GPU miner (Metal/OpenCL/CUDA-shaped Core miner lane) |
| `dinero-sv2-miner-...` | Stratum V2 CPU pool worker |
| `dinero-sv2-gpu-miner-...` | Stratum V2 GPU pool worker (Metal on macOS, OpenCL on Linux/Windows) |
| `dinero-sv2-pool-...` | Stratum V2 reference pool server for operators |
| `dinero-tp-...` | Stratum V2 Template Provider; binds `dinerod` and emits templates to the pool |
| `dinero-miner-...win64...zip` | Legacy miner-only preview; prefer the clearer `dinero-solo-miner-...` / `dinero-gpu-miner-...` lanes going forward |
| `utxo-snapshot-...` | AssumeUTXO / Utreexo bootstrap data, not an app |
| `SHA256SUMS.asc` | GPG signature for release checksums |
| `dinero-core-release.asc` | Public GPG release signing key |

## Mining And Pool Software

Dinero has several miner-shaped binaries. They are deliberately separate so
desktop users, node operators, solo miners, and pool operators do not download
the wrong thing.

| Lane | Binary | Repo | Audience | Notes |
|---|---|---|---|---|
| **Wallet mining** | embedded `dinero-solo-miner` | [`dinero-qt`](https://github.com/DineroLabs/dinero-qt) + [`dinero-solo-miner`](https://github.com/DineroLabs/dinero-solo-miner) | Ordinary desktop users | Use the Mining tab in `Dinero-Qt.app` / `dinero-qt.exe`. |
| **Standalone solo CPU miner** | `dinero-solo-miner` | [`dinero-solo-miner`](https://github.com/DineroLabs/dinero-solo-miner) | Power users / scripted solo miners | Connects to your local `dinerod` RPC and mines directly to your wallet address. |
| **Standalone solo GPU miner** | `dinero-gpu-miner` | [`Dinero-Coin`](https://github.com/DineroLabs/Dinero-Coin) | GPU solo miners | Core-side standalone GPU miner. Separate release lane planned; not bundled into Linux `.deb` operator packages. |
| **SV2 CPU pool worker** | `dinero-sv2-miner` | [`dinero-sv2`](https://github.com/DineroLabs/dinero-sv2) | Pool miners | Connects to an SV2 pool over Noise/SV2 and submits shares. |
| **SV2 GPU pool worker** | `dinero-sv2-gpu-miner` | [`dinero-sv2`](https://github.com/DineroLabs/dinero-sv2) | GPU pool miners | Metal on Apple Silicon, OpenCL on Linux/Windows. This is the “SV2 pool worker” GPU lane. |
| **SV2 pool server** | `dinero-sv2-pool` | [`dinero-sv2`](https://github.com/DineroLabs/dinero-sv2) | Pool operators | Accepts SV2 workers, validates shares, submits found blocks. |
| **SV2 Template Provider** | `dinero-tp` | [`dinero-sv2`](https://github.com/DineroLabs/dinero-sv2) | Pool operators | Talks to `dinerod` and serves templates to the pool. |

Rule of thumb: if you want a wallet, download `Dinero-Qt`. If you want a node,
download `dinero-core`. If you want to mine outside the wallet, choose the
solo-miner or SV2-worker lane explicitly.

## Release Channels

| Channel | Meaning |
|---|---|
| Stable release | Best choice for ordinary users |
| RC / pre-release | Operator or preview build; read the notes before using |
| Snapshot asset | Bootstrap data for Core; not an app |

## Default Datadirs

Dinero uses the native convention for each install mode. These paths are
intentional; do not treat every leading-dot directory as a bug.

| Platform / mode | Default datadir | Why |
|---|---|---|
| **Windows desktop / Core zip** | `%APPDATA%\Dinero` | Windows application-data convention |
| **macOS desktop** | `~/Library/Application Support/Dinero` | macOS application-data convention |
| **Linux packaged-service** | `/var/lib/dinero` | FHS/systemd daemon convention under the `dinero` service user |
| **Linux manual-user / developer mode** | `~/.dinero` | Standard Linux CLI/daemon dot-directory convention |

## Download

👉 **[Get the latest release](https://github.com/DineroLabs/dinero-releases/releases/latest)** — signed and Apple-notarized.

| Platform | Bundle | What's inside |
|---|---|---|
| **macOS (Apple Silicon, arm64)** | `Dinero-v2.2.6-rc1-macOS-arm64-qt.zip` | Signed and notarized `Dinero-Qt.app` with wallet-switch, datadir, fee-priority, embedded Core, and miner fixes. See the [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) page. |
| **Linux (Ubuntu 24.04+, amd64)** | `dinero-core_2.2.6.rc1-1_amd64.deb` | Packaged-service full node — `dinerod`, `dinero-cli`, `dinero-backup`, systemd unit, verified by `SHA256SUMS-v2.2.6-rc1` |
| **Linux portable tarballs** | `dinero-*-2.2.6-rc1-linux-{aarch64,x86_64}.tar.gz` | Portable Core, Qt, solo miner, GPU miner, SV2 workers, SV2 pool, and Template Provider tarballs are live on [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1). |
| **Windows desktop installer (64-bit, preview)** | `Dinero-Setup-2.2.6-rc1.exe` | Recommended Windows desktop download. NSIS installer, ~24 MB. Per-machine install to `C:\Program Files\Dinero\`, Start Menu shortcut, Add/Remove Programs entry. Uninstall preserves your wallet at `%APPDATA%\Dinero`. See the [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) page. |
| **Windows desktop portable (64-bit, preview)** | `dinero-qt-2.2.6-rc1-windows-win64-preview.zip` | Same binaries as the installer, packaged as a portable zip. Extracts to `Dinero-Qt/`; double-click `dinero-qt.exe`. Useful for testers and sandboxed environments. |
| **Windows server operator (64-bit, preview)** | `dinero-core-2.2.6-rc1-windows-win64-preview.zip` | Daemon + CLI only, no GUI — `dinerod.exe`, `dinero-cli.exe`, runtime DLLs, plus `install-service.ps1` / `uninstall-service.ps1` for Windows service registration. Canonical Windows datadir at `%APPDATA%\Dinero`. Extracts to `Dinero-Core/`. ~16 MB. See the [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1) page. |
| **Mining / pool software** | `dinero-solo-miner-*`, `dinero-gpu-miner-*`, `dinero-sv2-miner-*`, `dinero-sv2-gpu-miner-*`, `dinero-sv2-pool-*`, `dinero-tp-*` | Standalone artifacts are live for macOS arm64, Windows x64, Linux aarch64, and Linux x86_64 on [`v2.2.6-rc1`](https://github.com/DineroLabs/dinero-releases/releases/tag/v2.2.6-rc1). GPU binaries require vendor GPU drivers/OpenCL or Metal support, depending on platform. |

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

# 2. Pull the release artifacts. Replace TAG + DEB with the
#    values from the latest release page. Today (May 2026):
#      TAG=v2.2.6-rc1 ; DEB=dinero-core_2.2.6.rc1-1_amd64.deb
TAG=v2.2.6-rc1
DEB=dinero-core_2.2.6.rc1-1_amd64.deb
BASE=https://github.com/DineroLabs/dinero-releases/releases/download/$TAG
for f in $DEB SHA256SUMS-v2.2.6-rc1; do
    wget "$BASE/$f"
done

# 3. Verify checksum
sha256sum -c <(grep "$DEB" SHA256SUMS-v2.2.6-rc1)

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
# Use the bundled daemon, with the macOS-conventional datadir:
./Dinero-Qt.app/Contents/MacOS/dinerod --datadir "$HOME/Library/Application Support/Dinero"
```

Older development builds used `~/.dinero`. Current release guidance uses
`~/Library/Application Support/Dinero` on macOS, `%APPDATA%\Dinero` on
Windows, `/var/lib/dinero` for Linux packaged-service nodes, and `~/.dinero`
for Linux manual-user/dev mode. Uninstallers and upgrades must preserve those
datadirs.

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
