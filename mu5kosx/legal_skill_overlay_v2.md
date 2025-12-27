# LEGAL-SKILL OVERLAY v2 — Appellate Reasoning Specialization

**Overlay Date:** 2025-12-27  
**Base Requirement:** MU5KOSX v3.1+  
**Domain:** Appellate litigation (Florida emphasis, generalizable)  
**Origin:** Cohen v. Atlas review cycle — domain-specific learnings

---

## RELATIONSHIP TO MU5KOSX CORE

This overlay **instantiates** general MU5KOSX validators for legal/appellate context and **adds** legal-specific validators with no general equivalent.

| MU5KOSX Core Validator | Legal Instantiation |
|------------------------|---------------------|
| V-010 (Assertion/Evidence) | L-010: Finding/Record Mismatch |
| V-011 (Categorical Audit) | L-011: Order Categorical Finding Audit |
| V-012 (Source Precision) | L-012: Legal Citation Precision |
| V-013 (Completeness) | L-013: Issue Completeness Audit |
| V-015 (Actionable Output) | L-015: Remedy Specificity |
| *None (legal-specific)* | L-020: Applegate Risk Gate |
| *None (legal-specific)* | L-021: Preservation Status |
| *None (legal-specific)* | L-022: Standard of Review Matching |

---

## LEGAL-SPECIFIC VALIDATORS

### L-010: FINDING_RECORD_MISMATCH

**Instantiates:** MU5KOSX V-010  
**Trigger:** Any court order finding cited against record  
**Gate Type:** FAIL-CLOSED for COURT_READY

**Legal-Specific Rule:**
```
FOR each order finding F citing record location R:
  EXTRACT: What the order says happened/exists
  VERIFY: What R actually contains
  
  MISMATCH_TYPES:
    - OVERSTATEMENT: Order says "paid $X" but R shows "estimate"
    - CONTRADICTION: Order says "no notice" but R contains notice
    - OMISSION: Order ignores material evidence at R
    - MISCHARACTERIZATION: Order describes R inaccurately
    
  IF MISMATCH detected:
    - LOG as REVERSAL_LEVERAGE
    - CLASSIFY: facial error vs. requires transcript context
    - ASSIGN: de novo review (findings) vs. abuse of discretion
```

**Framing Guidance:**
```
WEAK: "This type of evidence cannot support this finding"
       (invites harmless error analysis, deference)
       
STRONG: "The order finds [X] at ¶[N]; the cited exhibit shows [Y]; 
         X and Y are inconsistent"
        (de novo reviewable, no deference to mismatch)
```

**Output Format:**
```
FINDING_RECORD_MISMATCH_REPORT:
  Order Location: R. [page], ¶[number]
  Order States: "[exact quote]"
  Cited Exhibit: R. [page]
  Exhibit Shows: "[description or quote]"
  Mismatch Type: [OVERSTATEMENT|CONTRADICTION|OMISSION|MISCHARACTERIZATION]
  Reversal Leverage: [HIGH|MEDIUM|LOW]
  Transcript Dependent: [YES|NO]
```

---

### L-011: ORDER_CATEGORICAL_FINDING_AUDIT

**Instantiates:** MU5KOSX V-011  
**Trigger:** Review of any court order for appeal  
**Gate Type:** REQUIRED

**Process:**
```
STEP 1: EXTRACT categorical language from order
  - "no evidence"
  - "failed to"
  - "did not"
  - "never"
  - "none"
  - "no [noun]"
  - "without [noun]"
  
STEP 2: FOR each categorical finding C:
  - IDENTIFY: What element does C address?
  - SEARCH: Record for ANY contradicting evidence
  - DOCUMENT: Contradiction location or "none found"
  
STEP 3: RANK contradictions by:
  - Strength (direct vs. inferential)
  - Reviewability (facial vs. transcript-dependent)
  - Remedy value (dispositive vs. limited)
```

**Example Application:**
```
ORDER CATEGORICAL FINDINGS:
  ¶18: "did not provide written notice" 
  ¶19: "presented no evidence"
  
AUDIT RESULTS:
  ¶18: CONTRADICTION at R.401 
       (Notice document titled "NOTICE OF INTENTION TO IMPOSE CLAIM")
       Strength: DIRECT (document exists with statutory citation)
       Reviewability: FACIAL (no transcript needed)
       Remedy: Reverse finding, remand for §83.49 analysis
       LEVERAGE: ⭐⭐⭐⭐⭐
       
  ¶19: CONTRADICTION at R.340, R.342-441
       (Sworn declaration + 100 pages exhibits)
       Strength: DIRECT (materials exist)
       Reviewability: PARTIAL (may need element-specific tie)
       Remedy: Remand for consideration of record evidence
       LEVERAGE: ⭐⭐⭐⭐
```

---

### L-012: LEGAL_CITATION_PRECISION

**Instantiates:** MU5KOSX V-012  
**Trigger:** Any legal citation in work product  
**Gate Type:** FAIL-CLOSED for COURT_READY

**Citation Types & Verification:**

| Type | Format | Verification Method |
|------|--------|---------------------|
| Record cite | R. [page] | PDF page extraction, content match |
| Case cite | Name, Vol. Rep. Page (Court Year) | Authority table or database |
| Statute cite | § [number] | Current statute text |
| Rule cite | Rule [number]([subdivision]) | Current rule text |
| Regulation cite | [Code] § [number] | Current regulation text |

**Subdivision Precision (CRITICAL):**
```
RULE: When citing rule for specific proposition, 
      cite the SUBDIVISION that contains the requirement.
      
WRONG: "Rule 1.510(a) requires reasons on the record"
       (if requirement is in different subdivision)
       
RIGHT: "Rule 1.510([correct subdivision]) requires reasons on the record"

VERIFICATION: 
  - Pull current rule text
  - Locate exact language supporting proposition
  - Cite that subdivision, not the rule generally
```

**Pinpoint Requirements:**
```
FOR case citations supporting specific proposition:
  - REQUIRED: pinpoint page where proposition appears
  - PROHIBITED: string citation without pinpoints for novel propositions
  - EXCEPTION: well-established standards (e.g., de novo review)
  
FOR record citations:
  - REQUIRED: page number
  - PREFERRED: page + paragraph/line for dense documents
  - VERIFICATION: extract text at pinpoint, confirm match
```

**Error Tolerance:** ZERO for COURT_READY outputs

---

### L-013: ISSUE_COMPLETENESS_AUDIT

**Instantiates:** MU5KOSX V-013  
**Trigger:** Appellate brief drafting or review  
**Gate Type:** REQUIRED

**Process:**
```
STEP 1: ENUMERATE all findings in order under review
  - Extract each numbered finding/conclusion
  - Extract each factual predicate
  - Extract each legal conclusion
  
STEP 2: FOR each finding, LOG:
  - CHECKED_NO_ISSUE: Record supports finding
  - CHECKED_ISSUE_FOUND: [describe mismatch/error]
  - NOT_CHECKED_REASON: [transcript needed | exhibit unavailable | etc.]
  
STEP 3: COMPARE brief issues to audit results
  - Are all ISSUE_FOUND items in brief? 
  - Are any brief issues NOT supported by audit?
  - Are NOT_CHECKED items potentially stronger than included issues?
  
STEP 4: FLAG
  - MISSED_STRONG_ISSUE: Audit found issue not in brief
  - WEAK_INCLUDED_ISSUE: Brief issue not supported by audit
  - INCOMPLETE_AUDIT: >20% findings unchecked
```

**"Hidden Argument" Prevention:**
```
The most common appellate drafting failure is:
  BRIEF DRAFTED FROM: "What went wrong at trial"
  SHOULD BE DRAFTED FROM: "What does the record prove is wrong with the order"
  
CORRECT SEQUENCE:
  1. Audit order findings against record
  2. Rank issues by: contradiction strength × reviewability × remedy value
  3. Draft issues in ranked order
  
WRONG SEQUENCE:
  1. List grievances from trial
  2. Find record support for grievances
  3. Draft issues based on grievance intensity
```

---

### L-015: REMEDY_SPECIFICITY

**Instantiates:** MU5KOSX V-015  
**Trigger:** Any appellate argument requesting relief  
**Gate Type:** REQUIRED

**Required Components:**
```
FOR each issue I requesting relief:
  
  ACTION: [Reverse | Vacate | Affirm in part | Remand | Certify]
  
  SCOPE: 
    - What is affected (entire judgment | specific count | specific finding)
    - What is NOT affected (if partial relief)
    
  REMAND_INSTRUCTION (if applicable):
    - What the trial court must do on remand
    - What standard to apply
    - What record evidence to consider
    
  LEGAL_BASIS:
    - Why this relief matches this error
    - Standard of review that permits this relief
```

**Remedy Matching:**
| Error Type | Appropriate Relief |
|------------|-------------------|
| Wrong legal standard (facial) | Vacate, remand for reconsideration under correct standard |
| Finding contradicted by record | Reverse finding, remand for determination consistent with record |
| Evidentiary error | Remand for new determination excluding/including evidence |
| Procedural error (preserved) | Reverse for new proceeding |
| Harmless error (if argued) | Affirm (or distinguish why not harmless) |

**Anti-patterns:**
```
VAGUE (REJECT):
  "Reverse and remand for further proceedings"
  
SPECIFIC (ACCEPT):
  "Vacate the summary judgment as to Count III; remand for 
   determination of §83.49 compliance consistent with R.401, 
   including analysis of statutory trigger date and notice content"
```

---

### L-020: APPLEGATE_RISK_GATE

**Legal-Specific:** No general equivalent  
**Trigger:** Any argument that depends on missing record materials  
**Gate Type:** REQUIRED (risk assessment)

**The Applegate Doctrine:**
```
Applegate v. Barnett Bank, 377 So. 2d 1150 (Fla. 1979):
  - Missing transcript → presumption trial court acted correctly
  - Appellant bears burden of providing record sufficient for review
  - Gaps in record are construed against appellant
```

**Risk Assessment:**
```
FOR each argument A:
  
  QUESTION 1: Does A require transcript/missing material?
    - NO → PROCEED (argument is record-based)
    - YES → Continue to Q2
    
  QUESTION 2: Why is material missing?
    - Appellant's fault (failed to order, stricken for noncompliance) → HIGH RISK
    - Court's fault (not transcribed, lost) → LOWER RISK
    - Appellee's fault (destroyed, withheld) → REFRAME as appellee misconduct
    
  QUESTION 3: Can argument be reframed as face-of-order/documentary?
    - YES → REFRAME and proceed
    - NO → ASSESS whether to include
    
  QUESTION 4: If included despite risk, what's adversary's Applegate response?
    - DRAFT preemptive counter-argument
    - OR drop issue if counter is weak
```

**Safe vs. Dangerous Arguments:**
| Record Status | SAFE | DANGEROUS |
|---------------|------|-----------|
| Transcript missing (your fault) | Face-of-order errors, documentary contradictions | "Court didn't state reasons orally" |
| Transcript missing (not your fault) | Above + reviewability/due process | — |
| Full record | All arguments | — |

**Cohen Example:**
```
ARGUMENT: "Order says 'reasons on record' but no reviewable reasons exist"
RISK: Transcripts stricken due to appellants' noncompliance
APPLEGATE RESPONSE: "Missing transcript → presume court stated adequate reasons"
ASSESSMENT: HIGH RISK — reframe or drop

REFRAME OPTION: 
  "The written order must supply reviewable reasons; 
   asking this Court to assume oral reasons existed 
   would be speculation prohibited by Applegate itself"
  
SAFER OPTION: Drop issue, focus on facial errors
```

---

### L-021: PRESERVATION_STATUS

**Legal-Specific:** No general equivalent  
**Trigger:** Any appellate issue  
**Gate Type:** REQUIRED

**Preservation Categories:**
```
[PRESERVED]: Issue raised below, ruling obtained, in record
[FUNDAMENTAL]: Reviewable despite no preservation (rare)
[STRUCTURAL]: Error affecting framework of trial (reviewable)
[UNPRESERVED]: Not raised below — likely waived
[UNCLEAR]: Preservation status requires transcript or further research
```

**Required Logging:**
```
FOR each issue I:
  - Preservation status: [category]
  - Preservation cite: R. [page] (motion/objection)
  - Ruling cite: R. [page] (court's ruling)
  - IF UNPRESERVED: Fundamental error argument available? [Y/N]
```

**Impact on Brief:**
- PRESERVED issues: Standard appellate argument
- FUNDAMENTAL/STRUCTURAL: Must argue why exception applies
- UNPRESERVED: Generally exclude unless fundamental error doctrine applies
- UNCLEAR: Flag for transcript review before including

---

### L-022: STANDARD_OF_REVIEW_MATCHING

**Legal-Specific:** No general equivalent  
**Trigger:** Each appellate issue  
**Gate Type:** REQUIRED

**Standard of Review Categories:**
| Standard | Deference | Appellant Burden | Best Issue Types |
|----------|-----------|------------------|------------------|
| De novo | None | Show error | Legal conclusions, SJ review |
| Abuse of discretion | High | Show unreasonable | Evidentiary rulings, sanctions |
| Clearly erroneous | Moderate | Show no support | Factual findings after trial |
| Per se reversal | None | Show error occurred | Structural errors, jurisdiction |

**Matching Rule:**
```
FOR each issue I:
  - IDENTIFY applicable standard
  - VERIFY argument is calibrated to that standard
  - IF mismatch: Reframe argument or reconsider issue
  
EXAMPLE MISMATCH:
  Issue: "Trial court abused discretion in granting summary judgment"
  Problem: SJ is reviewed de novo, not abuse of discretion
  Fix: "Summary judgment must be reversed because [de novo analysis]"
```

**Brief Structure Implication:**
```
RECOMMENDED: State standard of review for EACH issue, not just globally
REASON: DCAs process issues independently; SOR reminds panel of lens

STRUCTURE:
  ISSUE I: [Title]
  Standard of Review: [Standard] — [one-sentence basis]
  Argument: ...
```

---

## WORKFLOW INTEGRATION

**Appellate Brief Drafting Sequence:**
```
[1] ORDER AUDIT
    └── L-011: Extract and audit all categorical findings
    └── L-010: Check each finding against record
    
[2] ISSUE SELECTION  
    └── L-013: Completeness audit — all findings checked?
    └── L-020: Applegate risk assessment for each candidate
    └── L-021: Preservation status for each candidate
    └── RANK by: Contradiction strength × Reviewability × Remedy value
    
[3] BRIEF DRAFTING
    └── L-022: Match standard of review to each issue
    └── L-015: Specify remedy for each issue
    └── L-012: Verify all citations
    
[4] REVIEW (per MU5KOSX v3.1 multi-pass)
    └── Pass 1: Citation integrity (L-012)
    └── Pass 2: Doctrinal soundness (L-010, L-022)
    └── Pass 3: Strategic risk (L-020, L-021)
    └── Pass 4: Human judgment
```

**Appellate Brief Review Sequence:**
```
[1] CITATION VERIFICATION (L-012)
    └── All R. cites verified against record PDF
    └── All case cites verified against authority table
    └── All rule cites verified for correct subdivision
    
[2] ARGUMENT FRAMING CHECK (L-010)
    └── Each argument: assertion/evidence mismatch framing?
    └── Flag any "evidence can never prove X" patterns
    
[3] ISSUE COMPLETENESS (L-011, L-013)
    └── Are there order findings not addressed?
    └── Are any addressed issues unsupported?
    
[4] RISK ASSESSMENT (L-020)
    └── Applegate exposure for each argument
    └── Recommend reframe/drop for high-risk
    
[5] REMEDY AUDIT (L-015)
    └── Each issue: specific relief requested?
    └── Relief matches error type?
```

---

## OUTPUT TEMPLATES

### Issue Audit Report
```
ISSUE AUDIT: [Case Name], [Case No.]

ORDER REVIEWED: R. [pages]
RECORD AVAILABLE: [description of record status]
TRANSCRIPT STATUS: [available | missing-reason]

FINDINGS AUDIT:
| ¶ | Finding | Record Check | Result | Leverage |
|---|---------|--------------|--------|----------|
| 1 | [text]  | R. [page]    | [OK/MISMATCH] | [rating] |
...

ISSUES IDENTIFIED:
| Priority | Issue | Type | Record Anchor | SOR | Preservation | Applegate Risk |
|----------|-------|------|---------------|-----|--------------|----------------|
| 1 | ... | ... | R. [page] | De novo | PRESERVED | LOW |
...

MISSED ISSUE CHECK:
| Finding | Not in Brief Because | Recommendation |
|---------|---------------------|----------------|
...

RECOMMENDATION: [summary]
```

### Remedy Specification Template
```
ISSUE [N]: [Title]

ERROR: [One sentence description]
RECORD ANCHOR: R. [page] — "[key quote or description]"
ORDER CONFLICT: R. [page] ¶[N] — "[order's contrary statement]"

STANDARD OF REVIEW: [Standard]
BASIS: [Why this standard applies]

RELIEF REQUESTED:
  ACTION: [Reverse | Vacate | Remand]
  SCOPE: [What specifically]
  REMAND INSTRUCTION: [What trial court must do]
  
LEGAL BASIS: [Why this relief matches this error]
```

---

## VERSION CONTROL

```
LEGAL-SKILL OVERLAY v2
SHA-256: [to be computed on freeze]
Status: CANDIDATE
Base Requirement: MU5KOSX v3.1+
Domain: Appellate litigation
Effective: Upon user confirmation
```

**Confirmation Required:** Reply "FREEZE LEGAL-SKILL v2" to activate, or provide modifications.
