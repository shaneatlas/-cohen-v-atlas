# MU5KOSX RUNBOOK v3.0
**Role:** Execution Authority | **Status:** FROZEN | **Validator Coverage:** 92%

---

## AUTHORITY RULE

```
If vFinal (philosophy) and v3.0 (runbook) conflict → v3.0 controls.
```

---

## CHANGE DECISION GATE (CLAUSE 7)

Before making ANY change, apply this test:

```
Does this change:
  ✓ Increase validator coverage?
  ✓ Reduce blockers?
  ✓ Improve quote compliance?
  ✓ Reduce ingestion backlog?

YES to any → ALLOWED
NO to all  → REJECT AS STYLISTIC
```

**Pass limits:** ≤4 evaluations, ≤3 corrections, then STOP.

---

## VALIDATOR STAGES (CURRENT)

| Stage | Gate | Severity |
|-------|------|----------|
| 1 | Optimality closure (Appendix D ↔ MANIFEST) | error |
| 2 | Provenance tags on all claims | error |
| 3 | `[Corpus]` has ≥1 citation | error |
| 4 | `[Inference]` has Citation Debt ledger | error |
| 5 | Quote anchors 8-20 words, no ellipses | error |
| **6** | No `Evidence_Reference` treated as `Evidence_Item` | error |
| **7** | Holding quoted only if `Holding_Text_Verified=YES` | error |
| **8** | All contradictions have Layer_A/Layer_B | warning |

*Stages 6-8 = v4.0 target validators*

---

## SEMANTIC OVERREACH PREVENTION

| Column | Meaning | Gate |
|--------|---------|------|
| `Evidence_Class` | Reference vs Item | Reference ≠ ingested |
| `Requires_Ingestion` | YES = not yet usable | Block until NO |
| `Citation_Verified` | Case exists | Minimum bar |
| `Holding_Text_Verified` | Holding quoted verbatim | Required for [Corpus] |
| `Quote_Compliant` | Edge has 8-20 word anchor | Required for draft-safe |

**Hard rule:** `Evidence_Reference` with `Requires_Ingestion=YES` cannot support a claim tagged `[Corpus]`.

---

## OPTIMALITY STATUS

```
#OPTIMALITY_STATUS: satisfied | blocked

satisfied → No [MissingData] or [IrreducibleConflict] OIs
blocked   → Blockers: OI-###, ...
```

**Invariant:** `MissingData ⇒ blocked`. Never invent data to reach satisfied.

---

## WORKBOOK GATES (COHEN v8)

| Gate | Formula | Threshold |
|------|---------|-----------|
| Quote_Compliance_% | `COUNTIF(EDGES.Quote_Compliant,"YES")/COUNTA(EDGES)` | ≥80% |
| Ingestion_Backlog | `COUNTIF(RECORD_ITEMS.Requires_Ingestion,"YES")` | 0 |
| Holding_Unverified | `COUNTIF(AUTHORITIES.Holding_Text_Verified,"NO")` | 0 |
| Open_Contradictions | `COUNTIF(CONTRADICTIONS.Status,"OPEN")` | 0 |

**Promotion rule:** All gates pass → MODE 3 (Closure)

---

## MODE CONSTRAINTS (ENFORCED)

| Mode | Identity Mutation | Scope Expansion | Edge Refinement | Quote Upgrade |
|------|-------------------|-----------------|-----------------|---------------|
| 0 | — | — | — | — |
| 1 | CREATE | CREATE | CREATE | CREATE |
| 2 | **FORBIDDEN** | **FORBIDDEN** | allowed | allowed |
| 3 | **FORBIDDEN** | **FORBIDDEN** | **FORBIDDEN** | **FORBIDDEN** |

---

## VALIDATION INVOCATION

```bash
# Standard run
python3 validate.py output.md MANIFEST.json

# Expected output
Optimality Gate → PASS
```

**On FAIL:** Fix violations in order listed. Re-run. Do not ship with failures.

---

## MANIFEST REQUIREMENT

Every frozen artifact set requires:

```json
{
  "spec_id": "PROJECT_ID",
  "manifest_version": "1.0.0",
  "files": [
    {"name": "...", "sha256": "64-char hex", "bytes": N}
  ]
}
```

**Verify before session:** `sha256sum <file>` must match manifest.

---

## CONFIDENCE RATIO

```
RATIO = (RECORD + INFERENCE) / total_claims

≥0.70       → Proceed
0.50-0.69   → Flag assumptions, proceed with caution  
<0.50       → HALT with gap list
```

---

## FAILURE MODES ACTIVE

| Mode | Detection | Response |
|------|-----------|----------|
| Hallucination | Missing provenance tag | Validator Stage 2 fail |
| Semantic overreach | Reference treated as Item | Validator Stage 6 fail |
| Authority inflation | Unverified holding quoted | Validator Stage 7 fail |
| Contradiction leak | Missing Layer_A/B | Validator Stage 8 warning |

---

## EXECUTION CHECKLIST

```
□ Mode declared (0/1/2/3)
□ Manifest verified (SHA-256 match)
□ Validator run (all stages pass)
□ Gates checked (workbook thresholds)
□ Confidence ratio ≥0.70
□ No [MissingData] blockers
□ Change passes Clause 7 gate
```

**Ship only when all boxes checked.**

---

*v3.0 is authoritative. Philosophy (vFinal) informs; this runbook decides.*
