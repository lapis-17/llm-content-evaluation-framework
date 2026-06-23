# LLM Content Evaluation Framework

## Dataset Preview

![Dataset Preview](dataset_preview.jpg)

## Overview

This project is a structured evaluation framework for assessing AI-generated YouTube and short-form video content. It contains 30 hand-evaluated examples across four content categories, with per-example scores, written feedback, and a full analysis report covering output quality, hallucination patterns, and instruction-following behavior.

The framework was built to replicate the kind of human feedback workflow used in AI training pipelines: reviewing LLM outputs against defined criteria, scoring them across multiple dimensions, and producing written justifications that a model trainer or content team could act on directly.

---

## Objectives

- Build a reproducible scoring system for evaluating AI-generated content
- Identify common failure modes in LLM outputs across different content types
- Demonstrate domain-aware evaluation that goes beyond generic quality metrics
- Produce a dataset structured for use in AI training, fine-tuning, or RLHF workflows
- Show practical prompt engineering principles derived from real output analysis

---

## Dataset Statistics

| Metric | Value |
|---|---|
| Total Examples | 30 |
| Hook Evaluation | 10 |
| Script Quality Evaluation | 10 |
| Fact Accuracy Evaluation | 5 |
| Instruction Following Evaluation | 5 |
| Scoring Dimensions | 4 per example |
| Maximum Score per Example | 40 |
| Score Range (dataset) | 26 to 38 |
| Average Total Score | 34.3 |
| Pass | 12 (40%) |
| Conditional Pass | 15 (50%) |
| Fail | 3 (10%) |
| Hallucinations Detected | 4 (13.3%) |
| High Confidence Evaluations | 26 (86.7%) |
| Medium Confidence Evaluations | 4 (13.3%) |
| Files Provided | CSV, JSON, XLSX, Markdown |

---

## Evaluation Methodology

### Scoring Process

Each output is evaluated independently against its prompt before scores are assigned. The evaluator reads the prompt, reviews the output, identifies what the model did well and where it fell short, and then assigns scores per dimension. Scores are not adjusted after writing the justification; the written analysis and the scores are produced together to avoid post-hoc rationalization.

All scores use a 1-10 scale per dimension. Total scores are the sum of all four dimensions, giving a maximum of 40. No weighting is applied between dimensions unless the prompt specifies a category where one dimension is the primary concern (for example, instruction-following examples weight that dimension most heavily in the written analysis).

### Evaluation Criteria

**Clarity:** How easily a viewer or reader can understand the output. Covers sentence structure, word choice, and logical ordering of information.

**Engagement:** How well the output holds attention. For hooks this means retention and click-through appeal. For scripts it means pacing, story arc, and ending quality.

**Accuracy:** Whether factual claims are grounded and verifiable. Penalizes hallucinated figures, incorrect data, and misleading framing even when the writing is otherwise strong.

**Instruction Following:** Whether the model met the specific requirements in the prompt, including word limits, tone, format, perspective, and audience constraints.

### Human Preference Ranking

Each example receives a final output quality verdict: Pass, Conditional Pass, or Fail.

**Pass** means the output is usable as-is or requires only minor optional tweaks. It meets all core requirements and performs well on the dimensions most relevant to its category.

**Conditional Pass** means the output has real value but requires at least one specific fix before it can be used. The improvement_suggestions field identifies exactly what needs to change.

**Fail** means the output has a disqualifying problem: a significant factual error, a missed hard instruction requirement, or a structural failure that cannot be fixed with minor edits.

### Error Classification

Every output is assigned a single error type reflecting its primary failure mode. Outputs with no identifiable error are labeled "None." When an output has multiple issues, the most impactful one is selected as the primary error type.

---

## Error Taxonomy

**Hallucination:** The output states a specific fact, figure, or claim that is either unverifiable or demonstrably incorrect. This covers invented statistics, wrong numerical figures (production counts, measurements, dates), and fabricated attributions. Example: FA-003 stated 350 F-22 aircraft were built when the actual number was 187.

**Instruction Failure:** The output did not comply with a specific, explicit instruction in the prompt. This includes exceeding word limits, using the wrong grammatical person, ignoring format requirements, or misreading tone specifications. Example: IF-001 was asked for under 20 words and produced 27.

**Reasoning Error:** The output draws a conclusion that does not follow from the information presented, misrepresents cause and effect, or applies flawed logic to a claim. No examples of this type appeared in the current dataset, which reflects the relatively straightforward nature of the prompts used.

**Formatting Error:** The output uses incorrect structure, layout, or presentation relative to what was requested. This is distinct from Instruction Failure in that it applies to structural choices (paragraph breaks, list formatting, header use) rather than content-level requirements.

**Missing Context:** The output is factually defensible but omits information that is material to an accurate understanding of the claim. This often appears as presenting a single figure from a documented range without acknowledging the range, or stating a claim without its necessary qualifier.

**Incomplete Answer:** The output addresses the prompt but stops short of delivering on its full potential or stated requirements. The most common form in this dataset is a hook or script that sets up tension but does not resolve it with a specific, concrete detail.

---

## Benchmark Statistics

### Overall Performance

| Metric | All Outputs | Pass | Conditional Pass | Fail |
|---|---|---|---|---|
| Count | 30 | 12 | 15 | 3 |
| Average Score | 34.3 | 36.8 | 33.1 | 27.0 |
| Median Score | 35.5 | 36.5 | 34.0 | 27.0 |
| Highest Score | 38 | 38 | 36 | 28 |
| Lowest Score | 26 | 33 | 26 | 26 |

### Category Performance

| Category | Count | Avg Score | Pass Rate |
|---|---|---|---|
| Hook Evaluation | 10 | 32.8 | 30% |
| Script Quality Evaluation | 10 | 35.7 | 60% |
| Fact Accuracy Evaluation | 5 | 33.8 | 40% |
| Instruction Following Evaluation | 5 | 35.0 | 60% |

### Error Distribution

| Error Type | Count | % of Dataset |
|---|---|---|
| None | 14 | 46.7% |
| Missing Context | 8 | 26.7% |
| Hallucination | 4 | 13.3% |
| Instruction Failure | 2 | 6.7% |
| Incomplete Answer | 2 | 6.7% |
| Reasoning Error | 0 | 0% |
| Formatting Error | 0 | 0% |

### Confidence Distribution

| Confidence Level | Count | % of Dataset |
|---|---|---|
| High | 26 | 86.7% |
| Medium | 4 | 13.3% |
| Low | 0 | 0% |

---

## Scoring Rubric

| Score | Meaning |
|---|---|
| 9-10 | Excellent. Meets or exceeds the standard for this dimension. Minor tweaks only. |
| 7-8 | Good. Works for its purpose but has one identifiable weakness worth addressing. |
| 5-6 | Average. Functional but requires meaningful revision before use. |
| 3-4 | Below average. A significant problem on this dimension that affects usability. |
| 1-2 | Poor. Fails this dimension in a way that makes the output unusable as-is. |

Scores are assigned per dimension and summed. Total scores out of 40. No artificial normalization applied.

---

## Category Breakdown

### Hook Evaluation (10 examples)

Evaluates AI-generated YouTube hooks across five topic types: aviation incidents, military history, disaster events, mystery topics, and factual reveals. Assessment focuses on curiosity creation, specificity, retention potential, and click-through appeal.

Key finding: Outputs that included a concrete, verifiable detail (a specific number, date, or named person) consistently outperformed outputs that relied on vague dramatic framing. Hook evaluation had the lowest pass rate (30%) and widest score spread of any category.

### Script Quality Evaluation (10 examples)

Evaluates AI-generated 60-second Shorts-style scripts on pacing, hook quality, factual grounding, story arc, and ending strength. Topics span aviation history, military biography, technology, nature, and business history.

Key finding: The most common structural problem was weak endings that restated the script's content rather than introducing a final reframing or unexpected detail. Scripts that let factual events carry the narrative (SQ-001, SQ-005) scored highest without needing editorial embellishment.

### Fact Accuracy Evaluation (5 examples)

Tests LLM outputs against verifiable historical and technical facts. Includes examples with high accuracy, minor precision issues, and one significant factual error (F-22 production figures). Topics include aviation, military hardware, and documented historical events.

Key finding: Models perform reliably at the broad-fact level but generate errors on specific numerical claims, particularly production counts and technical measurements for military systems. The F-22 example (FA-003) is a clear hallucination on a specific numerical figure, not a rounding issue.

### Instruction Following Evaluation (5 examples)

Tests whether outputs comply with explicit prompt constraints: word limits, perspective requirements (second-person only), format requirements (question-first), tone requirements, and audience specifications.

Key finding: Models handle qualitative instructions (tone, perspective) more reliably than quantitative ones (word limits). Both failures in this category involved word count violations, not tone or format failures.

---

## Key Findings

1. **Specificity is the strongest quality driver.** Across all categories, outputs built around a concrete detail consistently outperformed outputs built around dramatic framing.

2. **Word limits are the most commonly violated constraint.** Two of five instruction-following examples exceeded the stated limit. Prompts with hard numerical constraints require explicit enforcement.

3. **Factual accuracy degrades at the detail level.** Broad historical facts are generally reliable. Specific figures, production numbers, and secondary details require independent verification regardless of how confidently the model presents them.

4. **Endings are consistently the weakest element.** Most scripts and hooks performed better on setup than on payoff. Generic closing lines that summarize rather than reveal are the default pattern when the model is not specifically directed otherwise.

5. **Hallucinations are more common on numerical claims than on narrative ones.** The model can describe the Alcatraz escape accurately at the event level while getting a crew count wrong. Evaluation frameworks should flag any specific number for verification.

6. **Missing context is the most common error type (26.7%).** Outputs frequently omit the qualifier that would make a claim fully defensible: a range instead of a point estimate, an attribution note for eyewitness-based claims, or a clarification of what a superlative ("fastest," "deadliest") actually means in context.

---

## Applications in AI Training

This dataset and framework are directly applicable to several AI training contexts:

**RLHF and preference labeling:** Each example includes a scored comparison between what was requested and what was produced, along with written justification. The output_quality_verdict field provides a three-tier preference signal (Pass / Conditional Pass / Fail) that maps directly to preference pair construction.

**Fine-tuning data quality review:** The fact accuracy examples demonstrate the process of identifying and flagging outputs that contain plausible but incorrect claims, a core step in curating clean training data. The hallucination_detected field makes these examples filterable.

**Instruction-following benchmarks:** The five instruction-following examples with explicit pass/fail assessments on specific constraint types can be adapted as evaluation benchmarks for measuring a model's instruction compliance on structured prompts.

**Prompt engineering iteration:** The improvement suggestions throughout the dataset function as before/after prompt refinement examples, showing how changing a single element of a prompt or output changes the result.

**Error taxonomy training:** The error_type field provides labeled examples of six failure mode categories. Outputs with error_type "None" can serve as positive examples. This structure is compatible with classification model training for automated error detection.

---

## Future Improvements

**Expand the dataset to 100+ examples.** Thirty examples is enough to demonstrate the evaluation methodology and identify patterns, but not enough to produce statistically reliable findings across all category combinations. A larger dataset would allow meaningful sub-category analysis.

**Add a second evaluator for inter-rater reliability scoring.** The current dataset reflects a single evaluator's judgments. Adding a second independent rater and computing Cohen's Kappa would make the confidence scores more defensible and align the methodology more closely with professional annotation standards.

**Include multi-model comparison entries.** The current dataset evaluates outputs without specifying which model produced them. Adding entries where the same prompt was run through multiple models rated side-by-side would add a preference comparison dimension useful for RLHF workflows.

**Add a revised output column.** Several entries include specific improvement suggestions. Showing what the improved version looks like would make the dataset more useful as a before/after training resource.

**Expand into additional content categories.** Adding categories like email drafting, product descriptions, or factual Q&A would broaden the framework's applicability to other AI training domains.

**Automate word count verification.** Both instruction-following failures in this dataset were word-limit violations. A simple automated pre-check that flags outputs exceeding stated limits before manual review would improve pipeline efficiency.

---

## Skills Demonstrated

- LLM output evaluation and structured scoring
- Human feedback workflow design (RLHF-adjacent)
- Factual accuracy assessment and hallucination detection
- Instruction-following analysis across multiple constraint types
- Error taxonomy design and application
- Domain-specific content quality judgment (YouTube, short-form video)
- Prompt engineering principles derived from evaluation findings
- Dataset design and documentation for AI training use cases
- JSON, CSV, and XLSX data structuring for machine-readable output
- Statistical analysis of evaluation results

---

## Repository Structure

```text
llm-content-evaluation-framework/
├── README.md
├── analysis_report.md
├── content_evaluation_dataset.csv
├── content_evaluation_dataset.json
├── content_evaluation_dataset.xlsx
└── dataset_preview.jpg

**README.md** - Project overview, methodology, findings, and documentation
**analysis_report.md** - Full written analysis covering all evaluation categories and cross-category findings
**content_evaluation_dataset.csv** - Structured dataset containing all 30 evaluated examples
**content_evaluation_dataset.json** - Machine-readable version of the evaluation dataset
**content_evaluation_dataset.xlsx** - Dataset with statistics and summary sheets
**dataset_preview.jpg** - Visual preview of the evaluation dataset

---

## Resume Description

**LLM Content Evaluation Framework | Personal Project**

Designed a 30-example benchmark for evaluating AI-generated content across hook quality, script effectiveness, factual accuracy, and instruction following. Developed structured scoring rubrics, confidence ratings, hallucination detection criteria, and error taxonomies to assess output quality. Produced machine-readable datasets in CSV, JSON, and XLSX formats and authored a detailed analysis report identifying common failure modes, content weaknesses, and quality improvement opportunities relevant to AI training and evaluation workflows.
