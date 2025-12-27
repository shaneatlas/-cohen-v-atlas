# COHEN v. ATLAS — Entity-Relation Graph Summary

**Extraction Date:** 2025-12-27
**Method:** CUARC-compatible (verbatim evidence anchors only)
**Status:** Options 2, 1, 3 complete; Option 4 pending

---

## ARCHITECTURE

```
┌─────────────────────────────────────────────────────────────────┐
│                    CANONICAL SPINE (Option 2)                   │
│                  ROA / Docket Backbone                          │
│  ┌─────────────┬─────────────┬─────────────┬─────────────┐     │
│  │   Parties   │   Judges    │  Docket     │  Evidence   │     │
│  │   (3)       │   (3)       │  Items (13) │  Items (7)  │     │
│  └──────┬──────┴──────┬──────┴──────┬──────┴──────┬──────┘     │
│         │             │             │             │             │
│         ▼             ▼             ▼             ▼             │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              Timeline + Jurisdictional Facts            │   │
│  │              (6 dates + 3 amounts)                      │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
           ┌──────────────────┼──────────────────┐
           ▼                  ▼                  ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│  Option 1        │ │  Option 3        │ │  Option 4        │
│  Defendants'     │ │  SFJ Court       │ │  Full Project    │
│  Reply           │ │  Findings        │ │  (PENDING)       │
│  (02/10/2025)    │ │  (08/02/2025)    │ │                  │
├──────────────────┤ ├──────────────────┤ ├──────────────────┤
│ Assertions: 12   │ │ FOF: 9           │ │                  │
│ Evidence: 3      │ │ COL: 3           │ │                  │
│ Authorities: 3   │ │ AD Rejected: 7   │ │                  │
│ CUARC Quotes: 4  │ │ Orders: 4        │ │                  │
│                  │ │ Authorities: 5   │ │                  │
│ TYPE: Motion     │ │ TYPE: Court      │ │                  │
│       Assertion  │ │       Finding    │ │                  │
└──────────────────┘ └──────────────────┘ └──────────────────┘
```

---

## FILES PRODUCED

| File | Entities | Categories | Purpose |
|------|----------|------------|---------|
| `ENTITY_RELATIONS_ROA_SPINE.json` | 53 | 8 | Canonical backbone |
| `ENTITY_RELATIONS_DEFENDANTS_REPLY.json` | 26 | 5 | Party assertions |
| `ENTITY_RELATIONS_SFJ_COURT_FINDINGS.json` | 38 | 7 | Court determinations |
| **TOTAL** | **117** | — | — |

---

## CRITICAL DISCOVERY: RECORD CONTRADICTION

The entity extraction revealed a **fatal contradiction** central to I-SECDEP:

| Source | Content | Type |
|--------|---------|------|
| **SFJ FOF-18** | "Defendants did not return the deposit and did not provide written notice of a claim against the deposit within the thirty (30) days" | Court_Finding |
| **E-SEC-01** | Notice of Intention to Impose Claim on Deposit dated **01/06/2018** | Evidence_Item |

**Implication:** The court's finding that no notice was provided is contradicted by record evidence of a notice dated within the lease period. This supports reversal on I-SECDEP.

---

## CROSS-REFERENCES IDENTIFIED

### Court vs. Defendants' Position on Spoliation

| Entity | Position |
|--------|----------|
| **AD-4-REJECTED** (SFJ) | "There is no evidence that Plaintiff destroyed or withheld any relevant evidence" |
| **REPLY-A5** (Defendants) | "Police Report #2019-00042552 documents Plaintiff's unauthorized removal and disposal of Defendant's property" |

**Status:** Conflict unresolved; Police Report not ingested in current packet.

### Damages Proof

| Entity | Position |
|--------|----------|
| **FOF-13** (SFJ) | "Plaintiff was forced to pay $2,026.49 for replacement of the washer and dryer" |
| **FOF-14** (SFJ) | "Plaintiff was forced to retain Miami Mold... at a cost of $6,500" |
| **REPLY-A8** (Defendants) | "unpaid estimates for a washer dryer and unsupported invoices for the alleged remediation, offering no proof of actual payment" |
| **REPLY-A9** (Defendants) | "Miami Mold's subpoenaed contemporaneous records... confirms 'no proof of payment exists'" |

**Status:** Court accepted Plaintiff's evidence; Defendants cite contrary subpoenaed records. CUARC validation shows R1 quote is from Miami Mold letter itself (evidence of gap, not contradiction).

---

## NEXT STEP: Option 4 (Full Project)

Once the above graphs are stable, Option 4 will:
1. Merge all entities into unified graph
2. De-duplicate shared entities (E-SEC-01, E-MOLD-01, etc.)
3. Add remaining project files (Authority Table, Issue Table, etc.)
4. Generate complete relationship map for brief drafting

---

## CUARC COMPLIANCE

All extractions follow:
- **Verbatim evidence anchors** (8-20 word quotes)
- **Typed claims** (Motion_Assertion vs. Court_Finding)
- **Spine linking** (all entities reference canonical ROA items)
- **No speculation** (unsupported items omitted or flagged)

**#OPTIMALITY_STATUS: satisfied** (for Options 2, 1, 3)
