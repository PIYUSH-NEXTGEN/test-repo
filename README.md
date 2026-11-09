<div align="center">

# 🗳️ OpenSequencer

**Censorship-resistant, transparent transaction ordering for rollups.**

A modular fair-sequencing layer that replaces the trusted centralized sequencer
with threshold cryptography + verifiable ordering proofs.





</div>


## The Problem

Most L2 rollups run a **single centralized sequencer**. That one entity decides:

1. **Which transactions get included** — and which are silently dropped (censorship).
2. **In what order** — enabling front-running, sandwiching, and back-running (MEV).
3. **When batches are published** — creating opaque timing advantages.

This is not theoretical. A sequencer can:

- Censor a specific address or DApp without detection by outsiders.
- Insert its own arbitrage before user transactions.
- Withhold a batch to extract private order flow value.

Existing mitigations each solve part of the problem: encrypted mempools hide
transaction contents, priority auctions price access, and decentralized sequencer
networks rotate the role — but almost all still rely on *some* actor you must trust
to order honestly, and none give users a **cryptographic proof that ordering was fair**.

## What OpenSequencer Does

OpenSequencer is a drop-in sequencing layer that provides three properties
simultaneously:

| Property | Mechanism |
|---|---|
| 🔒 **Unreadable until ordered** | Threshold encryption (DKG committee) — no single party can open a mempool tx |
| ⏱️ **Order-fair** | Slot-based batching with verifiable VRF shuffling inside each slot window |
| 📜 **Provably honest** | Every batch ships with a SNARK proving the operator followed the protocol |

If the sequencer deviates — reorders outside the allowed window, drops a
submitted tx, or decrypts early — it either **cannot produce a valid proof**
or is caught by watchers with a **slashing-eligible commitment**.

### Design goals

- **No new trust assumptions beyond an honest majority in a small committee** (t-of-n threshold crypto).
- **Sub-second latency overhead** vs. a plain sequencer (< 2 slots).
- **Rollup-agnostic**: OP Stack, Arbitrum Orbit, zkSync Era, Polygon CDK, or standalone.

## How It Works

```text
 User                    Gateway           Threshold Committee        Sequencer         L1
  │                         │                      │                     │              │
  │── EIP-712 signed tx ───▶│                      │                     │              │
  │                         │── encrypt(slot t) ───▶│                     │              │
  │◀── receipt w/ commit ────│                      │                     │              │
  │                         │                      │                     │              │
  │                         │   ...slot t closes... │                     │              │
  │                         │                      │── VRF shuffle seed ─▶│              │
  │                         │                      │                     │── order txs  │
  │                         │                      │── joint decrypt ────▶│   by (slot,  │
  │                         │                      │                     │   shuffle idx)│
  │                         │                      │                     │── build batch│
  │                         │                      │                     │── gen SNARK ─▶│
  │                         │                      │                     │              │── post
  │                         │                      │                     │              │   batch+proof
