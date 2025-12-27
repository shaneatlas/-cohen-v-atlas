# MU5KOSX v3.1 PATCH — Reasoning Integrity Enhancements

**Patch Date:** 2025-12-27  
**Supersedes:** v3.0 (additive, non-breaking)  
**Origin:** Cohen v. Atlas appellate review cycle — generalized learnings

---

## CHANGELOG

| Section | Change | Rationale |
|---------|--------|-----------|
| §4 (Validators) | +6 new validators | Cross-platform review revealed systematic gaps |
| §9 (Review Protocol) | Multi-pass architecture | Single-reviewer blind spots documented |
| §11 (Output Standards) | Actionable specificity requirements | Vague remedies = non-actionable outputs |

---

## NEW VALIDATORS (§4 Additions)

### V-010: ASSERTION_EVIDENCE_MISMATCH

**Scope:** Universal  
**Trigger:** Any output that claims X based on source Y  
**Gate Type:** FAIL-CLOSED

**Rule:**
```
FOR each assertion A citing source S:
  IF A.claim ≠ S.content → FLAG as MISMATCH
  IF A.claim ⊃ S.content (overstates) → FLAG as OVERREACH  
  IF A.claim ⊂ S.content (understates) → ACCEPTABLE with [PARTIAL] tag
```

**Rationale:** The strongest analytical position is always "source says X, conclusion says Y, X≠Y" — not "this type of evidence can never prove Z."

**Anti-pattern (WEAK):**
> "Estimates cannot prove actual damages"

**Correct pattern (STRONG):**
> "The finding states 'paid $2,026' but the cited exhibit is labeled 'ESTIMATE' — the finding overstates the source"

---

### V-011: CATEGORICAL_CLAIM_AUDIT

**Scope:** Universal  
**Trigger:** Any source containing categorical/absolute assertions  
**Gate Type:** REQUIRED (audit) + ADVISORY (exploitation)

**Rule:**
```
EXTRACT all categorical language from source:
  - "no evidence", "never", "did not", "failed to", "none", "always"
  
FOR each categorical assertion C:
  - SEARCH available corpus for ANY counterexample
  - IF counterexample EXISTS → LEVERAGE_OPPORTUNITY: HIGH
  - IF counterexample NOT_FOUND → Log as UNCONTRADICTED
  - IF corpus incomplete → Log as UNVERIFIED_CATEGORICAL
```

**Rationale:** Binary claims are falsifiable with single counterexamples. Categorical overstatement is a common error in adversarial documents, court orders, and reports.

**Application Examples:**

| Domain | Categorical Claim | Counterexample Search |
|--------|-------------------|----------------------|
| Legal | "Defendant presented no evidence" | Scan record for any sworn filing |
| Medical | "No contraindications exist" | Search literature for case reports |
| Engineering | "System never fails under load X" | Check test logs for any failure |
| Financial | "No material changes occurred" | Audit filings for any disclosure |

---

### V-012: SOURCE_PRECISION_VALIDATION

**Scope:** Universal  
**Trigger:** Any citation to external source  
**Gate Type:** FAIL-CLOSED

**Rule:**
```
FOR each citation C:
  - VERIFY source exists
  - VERIFY pinpoint (page/section/timestamp) contains claimed content
  - VERIFY quote (if any) is verbatim
  - TOLERANCE: ZERO
  
FAILURE_MODES:
  - Source not found → [UNVERIFIED_SOURCE]
  - Pinpoint doesn't support claim → [CITATION_ERROR]
  - Quote not verbatim → [MISQUOTE]
```

**Domain Instantiations:**

| Domain | Citation Format | Verification Method |
|--------|-----------------|---------------------|
| Legal | R.####, Case Name, Vol. Rep. Page | Record PDF page, case database |
| Scientific | DOI, PMID, arXiv ID | Database lookup + section check |
| Code | File:Line, Commit SHA | Repository verification |
| News | URL + archive date | Wayback/archive check |

**Rationale:** Citation errors destroy credibility disproportionate to their size. One wrong page number colors interpretation of all other citations.

---

### V-013: COMPLETENESS_AUDIT

**Scope:** Universal  
**Trigger:** Any analytical task with defined input corpus  
**Gate Type:** REQUIRED

**Rule:**
```
BEFORE concluding analysis:
  - ENUMERATE all checkable elements in source corpus
  - FOR each element, LOG one of:
    - CHECKED: [finding]
    - CHECKED: no issue found
    - NOT_CHECKED: [reason]
  
IF NOT_CHECKED count > threshold → FLAG as INCOMPLETE_ANALYSIS
```

**Rationale:** The most valuable finding may be in the unchecked portion. Structured completeness tracking prevents "best argument not in brief" failures.

**Application Pattern:**
```
CORPUS: Court order (47 findings)
CHECKED: Findings 1-15, 18-30, 35-47
NOT_CHECKED: Findings 16-17, 31-34 (transcript-dependent)
FINDING: Issue at Finding #18 (categorical mismatch)
POTENTIAL_MISSED: Findings 16-17, 31-34 require transcript review
```

---

### V-014: MULTI_PASS_REVIEW_PROTOCOL

**Scope:** Universal (process)  
**Trigger:** Any output requiring validation before delivery  
**Gate Type:** PROCESS

**Architecture:**
```
PASS 1: INTEGRITY
  - Source verification
  - Citation accuracy
  - Formal compliance
  - Deterministic, automatable
  
PASS 2: SUBSTANCE  
  - Logical soundness
  - Doctrinal/domain accuracy
  - Framing strength
  - Requires domain expertise
  
PASS 3: STRATEGY
  - Risk assessment
  - Unintended consequences
  - Adversarial response prediction
  - Requires judgment + context
  
PASS 4: HUMAN JUDGMENT
  - Tone, audience calibration
  - Preserved discretionary calls
  - Final authority
```

**Rationale:** Different passes catch different errors. Single-pass review has systematic blind spots regardless of reviewer quality.

**Reviewer Allocation:**
| Pass | Optimal Reviewer | Fallback |
|------|------------------|----------|
| Integrity | Automated/LLM | Human spot-check |
| Substance | Domain LLM + authority table | Human expert |
| Strategy | Human + LLM scenario modeling | Human only |
| Judgment | Human only | — |

---

### V-015: ACTIONABLE_OUTPUT_SPECIFICITY

**Scope:** Universal  
**Trigger:** Any output containing recommendations, next steps, or remedies  
**Gate Type:** REQUIRED

**Rule:**
```
FOR each recommended action R:
  REQUIRED components:
    - ACTION: What specifically to do
    - SCOPE: What it applies to (and what it doesn't)
    - CONDITION: When/if to execute
    - OWNER: Who executes (default: Claude unless physical)
    - VERIFICATION: How to confirm completion
    
  FAILURE_MODES:
    - Missing ACTION → VAGUE_RECOMMENDATION
    - Missing SCOPE → UNBOUNDED_ACTION
    - Missing VERIFICATION → UNCONFIRMABLE
```

**Anti-patterns:**

| Vague (REJECT) | Specific (ACCEPT) |
|----------------|-------------------|
| "Review and update" | "Update §3.2 to reflect X; verify by re-running validator Y" |
| "Consider alternatives" | "Evaluate options A, B, C against criteria X, Y, Z; select by [method]" |
| "Reverse and remand" | "Vacate finding ¶18; remand for determination of X consistent with R.401" |

---

## UPDATED SECTION §9: REVIEW PROTOCOL

**Previous (v3.0):** Single-pass validation  
**Updated (v3.1):** Multi-pass with focal differentiation

```
REVIEW_SEQUENCE:
  
  [PASS 1: INTEGRITY] ──→ FAIL? → STOP, remediate
                              ↓ PASS
  [PASS 2: SUBSTANCE] ──→ FAIL? → FLAG, may proceed with caveats
                              ↓ PASS  
  [PASS 3: STRATEGY]  ──→ FAIL? → REFRAME, do not proceed as-is
                              ↓ PASS
  [PASS 4: JUDGMENT]  ──→ Human decision point
                              ↓ APPROVED
  [OUTPUT]
```

**Pass Independence:** Each pass uses different criteria. Passing Pass 1 does not imply Pass 2 will pass. All passes required for COURT_READY or equivalent high-stakes outputs.

---

## UPDATED SECTION §11: OUTPUT STANDARDS

**New Requirement — Remedy/Recommendation Specificity:**

All outputs containing action items must satisfy V-015 (ACTIONABLE_OUTPUT_SPECIFICITY).

**Granularity Default:** Maximum specificity unless explicitly constrained by:
- User request for summary
- Scope limitation in project instructions
- Saturation (confidence plateau reached)

**Verification Hooks:** Every action item should include verification method. "Trust but verify" is not verification — specify the check.

---

## INTERACTION WITH EXISTING VALIDATORS

| Existing Validator | Relationship to New |
|--------------------|---------------------|
| NO_DUMMY_CITATIONS | Subsumed by V-012 (SOURCE_PRECISION) |
| RECORD_PAGE_EXISTS | Domain instance of V-012 |
| CONFIDENCE_RATIO | Unchanged; V-011 may surface new [INFERENCE] items |
| FRAMING_CHECK (new) | Implemented via V-010 |

---

## IMPLEMENTATION PRIORITY

| Validator | Priority | Rationale |
|-----------|----------|-----------|
| V-012 (Source Precision) | P0 | Foundation for all citation-based work |
| V-010 (Assertion/Evidence) | P0 | Strongest analytical framing |
| V-011 (Categorical Audit) | P1 | High-leverage attack surface detection |
| V-013 (Completeness) | P1 | Prevents missed-issue failures |
| V-015 (Actionable Specificity) | P1 | Output quality enforcement |
| V-014 (Multi-Pass) | P2 | Process improvement, not gate |

---

## VERSION CONTROL

```
MU5KOSX v3.1 PATCH
SHA-256: [to be computed on freeze]
Status: CANDIDATE
Effective: Upon user confirmation
Backward Compatible: YES (additive only)
```

**Confirmation Required:** Reply "FREEZE v3.1" to activate, or provide modifications.
