# Security Policy

## Reporting a vulnerability

**Please do not open public GitHub issues for security vulnerabilities.**

Email: **security@dinero-coin.com**

Include:
- A clear description of the issue
- Steps to reproduce or proof of concept
- The affected component (`dinerod`, `dinero-qt`, `dinero-sv2-miner`, etc.) and version
- Your contact info if you'd like credit in the fix announcement

You'll get an acknowledgement within 72 hours. We will keep you informed as the fix progresses, and (with your consent) credit you in the release notes that ship the patch.

## Scope

In scope:
- Consensus bugs in `dinerod`
- Wallet vulnerabilities (`dinero-qt`, `DineroDPI`, `dinero-cli`)
- Mining-protocol bugs in `dinero-sv2-*` and `dinero-stratum`
- Bridge contract / server (`wdin-bridge`)
- Cryptographic implementation flaws (P2MR, Taproot, Utreexo)

Out of scope:
- Denial-of-service against unspecified third-party reference pool (LA)
- Reports requiring physical access to the user's machine
- Vulnerabilities in dependencies that are already publicly disclosed and have an upstream fix pending

## Coordinated disclosure

We aim to have a fix shipped within 30 days of confirming a vulnerability. Critical bugs that put live funds at risk get same-week treatment with a coordinated disclosure window.

We do not currently run a paid bounty. Credits and acknowledgements in release notes are the recognition we offer.

## Verifying releases

All Qt app bundles are signed with **Developer ID Application: Mirsad Hajdarevic (JXJS6ZA5FJ)** and notarized by Apple. Verify offline:

```bash
spctl -a -t exec -vv Dinero-Qt.app
# Expected:
#   accepted, source=Notarized Developer ID
```

CLI binaries ship with `SHA256SUMS.txt`:

```bash
shasum -a 256 -c SHA256SUMS.txt
```

If a verification step fails, **do not run the binary** — open a security report immediately.
