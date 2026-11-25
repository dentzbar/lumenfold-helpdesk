# Data dictionary: `dataset/conversations.csv` / `conversations.jsonl`

One row per enriched conversation. The CSV and the JSONL hold the same rows; in the JSONL, `entities` and
`conversation_content` are parsed JSON rather than strings.

All enrichment fields are produced by an LLM pipeline after the conversation ended. They are summaries of the
transcript, not ground truth. The ticket facts and feedback fields come from other systems.

## Identifiers and timing

| column | meaning |
|---|---|
| `conversation_id` | Conversation identifier. |
| `tenant_id` | Customer identifier. One customer in this extract. |
| `created_at` | When the employee's first message was sent (local time, ISO 8601). |
| `evaluated_at` | When the enrichment pipeline processed the conversation. |
| `enrichment_version` | Version of the enrichment pipeline that produced the row (semver). |
| `model_id` | LLM used by the enrichment pipeline. |
| `input_tokens`, `output_tokens` | Token usage of the enrichment call. |
| `channel` | Where the conversation happened: `slack`, `teams`, `portal`. |

## Enrichment: what was asked

| column | meaning |
|---|---|
| `user_request` | One-line summary of what the employee asked for. |
| `user_intent` | The goal behind the request. |
| `category` | Coarse request category (title case). |
| `sub_category` | Finer category (snake_case). |
| `entities` | JSON list of systems, devices or places mentioned, as the pipeline extracted them. |
| `symptom` | What the employee observed, when the request is a problem rather than an ask. |
| `edge_cases` | Conditions the pipeline flagged as complicating the request. |
| `required_information` | What the agent needed to ask for. |
| `ticket_type` | `incident` (something is broken) or `request` (asking for something). |
| `desk_id`, `desk_name`, `subdesk_id`, `subdesk_name` | Which helpdesk and sub-queue the conversation was routed to. |
| `desk_confidence`, `desk_classification_method` | Confidence and method (`llm` or `rule`) of that routing. |

## Enrichment: how it went

| column | meaning |
|---|---|
| `solution_steps` | Numbered steps the pipeline believes were taken. |
| `resolution_insights` | Free-text summary of how the conversation was resolved. |
| `resolution` | Pipeline's judgment of the outcome: `resolved_by_harmony` (the agent resolved it), `escalated` (handed to IT), `unresolved`, `unknown`. |
| `resolution_reasoning` | Why the pipeline chose that value. |
| `sentiment`, `sentiment_reasoning` | Employee sentiment as read from the transcript: `positive`, `neutral`, `negative`. |
| `interest_level`, `interest_level_reasoning` | Pipeline's read of urgency: `low`, `medium`, `high`. |
| `turn_count` | Number of messages in the transcript. |

## Ticket facts (from the ticketing system, not the LLM)

| column | meaning |
|---|---|
| `ticket_created` | Whether a ticket exists for this conversation. |
| `ticket_id` | Its identifier, if so. |
| `human_takeover` | Whether a human IT operator posted in the conversation or took the ticket. |
| `reopened_within_7d` | Whether the same employee opened another conversation or ticket on the same topic within seven days. |

## Explicit feedback (sparse)

| column | meaning |
|---|---|
| `feedback_rating` | `thumbs_up`, `thumbs_down`, or empty. Most conversations have no rating. |
| `feedback_text` | Optional free text left with the rating. |

## Employee

| column | meaning |
|---|---|
| `employee_email` | Synthetic address. |
| `employee_site` | Office: `Austin`, `Berlin`, `Tel Aviv`. |
| `employee_department` | Department. |
| `employee_tenure_months` | Months since hire. |

## Transcript

| column | meaning |
|---|---|
| `conversation_content` | JSON list of turns. Each turn has `role` (`user` or `assistant`), `actor_role` (`employee`, `agent`, or `it_operator` when a human from IT wrote the message), `content`, and `ts`. |
