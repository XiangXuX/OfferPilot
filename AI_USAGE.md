# AI usage and decision log

This file records how OfferPilot was shaped and built. Each entry separates Sam's decisions from AI assistance and verification. GitHub becomes the authoritative copy when the repository is created.

## 2026-09-24 — Product goal and initial scope

- **Sam decided:** The product reviews an existing resume against a target JD for graduates and some career changers. It flags irrelevant or unprofessional text, missing technical detail and JD skill gaps; the user decides what to change. Suggested edits should be visibly marked in red and bold. Australian resume-convention checks are deferred.
- **AI contributed:** Structured those points in `docs/spec.md`, distinguished a skill absent from the resume from a skill the user does not possess, and recorded the previously discussed technical stack and engineering workflow.
- **Sam corrected and confirmed:** OfferPilot must not invent experience or skills and the MVP does not generate a new resume. This replaced an earlier AI proposal that included resume generation and export.
- **Verification still needed:** Sam reviews the written spec before the MVP boundary and acceptance criteria become final.

## 2026-09-24 — PDF annotation and job discovery

- **Sam decided:** Suggested changes are marked in the extracted resume text shown on the web page; the original PDF is kept for reference.
- **Sam asked:** Whether his existing Codex-assisted job discovery can be added to OfferPilot.
- **AI contributed:** Proposed importing only jobs Sam has already screened into the MVP, while evaluating automated scheduled discovery as a separate slice. This is a scope proposal, not a confirmed product decision.
- **Sam's subsequent decision:** Remove job discovery and Codex-result import from the two-week MVP. The user will supply the target JD directly; the earlier AI proposal is not part of the current scope.

## 2026-09-24 — Development roadmap

- **Sam decided:** Pause the first document commit until a concrete map covers MVP scope, daily work, tools, time and the repeatable development process.
- **AI contributed:** Drafted `docs/roadmap.md` with a provisional 14-day schedule and a 4–5 focused hours/day assumption, then updated `docs/progress.md` to show this review gate.
- **Sam corrected:** The fixed 14-day, 4–5-hours-per-day schedule was too scattered, did not clearly define scope or an Agile feedback loop, and incorrectly deferred CI/CD planning. He does not need a rigid deadline.
- **AI revised:** Replaced day-by-day tasks with an explicit P0/out-of-scope boundary, prioritized vertical-slice backlog, acceptance examples and Definition of Done. CI checks start at the runnable baseline; CD target and triggers are set now, while Azure resource selection awaits an ADR and cost decision. Updated `docs/spec.md`, `docs/roadmap.md` and `docs/progress.md` as drafts.
- **Verification still needed:** Sam reviews these scope and delivery choices; no code, commit, GitHub Issue or deployment has been done by AI.

## 2026-09-24 — Analysis record scope

- **Sam confirmed:** One existing resume may be used with multiple target JDs. Each resume–JD analysis has its own result and suggestion review choices.
- **AI contributed:** Added this rule and a two-JD acceptance example to `docs/spec.md`; updated the progress log.
- **Verification still needed:** The remaining MVP interaction rules are being reviewed one at a time. No implementation has begun.

## 2026-09-24 — Long JD extraction question

- **Sam asked:** Whether finding specific requirements in a long JD requires RAG.
- **AI proposed:** Extract requirements from the whole JD, or from its original sections if the selected model cannot process it at once; preserve exact source spans and verify them before matching resume evidence. RAG/vector retrieval is not proposed for this one-document MVP, because a top-ranked subset may omit a mandatory condition.
- **Verification still needed:** Sam reviews this architecture suggestion; no provider, extraction schema or implementation has been selected.

## 2026-09-24 — JD requirement hierarchy example

- **Sam supplied:** A Core Flow AI JD and clarified that qualification and experience requirements should be extracted explicitly, while the candidate's evidence should align with the main responsibilities.
- **AI contributed:** Added a compact acceptance example to `docs/spec.md` and refined the diagnosis backlog item. It preserves the degree-versus-substitution wording, separates listed requirements from responsibilities, and does not infer citizenship or PR from the generic phrase “eligible applicants.”
- **Verification still needed:** Review this interpretation against the pasted JD and later test extraction with other JD formats; no code or external company claim was added.

## 2026-09-24 — TypeScript gate correction

- **Sam corrected:** TypeScript in this JD is a qualification gate even though it appears under `Key responsibilities`; it must be singled out prominently.
- **AI changed:** Replaced a section-heading-only classification with a rule that explicit core technology requirements in responsibilities can be technical qualification gates. The TypeScript bullet is a concrete acceptance example. A resume lacking TypeScript evidence is marked for verification rather than declared ineligible.
- **Verification still needed:** Test the distinction between a technical gate and general responsibility against other JDs; no implementation has begun.

## 2026-09-24 — Skill mention versus experience evidence

- **Sam confirmed:** A skill listed only in a resume's Skills section is not sufficient experience evidence for a JD's technical gate.
- **AI contributed:** Added three visible match states to `docs/spec.md`: concrete experience evidence, mentioned without experience evidence, and not shown in the resume. The TypeScript JD bullet is the acceptance example.
- **Verification still needed:** Test this rule against real resume wording and decide how to handle vague experience bullets; no resume text was changed.

## 2026-09-24 — Options for irrelevant resume content

- **Sam clarified:** When a resume passage is unrelated to a target JD, the suggestion can be to remove it or change the emphasis to relevant content.
- **AI contributed:** Wrote the two paths into the spec with an acceptance example: refocus only on related work that actually happened and is evidenced; otherwise suggest removal. The application only annotates; Sam's end user chooses any edit.
- **Verification still needed:** Test the rule on concrete resume/JD examples; no resume was rewritten.

## 2026-09-24 — Consolidated scope review

- **Sam requested:** Stop asking one question at a time; list the remaining decisions together for a joint discussion.
- **AI contributed:** Added eight numbered, explicitly unconfirmed product choices with proposed defaults to `docs/spec.md`. Implementation details such as storage, provider and Azure resources are due before their corresponding slice, while existing CI/CD principles stay in `docs/roadmap.md`.
- **Verification still needed:** Sam reviews and changes the defaults in one pass; none of these eight proposals has yet been accepted as a requirement.

## 2026-09-24 — Recruiter-first reading order

- **Sam decided:** Evaluate the product from an HR/recruiter point of view; people a senior engineer or company leader value may differ from those visible to an initial recruiter screen.
- **AI contributed:** Updated `docs/spec.md` and the diagnosis backlog to put explicit conditions and visible resume evidence in a concise first screen, with deeper technical evidence as an expandable second layer. This is a product design interpretation, not a prediction of any employer's actual decision.
- **Verification still needed:** Sam reviews the full set of eight remaining proposed defaults together.

## 2026-09-24 — Consolidated decisions and JD format correction

- **Sam confirmed:** Items 1–4 and 6–8 of the consolidated scope list. JD input is pasted text; item 5 must not imply uploading a JD PDF.
- **AI changed:** Reworded the input contract in `docs/spec.md` to separate a pasted JD from the existing resume PDF, promoted the accepted defaults to confirmed rules, and left resume PDF extraction-error handling open rather than inferring approval.
- **Verification still needed:** Decide the resume PDF failure path before its upload slice; provider, storage and Azure details remain implementation-stage choices. No code or repository commit has been made.
