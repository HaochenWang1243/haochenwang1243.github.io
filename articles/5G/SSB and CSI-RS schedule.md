Good — now we’re asking the right question.

Short answer:

> ✔ You can determine the *theoretical schedule* of SSB and periodic CSI-RS from RRC + MIB.
> ❌ You cannot determine the *exact UE processing moments* from RRC logs alone.

You can compute **when the signals exist in the grid**, not when the modem actually woke up to process them.

Let’s separate the two.

---

# 1️⃣ SSB Schedule — What You Can Derive

Defined in:

* 3GPP TS 38.211
* 3GPP TS 38.213

---

## Required Information

From logs you need:

### From MIB:

* `subCarrierSpacingCommon`
* `ssb-SubcarrierOffset`

### From ServingCellConfig (RRC):

* `ssb-PeriodicityServingCell`

---

## What RRC gives you

It gives:

```
SSB periodicity (e.g., 20 ms)
```

SSB burst timing is deterministic.

---

## How to compute SSB slots

Let:

[
\mu = \log_2(SCS / 15kHz)
]

Slots per frame:

[
N_{slot}^{frame} = 10 \cdot 2^\mu
]

If SSB periodicity = T ms:

[
N_{period} = T \cdot 2^\mu
]

Then SSB occurs in:

[
slot = k \cdot N_{period} + offset(index)
]

The offset(index) comes from SSB case pattern tables in 38.213.

So:

✔ You can compute exact slot indices where SSB exists.

---

# 2️⃣ CSI-RS Schedule — What You Can Derive

Defined in 3GPP TS 38.211

---

## From RRC logs you look for:

```
csi-ResourceConfig
  periodicityAndOffset
```

If periodic CSI-RS:

```
(periodicity = P slots, offset = O)
```

Then CSI-RS exists in:

[
(slot - O) \bmod P = 0
]

✔ Exact deterministic slots.

---

## If aperiodic CSI-RS:

RRC only defines the resource.

Actual timing depends on:

```
DCI CSI request field
```

So:

❌ You cannot know timing from RRC alone.
You must parse DCI logs.

---

# 3️⃣ What You Cannot Know from RRC Logs

RRC does NOT tell you:

* Whether UE was in DRX sleep
* Whether RF was powered down
* Whether measurement gaps were used
* Whether UE skipped monitoring due to power saving

So RRC tells you:

> When signals are transmitted.

It does not tell you:

> When modem physically processed them.

---

# 4️⃣ Summary Table

| Signal Type            | Can derive schedule from RRC? | Need DCI? |
| ---------------------- | ----------------------------- | --------- |
| SSB                    | ✔ Yes                         | ❌         |
| Periodic CSI-RS        | ✔ Yes                         | ❌         |
| Semi-persistent CSI-RS | ✔ Yes (while active)          | MAC CE    |
| Aperiodic CSI-RS       | ❌                             | ✔ Yes     |

---

# 5️⃣ What You Actually Should Do

From your logs:

1. Extract numerology
2. Extract SSB periodicity
3. Extract CSI periodicityAndOffset
4. Reconstruct global slot timeline
5. Apply modulo equations

That gives you exact theoretical SSB/CSI-RS slots.

---

# Final Answer

✔ Yes — you can compute SSB and periodic CSI-RS schedule from RRC logs.
❌ No — RRC logs alone cannot prove the modem actually processed them.
✔ Aperiodic CSI-RS timing requires DCI parsing.

---

If you want, I can now give you a small Python function that takes (μ, periodicity, offset, SFN, slot) and outputs all SSB/CSI-RS slots in your capture.
