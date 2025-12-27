# Session Changelog: 2025-12-27

## Session: Entity-Relation Graph Extraction & CUARC Validation

### Files Created This Session

| File | Size | Description |
|------|------|-------------|
| `ENTITY_RELATIONS_ROA_SPINE.json` | 15 KB | 47 entities: parties, judges, attorneys, docket items, evidence, hearings, timeline, jurisdictional facts |
| `ENTITY_RELATIONS_DEFENDANTS_REPLY.json` | 9.6 KB | 26 entities: 12 motion assertions, 3 evidence refs, 3 authorities, 4 spine links, 4 CUARC quotes |
| `ENTITY_RELATIONS_SFJ_COURT_FINDINGS.json` | 15 KB | 32 entities: 9 FOF, 3 COL, 7 AD rejections, 4 orders, 5 authorities, 1 contradiction |
| `ENTITY_RELATION_GRAPH_SUMMARY.md` | 6.5 KB | Architecture documentation for three-layer graph |
| `Cohen_CUARC_Entity_Map_v4_merged.xlsx` | 31 KB | Merged workbook: 98 entities, 57 edges across all layers |
| `REPLY_BRIEF_CUARC_PASS.md` | 4.3 KB | CUARC validation pass for I-DAMAGES and I-SPOLIATION |
| `REPLY_BRIEF_DRAFT_SAFE_SECTIONS.md` | 6.5 KB | Draft-safe Reply Brief sections (DAMAGES + SPOLIATION) |

### Key Discoveries

1. **CONTRADICTION-SECDEP**: Court's FOF-18 states "Defendants did not provide written notice" but E-SEC-01 (Notice dated 01/06/2018) exists in record — HIGH IMPACT reversal ground

2. **Miami Mold Payment Conflict**: Same document contains both "no proof of payment exists" AND payment entry showing $6,500 cash — use as credibility/dispute argument, not categorical assertion

3. **Evidence Reference Demotion**: Police Report #2019-00042552 referenced in motion but NOT ingested — flagged as EVIDENCE_REFERENCE with REQUIRES_INGESTION

### Schema Principles Implemented

- Verbatim anchor vs Summary split (prevents quote drift)
- Evidence Item vs Evidence Reference demotion (prevents phantom exhibits)
- Provenance typing: ROA_SPINE | COURT_FINDING | MOTION_ASSERTION | EVIDENCE_ITEM | EVIDENCE_REFERENCE
- Contradictions as first-class entities with Impact/Resolution_Path
- Authority hygiene: Citation_Verified vs Holding_Text_Verified

### Recommended Commit Message

```
feat(cuarc): entity-relation graph extraction + v4 merged workbook

- Extract 105 entities across ROA spine, Defendants' Reply, SFJ
- Implement CUARC schema: verbatim anchors, provenance typing, evidence demotion
- Discover SECDEP contradiction (FOF-18 vs E-SEC-01) - HIGH IMPACT
- Merge to Cohen_CUARC_Entity_Map_v4 (98 entities, 57 edges)
- Add draft-safe Reply Brief sections for I-DAMAGES, I-SPOLIATION
```

### Next Actions (P1)

1. Ingest Police Report #2019-00042552 → upgrade from EVIDENCE_REFERENCE to EVIDENCE_ITEM
2. Pull clean security deposit notice from PDF for I-SECDEP argument
3. Add Valcin proposition text from approved source
