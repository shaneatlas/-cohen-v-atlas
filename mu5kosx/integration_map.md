# INTEGRATION MAP: MU5KOSX v3.1 ↔ Legal-Skill Overlay v2

**Created:** 2025-12-27  
**Purpose:** Show layered architecture and inheritance

---

## ARCHITECTURE DIAGRAM

```
┌─────────────────────────────────────────────────────────────────┐
│                     MU5KOSX v3.1 CORE                           │
│                  (Universal Reasoning Layer)                     │
├─────────────────────────────────────────────────────────────────┤
│  V-010: Assertion/Evidence Mismatch    [GENERAL]                │
│  V-011: Categorical Claim Audit        [GENERAL]                │
│  V-012: Source Precision Validation    [GENERAL]                │
│  V-013: Completeness Audit             [GENERAL]                │
│  V-014: Multi-Pass Review Protocol     [PROCESS]                │
│  V-015: Actionable Output Specificity  [GENERAL]                │
└──────────────────────────┬──────────────────────────────────────┘
                           │
                           │ INHERITS + INSTANTIATES
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                 LEGAL-SKILL OVERLAY v2                          │
│              (Appellate Domain Specialization)                   │
├─────────────────────────────────────────────────────────────────┤
│  L-010: Finding/Record Mismatch        ← V-010 instantiation    │
│  L-011: Order Categorical Audit        ← V-011 instantiation    │
│  L-012: Legal Citation Precision       ← V-012 instantiation    │
│  L-013: Issue Completeness Audit       ← V-013 instantiation    │
│  L-015: Remedy Specificity             ← V-015 instantiation    │
│  ─────────────────────────────────────────────────────────────  │
│  L-020: Applegate Risk Gate            [LEGAL-ONLY, NEW]        │
│  L-021: Preservation Status            [LEGAL-ONLY, NEW]        │
│  L-022: Standard of Review Matching    [LEGAL-ONLY, NEW]        │
└─────────────────────────────────────────────────────────────────┘
```

---

## INHERITANCE TABLE

| General (MU5KOSX) | Legal Instantiation | What Changes |
|-------------------|---------------------|--------------|
| V-010: Assertion ↔ Evidence | L-010: Finding ↔ Record | Terminology, citation format, mismatch types |
| V-011: Categorical Audit | L-011: Order Finding Audit | Target = court orders, leverage = reversal |
| V-012: Source Precision | L-012: Legal Citation | R.####, case cites, rule subdivisions |
| V-013: Completeness | L-013: Issue Completeness | Order findings as checklist, brief coverage |
| V-014: Multi-Pass | Workflow integration | Pass content = integrity→doctrine→strategy |
| V-015: Actionable Output | L-015: Remedy Specificity | Action/Scope/Remand structure |

---

## LEGAL-ONLY VALIDATORS (No General Equivalent)

| Validator | Why Legal-Only |
|-----------|----------------|
| L-020: Applegate Risk | Doctrine specific to appellate record gaps |
| L-021: Preservation | Procedural requirement for appellate review |
| L-022: SOR Matching | Standard of review is appellate-specific concept |

**Generalization Notes:**
- Applegate could generalize to "missing evidence attribution" but the legal doctrine has specific contours
- Preservation maps loosely to "issue properly raised" but legal preservation rules are technical
- SOR has no direct general equivalent; closest is "deference level" in regulatory/administrative contexts

---

## VALIDATOR ACTIVATION BY MODE

| Mode | MU5KOSX Core | Legal-Skill Overlay |
|------|--------------|---------------------|
| Mode 1 (Planning) | V-013, V-015 | — |
| Mode 2 (Reasoning) | V-010, V-011, V-012 | L-010, L-011, L-012, L-020, L-021, L-022 |
| Mode 3 (Output) | V-014, V-015 | L-013, L-015 |
| COURT_READY | ALL (fail-closed) | ALL (fail-closed) |

---

## EXAMPLE: Same Principle, Different Domains

**Principle:** Categorical assertions are falsifiable with single counterexamples

| Domain | Categorical Claim | Counterexample Search | Validator |
|--------|-------------------|----------------------|-----------|
| **Legal** | "Defendant presented no evidence" | Record for any sworn filing | L-011 |
| **Medical** | "No adverse events reported" | FAERS/literature for any report | V-011 + Medical overlay |
| **Engineering** | "System never fails" | Test logs for any failure | V-011 + Engineering overlay |
| **Financial** | "No material changes" | SEC filings for any 8-K | V-011 + Financial overlay |

**Implementation:** V-011 provides the pattern; domain overlays provide the search targets and evidence standards.

---

## CROSS-DOMAIN TRANSFERABILITY

Lessons extracted from Cohen appellate review that apply to other domains:

| Legal Lesson | Generalized Principle | Other Domain Application |
|--------------|----------------------|--------------------------|
| Finding/record mismatch framing | Assertion/evidence framing | Medical: diagnosis vs. test results |
| Applegate (record gap risk) | Missing evidence attribution | Engineering: untested condition assumptions |
| Remedy specificity | Actionable output structure | Project management: task definition |
| Issue completeness | Attack surface enumeration | Security: vulnerability audit |
| Citation precision | Source verification | Research: reference accuracy |

---

## DEPLOYMENT CHECKLIST

**To activate MU5KOSX v3.1:**
1. ☐ User confirms "FREEZE v3.1"
2. ☐ Update MANIFEST.json with SHA-256
3. ☐ Add to MU5KOSX folder in repository

**To activate Legal-Skill Overlay v2:**
1. ☐ Confirm MU5KOSX v3.1 is active (dependency)
2. ☐ User confirms "FREEZE LEGAL-SKILL v2"
3. ☐ Update project instructions to reference overlay
4. ☐ Add to skills/user/legal-skill/ or project files

**To create new domain overlay:**
1. ☐ Identify which V-0XX validators need domain instantiation
2. ☐ Define domain-specific terminology mapping
3. ☐ Identify domain-only validators with no general equivalent
4. ☐ Define workflow integration sequence
5. ☐ Create output templates

---

## QUICK REFERENCE: When to Use What

```
QUESTION: "Is this a general reasoning issue or domain-specific?"

IF the validator applies regardless of domain → MU5KOSX Core
   Examples: Source must exist, citations must be accurate, 
             categorical claims are attackable

IF the validator requires domain knowledge → Domain Overlay
   Examples: What counts as "preservation" in law,
             what counts as "adverse event" in medicine,
             what counts as "material change" in finance

IF unsure → Check if the validator would make sense 
            to someone outside the domain
   YES → General
   NO → Domain-specific
```

---

**Files Created:**
1. `/home/claude/mu5kosx_v3_1_patch.md` — General reasoning enhancements
2. `/home/claude/legal_skill_overlay_v2.md` — Legal/appellate specialization  
3. `/home/claude/integration_map.md` — This file (architecture documentation)

**Awaiting:** Confirmation to freeze and deploy
