# RLHF Preference Annotation Guideline for Indonesian Prompts
**Version 1.0 | May 2026**  
**Author:** Calvin Tangka — AI Data Annotation Specialist | Bilingual RLHF Evaluator (EN–ID)  
**Contact:** fxcalvintangka@gmail.com

---

> **Key Principle:** You are not evaluating which response YOU personally prefer. You are evaluating which response a typical Indonesian-speaking user would find most helpful, accurate, and natural. Always annotate from the perspective of the intended end user.

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Task Types and Interface Overview](#2-task-types-and-interface-overview)
3. [Annotation Dimensions](#3-annotation-dimensions)
4. [Scoring Rubrics](#4-scoring-rubrics)
5. [Indonesian-Specific Annotation Guidelines](#5-indonesian-specific-annotation-guidelines)
6. [Edge Cases and Decision Framework](#6-edge-cases-and-decision-framework)
7. [Inter-Annotator Agreement (IAA)](#7-inter-annotator-agreement-iaa)
8. [Common Annotator Errors and How to Avoid Them](#8-common-annotator-errors-and-how-to-avoid-them)
9. [Sample Annotated Examples](#9-sample-annotated-examples)
10. [Annotator Workflow and Best Practices](#10-annotator-workflow-and-best-practices)
11. [Quick Reference Card](#11-quick-reference-card)
12. [Appendices](#12-appendices)

---

## 1. Introduction

This guideline establishes the annotation framework for conducting Reinforcement Learning from Human Feedback (RLHF) preference evaluations on AI-generated responses to Indonesian-language prompts. It is designed for annotators working on multilingual AI training datasets, particularly those evaluating Large Language Model (LLM) outputs in Bahasa Indonesia.

### 1.1 Purpose and Scope

The primary goal of this guideline is to ensure that annotators can consistently and accurately identify which AI-generated response best serves the needs of Indonesian-speaking users. Consistent annotation is critical because RLHF relies directly on human preference signals to train reward models — errors or inconsistencies in annotation propagate into model behavior at scale.

This guideline covers:
- Preference annotation tasks (A vs. B pairwise comparison)
- Single-response quality rating tasks
- Edge cases specific to Indonesian language and culture
- Inter-annotator agreement (IAA) procedures and calibration
- Escalation protocols for ambiguous or sensitive content

### 1.2 Who This Guide Is For

- Primary annotators performing pairwise preference labeling
- Quality review leads checking annotation consistency
- Onboarding trainers introducing new annotators to Indonesian RLHF tasks
- Project managers auditing dataset quality

### 1.3 Background: What Is RLHF?

Reinforcement Learning from Human Feedback (RLHF) is a technique used to align Large Language Models (LLMs) with human values and preferences. The process:

1. A base language model generates multiple responses to the same prompt.
2. Human annotators compare the responses and select the preferred one (or rate them on a scale).
3. These preference signals are used to train a reward model that learns to predict human preferences.
4. The base model is then fine-tuned using this reward signal via reinforcement learning.

**The quality of the final model is directly dependent on the quality and consistency of step 2. This guideline governs step 2 for Indonesian-language tasks.**

---

## 2. Task Types and Interface Overview

### 2.1 Pairwise Preference (A vs. B)

The most common task type. You will be shown a single user prompt alongside two AI-generated responses (Response A and Response B). Your job is to select the response that better serves the user.

**Task Interface — What You Will See:**
```
PROMPT:     [User's message in Indonesian or mixed language]
RESPONSE A: [First model output]
RESPONSE B: [Second model output]
YOUR TASK:  Select A, B, or Tie (with optional notes)
```

| Selection | When to Use |
|-----------|-------------|
| **Response A is Better** | A is clearly superior on the majority of quality dimensions |
| **Response B is Better** | B is clearly superior on the majority of quality dimensions |
| **Roughly Tied** | Both responses are similar in quality; neither meaningfully outperforms the other |
| **Both are Bad** | Neither response meets minimum quality standards — use sparingly, always add a note |

### 2.2 Single-Response Rating

In some tasks, you will rate a single response on a numeric scale (1–5 per dimension) rather than comparing two responses. See Section 4 for full rating rubrics.

### 2.3 Task Interface Notes

- Always read the full prompt before reading either response
- Read both responses fully before forming any judgment
- Complete responses fully before making your selection — avoid anchoring on the first few sentences
- Use the notes field whenever your selection was difficult or you selected Tie or Both Bad

---

## 3. Annotation Dimensions

When evaluating Indonesian-language responses, annotators assess performance across **five core dimensions**, listed in priority order. When dimensions conflict, higher-priority dimensions take precedence.

| Priority | Dimension | Weight |
|----------|-----------|--------|
| 1 | Accuracy & Factual Correctness | **Critical** |
| 2 | Instruction Following | High |
| 3 | Language Quality (Indonesian) | High |
| 4 | Helpfulness & Completeness | Medium |
| 5 | Format & Presentation | Low |

### 3.1 Accuracy & Factual Correctness

A response must not contain false information, even if it is beautifully written in perfect Bahasa Indonesia. **Factual errors are the most serious defect.**

- Verify factual claims against your general knowledge. If uncertain, flag for review rather than guessing.
- Pay special attention to: dates, statistics, names, laws, medical advice, and technical instructions.
- Hallucinated citations (fake sources, fake links) are treated as severe factual errors.
- Partially correct responses should be scored proportionally.

> **⚠️ Common Accuracy Pitfalls in Indonesian RLHF Tasks:**
> - Indonesian law and regulation references — models frequently cite outdated or non-existent regulations
> - Local geography — kabupaten/kota names, regional capitals, and administrative boundaries
> - Currency and economic figures — IDR amounts may be stale or fabricated
> - Adat (customary) practices — models may generalize practices from one ethnic group to all of Indonesia
> - Historical dates — particularly around independence, reformasi, and regional events

### 3.2 Instruction Following

The response must address what the user actually asked.

- **Explicit instructions:** Did the response fulfill all stated requirements? (e.g., "Tulis dalam 3 paragraf")
- **Implicit instructions:** Did the response understand the implied goal?
- **Scope:** Did the response stay focused on the topic?
- **Language instructions:** If the user wrote in Indonesian, the response should be in Indonesian unless translation was explicitly requested.

### 3.3 Language Quality (Indonesian)

This is the most distinctive dimension for Indonesian RLHF tasks. Language quality assessment requires genuine bilingual competence.

#### 3.3.1 Grammatical Correctness
- Standard Indonesian grammar (EYD / PUEBI spelling rules)
- Subject-verb-object structure and particle usage (di-, me-, -kan, -i)
- Proper use of conjunctions (karena, sehingga, meskipun, agar)
- Watch for: Literal translation from English producing unnatural Indonesian sentence structure

#### 3.3.2 Register Appropriateness

| Context | Expected Register |
|---------|------------------|
| Formal business or legal query | Bahasa Indonesia baku (formal standard Indonesian) |
| Casual conversational prompt | Bahasa Indonesia santai / Bahasa informal |
| Academic or technical question | Formal with domain-appropriate terminology |
| Emotional support request | Warm, informal, empathetic — not stiff formal language |
| Youth/pop culture question | Informal, may include accepted colloquialisms |

#### 3.3.3 Code-Switching Naturalness

Code-switching between Indonesian and English is common and natural in Indonesian digital communication. Evaluate whether code-switching is:
- **Natural** — consistent with how educated urban Indonesians would write
- **Appropriate** — not switching unnecessarily when a perfectly good Indonesian term exists
- **Consistent** — not switching in one paragraph but not another without reason

**✅ Good Code-Switching Example:**
```
Prompt: "Gimana cara ngatur work-life balance yang bagus?"

Good response: "Untuk menjaga work-life balance yang sehat, kamu perlu menetapkan 
batasan yang jelas antara jam kerja dan waktu pribadi. Setting up a clear routine 
dan mematikan notifikasi setelah jam kantor bisa sangat membantu."

Why this works: English terms (work-life balance, setting up, routine) are natural 
borrowings commonly used in Indonesian professional contexts.
```

**❌ Poor Code-Switching Example:**
```
Prompt: "Gimana cara ngatur work-life balance yang bagus?"

Poor response: "To maintain a good work-life balance, you should create boundaries 
between your work hours and personal time. Ini akan help you to feel more refreshed 
dan productive setiap hari."

Why this fails: Switches to English unnecessarily, inconsistent structure, mixes 
registers awkwardly.
```

#### 3.3.4 Vocabulary Appropriateness
- Is the vocabulary accessible for the apparent level of the user?
- Does the response use correct Indonesian technical terms where they exist?
- Are anglicisms avoided when a standard Indonesian equivalent is available and natural?
- Watch for: responses that use overly bureaucratic vocabulary for casual questions

### 3.4 Helpfulness & Completeness

- Does the response answer the question fully, or does it stop short?
- Does it anticipate likely follow-up needs without being unnecessarily verbose?
- Is the length appropriate to the complexity of the question?
- Does the response include actionable next steps where appropriate?

### 3.5 Format & Presentation

Format should serve the content, not the reverse.
- Is the use of bullet points, numbered lists, or headers appropriate?
- Does the response avoid excessive markdown formatting in conversational contexts?
- **Format errors are lower priority than accuracy or language errors. Never prefer a beautifully formatted incorrect response over a plainly written correct one.**

---

## 4. Scoring Rubrics

### 4.1 Pairwise Preference Rubric

Decision process for pairwise tasks:

1. Check for disqualifying errors in either response (severe factual errors, harmful content, complete instruction failure). If one response has a disqualifying error, prefer the other regardless of other factors.
2. Compare across the five dimensions, starting from Accuracy downward.
3. Identify which response wins on more high-priority dimensions.
4. Select the winning response. If genuinely equal, select Tie.

### 4.2 Single-Response Rating Scale (1–5 per dimension)

| Score | Label | Criteria |
|-------|-------|----------|
| **5** | Excellent | Fully meets or exceeds expectations with no meaningful flaws |
| **4** | Good | Mostly meets expectations with minor, non-impactful issues |
| **3** | Acceptable | Partially meets expectations; some noticeable issues affecting quality |
| **2** | Poor | Significant issues that meaningfully impact quality |
| **1** | Failing | Fails to meet minimum standard on this dimension |

### 4.3 Overall Composite Score (Weighted)

| Dimension | Weight |
|-----------|--------|
| Accuracy & Factual Correctness | 30% |
| Instruction Following | 25% |
| Language Quality (Indonesian) | 25% |
| Helpfulness & Completeness | 15% |
| Format & Presentation | 5% |

**Example:** Scores of 5, 4, 3, 4, 5 → Composite = (5×0.30) + (4×0.25) + (3×0.25) + (4×0.15) + (5×0.05) = **4.10 / 5.00**

---

## 5. Indonesian-Specific Annotation Guidelines

### 5.1 The Dialect and Register Spectrum

| Variety | Annotation Notes |
|---------|-----------------|
| **Bahasa Indonesia Baku** | Standard formal Indonesian per PUEBI rules. Expected in formal/academic/professional prompts. Responses using baku when user used informal = over-formal, penalize lightly. |
| **Bahasa Indonesia Sehari-hari** | Everyday Indonesian. Appropriate for most conversational prompts. Watch for gue/lo vs. saya/kamu register switching. |
| **Bahasa Gaul / Slang** | Youth slang (e.g., 'gabut', 'receh', 'bestie'). Acceptable only if user's prompt uses similar slang. |
| **Regional Influence** | Influences from Javanese, Sundanese, Batak, Minang, etc. Evaluate whether terms are used appropriately and understandable to a general Indonesian audience. |

### 5.2 Cultural Sensitivity and Local Context

Annotators should flag and downgrade responses that:
- Disrespect religious practices — Indonesia's six recognized religions deserve equal, respectful treatment
- Misrepresent adat or generalize regional practices to all Indonesians
- Use communication styles inappropriate for the hierarchical social context
- Apply Western individualistic frameworks to Indonesian community-oriented (gotong royong) contexts without acknowledgment
- Miss the nuance of *halus* (refined, indirect) communication in formal or hierarchical contexts

> **Cultural Nuance Example — Hierarchy in Communication:**  
> Prompt: *"Bagaimana cara saya menyampaikan ketidaksetujuan kepada atasan saya?"*  
> A poor response suggests blunt direct confrontation. A better response suggests indirect, face-saving strategies appropriate to Indonesian workplace hierarchy (menyampaikan secara pribadi, menggunakan bahasa yang sopan, framing as a question).  
> This is a **cultural appropriateness** issue, not a factual accuracy issue.

### 5.3 Religious and Sensitivity Considerations

- Responses that assume all Indonesian users are Muslim are inappropriate
- Religious advice (fatwa, hukum, ibadah) — flag for accuracy but do not evaluate theological correctness yourself
- Halal/haram references should be presented accurately, not assumed or invented
- Responses on sensitive religious topics should be neutral unless the user specifically requested a religious perspective

### 5.4 Evaluating Indonesian-English Mixed Prompts

- The response should match the dominant language and register of the prompt
- Technical English terms without common Indonesian equivalents may remain in English

| English Term | Preferred Indonesian (Formal Context) |
|-------------|--------------------------------------|
| download | unduh / unduhan |
| upload | unggah / unggahan |
| smartphone | ponsel pintar |
| meeting | rapat / pertemuan |
| password | kata sandi |
| email | surel (formal) / email (most contexts) |
| feedback | masukan / umpan balik |
| update | pembaruan / perbarui |

---

## 6. Edge Cases and Decision Framework

### 6.1 When Both Responses Are Clearly Bad

Selecting 'Both Bad' should be rare (target: **<5% of annotations**).

1. At least one critical dimension (Accuracy or Instruction Following) must score 1 or 2 on BOTH responses.
2. Always provide a written note explaining why both fail.
3. Do not select Both Bad simply because you could have written a better response.

### 6.2 Accuracy vs. Helpfulness Conflict

**Decision Tree:**
```
Q1: Is the inaccuracy a factual error (wrong information)?
  → YES: Prefer the accurate response. Factual errors cause real harm.
  → NO (incompleteness or imprecision): Continue to Q2.

Q2: Would the inaccuracy mislead the user or cause harm if acted upon?
  → YES: Prefer the accurate response.
  → NO: Consider the helpfulness advantage.

Q3: Is the difference in helpfulness substantial?
  → YES: Prefer the helpful response if the inaccuracy is minor and harmless.
  → NO: Prefer Tie or the accurate response.
```

### 6.3 Response Length Disagreements

| Scenario | Annotation Guidance |
|----------|-------------------|
| Short prompt, very long response | Penalize if response adds unnecessary padding or tangential content |
| Complex prompt, very short response | Penalize for incompleteness |
| Both accurate, one concise, one verbose | Prefer concise if the verbose one adds no meaningful value |
| One response longer AND more complete | Prefer the longer one — length is acceptable when justified by content |

### 6.4 Formal vs. Informal Indonesian

- If both responses are accurate and helpful, prefer the register-matched response
- If the formal response is substantially more accurate, prefer it despite register mismatch
- Never penalize formal Indonesian in responses to formal prompts

### 6.5 Sensitive Topics Requiring Escalation

Do not attempt to evaluate independently. Flag for escalation:
- Medical advice that could cause harm if followed incorrectly
- Legal advice about Indonesian law
- Content that may constitute hate speech against any ethnic, religious, or regional group
- Political content about current Indonesian political figures or parties
- Content involving minors in any sensitive context
- Responses that appear to endorse illegal activities under Indonesian law

**Escalation procedure:** Select your best answer, check the 'Needs Review' flag, and add a note. Do not leave the task blank.

---

## 7. Inter-Annotator Agreement (IAA)

### 7.1 IAA Measurement

For pairwise preference tasks: **Cohen's Kappa (κ)**  
For rating tasks: **Weighted Kappa or Intraclass Correlation Coefficient (ICC)**

| Cohen's Kappa (κ) | Interpretation |
|-------------------|----------------|
| < 0.40 | Poor — major calibration session required |
| 0.40–0.59 | Moderate — guideline clarification needed |
| 0.60–0.79 | **Good — acceptable for production** |
| ≥ 0.80 | **Excellent — highly reliable training signal** |

### 7.2 Calibration Procedure

At the start of each new annotation batch:

1. Complete 20 calibration examples independently without discussing with other annotators.
2. Submit answers to the calibration system.
3. Review the consensus answers and reasoning for each.
4. Discuss any differences — understand *why*, not just *what*.
5. **Pass threshold: ≥80% agreement with consensus answers.** Annotators below this must repeat calibration before accessing production tasks.

### 7.3 Ongoing Quality Monitoring

- 5% of production annotations are randomly selected for quality review each week
- Annotators whose agreement rate drops below 70% will be flagged for re-calibration
- Systematic disagreement patterns will be addressed in individual feedback sessions

### 7.4 Acceptable vs. Unacceptable Disagreement

| Type | Description |
|------|-------------|
| **Acceptable subjective disagreement** | Both annotators follow guidelines but weight dimensions slightly differently. Expected and acceptable. |
| **Guideline gap** | Case not covered by guidelines. Flag for guideline update. |
| **Inconsistent application** | Applying guidelines correctly in one case but not a similar one. Addressed via feedback. |
| **Systematic bias** | Consistently favoring a particular dimension, language, length, or style. Requires re-training. |

---

## 8. Common Annotator Errors and How to Avoid Them

### 8.1 Primacy Bias
Annotators tend to prefer whichever response they read first.

**How to avoid:**
- Read both responses fully before forming any judgment
- After reading both, write down strengths and weaknesses of each before selecting
- If you consistently select Response A, check your recent annotations for bias
- The platform randomizes A/B ordering — your ratio should be roughly 50/50

### 8.2 Verbose Response Bias
Longer responses feel more thorough, but length ≠ quality.

- Always ask: does the extra content actually help the user?
- Padding, repetition, and unnecessary caveats are **negative signals**, not neutral
- A 3-sentence accurate answer is often better than a 10-sentence padded one

### 8.3 Native Speaker Overcorrection
Bilingual annotators sometimes penalize responses for not matching their personal dialect.

- Evaluate against standard Indonesian (PUEBI), not personal dialect preference
- Regional expressions that are correct and natural should not be penalized
- When uncertain whether something is 'wrong' or just different: check KBBI before penalizing

### 8.4 Sycophancy Detection Failure

> **Sycophancy Red Flags:**
> - Excessive complimenting of the user's question before answering
> - Agreeing with factually incorrect premises in the user's prompt
> - Avoiding direct answers to avoid potential disagreement
> - Ending with empty affirmations ('Semoga berhasil! Pertanyaan yang sangat bagus!')
> - Providing praise disproportionate to what the user asked for

Sycophantic responses should be scored lower on Helpfulness even if grammatically correct and factually harmless.

### 8.5 Errors vs. Stylistic Choices

| Error (Penalize) | Stylistic Choice (Do Not Penalize) |
|-----------------|-----------------------------------|
| Using the wrong affix (me-/di-) changing meaning | Using a different but valid synonym |
| Factually incorrect statement | Taking a different but defensible perspective |
| Register mismatch (formal for casual) | Slightly different level of formality within the same register |
| Wrong grammar rule | Using an older but still valid grammatical form |

---

## 9. Sample Annotated Examples

### Example 1: Factual Accuracy Wins Over Language Quality

**PROMPT:** *"Kapan Indonesia memproklamasikan kemerdekaannya dan siapa yang membacakannya?"*

**Response A:**  
*"Proklamasi kemerdekaan Indonesia dibacakan pada tanggal 17 Agustus 1945 oleh Soekarno dan Hatta. Peristiwa bersejarah ini berlangsung di Jalan Pegangsaan Timur 56, Jakarta."*  
Language: Slightly formal but grammatically perfect. Clear and direct.

**Response B:**  
*"Kemerdekaan Indonesia diproklamasikan pada 17 Agustus 1945. Teks proklamasi dibacakan oleh Presiden Soekarno di halaman Istana Merdeka, Jakarta, disaksikan oleh ribuan rakyat Indonesia."*  
Language: Fluent, natural Indonesian. **BUT: Contains factual error** — the proclamation was read at Jalan Pegangsaan Timur 56, not Istana Merdeka.

**✅ CORRECT ANNOTATION: Response A is Better**  
*Reasoning: Despite Response B's more natural language flow, it contains a factual error about the location of the proclamation. Accuracy is the highest-priority dimension. Response A is factually correct and grammatically sound. Note: 'B contains location error.'*

---

### Example 2: Register Mismatch Penalization

**PROMPT:** *"Eh, lu tau ga sih cara bikin akun Google? Gue lupa caranya haha"*

**Response A:**  
*"Hai! Untuk membuat akun Google, silakan ikuti langkah-langkah berikut ini dengan seksama: Pertama, buka peramban web Anda dan kunjungi laman www.google.com. Kedua, klik tombol 'Masuk' yang terdapat di sudut kanan atas halaman..."*  
Language: Perfect formal Indonesian (baku). **BUT: User is clearly casual — register mismatch.**

**Response B:**  
*"Gampang banget kok! Buka google.com, terus klik 'Buat Akun' di pojok kanan atas. Isi nama, bikin username buat Gmail-nya, terus masukin password. Ikutin aja langkah-langkahnya sampe selesai, ga pake 5 menit!"*  
Language: Casual, natural, matches user's register perfectly. Correct and complete.

**✅ CORRECT ANNOTATION: Response B is Better**  
*Reasoning: Both responses are accurate and complete. Response B's register matches the user's casual, informal prompt. Response A feels cold and robotic for this context. Register matching is part of Language Quality and Helpfulness.*

---

## 10. Annotator Workflow and Best Practices

### 10.1 Pre-Annotation Preparation
- Read the full task brief for each new project
- Complete all calibration examples before starting production tasks
- Work in a quiet, focused environment
- Take a short break every 45–60 minutes to maintain quality

### 10.2 Per-Task Workflow

1. Read the full prompt. Identify: language, register, domain, explicit instructions.
2. Read Response A completely. Do not evaluate yet — just understand it.
3. Read Response B completely. Do not evaluate yet — just understand it.
4. Evaluate each response against the five dimensions, starting from Accuracy.
5. Identify which response wins on the highest-priority dimensions.
6. Make your selection. Add a note if the case was close or unusual.
7. Proceed to the next task. Trust your calibrated judgment.

### 10.3 Note-Writing Guidelines

Notes are strongly encouraged for: Tie selections, Both Bad selections, uncertain cases, and guideline gaps.

**Good notes are:**
- **Specific:** "Response A has wrong date for Sumpah Pemuda (1928, not 1927)" not "A has error"
- **Concise:** 1–3 sentences is sufficient in most cases
- **Objective:** Describe what you observed, not how you feel about it

### 10.4 Productivity Benchmarks (After Calibration)

| Task Type | Target Throughput |
|-----------|-----------------|
| Simple pairwise preference (short responses) | 15–20 tasks/hour |
| Complex pairwise preference (long technical responses) | 8–12 tasks/hour |
| Single-response rating (all 5 dimensions) | 10–15 tasks/hour |
| Flagged / escalation cases | No target — accuracy only |

> **Note:** These are targets, not quotas. Never rush at the expense of accuracy.

---

## 11. Quick Reference Card

### Five Dimensions — Priority Order

| Priority | Dimension |
|----------|-----------|
| **1 — Critical** | Accuracy & Factual Correctness |
| **2 — High** | Instruction Following |
| **3 — High** | Language Quality (Indonesian) |
| **4 — Medium** | Helpfulness & Completeness |
| **5 — Low** | Format & Presentation |

### Decision Quick Rules

- ✅ Factual error → penalize, even if language is perfect
- ✅ Register mismatch → penalize on Language Quality
- ✅ Longer ≠ better → evaluate content value, not word count
- ✅ Sycophantic ≠ helpful → penalize flattery without substance
- ✅ Dialect difference ≠ error → evaluate against PUEBI standard
- ✅ Code-switching OK if natural → penalize only if inappropriate or inconsistent
- ✅ Uncertain? → add a note and flag for review, do not guess

### Escalation Triggers

- Medical advice with potential for harm
- Legal advice about Indonesian law
- Content targeting any ethnic/religious/regional group
- Current Indonesian political content
- Any content involving minors

### IAA Targets

| Metric | Target |
|--------|--------|
| Cohen's Kappa (pairwise) | κ ≥ 0.60 (Good), κ ≥ 0.80 (Excellent) |
| Calibration pass rate | ≥ 80% agreement with consensus |
| Weekly quality review agreement | ≥ 70% with reviewers |
| Both Bad rate | < 5% of all annotations |

---

## 12. Appendices

### Appendix A: Glossary

| Term | Definition |
|------|------------|
| **RLHF** | Reinforcement Learning from Human Feedback — a technique for aligning AI models with human preferences using preference labels. |
| **Pairwise Preference** | A task where annotators compare two responses and select the better one. |
| **IAA** | Inter-Annotator Agreement — a measure of how consistently multiple annotators label the same data. |
| **Cohen's Kappa (κ)** | A statistical measure of annotator agreement that accounts for chance agreement. |
| **Bahasa Indonesia Baku** | Standard formal Indonesian language as defined by PUEBI spelling rules. |
| **Bahasa Gaul** | Indonesian youth slang and informal colloquial expressions. |
| **Code-switching** | The practice of alternating between two or more languages within a conversation. |
| **Register** | The level of formality in language use (formal, semi-formal, informal). |
| **PUEBI** | Pedoman Umum Ejaan Bahasa Indonesia — the official Indonesian spelling and grammar standard. |
| **KBBI** | Kamus Besar Bahasa Indonesia — the official Indonesian dictionary. |
| **Sycophancy** | AI behavior characterized by excessive flattery, agreement, or validation regardless of accuracy. |
| **Adat** | Traditional Indonesian customary laws and practices, varying by region and ethnic group. |
| **Halus** | A concept in Indonesian/Javanese culture referring to refined, indirect, face-saving communication. |

### Appendix B: Useful Resources

- **KBBI Online:** [kbbi.kemdikbud.go.id](https://kbbi.kemdikbud.go.id)
- **PUEBI (Official Spelling Guide):** [puebi.readthedocs.io](https://puebi.readthedocs.io)
- **EYD+ Revision Notes:** [badanbahasa.kemdikbud.go.id](https://badanbahasa.kemdikbud.go.id)
- **Ethnologue — Indonesian Language Data:** [ethnologue.com/language/ind](https://ethnologue.com/language/ind)

### Appendix C: Version History

| Version | Changes |
|---------|---------|
| **v1.0 (May 2026)** | Initial release. Covers pairwise and single-response rating tasks for Indonesian RLHF. Includes IAA procedures, 5 annotation dimensions, 8 common error types, and 2 annotated examples. |

---

*End of Document — RLHF Preference Annotation Guideline for Indonesian Prompts v1.0*  
*Prepared by Calvin Tangka | fxcalvintangka@gmail.com | May 2026*
