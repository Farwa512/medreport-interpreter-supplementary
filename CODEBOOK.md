# Codebook — MedReport Interpreter Maintainability Survey

## Study Reference
**Paper:** Evaluating Maintainability of AI-Powered Healthcare Applications Through Object-Oriented Design Metrics: A Case Study of MedReport Interpreter  
**Author:** Umm-e-Farwa, Department of Computer Science, University of Engineering and Technology, Lahore, Pakistan  

---

## Files Description

| File | Panel | Rows | Description |
|------|-------|------|-------------|
| `SE_panel_raw.csv` | Software Engineers & Researchers | 60 | Main expert panel (scenarios C0–C57 in paper + 2 incomplete removed at analysis stage) |
| `Clinician_panel_raw.csv` | Healthcare Professionals | 21 | Clinician sub-panel used for Mann–Whitney U comparison |
| `combined_all_panels.csv` | Both | 81 | SE + Clinician combined, panel column distinguishes groups |

---

## Column Definitions

### Demographic Columns

| Column | Description | Values |
|--------|-------------|--------|
| `timestamp` | Survey submission time (converted from Excel serial) | YYYY-MM-DD HH:MM:SS |
| `role` | Respondent's current professional role | Free text (see categories below) |
| `oop_experience` | Years working with OOP/UML design | `Less than 1 year`, `1–3 years`, `4–7 years`, `8+ years` |
| `ai_quality_familiarity` | Self-reported familiarity with AI system quality | `None — first time`, `Basic awareness`, `Working knowledge`, `Expert level` |
| `healthcare_exposure` | Exposure to healthcare/medical software systems | `None`, `Academic / coursework only`, `Some industry exposure`, `Significant experience` |
| `panel` | Which panel the respondent belongs to | `SE` (Software Engineer panel), `Clinician` (Healthcare sub-panel) |

---

### Quantitative Questions (Likert Scale 1–5)

All quantitative questions use a **1–5 Likert scale**:
- `1` = Strongly Disagree
- `2` = Disagree  
- `3` = Neutral
- `4` = Agree
- `5` = Strongly Agree

| Column | Module | Characteristic | Full Question Text |
|--------|--------|---------------|-------------------|
| `Q1` | M5 | Retrainability | The separation of ModelManager and MonitoringService supports safe, independent retraining. |
| `Q2` | M5 | Retrainability | The rollback(version) method is sufficient to recover from failed deployment. |
| `Q3` | M5 | Monitorability | MonitoringService tracking methods provide adequate coverage to detect degradation. |
| `Q4` | M5 | Monitorability | The alertDegradation() method triggering ModelManager is a clean design. |
| `Q6` | M4 | Reproducibility | AuditLogger captures sufficient info to reconstruct past interpretation sessions. |
| `Q7` | M4 | Reproducibility | Storing fileHash is necessary for verifying input integrity. |
| `Q8` | M4 | Reproducibility | AuditLogger design as a separate class is correct for independent audit concerns. |
| `Q10` | M2 | Explainability | explainAbnormality() is correctly placed and has the right parameters. |
| `Q11` | M2 | Explainability | RAGEngine.groundResponse() meaningfully supports explainability. |
| `Q12` | M2 | Explainability | Using subclasses (Layman/Clinical) is correct for different audiences. |
| `Q13` | M2 | Reproducibility | sessionId/modelVersion + AuditLogger are sufficient for reproduction. |
| `Q15` | M1/M3 | Monitorability | OCR confidence attribute is a meaningful signal for quality degradation. |
| `Q16` | M1/M3 | Reproducibility | Redundant fileHash storage is justified for verification. |
| `Q17` | M1/M3 | Explainability | Trend classification labels (Stable/Recovering) contribute to explainability. |
| `Q18` | M1/M3 | Retrainability | Independent design of HistoricalAnalyzer from the AI model is correct. |
| `Q19` | Overall | Retrainability | Retrainability: Supports model retraining as a routine, low-risk operation. |
| `Q20` | Overall | Reproducibility | Reproducibility: Traces every output via AuditLogger and fileHash. |
| `Q21` | Overall | Explainability | Explainability: Delivers appropriate explainability for medical AI. |
| `Q22` | Overall | Monitorability | Monitorability: Design is sufficient to prevent silent AI degradation. |

---

### Categorical Questions

| Column | Full Question | Values |
|--------|--------------|--------|
| `Q23` | Which characteristic is LEAST well-supported by the current class design? | `Retrainability`, `Reproducibility`, `Explainability`, `Monitorability` |
| `Q24` | Would you use this class diagram as a reference OOP design for building a similar system? | `Yes, as-is`, `Yes, with modifications`, `No` |

---

### Open-Ended Questions (Free Text)

| Column | Full Question |
|--------|--------------|
| `Q5_open` | What additional attribute or method, if any, would strengthen M5's retrainability or monitorability? |
| `Q9_open` | Is there any data that AuditLogger is currently NOT storing that would be critical? |
| `Q14_open` | Is there an explainability concern in this module that the design does NOT address? |
| `Q25_open` | Suggest one specific change to the class diagram that would most improve the system. |

---

## Proxy Score Computation (from paper)

These scores are computed from the Likert items above, normalised to 0–1 via `s = (x−1)/4`:

| Characteristic | Formula | Items Used |
|----------------|---------|-----------|
| Retrainability | mean(Q1, Q2) | Q1, Q2 |
| Reproducibility | mean(Q6, Q7, Q16, Q8, Q13) | Q6, Q7, Q8, Q13, Q16 |
| Explainability | mean(Q10, Q11, Q12, Q17, EF) | Q10, Q11, Q12, Q17 + EF proxy |
| Monitorability | mean(Q3, Q4, Q15) | Q3, Q4, Q15 |

> **Note:** EF (Explanation Fidelity) is a binary design proxy assessed from `ExplainableOutput.getFeatureAttributions()` — see paper Section III-C-3 for details.

---

## Missing Data

- Rows with no `role` value were dropped before analysis (1 incomplete SE response removed).
- Blank open-ended responses (`Q5_open`, `Q9_open`, `Q14_open`, `Q25_open`) indicate the respondent left that field empty — treated as missing, not as a negative response.
- The Clinician sub-panel was collected via a **separate form** and is **not** included in the n=58 regression analysis in the paper; it is used only for the Mann–Whitney U test (Section IV-J).
