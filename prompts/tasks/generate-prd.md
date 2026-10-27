# Task prompt: transcript → PRD (+ self-review loop)

Source: `mistral/agents/non_framework/transcript_linearticket_agent/TranscriptToLinearTicketAgent.ipynb`. Full workflow: `workflows/transcript-to-prd.md`.

## Step 1 — initial PRD (attach the transcript)

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

## Step 3 — apply feedback

```text
Incorporate the feedback into the PRD and provide the full revised PRD.
Do not comment on the changes.
```
