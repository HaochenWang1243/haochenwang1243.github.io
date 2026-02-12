Excellent question — this is a core architectural distinction in NR.

You are comparing:

* **`spCellConfig` in RRCSetup / RRCReconfiguration**
* **`ServingCellConfigCommon` in SIB1**

They look similar because they both describe “a cell”, but they serve different layers and scopes.

Let’s make this precise.

---

# 1️⃣ ServingCell in SIB1 (Broadcast, Cell-Common)

Location:

```
SIB1
 └─ ServingCellConfigCommon
      ├─ downlinkConfigCommon
      ├─ uplinkConfigCommon
      ├─ ssb-PositionsInBurst
      ├─ tdd-UL-DL-ConfigurationCommon
      └─ ...
```

### What it represents

This is:

> The baseline physical configuration of the cell itself.

It is:

* Broadcast
* Identical for all UEs
* Required for initial access
* Independent of RRC connection

It defines:

* Carrier frequency
* Band
* PointA
* Carrier bandwidth
* Initial DL/UL BWP span (BWP#0)
* CORESET#0
* SI scheduling
* RACH parameters
* TDD pattern
* SSB periodicity

This is essentially:

> “How this cell physically exists on air.”

Every UE that camps on the cell reads this.

---

# 2️⃣ SpCellConfig in RRCSetup/Reconfiguration (UE-Specific)

Location:

```
RRCReconfiguration
 └─ spCellConfig
      ├─ servingCellConfig
      └─ reconfigurationWithSync (optional)
```

### What it represents

This is:

> How this specific UE should use the Special Cell.

SpCell = Special Cell = primary serving cell for this UE.

In SA:

* SpCell = PCell

In DC:

* SpCell = primary cell of MCG

This is:

* UE-specific
* Only exists after connection
* May modify or extend common config

It defines:

* Additional BWPs
* Dedicated PDCCH/PDSCH config
* BWP switching behavior
* CSI config
* SRS config
* Power control
* Inactivity timers
* Per-UE search spaces
* etc.

This is not the “cell definition” — it is:

> The per-UE control plane overlay on top of the cell.

---

# 3️⃣ Key Difference in One Sentence

* **ServingCellConfigCommon (SIB1)** = describes the cell.
* **spCellConfig (RRC)** = describes how this UE operates within that cell.

---

# 4️⃣ Why They Look Similar

Because during:

```
reconfigurationWithSync
   spCellConfigCommon
```

the network embeds a copy of cell-common config inside RRC.

Why?

Because during handover, the UE cannot rely on previously stored SIB1.

So the network injects the essential common parameters inline.

That does NOT mean SIB1 and spCellConfig are the same concept.

It means:

> RRC temporarily carries cell-common data for sync purposes.

---

# 5️⃣ Conceptual Layering

Think in layers:

### Layer 0 — Cell existence

Broadcast via SIB1:

```
Carrier
Frequency
Grid
TDD pattern
Initial BWP
CORESET#0
RACH
```

### Layer 1 — UE anchor

RRCSetup assigns:

```
C-RNTI
SpCell association
```

### Layer 2 — UE optimization

RRCReconfiguration adds:

```
Extra BWPs
Dedicated CORESETs
Search spaces
SRS
CSI
Timers
```

---

# 6️⃣ Another Important Clarification

ServingCell in SIB1 is always present.

SpCell in RRC exists only after:

* UE is connected
* That cell is chosen as primary for that UE

So:

* A cell can exist without being someone’s SpCell.
* But a SpCell always corresponds to a real cell defined by SIB1.

---

# 7️⃣ When Tracing Logs

If you're reconstructing:

### Physical carrier layout

→ Use SIB1 (ServingCellConfigCommon)

### UE BWP behavior

→ Use spCellConfig → servingCellConfig

### Cell switch event

→ Look for spCellConfig with reconfigurationWithSync

---

# 8️⃣ One More Subtlety (Very Important)

SIB1 defines only:

* Initial BWP (ID 0)

Additional BWPs (ID 1..3) are never broadcast.

They exist only inside:

```
spCellConfig → ServingCellConfig → downlinkBWP-ToAddModList
```

So:

> The cell defines a minimal anchor.
> The UE defines extended bandwidth usage.

---

# 9️⃣ Ultra-Compressed Mental Model

SIB1:

> “This is the radio.”

spCellConfig:

> “This is how you use the radio.”
