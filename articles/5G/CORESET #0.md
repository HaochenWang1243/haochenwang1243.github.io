No — that statement is **not strictly correct**, but it captures the common case.

Let’s be precise.

---

# 1️⃣ What CORESET #0 actually is

CORESET #0 is defined in:

3GPP TS 38.213

It is a **special, predefined CORESET** used before RRC configuration.

It is:

* Derived from SSB
* Configured via MIB
* Used before UE has full PDCCH configuration
* Always associated with SearchSpace #0

---

# 2️⃣ What is carried on CORESET #0?

Initially, CORESET #0 is used for:

### ✔️ DCI format 1_0 scheduling:

* SIB1 (SI-RNTI)
* Random Access Response (RA-RNTI)
* Paging (P-RNTI)
* Initial DL scheduling

So yes — it **does carry DCI scheduling SIB1**.

But not only that.

It also carries:

* RA response DCIs
* Paging DCIs
* Possibly other common-search-space DCIs

---

# 3️⃣ Is CORESET #0 only for SIB?

❌ No.

It is for **Common Search Space (CSS)** DCIs before dedicated RRC config exists.

SIB1 is just one use case.

---

# 4️⃣ What about other DCIs after RRC setup?

After `RRCSetup` or `RRCReconfiguration`:

UE receives:

```
pdcch-ConfigCommon
pdcch-Config
```

These configure additional:

* CORESETs (ID 1, 2, …)
* SearchSpaces
* UE-specific PDCCH monitoring

After that:

> Most UE-specific scheduling DCIs use non-zero CORESETs.

Typical mapping:

| Purpose              | CORESET                     |
| -------------------- | --------------------------- |
| SIB1                 | #0                          |
| Random Access        | #0                          |
| Paging               | #0                          |
| UE-specific DL grant | Dedicated CORESET (e.g. #1) |
| UL grant             | Dedicated CORESET           |

---

# 5️⃣ Why is CORESET #0 special?

Because:

* UE must decode it before any RRC signaling
* Its position is derived from SSB
* It works in initial DL BWP only

It is essentially the **bootstrap control channel**.

---

# 6️⃣ Important nuance

Even after RRC setup:

CORESET #0 may still exist.

But UE-specific scheduling usually moves to:

```
CORESET defined in pdcch-Config
```

CORESET #0 remains for:

* System Information
* Paging
* Random Access

---

# 7️⃣ Clean conclusion

| Statement                                       | Correct?                  |
| ----------------------------------------------- | ------------------------- |
| CORESET #0 only carries SIB DCI                 | ❌ No                      |
| CORESET #0 mainly carries common DCIs           | ✅ Yes                     |
| UE-specific DCIs use other CORESETs after setup | ✅ Yes                     |
| CORESET #0 is mandatory                         | ✅ Yes (at initial access) |

---

If you'd like, I can draw the full bootstrap timeline:

SSB → MIB → CORESET #0 → SIB1 → RRCSetup → dedicated CORESETs

That makes the architecture very clear.
