# Dinero (DIN) — Tokenomics & Distribution

## Overview

Dinero is a fair-launch proof-of-work cryptocurrency. There was no premine, no ICO, no token sale, and no venture capital allocation. 100% of DIN is distributed through SHA256d mining.

## Key Parameters

| Parameter | Value |
|---|---|
| **Ticker** | DIN |
| **Smallest unit** | una (1 DIN = 100,000,000 una) |
| **Decimals** | 8 |
| **Consensus** | SHA256d Proof-of-Work |
| **Genesis block** | Height 0, unspendable (OP_RETURN) |
| **Initial block reward** | 100 DIN |
| **Halving interval** | 1,314,000 blocks |
| **Tail emission** | 1 DIN per block (perpetual floor) |
| **Maximum supply** | ~97,850,000 DIN (asymptotic) |
| **Launch date** | March 7, 2026 |

## Emission Schedule

All DIN enters circulation exclusively through block rewards paid to miners.

| Epoch | Block Heights | Reward per Block | Epoch Supply |
|---|---|---|---|
| 0 | 1 – 1,314,000 | 100 DIN | 131,400,000 DIN |
| 1 | 1,314,001 – 2,628,000 | 50 DIN | 65,700,000 DIN |
| 2 | 2,628,001 – 3,942,000 | 25 DIN | 32,850,000 DIN |
| 3 | 3,942,001 – 5,256,000 | 12.5 DIN | 16,425,000 DIN |
| 4 | 5,256,001 – 6,570,000 | 6.25 DIN | 8,212,500 DIN |
| 5 | 6,570,001 – 7,884,000 | 3.125 DIN | 4,106,250 DIN |
| 6 | 7,884,001 – 9,198,000 | 1.5625 DIN | 2,053,125 DIN |
| 7+ | 9,198,001+ | 1 DIN (tail) | 1,314,000 DIN per epoch |

After epoch 6, the block reward reaches the tail emission floor of 1 DIN per block, which continues indefinitely to permanently incentivize miners and secure the network.

## Subsidy Function

The block subsidy is a pure function of block height, defined in consensus code:

```
Height 0:       0 DIN (genesis, unspendable)
Height 1+:      max(INITIAL_SUBSIDY >> halvings, TAIL_EMISSION)

Where:
  INITIAL_SUBSIDY  = 100 DIN (10,000,000,000 una)
  HALVING_INTERVAL = 1,314,000 blocks
  TAIL_EMISSION    = 1 DIN (100,000,000 una)
  halvings         = (height - 1) / HALVING_INTERVAL
```

## Supply Curve

The total supply approaches ~97.85 million DIN from halving-era emissions, plus tail emission adds ~1,314,000 DIN per halving interval indefinitely. The tail emission ensures that:

1. Miners always have a block reward incentive beyond transaction fees
2. Lost coins are gradually replaced over time
3. The network maintains long-term security without relying solely on fee markets

## Distribution

- **Premine:** None (0%)
- **ICO/Token Sale:** None (0%)
- **Team/Foundation allocation:** None (0%)
- **Mining rewards:** 100% of all DIN ever created

## Wrapped DIN (wDIN)

wDIN is an ERC-20 representation of DIN on Base (Ethereum L2). It is backed 1:1 by DIN locked in the bridge. wDIN does not change the total supply — it is a wrapped representation, not additional issuance.

- **Contract:** [0xCD91b5C0aaD48E49F992BA690647C244f535C90B](https://basescan.org/token/0xCD91b5C0aaD48E49F992BA690647C244f535C90B)
- **Network:** Base (Ethereum L2)

## Source Code

The subsidy function is defined in [`include/consensus/stateless_verification.h`](https://github.com/Trucker2827/dinero-releases) and enforced by every full node. The emission schedule cannot be changed without a hard fork of the network.
