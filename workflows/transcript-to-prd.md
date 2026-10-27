# Workflow: transcript to PRD to feature list

Source pattern: `mistral/agents/non_framework/transcript_linearticket_agent/TranscriptToLinearTicketAgent.ipynb`. Condensed from `docs/04-multi-step-patterns.md` (pattern A2).

Use when: the user has a call transcript, meeting recording, or discovery notes and wants a Product Requirements Document, optionally followed by a feature list.

One step = one message in the same chat. Paste the previous step's output into the next prompt. Attach the transcript as a file in step 1.

---

## Step 1 — initial PRD

```text
Based on the following call transcript, create an initial Product Requirements Document
(PRD) with some or all of these sections:
1. Title
2. Purpose
3. Scope
4. Features and Requirements
5. User Personas
6. Technical Requirements
7. Constraints
8. Success Metrics
9. Timeline and Milestones

Transcript:
{attach transcript file}

Align everything only with the information provided in the transcript. If any section is
not present in the transcript, you can skip it in the PRD.
```

## Step 2 — self-review against the source

```text
Review the following PRD based on the original call transcript. Provide feedback on:
- Missing information in the PRD that is present in the transcript.
- Inconsistencies in the PRD that are not aligned with the transcript.

Transcript:
{transcript}

Current PRD:
{PRD from step 1}

Align the feedback only with the information provided in the transcript. We are not
looking for additional information based on your knowledge.

If no feedback is required, respond with "None." and don't provide any further feedback.
```

If the answer is "None.", skip step 3. Otherwise continue.

## Step 3 — apply feedback

```text
Incorporate the feedback into the PRD and provide the full revised PRD.
Do not comment on the changes.
```

## Step 4 — feature list for tickets

```text
You are an AI assistant helping to create a Features list and their descriptions from a
Product Requirements Document (PRD). The description should contain a brief explanation
of the feature that includes Technical requirements (if any), Constraints (if any),
Success metrics (if any), User personas (if any), and Timeline and Milestones (if any).

PRD:
{revised PRD from step 3}
```

## Output format

Use `templates/prd.md` for step 1 output structure and `templates/feature-list.md` for step 4.

## Rules
- Never add information that is not in the transcript.
- If a PRD section has no source in the transcript, omit the section.
- Keep the language of the transcript in the PRD.
