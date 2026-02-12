Yes — for **non-initial BWPs (ID ≥ 1)**, both `bwp-Common` and `bwp-Dedicated` are carried inside **RRCSetup / RRCReconfiguration** when the network configures them.

But the architectural meaning is different. Let’s be precise.

---

# 1️⃣ Where they appear in RRC

Inside:

```
ServingCellConfig
 └─ downlinkBWP-ToAddModList
      └─ BWP-Downlink
           ├─ bwp-Id
           ├─ bwp-Common
           └─ bwp-Dedicated
```

and similarly for UL under:

```
uplinkConfig
 └─ uplinkBWP-ToAddModList
      └─ BWP-Uplink
           ├─ bwp-Common
           └─ bwp-Dedicated
```

So yes — when BWPs are added/modified, **both parts are in the same RRC message**.

---

# 2️⃣ What `bwp-Common` means

This defines **frequency-domain structure and cell-common channel behavior within that BWP**.

Typical contents:

* `genericParameters`

  * `locationAndBandwidth`  ← (RIV → RB start + length)
  * `subcarrierSpacing`
  * `cyclicPrefix`
* `pdcch-ConfigCommon`
* `pdsch-ConfigCommon`
* (UL side: `rach-ConfigCommon`, `pusch-ConfigCommon`, `pucch-ConfigCommon`)

### Interpretation

This defines:

> “If this BWP exists, this is its frequency span and baseline channel configuration.”

It describes **what the BWP physically is**.

Without `bwp-Common`, the UE would not know:

* where the BWP sits in frequency
* how wide it is
* which SCS applies

---

# 3️⃣ What `bwp-Dedicated` means

This defines **UE-specific control and scheduling behavior inside that BWP**.

Typical contents:

* `pdcch-Config`
* `pdsch-Config`
* `pucch-Config`
* `pusch-Config`
* `srs-Config`
* `configuredGrantConfig`
* RLM / beamFailure configs

### Interpretation

This defines:

> “How this specific UE should be scheduled and monitored inside this BWP.”

It does **not** define the frequency span.

---

# 4️⃣ Why both exist

Because NR separates:

### Structural definition (Common)

Frequency geometry + baseline channel config
→ Must be consistent with cell design

### Scheduling behavior (Dedicated)

Aggregation levels, search spaces, grants, SRS
→ Per-UE optimization

Same design philosophy as:

* `pdcch-ConfigCommon` vs `pdcch-Config`
* `initialDownlinkBWP` (common in SIB1) vs dedicated augment

---

# 5️⃣ Important subtlety about Initial BWP (ID=0)

This is where confusion often happens.

For **BWP#0**:

* `bwp-Common` comes from **SIB1** (`ServingCellConfigCommon`)
* `bwp-Dedicated` (if present) comes from **RRCSetup/Reconfig**

So for ID=0, common and dedicated are split across **different messages**.

For ID≥1, both common and dedicated are inside the same RRC message.

---

# 6️⃣ Practical tracing implication

If you're extracting bandwidth:

👉 Only read from:

```
bwp-Common → genericParameters → locationAndBandwidth
```

Never from `bwp-Dedicated`.

If you're extracting scheduling behavior:

👉 Only read from:

```
bwp-Dedicated
```

---

# 7️⃣ Conceptual mental model

Think of a BWP as two layers:

```
Layer A: Physical envelope
    - frequency span
    - SCS
    - CP

Layer B: UE scheduling personality
    - CORESETs
    - search spaces
    - grants
    - SRS
```

`bwp-Common` = Layer A
`bwp-Dedicated` = Layer B

---

# 8️⃣ One-line summary

Yes — for non-initial BWPs, both `bwp-Common` and `bwp-Dedicated` are carried together in RRCSetup/Reconfiguration.

But:

> `bwp-Common` defines what the BWP is.
> `bwp-Dedicated` defines how this UE uses it.
