Excellent — now we’re stepping into **NR channel taxonomy**.
This is where many people get confused because 3GPP mixes *logical*, *transport*, and *physical* channels.

Let’s untangle it cleanly.

---

# First: Do You Remember the 3-Layer Mapping?

In LTE/NR, data flows through:

```
Logical Channel → Transport Channel → Physical Channel
```

Before I explain anything:

👉 Do you recall what layer **PDCCH** and **PDSCH** belong to?

(Answer mentally — that will anchor everything.)

They are **physical channels**.

Now let’s place everything properly.

---

# 1️⃣ Physical Channels (PHY layer)

These are actual radio wave carriers.

You already know:

* PDCCH
* PDSCH
* PUSCH
* PUCCH

Now adding:

## 🟣 PBCH = Physical Broadcast Channel

Purpose:

> Transmits the MIB.

It carries:

* System Frame Number
* Subcarrier spacing info
* SSB related info

It is always transmitted inside the **SSB (Synchronization Signal Block)**.

So:

SSB = PSS + SSS + PBCH

Let me ask you:

👉 Can PBCH carry user data?

(No. Only system broadcast info.)

---

# 2️⃣ Transport Channels (MAC → PHY interface)

These describe **how data is delivered over the air**, not what it contains.

Examples:

* DL-SCH = Downlink Shared Channel
* UL-SCH = Uplink Shared Channel
* BCH = Broadcast Channel
* RACH = Random Access Channel

These are between MAC and PHY.

---

### DL-SCH

Used for:

* User data
* SIB1
* RRC messages (after connection)
* Almost everything DL except MIB

Mapped onto:

* PDSCH

So:

DL-SCH → PDSCH

---

### BCH

Used for:

* MIB only

Mapped onto:

* PBCH

So:

BCH → PBCH

---

# 3️⃣ Logical Channels (RLC/MAC interface)

These describe **what kind of information it is**.

Examples:

* BCCH = Broadcast Control Channel
* PCCH = Paging Control Channel
* CCCH = Common Control Channel
* DCCH = Dedicated Control Channel
* DTCH = Dedicated Traffic Channel

---

Now your question:

> What is DL_CCH?

Actually in NR spec it’s usually:

* DL-CCCH
* DL-DCCH

They are **logical channels**.

---

# Let’s Map One Example End-to-End

Example: RRCReconfiguration

What is it?

Control message for one UE.

So:

Logical channel: DL-DCCH
Transport channel: DL-SCH
Physical channel: PDSCH

Flow:

RRCReconfiguration
→ DL-DCCH
→ DL-SCH
→ PDSCH

---

# Another Example: SIB1

SIB1 is broadcast.

Logical channel: BCCH
Transport channel: DL-SCH
Physical channel: PDSCH

---

# Another Example: MIB

Logical channel: BCCH
Transport channel: BCH
Physical channel: PBCH

---

# Big Picture Table

| Logical | Transport | Physical                        |
| ------- | --------- | ------------------------------- |
| BCCH    | BCH       | PBCH                            |
| BCCH    | DL-SCH    | PDSCH                           |
| PCCH    | DL-SCH    | PDSCH                           |
| DL-DCCH | DL-SCH    | PDSCH                           |
| UL-DCCH | UL-SCH    | PUSCH                           |
| CCCH    | UL-SCH    | PUSCH                           |
| —       | —         | PDCCH (control scheduling only) |

---

# Important Clarification

PDCCH is special.

It does NOT carry:

* User data
* RRC messages
* SIB

It only carries:

DCI (Downlink Control Information)

Which tells the UE:

* Where PDSCH is
* Where PUSCH is
* MCS
* RB allocation

So PDCCH schedules DL-SCH and UL-SCH.

---

# Step 1 — Start From What You Already Know

You know:

* DL-DCCH carries RRC messages
* BCCH carries system info
* DTCH carries user data

Question:

👉 Does the PHY layer care whether the bits are RRC, SIB, or user IP data?

Or does it only care about things like:

* Coding rate
* HARQ
* Scheduling
* CRC
* Retransmission

Which one?

---

The PHY/MAC does **not care about the semantic meaning** of the bits.
It only cares about **how to transmit them reliably**.

That’s where transport channels come in.

---

# Step 2 — What “How” Means Technically

Transport channels define:

* Is HARQ used?
* Is link adaptation used?
* Is scheduling dynamic or fixed?
* Is it broadcast or UE-specific?
* Is retransmission allowed?
* What CRC size?
* What coding scheme?

They define the **transmission mechanism**.

They do *not* define whether the bits are:

* SIB
* Paging
* RRC
* TCP payload

That classification happens at the **logical channel** layer.

---

# Concrete Example

Take two different logical channels:

* BCCH (SIB1)
* DL-DCCH (RRCReconfiguration)

Both are carried over:

DL-SCH
→ mapped to PDSCH

Now ask yourself:

👉 Does DL-SCH behave differently because it carries SIB1 vs RRCReconfiguration?

No.

DL-SCH:

* Uses HARQ
* Uses link adaptation
* Is scheduled by DCI
* Uses TB + CRC

It doesn’t care what the bits represent.

---

# Now Compare With BCH

BCH (transport channel) is different:

* No HARQ
* Fixed coding
* Broadcast
* Very robust
* Always mapped to PBCH

Why?

Because its delivery mechanism is different.

It’s designed for:

* Initial access
* No UE identity yet
* Maximum coverage

---

# So the Hierarchy Is:

### Logical channel → defines **what the bits are**

* Control vs traffic
* Broadcast vs dedicated

### Transport channel → defines **how the bits are transmitted**

* Scheduled?
* Retransmitted?
* Broadcast?
* Coding robustness?
* HARQ?

### Physical channel → defines **where in the grid**

* Which PRBs
* Which symbols

---

# Let Me Test You

Imagine we invent a brand new logical channel tomorrow:

“EmergencyAlertChannel”

If we map it to DL-SCH…

👉 Would we need to redesign the PHY?

Or would it just behave like any other DL-SCH transmission?

Think about that carefully.

4. Does PDCCH carry RRC messages?

Answer those one by one.
Keep them short.
