# Dinero Chain Reset Runbook (2026-02-24)

Use this runbook for Tower, DineroLA, DineroVA, and local development machines when moving to the premine/utreexo-v2 aligned chain.

## 1. Stop all node processes

```bash
pkill -f dinerod || true
```

## 2. Wipe all persisted chain state

```bash
rm -rf ~/.dinero/blockchain ~/.dinero/blocks ~/.dinero/headers ~/.dinero/utreexo \
       ~/.dinero/blockchaindb ~/.dinero/mempool.db ~/.dinero/peers.db ~/.dinero/blocks.db
```

If your node uses `-datadir`, run the same removal command against that directory instead of `~/.dinero`.

## 3. Start upgraded daemon

```bash
./dinerod --server --daemon
```

## 4. Verify chain anchors

```bash
./dinero-cli getblockhash 0
./dinero-cli getblockhash 1
```

Expected:

- `0000002ef4d930597e7978d7b3b6864fe212bef920dc95ddb9794184dbcfecad`
- `00000026ec31b18fca329fc77084cdfaa9c446a8097cc76061098a54c467bad4`

## 5. DineroDPI full wipe policy

DineroDPI upgrade path is configured for full reset (wallet + chain data) on first launch of the forced-reset build.
