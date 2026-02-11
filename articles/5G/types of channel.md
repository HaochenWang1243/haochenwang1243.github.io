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

# Quick Check for You

1. Which physical channel carries SIB1?
2. Which physical channel carries MIB?
3. Which physical channel carries RRCReconfiguration?
4. Does PDCCH carry RRC messages?

Answer those one by one.
Keep them short.
