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

## Download

👉 **[Get the latest release](https://github.com/DineroLabs/dinero-releases/releases/latest)** — signed and Apple-notarized.

| Platform | Bundle | What's inside |
|---|---|---|
| **macOS (Apple Silicon, arm64)** | `Dinero-vX.Y.Z-macOS-arm64-qt.zip` | Signed `Dinero-Qt.app` with all helpers embedded |
| **macOS (CLI, arm64)** | `Dinero-vX.Y.Z-macOS-arm64.tar.gz` | `dinerod`, `dinero-cli`, all miners (CPU/GPU, V1/SV2) |

> Linux and Windows builds are not part of this release line. Build from source — see [Dinero-Coin](https://github.com/DineroLabs/Dinero-Coin).

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

### Run a node
```bash
# Use the bundled daemon, with your own datadir:
./Dinero-v2.1.25-macOS-arm64/dinerod --datadir ~/.dinero
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
| [Dinero-Coin](https://github.com/DineroLabs/Dinero-Coin) | Full-node daemon source (`dinerod`) |
| [dinero-qt](https://github.com/DineroLabs/dinero-qt) | Cross-platform desktop wallet |
| [DineroDPI](https://github.com/DineroLabs/DineroDPI) | iOS wallet + phone-resident miner |
| [dinero-sv2](https://github.com/DineroLabs/dinero-sv2) | Stratum V2 protocol + CPU/GPU pool miners |
| [dinero-stratum](https://github.com/DineroLabs/dinero-stratum) | Legacy Stratum V1 server (ASIC compatibility) |
| [dinero-explorer](https://github.com/DineroLabs/dinero-explorer) | Web block explorer |
| [wdin-bridge](https://github.com/DineroLabs/wdin-bridge) | wDIN ↔ Base wrapped-token bridge |

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
