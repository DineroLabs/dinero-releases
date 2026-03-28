# Dinero: A Two-Lane Monetary System

**Version 0.1 — March 2026**

**Armin Hajdarevic**
**arminhaj@stanford.edu**

---

## Abstract

Dinero is a two-lane monetary system that unifies transparent and private value transfer within a single cryptocurrency. Existing systems typically force a tradeoff: transparent ledgers provide public auditability but weak financial privacy, while privacy-first ledgers provide stronger confidentiality but make some exchange, compliance, and public-accounting workflows more difficult. Dinero is designed to combine both models on one chain, under one monetary policy, and within one native asset.

The protocol contains two explicit transaction domains. The transparent lane provides a Bitcoin-style UTXO environment with visible balances, auditable transfers, and standard address-based settlement. The private lane provides receiver privacy through stealth addressing, amount privacy through confidential transactions, and sender ambiguity through mandatory ring signatures. Funds move between these domains through explicit shielding and unshielding operations. Once funds enter the private lane, the applicable privacy rules are mandatory rather than optional, creating a coherent privacy pool instead of a fragmented set of partially private transactions.

Dinero's design is motivated by a practical observation: different users require different monetary properties at different times. Businesses, exchanges, miners, and counterparties may require transparent settlement and auditable balances. Individuals and treasury operators may require stronger privacy, improved fungibility, and protection from permanent public transaction history. Dinero aims to support both needs within a single system while preserving clear rules, explicit transitions between domains, and consensus-level enforcement of private transaction validity.

This paper presents the rationale, architecture, transaction model, privacy system, activation framework, and security assumptions of Dinero's two-lane design.

---

## 1. Introduction

Blockchains have historically split into two broad monetary models.

The first model prioritizes transparency. In transparent UTXO systems, transaction history is publicly visible, balances can often be inferred, and asset movement is permanently linkable on-chain. This model supports public auditability, operational simplicity, and straightforward integration for exchanges, miners, businesses, and infrastructure providers. Its weakness is that financial history becomes durable and globally observable, reducing privacy and weakening fungibility over time.

The second model prioritizes privacy. In privacy-focused systems, transaction metadata is concealed so that observers cannot easily determine the sender, receiver, or amount. This model better approximates the behavior of physical cash, improves confidentiality, and reduces the ability of third parties to build complete financial surveillance graphs. Its weakness is that always-private systems can make some public-accounting, compliance, and exchange workflows more difficult.

Dinero is built on the premise that these two monetary behaviors do not need to exist on separate networks or require separate assets. Instead, they can coexist within one protocol, provided that the boundary between them is explicit and each domain has clear, enforceable rules.

In Dinero, the transparent lane preserves the operational strengths of a Bitcoin-style UTXO chain. The private lane adds three consensus-level protections: stealth addresses for receiver privacy, confidential transactions for hidden amounts, and ring signatures for sender ambiguity. The design intent is not to bolt a few privacy features onto an otherwise transparent chain, but to create a distinct private monetary domain with its own validity rules and explicit entry and exit points.

This distinction matters because optional or partial privacy tends to produce weak anonymity sets. When only a small subset of users opts into privacy, those users often become easier to isolate rather than harder to analyze. Dinero therefore treats privacy inside the private lane as a protocol property, not a cosmetic user option. Shielding moves funds into the private domain; unshielding moves them out. Within that domain, the privacy rules are mandatory.

The resulting system is intended to serve real-world monetary needs more flexibly than either extreme alone. A miner may prefer transparent rewards for accounting purposes while later using private funds for everyday spending. A business may use public receiving addresses for invoices while managing treasury flows privately. An individual may need auditable records for tax reporting while also wanting protection from lifelong public exposure of personal spending behavior.

Dinero's objective is not merely to add privacy to a UTXO chain, but to create a monetary system in which transparency and privacy are both native, both intentional, and both governed by explicit consensus rules.

---

## 2. Design Goals

Dinero's architecture is guided by the following design goals.

### 2.1 Unify transparent and private settlement in one asset

Dinero is designed to avoid splitting monetary use cases across separate coins or separate chains. Users should be able to operate publicly or privately within the same monetary system, under one supply policy and one unit of account.

### 2.2 Preserve a clear transparent lane

The transparent lane should remain useful for public settlement, auditable balances, exchange integration, mining rewards, operational accounting, and any workflow that benefits from visible on-chain state. Transparent behavior must remain simple, predictable, and legible.

### 2.3 Provide a coherent private lane

The private lane should not be a patchwork of optional privacy features. It should provide a full privacy stack composed of:

- Receiver privacy via stealth addresses
- Amount privacy via confidential transactions
- Sender ambiguity via ring signatures
- Double-spend prevention via key images

### 2.4 Make transitions explicit

Movement between public and private domains must be intentional. Shielding and unshielding should be first-class protocol actions, not ambiguous wallet behavior. This keeps the system understandable for users and easier to reason about for auditors, implementers, and wallet software.

### 2.5 Improve fungibility in the private domain

A core purpose of the private lane is to reduce history-based discrimination between units. By obscuring sender, receiver, and amount information, the private domain should behave more like cash and less like a permanently tagged asset graph.

### 2.6 Enforce privacy rules at consensus level

Private transaction validity must be enforced by consensus rather than delegated to external services, mixers, or wallet-side conventions. This includes ring verification, key-image uniqueness, eligibility of private ring members, and commitment-balance validation.

### 2.7 Keep the protocol operationally practical

The system should remain practical for full nodes, wallets, miners, and broader infrastructure. Privacy must be engineered as part of the protocol, but operational resilience, activation discipline, recovery correctness, and maintainability remain essential.

### 2.8 Avoid ambiguous half-private behavior

Dinero should avoid states where users believe they are transacting privately but the protocol only hides limited information. Inside the private lane, privacy properties should be coherent and mandatory so that users are not misled by partial protection.

### 2.9 Support predictable activation and lifecycle correctness

Because the private lane introduces new transaction rules and state, activation must be explicit and carefully gated. Wallet behavior, mempool policy, consensus validation, reorg handling, restart recovery, and rescan correctness must all remain aligned before and after activation.

### 2.10 Provide a foundation for long-term monetary flexibility

Dinero is intended to support both public and private monetary behavior over time without forcing the ecosystem into a single ideological model. The protocol should give users, businesses, and infrastructure operators the ability to choose the monetary properties appropriate to each use case while remaining within one shared system.

---

## 3. System Overview

Dinero is a single-chain cryptocurrency with one native asset and two explicit transaction domains: a transparent lane and a private lane. Both lanes operate under the same monetary policy and settle on the same blockchain, but they differ in the visibility and validation rules that apply to their transactions.

The transparent lane follows a Bitcoin-style UTXO model. Outputs are addressed and spent in a publicly auditable way, balances can be derived from on-chain history, and transactions remain visible for accounting, exchange operations, mining rewards, and other use cases that benefit from transparency.

The private lane is a distinct monetary domain within the same chain. It is designed to provide three privacy properties simultaneously: receiver privacy, amount privacy, and sender ambiguity. Receiver privacy is provided through stealth addressing, which derives one-time destinations that cannot be linked back to a stable public receiving address. Amount privacy is provided through confidential transactions, which allow the network to verify balance correctness without revealing transferred values. Sender ambiguity is provided through ring signatures, which allow a valid spend to be proven without revealing which eligible prior output in the ring was actually spent.

Movement between the two lanes is explicit rather than implicit. Funds enter the private lane through shielding and leave it through unshielding. This boundary is an intentional part of the protocol design. It allows transparent and private monetary behavior to coexist without blurring the rules that govern each. Transparent funds remain transparent. Funds inside the private lane are subject to private-lane validity rules, including confidential balance checks, ring-based spend validation, and key-image uniqueness enforcement.

From a systems perspective, Dinero therefore operates as one chain with two transaction semantics. The transparent semantics prioritize visibility and auditability. The private semantics prioritize fungibility and confidentiality. The protocol does not treat these as competing assets or separate networks, but as two monetary modes within one consensus system.

### 3.1 Transparent lane

The transparent lane is intended for users and institutions that require public settlement and legible transaction history. It supports standard address-based payments, public transaction tracing, and straightforward infrastructure integration. For miners, exchanges, businesses, and regulated service providers, this lane provides the operational clarity expected from a conventional UTXO chain.

### 3.2 Private lane

The private lane is intended for cash-like privacy. It is not merely a wallet feature layered on top of transparent transactions. It is a protocol-defined domain with its own transaction rules, validation requirements, and state-tracking mechanisms.

### 3.3 Explicit lane transitions

Shielding and unshielding are first-class transitions between the public and private domains. Shielding consumes transparent value and creates private outputs. Unshielding consumes private outputs and reintroduces value into the transparent domain. This explicit crossing is important for both usability and protocol reasoning. It keeps the ledger understandable, makes wallet behavior clearer, and preserves a clean mental model for users: public funds remain public until intentionally moved into the private lane, and private funds remain private until intentionally brought back out.

### 3.4 Consensus role of the private lane

The private lane is enforced by consensus, not by external services or user convention. A private spend must satisfy all applicable private-lane rules to be valid. Those rules include valid ring proof construction, non-reuse of key images, eligibility of ring members, and correct confidential balance relations.

---

## 4. Transaction Types

Dinero supports a small set of transaction behaviors that together define the two-lane monetary model. These behaviors are best understood not as unrelated transaction categories, but as the allowed movements of value within and between the transparent and private domains.

### 4.1 Transparent transfer

A transparent transfer consumes transparent outputs and creates new transparent outputs. This is the standard transaction form in the transparent lane. Inputs reference specific previous outputs directly, values are visible, and the transaction is fully auditable on-chain.

Transparent transfers are suited for exchange deposits and withdrawals, mining-related accounting, merchant invoices, public treasury movements, and any setting where external verifiability is desirable.

### 4.2 Shielding transaction

A shielding transaction moves value from the transparent lane into the private lane. It consumes transparent inputs and creates private outputs. Because the consumed inputs are transparent, their origin is publicly visible; the privacy gain begins at the point value enters the private domain.

Shielding is an explicit boundary-crossing action. It represents the point at which funds stop participating in the transparent transaction graph and begin participating in the private-lane rules.

### 4.3 Private transfer

A private transfer consumes private outputs and creates new private outputs within the private lane. This is the core spend type of the private domain. It combines the three privacy protections of the system:

- Stealth-derived destinations for receiver privacy
- Confidential amounts for amount privacy
- Ring-based spending for sender ambiguity

A private transfer must also satisfy the anti-double-spend mechanism of the lane through valid key-image usage.

### 4.4 Unshielding transaction

An unshielding transaction moves value from the private lane back into the transparent lane. It consumes private inputs under private-lane rules and creates transparent outputs that re-enter the public transaction graph.

This action is useful when funds need to become publicly auditable again, for example when moving value to an exchange, a public business address, or a workflow that depends on visible accounting.

### 4.5 Why these four transaction types matter

These transaction types are enough to express Dinero's two-lane design cleanly:

- Transparent transfer keeps value in the public domain
- Shielding moves value into the private domain
- Private transfer keeps value inside the private domain
- Unshielding returns value to the public domain

This structure avoids ambiguous transaction behavior. It also avoids presenting privacy as a toggle on otherwise transparent transactions. Instead, the protocol defines a clear state machine for value movement between domains.

### 4.6 Validation distinction between transparent and private inputs

The most important distinction in Dinero's transaction model is not the outputs alone, but the nature of the inputs being spent.

Transparent inputs are validated using direct reference semantics: a specific prior output is identified and spent according to the normal transparent rules.

Private inputs are validated differently. They require proof that one member of an eligible ring is being spent without revealing which one, along with key-image uniqueness and confidential balance correctness.

### 4.7 Mandatory privacy inside the private lane

Once value is inside the private lane, privacy protections are mandatory for private spends. This is a central design choice. Optional privacy weakens the anonymity set and creates weaker, less coherent privacy behavior. Dinero therefore treats private-lane spending as a domain with required rules, not optional concealment features layered on otherwise public transactions.

### 4.8 Transaction versioning and activation

Because the private lane introduces distinct transaction structure and validation rules, private-capable transaction forms are activation-gated. This ensures that old and new transaction rules are not ambiguously mixed and that private-lane behavior enters consensus in a controlled manner.

---

## 5. Private Lane Architecture

Dinero's private lane is a protocol-defined transaction domain that combines receiver privacy, amount privacy, and sender ambiguity within a single spend model. Its purpose is not only to hide selected transaction fields, but to create a coherent monetary environment in which privately held units are less traceable, more fungible, and less dependent on visible transaction history.

### 5.1 Receiver privacy via stealth addressing

Private receipt in Dinero is based on stealth addressing. Rather than sending funds to a stable on-chain destination that can be linked back to a user over time, the sender derives a one-time destination for each payment. This prevents observers from correlating multiple incoming transactions to the same recipient identity.

In practice, this means that a recipient may publish a reusable public receiving identity, but each private payment produces a distinct output destination on-chain. Only the intended recipient, using the corresponding private key material and scanning logic, can detect that the output belongs to them. The private lane therefore avoids the transparent-lane model in which repeated receipt to the same address gradually reveals a complete payment history.

### 5.2 Amount privacy via confidential transactions

Private-lane transfers hide transaction amounts through confidential transactions. Instead of exposing numeric values directly on-chain, Dinero represents value in a commitment form that allows the network to verify conservation of value without revealing the amounts themselves.

The essential property is that nodes can confirm that no value was created or destroyed even though they cannot read the explicit transferred amount from the transaction. This allows private transfers to preserve monetary integrity while withholding sensitive financial information from public observers.

### 5.3 Sender ambiguity via ring signatures

Private spending in Dinero uses ring signatures to prevent a private input from identifying a unique spend origin on-chain. Instead of directly pointing to one prior output in the same way transparent inputs do, a private input references a ring of eligible outputs and proves that the spender controls one of them without revealing which one.

This is the core sender-privacy mechanism of the lane. It changes the spend model from direct input disclosure to ambiguity within a protocol-defined candidate set.

### 5.4 Key images and double-spend prevention

Sender ambiguity requires a separate mechanism to prevent undetectable double-spending. Dinero uses key images for this purpose. A key image is a one-way cryptographic representation tied to the true private spend, but it does not reveal which ring member was actually spent.

The protocol tracks key images as consensus-relevant state. If a second transaction attempts to spend using the same key image, it is rejected as a double-spend.

### 5.5 Ring member eligibility and decoy selection

Private sender ambiguity depends not only on signature correctness, but also on the quality and structure of the ring itself. Dinero's design requires that ring members be drawn from an eligible set of private outputs rather than arbitrarily from the transparent UTXO set. This prevents category mixing and preserves a meaningful anonymity set inside the private domain.

Ring-member selection is a central algorithmic requirement. Ring members must be compatible with the private lane, sampled from an appropriate candidate universe, and chosen in a way that does not trivially reveal the real spend.

### 5.6 Private-lane state

The private lane introduces persistent protocol state beyond ordinary transparent UTXO tracking. At minimum, this includes:

- Private outputs eligible for ring construction
- Key-image state for double-spend detection
- Wallet-side spend and recovery state for owned private outputs
- Confidential balance relations needed for private validation

This state must remain correct across mempool admission, block connection, disconnect, restart, and reorg. The private lane is not a cosmetic extension to a transparent chain, but an additional consensus domain with its own lifecycle requirements.

### 5.7 Why the private lane is mandatory once entered

Dinero's design does not treat privacy inside the private lane as optional. Once funds are moved into that domain, private spends must satisfy the lane's privacy rules rather than selectively disabling them. This design choice is important because a fragmented or optional privacy model weakens the anonymity set and makes privately acting users easier to isolate.

---

## 6. Consensus Validation Rules

Dinero's two-lane architecture requires distinct validation rules for transparent and private transaction behavior. Transparent transactions continue to follow ordinary direct-reference UTXO semantics. Private-lane transactions introduce additional consensus obligations related to ring validity, confidential balance, and key-image uniqueness.

### 6.1 Transparent input validation

Transparent inputs are validated by directly referencing a specific prior output and proving authorization to spend it under the transparent rules of the chain. The referenced output must exist, be unspent, satisfy maturity and script conditions where applicable, and not violate the consensus rules governing transparent value transfer.

### 6.2 Private input validation

Private inputs are not validated by revealing a single previous output as the true spend origin. Instead, each private input must demonstrate all of the following:

- The ring structure is well formed
- All referenced ring members are eligible private outputs
- The ring signature verifies under the canonical signing rules
- The associated key image has not appeared before
- The transaction satisfies confidential balance requirements

### 6.3 Ring-size enforcement

The protocol enforces a defined ring size for private spends. This prevents privacy degradation through undersized rings and avoids heterogeneous spend classes that could fragment the anonymity set. Ring-size enforcement is a consensus rule, not merely a wallet preference.

### 6.4 Ring-member existence and eligibility

A ring signature cannot be valid if it is constructed over nonexistent or ineligible members. Dinero therefore requires that every referenced ring member correspond to a real, protocol-eligible private output in the candidate set.

This prevents attackers from constructing meaningless rings, mixing incompatible output domains, or fabricating anonymity where no valid spend candidates exist.

### 6.5 Key-image uniqueness

Every private spend must contain a key image that is globally unique with respect to prior accepted private spends. If a key image already exists in consensus state or conflicts in the mempool, the transaction is invalid as a double-spend attempt.

This is the mechanism that preserves spend uniqueness without sacrificing sender ambiguity.

### 6.6 Confidential balance correctness

Private transactions must preserve value even though amounts are hidden. Dinero therefore requires confidential-balance verification as part of private transaction validation. The relation between committed inputs, committed outputs, and fees must hold exactly.

This rule serves the same monetary purpose as explicit-value conservation in transparent transactions, but does so without exposing the transferred amounts publicly.

### 6.7 Shielding and unshielding validation

Shielding and unshielding transactions cross the boundary between transparent and private domains, so they are subject to mixed validation conditions.

A shielding transaction must consume valid transparent inputs and create valid private outputs under the private-lane receive rules. An unshielding transaction must consume valid private inputs under ring and confidential rules, while producing valid transparent outputs in the public lane.

### 6.8 Activation-gated private validation

Private-lane transaction forms are introduced through explicit activation rules rather than silently coexisting with transparent-only semantics from genesis. This protects the chain from ambiguous rule interpretation and ensures that nodes apply private-lane rules only when the activation conditions are satisfied.

### 6.9 Mempool and block-level consistency

Consensus safety requires that the same underlying private-lane validity model be reflected in both mempool policy and block acceptance. A private transaction that is non-admissible due to key-image conflict, malformed ring structure, or failed confidential balance relation should not be treated as valid merely because it appears in a later block, except where the distinction is explicitly policy-level rather than consensus-level.

### 6.10 Disconnect and reorg correctness

Because the private lane introduces persistent state such as key images and private-output eligibility sets, disconnect and reorg handling are part of consensus-critical correctness. On block disconnect, any private-lane state introduced by the disconnected block must be reverted. On reconnect, it must be reapplied deterministically.

This requirement is especially important because private-lane correctness is not limited to transaction-by-transaction acceptance; it also depends on consistent state evolution across restarts, reorgs, and chain recovery.

---

## 7. Activation and Deployment Model

Dinero's private lane is introduced through an explicit activation process rather than by silently changing the meaning of existing transaction forms. This is necessary because private-lane spending introduces new transaction structure, new state requirements, and new consensus rules.

### 7.1 Activation principles

Dinero's activation model is based on four principles.

First, activation must be explicit. The network should have a clearly defined transition point at which ring-capable private spending becomes part of consensus.

Second, activation must be gated by readiness. The network should only permit private-lane spending when the private output pool is large enough to support meaningful ring construction.

Third, activation must be deterministic. Honest nodes running the updated rules must agree on when a transaction is valid or invalid, both before and after the transition.

Fourth, activation must be operationally safe. Wallets, mempools, miners, and full nodes must all enforce compatible behavior at the boundary so that private-lane activation does not create unexpected consensus splits or state corruption.

### 7.2 Activation conditions

Dinero activates ring-based private spending only when two conditions are met:

- The chain has reached the designated activation height
- The private output pool contains at least a minimum number of eligible confidential outputs

Height-based activation provides a deterministic network-wide rule boundary. Pool-depth gating ensures that private spends do not begin in a trivially small candidate universe.

### 7.3 Transaction versioning and rule gating

Private-lane transactions use transaction forms and validation semantics that are distinct from ordinary transparent transfers. Dinero treats ring-capable private spending as a version-gated transaction class.

Before activation, ring-capable private spends are invalid, wallets must not construct them, mempools must not admit them, and miners must not include them in block templates. After activation, ring-capable private spends become valid if all private-lane conditions are satisfied.

### 7.4 Deployment stages

A practical deployment sequence for the private lane consists of four stages.

**Stage 1: Dormant deployment.** Ring-capable software is released to nodes, wallets, and miners, but private spending remains consensus-disabled.

**Stage 2: Pool formation.** Users may shield value into the private domain and receive private outputs, allowing the eligible private pool to grow toward the minimum launch depth.

**Stage 3: Activation boundary.** At the designated height, updated nodes begin enforcing the activation rule set. Private spending remains gated until the private pool depth requirement is satisfied.

**Stage 4: Live private circulation.** Once both activation conditions are met, ring-based private spending becomes valid and mandatory for funds moving within the private lane.

---

## 8. Security Assumptions

Dinero's security model combines the assumptions of a proof-of-work UTXO chain with the additional assumptions introduced by confidential transactions, stealth addressing, and ring-based private spending. The private lane does not replace the need for ordinary blockchain security; it extends it.

### 8.1 Base chain security

Dinero assumes the ordinary security properties of a proof-of-work blockchain: honest nodes converge on the valid chain under the protocol's consensus rules, adversaries cannot cheaply rewrite deep history without controlling substantial mining power, and full nodes independently validate block and transaction correctness.

### 8.2 Soundness of confidential balance proofs

The private lane assumes the soundness of the confidential transaction system used to hide amounts while preserving value conservation. If the commitment system or associated proofs were unsound, an attacker could potentially create value invisibly inside the private lane.

### 8.3 Soundness of ring-signature verification

Dinero's sender-ambiguity model assumes that the ring-signature system is correctly implemented and that honest validators verify the exact canonical message required by consensus. If the signer and verifier disagree on the message being signed, valid spends may fail or invalid spends may be admitted.

### 8.4 Unforgeability and unlinkability

Dinero assumes that an attacker cannot forge a valid private spend without controlling the secret material corresponding to one true ring member, and cannot derive the actual spender from the ring proof alone beyond what is permitted by the statistical limits of the anonymity set.

### 8.5 Key-image correctness

The protocol assumes that the key-image mechanism is collision-resistant for honest use and that the network's key-image tracking remains correct across mempool admission, block connection, disconnect, restart, and reorg.

### 8.6 Ring-member eligibility and output indexing

The private lane assumes that the protocol's private-output index accurately reflects the eligible ring-member universe and that wallets select ring members only from valid private outputs. Sender ambiguity is only meaningful when rings are constructed from a real and coherent candidate set.

### 8.7 Stealth-address security

Receiver privacy assumes the correctness of Dinero's stealth-address derivation, one-time destination construction, and wallet scanning logic.

### 8.8 Privacy quality depends on usage, not only validity

Dinero distinguishes between valid private spending and strong private spending. A transaction may be fully valid under consensus while still providing weaker privacy than intended if the private pool is too small, ring-member selection is poor, user behavior leaks information across lanes, wallet defaults create recognizable patterns, or too few users participate in the private lane.

For this reason, Dinero's security model includes an economic and behavioral dimension in addition to formal validity. The protocol treats private-lane rules as mandatory once entered and gates activation on pool depth.

### 8.9 Activation-boundary security

The activation boundary itself is part of the security model. A private-lane system can be cryptographically correct yet still fail operationally if old nodes and new nodes disagree on validity, wallets build ring spends before consensus permits them, miners include not-yet-valid private transactions, reorgs across activation height mis-handle private state, or restart and rescan logic fails to reconstruct private spentness correctly.

### 8.10 Scope and limitations

Dinero's private lane is intended to provide strong on-chain financial privacy within its defined threat model. It does not eliminate all forms of information leakage. Network-layer observation, wallet fingerprinting, user behavior, exchange disclosure, timing analysis, and off-chain metadata may still reveal information outside the cryptographic envelope of the private transaction protocol.

The private lane should be understood as providing strong protocol-level confidentiality and improved fungibility, not absolute invisibility against every possible adversary.

---

## 9. Cryptographic Foundations

Dinero's two-lane system is built on a unified cryptographic foundation rooted in secp256k1, the same elliptic curve used by Bitcoin. This section describes the specific cryptographic constructions used by the protocol.

### 9.1 Elliptic curve: secp256k1

All Dinero cryptographic operations — key generation, signing, verification, Pedersen commitments, range proofs, stealth addressing, and ring signatures — are performed on the secp256k1 curve. This is a deliberate design choice: secp256k1 is the most widely deployed and heavily scrutinized elliptic curve in cryptocurrency, with mature, constant-time implementations available through libsecp256k1 and its zero-knowledge extension secp256k1-zkp.

### 9.2 Transparent lane: Taproot and Schnorr signatures

The transparent lane uses Taproot (BIP341/BIP342) as its primary spending mechanism. Taproot provides key-path spending via Schnorr signatures and script-path spending via Merklized Abstract Syntax Trees (MAST). All transparent addresses use the `din1` prefix (Bech32m encoding with a Dinero-specific human-readable part).

### 9.3 Confidential transactions: Pedersen commitments and range proofs

Amount privacy in the private lane is provided through Pedersen commitments on secp256k1. A Pedersen commitment takes the form `C = blind * G + amount * H`, where `G` is the standard secp256k1 generator, `H` is an alternative generator with no known discrete-log relationship to `G`, `blind` is a random blinding factor, and `amount` is the committed value.

The homomorphic property of Pedersen commitments allows the network to verify that `sum(input_commitments) = sum(output_commitments) + fee * H` without learning any individual amount. This commitment-balance check is performed using `secp256k1_pedersen_verify_tally` from the secp256k1-zkp library.

Each confidential output also carries a range proof demonstrating that the committed amount lies in the range [0, 2^64). This prevents an attacker from committing to a negative value, which would allow invisible inflation. Range proofs are generated using the secp256k1-zkp rangeproof module.

Private outputs use the `dina1` address prefix (Bech32m encoding), distinguishing them from transparent `din1` addresses at the address level.

### 9.4 Stealth addresses: ECDH-based one-time destinations

Receiver privacy uses a dual-key stealth address scheme. Each recipient has two key pairs: a scan key pair `(a, A = a*G)` and a spend key pair `(b, B = b*G)`. The scan key enables detection of incoming payments; the spend key enables spending received funds.

To send a private payment, the sender generates a random ephemeral key pair `(r, R = r*G)` and computes the shared secret `s = H(r * A)`. The one-time destination public key is `P = s*G + B`. Only the recipient can compute `s = H(a * R)` (using their private scan key) and derive the corresponding private key `p = s + b` to spend the output.

This ensures that each payment creates a unique on-chain destination that cannot be linked to the recipient's public stealth address without knowledge of the scan private key.

### 9.5 Ring signatures: CLSAG on secp256k1

Sender ambiguity is provided through CLSAG (Compact Linkable Spontaneous Anonymous Group signatures), the same ring signature scheme used by Monero since November 2020. Dinero implements CLSAG on secp256k1 rather than Monero's ed25519, but the algebraic construction is identical.

CLSAG signs over two keys per ring member: the output public key `P_i` and the Pedersen commitment `C_i`. This allows the signature to simultaneously prove that the signer controls one output AND that the commitment difference between the real input and the pseudo-output commitment is a commitment to zero (proving the amounts balance without revealing them).

**Ring size:** Dinero uses a mandatory fixed ring size of 16. All private-lane input spends must reference exactly 16 ring members (1 real + 15 decoys). The ring size is not configurable — heterogeneous ring sizes would fragment the anonymity set.

**Key image:** Each private spend produces a key image `KI = x * H_p(P)`, where `x` is the private key, `P` is the output public key, and `H_p` is a hash-to-point function (try-and-increment on secp256k1). The key image is deterministic: the same output always produces the same key image, enabling double-spend detection without revealing which ring member was spent.

**Signing message:** The CLSAG signature is computed over the transaction's non-witness serialization (txid). Ring data — including the ring member indices, key image, CLSAG signature, and pseudo-output commitment — is excluded from the txid and included only in the witness serialization (wtxid). This follows the same separation as Bitcoin's SegWit: the signature cannot be part of the data it signs.

### 9.6 Commitment prefix handling

A subtle but consensus-critical detail: secp256k1-zkp serializes Pedersen commitments with prefix bytes `0x08`/`0x09`, using quadratic residuosity (is_square) to determine the prefix, rather than the `0x02`/`0x03` even/odd parity convention used for standard public keys. These two conventions do not correspond — approximately 50% of curve points have different classifications under the two schemes. Dinero uses `secp256k1_pedersen_commitment_to_pubkey` to correctly convert between the two representations when ring signature operations require point arithmetic on commitments.

### 9.7 Hash-to-point

The hash-to-point function `H_p` used in key image computation maps arbitrary data to a secp256k1 curve point with no known discrete-log relationship to the generator `G`. Dinero uses the try-and-increment method: `SHA256("CLSAG_hash_to_point" || data || counter)` is computed for incrementing counter values until a valid x-coordinate is found. This is deterministic and produces a point suitable for key image generation.

---

## 10. Node Architecture and Scalability

### 10.1 Utreexo accumulator

Dinero uses Utreexo, a hash-based dynamic accumulator for the UTXO set, to reduce the storage requirements for full validation nodes. In a traditional UTXO-based blockchain, every full node must store the entire unspent transaction output set in order to validate new transactions. As the chain grows, this set becomes a significant storage burden.

With Utreexo, nodes store only a compact cryptographic accumulator (a set of Merkle tree roots) rather than the full UTXO set. Transactions include inclusion proofs demonstrating that their inputs exist in the accumulator. This allows nodes to validate transactions and update the accumulator state without maintaining the complete UTXO database.

Utreexo bridge nodes maintain the full UTXO set and generate proofs for compact nodes, enabling a heterogeneous network where different nodes make different storage/bandwidth tradeoffs.

### 10.2 CT output index

The private lane requires a separate index of all confidential outputs for decoy selection during ring construction. This index is maintained alongside the Utreexo accumulator and provides sequential random access to all private-lane outputs. It supports gamma-distribution-based sampling for decoy selection, ensuring that decoy ages approximate real spending patterns.

### 10.3 Key image database

A persistent key image database tracks all spent key images across the chain's history. This database is updated on block connection (add key images) and block disconnection (remove key images), maintaining consistency across reorgs and restarts. It uses a simple key-value store (RocksDB) with the key image bytes as the key and the block height and transaction ID as the value.

---

## 11. Monetary Policy

Dinero uses a disinflationary proof-of-work emission schedule with a perpetual tail emission floor.

### 11.1 Supply schedule

- **Genesis block (height 0):** 100 DIN burned via OP_RETURN (unspendable, symbolic).
- **PoW emission (height 1+):** 100 DIN initial block reward, halving every 1,314,000 blocks (approximately 5 years at 2-minute block intervals).
- **Tail emission:** When the halving subsidy falls below 1 DIN, the block reward is floored at 1 DIN per block permanently.

### 11.2 Denomination

The smallest unit of Dinero is the **una**. 1 DIN = 100,000,000 una (8 decimal places, matching Bitcoin's satoshi convention).

### 11.3 No premine, no ICO

Dinero was launched through a fair launch with no premine, no initial coin offering, and no pre-allocated supply. All coins in circulation were created through proof-of-work mining.

### 11.4 Supply curve

The halving schedule produces approximately 260.75 million DIN through the first 7 epochs (~35 years). After that, tail emission adds approximately 1.314 million DIN per 5-year epoch. The supply is not hard-capped but inflation rate approaches zero asymptotically.

---

## 12. Address System

Dinero uses distinct address prefixes for transparent and private outputs, providing unambiguous identification of the intended lane at the address level.

| Address type | Prefix | Encoding | Lane | Contents |
|---|---|---|---|---|
| Taproot (transparent) | `din1` | Bech32m | Transparent | 32-byte Taproot output key |
| Confidential | `dina1` | Bech32m | Private | Taproot key + view public key |
| Stealth | `dsa1` | Bech32m | Private | ECDH stealth payload for one-time addresses |
| Confidential (self-contained) | `dinc1` | Bech32m | Private | Spend key + view key in payload |

The private lane uses multiple address formats for different roles:

**`dina1` (Dinero Anon)** is the confidential address format. It encodes a Taproot output key and a view public key. Funds sent to a `dina1` address produce confidential outputs with hidden amounts. The view key enables the recipient to detect and decrypt incoming payments.

**`dsa1` (Dinero Stealth Address)** is the stealth address format used for unlinkable private receiving. When a sender pays a `dsa1` address, the protocol performs an ECDH key exchange to derive a fresh one-time destination for each payment. No two payments to the same stealth address produce the same on-chain output. This is the mechanism that provides receiver privacy: the `dsa1` address is reusable, but the resulting on-chain outputs are unlinkable. The `sendprivate` RPC uses stealth addresses for full sender-receiver unlinkability.

**`dinc1` (self-contained confidential)** is a compact format that carries both spend and view keys in the address payload, suitable for direct confidential transactions.

This addressing convention ensures that wallets, exchanges, and users can immediately distinguish between transparent destinations (`din1`), confidential destinations (`dina1`/`dinc1`), and fully private stealth destinations (`dsa1`) without inspecting transaction internals.

---

## 13. Conclusion

Dinero is a two-lane monetary system that provides both transparent and private value transfer within one chain, one asset, and one monetary policy.

The transparent lane preserves the strengths of a Bitcoin-style UTXO system: public auditability, operational clarity, and straightforward integration for exchanges, businesses, and miners.

The private lane provides cash-like privacy through stealth addressing, confidential transactions, and mandatory ring signatures. Privacy inside this lane is enforced by consensus, not delegated to external tools or optional wallet behavior.

The boundary between lanes is explicit. Shielding moves funds in. Unshielding moves funds out. Within the private lane, the rules are mandatory.

The central thesis is straightforward: transparent and private money do not need to exist on separate chains. They can coexist within one system, provided the boundary between them is explicit, the rules of each lane are clear, and the protocol enforces those rules consistently.

Dinero gives users both transparent and private monetary behavior on one chain, with clear rules for each.

---

*This document describes the Dinero protocol as designed and implemented. Specific parameters (ring size, activation height, pool depth thresholds) may be adjusted through the project's governance process before mainnet activation of the private lane.*

---

## Acknowledgments

Dinero does not exist in a vacuum. It builds directly on foundational work by researchers, cryptographers, and engineers whose contributions made this design possible. We acknowledge them here not as a formality, but because intellectual honesty requires it: every major component of Dinero traces back to ideas that others created first.

**Bitcoin and the UTXO model.** The transparent lane, proof-of-work consensus, the UTXO transaction model, script-based spending conditions, and the fundamental architecture of a decentralized electronic cash system originate with Satoshi Nakamoto's 2008 paper *"Bitcoin: A Peer-to-Peer Electronic Cash System."*

**Taproot, Schnorr signatures, and SegWit.** Dinero's transparent lane uses Taproot (BIP341/BIP342), designed by Pieter Wuille and Jonas Nick, building on Schnorr signature proposals by Pieter Wuille, Jonas Nick, and Tim Ruffing (BIP340). The Segregated Witness transaction structure (BIP141) was designed by Pieter Wuille, Eric Lombrozo, and Johnson Lau. Dinero's separation of ring data from the txid follows the same architectural principle as SegWit's separation of witness data.

**secp256k1 and libsecp256k1.** All Dinero cryptography operates on the secp256k1 elliptic curve. The high-performance, constant-time libsecp256k1 library was primarily authored by Pieter Wuille with contributions from Gregory Maxwell, Andrew Poelstra, and others in the Bitcoin Core project.

**Pedersen commitments and confidential transactions.** Amount privacy through Pedersen commitments was proposed for blockchain use by Gregory Maxwell in his 2015 *"Confidential Transactions"* design, building on the commitment scheme introduced by Torben Pedersen in 1991. The secp256k1-zkp library implementing Pedersen commitments and range proofs for Dinero was developed by the ElementsProject (Blockstream), primarily by Andrew Poelstra, Pieter Wuille, and Gregory Maxwell.

**Range proofs and Bulletproofs.** The range proof system ensuring that committed amounts are non-negative derives from Bulletproofs, introduced by Benedikt Bunz, Jonathan Bootle, Dan Boneh, Andrew Poelstra, Pieter Wuille, and Gregory Maxwell in *"Bulletproofs: Short Proofs for Confidential Transactions and More"* (2018). Dinero uses the secp256k1-zkp rangeproof implementation.

**CryptoNote and stealth addresses.** The stealth address construction used for receiver privacy in Dinero's private lane was introduced in the CryptoNote protocol, described by Nicolas van Saberhagen in *"CryptoNote v2.0"* (2013). The dual-key stealth address scheme (scan key + spend key) was refined and deployed by the Monero project.

**Ring signatures and linkable ring signatures.** The concept of ring signatures was introduced by Ronald Rivest, Adi Shamir, and Yael Tauman in *"How to Leak a Secret"* (2001). Linkable ring signatures, which enable double-spend detection without revealing the signer, were developed by Joseph Liu, Victor Wei, and Duncan Wong in *"Linkable Spontaneous Anonymous Group Signature for Ad Hoc Groups"* (LSAG, 2004).

**CLSAG.** The Compact Linkable Spontaneous Anonymous Group signature scheme used by Dinero was introduced by Brandon Goodell, Sarang Noether, and Arthur Blue in *"Concise Linkable Ring Signatures and Forgery Against Adversarial Keys"* (2019). CLSAG achieves the same security as MLSAG with approximately half the signature size and was adopted by the Monero project in November 2020.

**RingCT.** The integration of ring signatures with Pedersen commitments for simultaneous sender ambiguity and amount privacy was formalized by Shen Noether in *"Ring Confidential Transactions"* (2015), building on the Monero Research Lab's work.

**Monero.** The Monero project, initiated in 2014 as a fork of Bytecoin, has been the primary proving ground for ring signatures, stealth addresses, confidential transactions, and key-image-based double-spend prevention in a production cryptocurrency. Dinero's private lane design draws extensively on the lessons, research, and operational experience of the Monero community and the Monero Research Lab.

**Utreexo.** The Utreexo cryptographic accumulator used for UTXO set compression in Dinero was proposed by Thaddeus Dryja in *"Utreexo: A dynamic hash-based accumulator optimized for the Bitcoin UTXO set"* (2019). Dinero's production implementation of Utreexo as a consensus-critical component is documented separately in the Dinero Utreexo whitepaper.

**Bech32 and Bech32m address encoding.** The address encoding used for both `din1` and `dina1` addresses is based on Bech32 (BIP173) by Pieter Wuille and Greg Maxwell, and Bech32m (BIP350) by Pieter Wuille.

**BIP32 hierarchical deterministic wallets.** Dinero's HD wallet derivation follows BIP32, authored by Pieter Wuille.

**BIP39 mnemonic seed phrases.** Wallet backup and recovery uses the BIP39 mnemonic standard, authored by Marek Palatinus, Pavol Rusnak, Aaron Voisine, and Sean Bowe.

**Decoy selection research.** The gamma-distribution-based decoy selection strategy used for ring member sampling was developed through research by the Monero Research Lab, with foundational analysis by Malte Moser, Kyle Soska, Ethan Heilman, Kevin Lee, Henry Heffan, Shashvat Srivastava, Kyle Hogan, Jason Hennessey, Andrew Miller, Arvind Narayanan, and Nicolas Christin in empirical studies of Monero's traceability and anonymity set quality.

---

Dinero is an integration project. The cryptographic and protocol-design innovations acknowledged above were created by their respective authors. Dinero's contribution is the specific architectural combination: a two-lane monetary system that places these constructions into a unified protocol with explicit domain boundaries, mandatory privacy rules inside the private lane, and a shared transparent lane grounded in the Bitcoin UTXO model. We are grateful to the researchers and engineers whose work made this design possible.
