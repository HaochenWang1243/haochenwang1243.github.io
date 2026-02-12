This is a **very fundamental split** in NR RRC design:

> **Common = cell-wide, broadcast (SIB1)**
> **Dedicated = UE-specific, signaled in RRCSetup / RRCReconfiguration**

Let’s break it down precisely.

---

# 1️⃣ `pdcch-ConfigCommon` (in SIB1)

Location:

```
ServingCellConfigCommon
 └─ downlinkConfigCommon
     └─ initialDownlinkBWP
         └─ pdcch-ConfigCommon
```

### What it configures

* **CORESET#0**
* SearchSpaces for:

  * SI-RNTI (SIB scheduling)
  * RA-RNTI (Random Access)
  * P-RNTI (Paging)
  * sometimes temporary C-RNTI during RA

### Who uses it?

* All UEs in cell
* Before RRC connection
* During:

  * SIB acquisition
  * Random access
  * Paging
  * Early connection establishment

### Key characteristics

* Broadcast in **SIB1**
* Always tied to **initial DL BWP (#0)**
* Defines:

  * CORESET frequency region
  * monitoring symbols
  * search space periodicity
* Minimal configuration — just enough to bootstrap control channel decoding

This is the **bootstrap control plane**.

---

# 2️⃣ `pdcch-Config` (Dedicated, in RRCSetup/Reconfig)

Location:

```
ServingCellConfig
 └─ downlinkBWP-ToAddModList
      └─ BWP-Downlink
          └─ bwp-Dedicated
              └─ pdcch-Config
```

or

```
ServingCellConfig
 └─ initialDownlinkBWP
      └─ pdcch-Config
```

### What it configures

* UE-specific:

  * Additional CORESETs
  * UE-specific SearchSpaces
  * C-RNTI monitoring
  * Aggregation levels
  * Candidate sets
  * Cross-carrier scheduling
  * BWP switching DCI monitoring

### Who uses it?

* Only this UE
* After RRC connection
* During connected mode scheduling

This is the **full scheduling control plane**.

---

# 3️⃣ Conceptual Difference

| Aspect                       | pdcch-ConfigCommon     | pdcch-Config                  |
| ---------------------------- | ---------------------- | ----------------------------- |
| Signaling                    | SIB1 (broadcast)       | RRCSetup/Reconfig (dedicated) |
| Scope                        | Cell-wide              | UE-specific                   |
| Used before RRC?             | Yes                    | No                            |
| Used for SI/RA/Paging        | Yes                    | No                            |
| Used for C-RNTI scheduling   | No                     | Yes                           |
| Can define multiple CORESETs | Usually only CORESET#0 | Yes                           |
| Per-BWP configurable         | Only initial BWP       | Every configured BWP          |

---

# 4️⃣ Why this split exists

Because NR must support:

1. **UE not connected yet**
   → needs to decode system info & RA
   → must rely only on broadcast config.

2. **UE connected**
   → network can optimize PDCCH:

   * aggregation levels
   * control overhead
   * BWP-specific search spaces
   * cross carrier scheduling

So common = minimum universal access
Dedicated = optimized per-UE scheduling

---

# 5️⃣ Typical Timeline

### Idle UE:

Uses:

```
pdcch-ConfigCommon
```

### After RRCSetup:

Uses:

```
pdcch-Config (dedicated)
```

But:

* CORESET#0 may still exist
* UE may monitor both common and dedicated search spaces depending on state

---

# 6️⃣ Practical QXDM Interpretation

If you're tracing logs:

### Before RRCSetup:

Only CORESET#0 activity
Search spaces: SI-RNTI / RA-RNTI

### After RRCSetup:

You should see:

* C-RNTI assigned
* New CORESETs added
* Dedicated search spaces appear
* Aggregation levels change

If you do not see new CORESETs in RRCReconfiguration:
UE may still operate only in BWP#0.

---

# 7️⃣ One sentence summary

> `pdcch-ConfigCommon` is the broadcast bootstrap control configuration.
> `pdcch-Config` is the UE-specific scheduling control configuration.

---
# similar pattern exists for bwp config?
Yes — **exactly the same architectural split exists for BWP configuration**:

> **Common (cell-wide, broadcast in SIB1)**
> **Dedicated (UE-specific, signaled in RRCSetup / RRCReconfiguration)**

Let’s map it precisely.

---

# 1️⃣ Common BWP Configuration (SIB1)

Location:

```
ServingCellConfigCommon
 ├─ downlinkConfigCommon
 │    └─ initialDownlinkBWP  →  BWP-DownlinkCommon
 └─ uplinkConfigCommon
      └─ initialUplinkBWP    →  BWP-UplinkCommon
```

### What it defines

This is the **initial BWP (BWP#0)**.

It contains:

* `genericParameters` → **locationAndBandwidth (RIV)**
* `subcarrierSpacing`
* `cyclicPrefix`
* `pdcch-ConfigCommon`
* `pdsch-ConfigCommon`
* `rach-ConfigCommon`
* `pusch-ConfigCommon`
* `pucch-ConfigCommon`

### Who uses it?

* All UEs in the cell
* Before RRC connection
* During:

  * MIB/SIB acquisition
  * Random access
  * Paging
  * Early connection establishment

### Important property

The **initial BWP always exists**.

Even if the network never configures additional BWPs, the UE operates in BWP#0.

This is the **bootstrap bandwidth part**.

---

# 2️⃣ Dedicated BWP Configuration (RRCSetup / Reconfiguration)

Location:

```
ServingCellConfig
 ├─ initialDownlinkBWP           → BWP-DownlinkDedicated
 ├─ downlinkBWP-ToAddModList     → BWP-Downlink
 ├─ firstActiveDownlinkBWP-Id
 ├─ defaultDownlinkBWP-Id
 ├─ bwp-InactivityTimer
 └─ uplinkConfig
      ├─ initialUplinkBWP
      ├─ uplinkBWP-ToAddModList
      ├─ firstActiveUplinkBWP-Id
```

### What it defines

UE-specific behavior:

* Dedicated PDCCH/PDSCH configs
* Multiple additional BWPs (max 4)
* Active BWP switching behavior
* Inactivity fallback
* DCI-based switching mapping
* Per-BWP CORESETs
* Per-BWP search spaces

### Who uses it?

* Only that UE
* After RRC connection
* In connected mode

This is the **adaptive bandwidth control layer**.

---

# 3️⃣ Structural Analogy with PDCCH

| Concept                          | Common                        | Dedicated                              |
| -------------------------------- | ----------------------------- | -------------------------------------- |
| PDCCH                            | `pdcch-ConfigCommon`          | `pdcch-Config`                         |
| BWP                              | `initialDownlinkBWP` (Common) | `downlinkBWP-ToAddModList` + Dedicated |
| Scope                            | Cell-wide                     | UE-specific                            |
| Signaled in                      | SIB1                          | RRCSetup / Reconfig                    |
| Used pre-connection              | Yes                           | No                                     |
| Used for scheduling optimization | No                            | Yes                                    |

The pattern is identical.

---

# 4️⃣ Why BWP is split this way

Because NR must allow:

### Stage 1: Cell Access

UE does not yet have:

* C-RNTI
* Dedicated config
* Scheduling grants

So it must:

* Decode SIB1 inside a known frequency region
* Perform RACH inside known UL region

Therefore:

> The cell must broadcast a guaranteed minimal BWP (BWP#0).

---

### Stage 2: Connected Mode Optimization

Once connected:

The network can:

* Shrink BWP to save UE power
* Expand BWP for high throughput
* Configure multiple BWPs
* Switch via DCI dynamically
* Use different SCS per BWP

This requires UE-specific config → Dedicated RRC.

---

# 5️⃣ Key Conceptual Insight

The **initial BWP (ID = 0)** is special:

* Defined in Common
* Always exists
* May or may not have dedicated augmentation
* Cannot be removed
* DCI switching may not support returning to it (if 4 BWPs configured)

Think of it as:

> "Safe mode frequency region"

---

# 6️⃣ Important Subtlety (Where people get confused)

`ServingCellConfig.initialDownlinkBWP`

is **NOT** defining frequency span.

It only augments:

```
BWP-DownlinkDedicated
```

The frequency span (`locationAndBandwidth`) lives only in:

```
ServingCellConfigCommon
  → initialDownlinkBWP
      → genericParameters
```

This is by design.

---

# 7️⃣ Big Picture Mental Model

NR control architecture has three layers:

```
Layer 0: Broadcast bootstrap
    - initial BWP (Common)
    - CORESET#0
    - SI / RA / Paging

Layer 1: UE-specific baseline
    - Dedicated initial BWP parameters
    - firstActiveBWP

Layer 2: Dynamic optimization
    - Additional BWPs
    - DCI switching
    - Inactivity fallback
```

---

# 8️⃣ Practical Debugging Insight (For Your Logs)

If you want to reconstruct:

### Frequency span of BWP#0

→ Parse SIB1 → ServingCellConfigCommon

### Whether additional BWPs exist

→ Look for `downlinkBWP-ToAddModList` in RRCReconfiguration

### Which BWP is active

→ Check:

* `firstActiveDownlinkBWP-Id`
* DCI BWP indicator field
* Inactivity timer expiry

---

# 9️⃣ One-line summary

Yes — BWP configuration follows the exact same:

> Common (broadcast bootstrap)
> Dedicated (UE-specific optimization)

design philosophy as PDCCH.

