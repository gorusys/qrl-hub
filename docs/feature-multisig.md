# Multisignature

QRL **multisig** addresses hold funds that can only move when a **threshold** of weighted votes is reached across up to **100** participant addresses ([multisig overview](https://docs.theqrl.org/use/tools/multisig/overview)).

## Lifecycle

1. Each party creates a normal QRL address and secures its keys.
2. One party broadcasts a **multisig create** transaction listing all signatories, weights, and threshold.
3. Funds are sent to the **multisig address** (no single private key exists for that address).
4. A signatory initiates a **spend proposal** (recipient, amounts, **expiry block**).
5. Signatories **vote** approve/reject until threshold or expiry.
6. When approved, the protocol transfers funds as proposed.

## OTS consumption

**Every on-chain transaction** spends an OTS from the **signing address**. Multisig flows therefore consume OTS from:

- the **creator** (create tx),
- the **spend proposer**,
- **each voter** on the proposal,

as detailed in the official overview ([OTS key usage section](https://docs.theqrl.org/use/tools/multisig/overview)).

Plan OTS budgets carefully: multisig is powerful but **more OTS-hungry** than single-sig transfers.

## Weights and threshold

- Each signatory has a **weight**.
- **Threshold** is the cumulative **yes** weight required to execute a spend.
- Votes can be changed until the threshold is met with approvals or the **expiry block** passes ([overview](https://docs.theqrl.org/use/tools/multisig/overview)).

## Example patterns (from docs)

- **Joint account**: threshold 1 with multiple signatories — any member can propose and approve alone.
- **Board / majority**: threshold > 50% of total weight.
- **Escrow with unequal weights**: e.g. escrow weight 2, traders weight 1 each — requires escrow **plus** one trader ([overview](https://docs.theqrl.org/use/tools/multisig/overview)).

## Tooling links

- [Multisig overview](https://docs.theqrl.org/use/tools/multisig/overview)
- [Spend proposal](https://docs.theqrl.org/use/tools/multisig/spend-proposal)
- [Vote](https://docs.theqrl.org/use/tools/multisig/vote)
