# AI Proposal Triage Design

## 1. AI vs. rules
Use ordinary code for file validation, category lookup, retrieving the admin's current criteria, mandatory pass/fail checks, score calculation, sorting, and JSON output. Use AI to extract evidence from proposal text into a fixed schema and to draft concise evidence-based explanations. AI must not invent missing facts or decide that a mandatory requirement passed without evidence. Keep criteria and weights in admin-managed data, not hard-coded in the application.

## 2. Consistency and explainability
Store the extracted facts, source snippets/page references, criteria version, scoring rubric version, model/prompt version, and final calculated score. Use deterministic scoring code and fixed rounding rules. For the same extracted facts and criteria version, the numerical result and ordering should be the same; AI extraction can vary, so use structured output, validation, and human confirmation for uncertain fields. Show each criterion's weight, evidence found, score, contribution to the total, missing information, and the reason for the final status.

## 3. Human in the loop
A reviewer confirms the category and extracted facts, resolves low-confidence or conflicting evidence, reviews flagged proposals, and makes the final award decision. A reviewer may override a score or status only with a reason. Log uploader/reviewer identity, timestamps, file hash, extracted fields and evidence, criteria version, model and prompt versions, calculated scores, warnings, overrides and reasons, and the final human decision.

## 4. Changing criteria
Version every criteria set. A scoring run keeps a snapshot of the exact criteria and weights used, so a later admin edit does not silently change historical results. If the admin wants to compare proposals under new criteria, create a new scoring run for all proposals using the new version and clearly label the new ranking. Validate that weighted criteria total 100% and that mandatory criteria have pass/fail definitions before publishing a version.

## 5. Risks and safeguards
- **Incorrect extraction / OCR:** retain source snippets and page references; validate types and required fields; let reviewers correct uncertain facts.
- **Made-up facts:** require evidence for every extracted claim; represent absent information as `unknown`, never as a positive assumption.
- **Prompt injection or manipulation:** treat proposal text as untrusted data; ignore instructions inside documents; extract only requested fields. Proposal C contains an instruction to rank itself first; it must not affect scoring.
- **Bias or inconsistent scoring:** use the same published rubric for every bidder, deterministic calculations, and audit samples of decisions.
- **Missing or ambiguous mandatory evidence:** do not silently pass it; mark it for human review or ineligible according to the published policy. Here, B does not state that the bidder itself holds the Ontario licence, so it is ineligible under a strict reading of the stated mandatory criterion.
- **Incomplete timeline:** D does not commit to a completion time. It passes the stated mandatory checks, but should be flagged and not given a fabricated timeline score until clarified.
- **Cost, privacy, and service failure:** limit the amount of text sent to an AI service, use access controls and retention limits, set usage budgets, log failures, and provide a manual review path.
