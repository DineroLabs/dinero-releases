# Contributing to Dinero

Thanks for considering a contribution. This repo (`dinero-releases`) is the binary distribution channel; **most code lives in sibling repos**, see [README.md](README.md) for the project layout.

## Where to send what

| Topic | Repo |
|---|---|
| Daemon / consensus / chain | [Dinero-Coin](https://github.com/DineroLabs/Dinero-Coin) |
| Desktop wallet (Qt) | [dinero-qt](https://github.com/DineroLabs/dinero-qt) |
| iOS wallet + phone miner | [DineroDPI](https://github.com/DineroLabs/DineroDPI) |
| Stratum V2 protocol / miners | [dinero-sv2](https://github.com/DineroLabs/dinero-sv2) |
| Block explorer (web) | [dinero-explorer](https://github.com/DineroLabs/dinero-explorer) |
| Build / release issues | this repo |

For **security vulnerabilities**, do not open a public PR or issue — see [SECURITY.md](SECURITY.md).

## Pull requests

1. Fork the relevant repo.
2. Create a topic branch off `main` (or the active development branch — usually `p2p-fix` for `Dinero-Coin`).
3. Make focused commits with descriptive messages. We prefer the [Conventional Commits](https://www.conventionalcommits.org/) shape (`fix:`, `feat:`, `chore:`).
4. Open the PR against the same branch you forked from.
5. Each PR should compile cleanly and pass any tests in the repo's CI.

## Code style

- **C++** (`Dinero-Coin`, `dinero-qt`): follow the existing conventions in surrounding code. Keep functions small. New consensus rules require a regression test.
- **Rust** (`dinero-sv2`, `dinero-rust`): `cargo fmt` + `cargo clippy -- -D warnings` clean.
- **Swift** (`DineroDPI`): match the existing style; SwiftLint not yet enforced but PRs that introduce warnings will be asked to fix them.

## Discussion

For non-trivial design changes (new consensus rules, new RPCs, protocol-level work), please open a discussion or issue first to align on direction before sinking time into a PR.

## License

By submitting a contribution, you agree it will be released under the project's MIT license. You retain copyright; we just ask that the code can be redistributed under the same terms.
