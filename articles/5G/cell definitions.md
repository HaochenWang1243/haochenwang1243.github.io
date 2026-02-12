In **5G NR**, the term *cell* has a very specific architectural meaning. I’ll define it rigorously from both the radio and protocol perspectives.

---

# 1️⃣ Formal Definition (3GPP View)

A **cell** in NR is:

> A logical radio coverage area associated with a specific **carrier frequency** and transmitted by a **gNB**, identified by a **Physical Cell ID (PCI)**.

More concretely:

A cell is defined by the tuple:

```
(gNB, NR-ARFCN, PCI)
```

Where:

* **gNB** = the base station
* **NR-ARFCN** = carrier frequency
* **PCI** = Physical Cell Identity (0–1007)

---

# 2️⃣ Physical Layer Perspective

From a PHY standpoint, a cell is:

* A transmitter broadcasting:

  * Synchronization signals (SSB)
  * PBCH (Physical Broadcast Channel)
  * System Information
* Using a specific carrier frequency
* With a specific numerology (SCS, bandwidth)

The UE identifies a cell by detecting its **SSB**.

---

## 🔹 Synchronization Signals

![Image](https://www.researchgate.net/publication/344690032/figure/fig2/AS%3A966507894931458%401607444847978/G-NR-Synchronization-Signal-Block-SSB-definition-and-burst-structure.png)

![Image](https://www.sharetechnote.com/html/5G/image/NR_SS_Block_ComparisionTo_LTE_02.png)

![Image](https://media.licdn.com/dms/image/v2/D4D12AQH9ANgXC3ZmIg/article-cover_image-shrink_720_1280/article-cover_image-shrink_720_1280/0/1691246632677?e=2147483647\&t=01UQrdbgWjMSZ6w1k8OGirpdjB2gj3rhLq-1HdeuaNI\&v=beta)

![Image](https://www.sharetechnote.com/html/5G/image/NR_SSB_BeamSweeping_01.png)

Each cell transmits:

* **PSS** (Primary Sync Signal)
* **SSS** (Secondary Sync Signal)
* **PBCH** (Broadcast Channel)

Together these form the **SSB (Synchronization Signal Block)**.

The UE:

1. Detects PSS → coarse timing
2. Detects SSS → determines PCI
3. Decodes PBCH → obtains MIB

At that point, the UE has identified a **cell**.

---

# 3️⃣ Logical / RRC Perspective

At higher layers, a cell is:

* A serving radio node that provides:

  * RRC connection
  * Scheduling
  * Mobility control

In RRC signaling (defined in **3GPP TS 38.331**), a cell is referenced via:

* `CellIdentity`
* `PhysCellId`
* `ServingCellConfig`

---

# 4️⃣ Types of Cells in NR

### 🔹 SpCell (Special Cell)

The primary serving cell in an RRC connection.

* In SA → the only cell
* In NSA → typically LTE anchor

### 🔹 PCell (Primary Cell)

Primary cell in a serving cell group.

### 🔹 SCell (Secondary Cell)

Additional cell for carrier aggregation.

### 🔹 PSCell

Primary cell in Secondary gNB (Dual Connectivity).

---

# 5️⃣ One gNB Can Have Multiple Cells

A single gNB can transmit:

* Multiple frequencies → multiple cells
* Same frequency, different beams → still one cell (beam ≠ cell)
* Same frequency, different PCI → different cells

---

# 6️⃣ What a Cell Is NOT

| Misconception                | Reality |
| ---------------------------- | ------- |
| A cell = one antenna         | ❌ No    |
| A cell = one beam            | ❌ No    |
| A cell = one BWP             | ❌ No    |
| A cell = one carrier         | ✅ Yes   |
| A cell = one PCI + frequency | ✅ Yes   |

---

# 7️⃣ Cell vs Beam (Important in NR)

NR introduces beamforming, but:

> A beam is a directional transmission of a cell.
> It is NOT a separate cell.

---

## 🔹 Beam Sweeping Example

![Image](https://media.licdn.com/dms/image/v2/C5112AQHDj6pGBMjzqQ/article-cover_image-shrink_720_1280/article-cover_image-shrink_720_1280/0/1531388710517?e=2147483647\&t=f_fEx0jUi25-Blf56Pj0Y774DgtcqHWzWtu1uHICV_A\&v=beta)

![Image](https://www.sharetechnote.com/html/5G/image/NR_BeamManagement_P2_01.png)

![Image](https://www.sharetechnote.com/html/5G/image/NR_BeamManagement_Overview_06.png)

![Image](https://www.mdpi.com/electronics/electronics-12-04131/article_deploy/html/images/electronics-12-04131-g001.png)

A single cell:

* May transmit multiple SSB beams
* UE selects best beam
* Still same PCI → same cell

---

# 8️⃣ Practical Example (From Logs)

If in QXDM you see:

* PCI = 123
* ARFCN = n41
* gNB ID = X

That combination defines the cell.

If frequency changes → new cell
If PCI changes → new cell
If beam changes → same cell

---

# 9️⃣ Mathematical View

You can think of a cell as defining:

* A specific OFDM grid
* A specific synchronization reference
* A specific scheduling domain

Formally:

[
\text{Cell} = \text{Radio Resource Domain with unique PCI and ARFCN}
]

---

# 🔟 Compact Technical Definition

> A 5G NR cell is a radio coverage area defined by a unique PCI and carrier frequency, transmitted by a gNB, and providing synchronization, broadcast information, and scheduling to UEs.

---

If you want, I can next explain:

* Difference between cell and carrier aggregation
* Cell vs BWP
* Cell vs CORESET
* Cell vs sector
* Cell identity hierarchy (NCI vs PCI)

Given your PHY focus, I can also formalize it from OFDM resource-grid perspective.
