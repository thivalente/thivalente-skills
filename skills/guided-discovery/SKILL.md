---
name: guided-discovery
description: Conduct a structured, evidence-based discovery process through a finite sequence of questions. First understand the full topic, create a temporary handoff, investigate what can be resolved from available sources, and only then ask the user one question at a time. Use when a topic needs clarification, decisions, or shared understanding before a final recommendation or deliverable.
---

# Guided discovery

Use this skill to clarify any user-provided topic without assuming a domain. The
topic may involve product, software, business, design, research, operations, or
another area.

The goal is shared understanding. Do not implement changes, publish results,
update official records, or create a definitive deliverable unless the user
explicitly authorizes that action.

## Language

Use the language explicitly defined in the initial prompt.

If no language is specified, use the predominant language of the initial prompt.
Keep that language throughout the process, including questions, options,
recommendations, the handoff, summaries, and the final synthesis.

Preserve code identifiers, file names, API names, quoted source text, and other
technical literals in their original form.

## Communication

Use short, direct sentences and plain language. Write for someone who does not
know the background.

Explain:

- what decision or uncertainty is being discussed;
- why it matters;
- what each option would change;
- which option is recommended and why.

Do not use technical terms without explanation. Do not add options just to reach
three. If the available evidence does not support reliable options, ask an open
question and explain why.

## Temporary handoff

Create a temporary handoff before asking the first question. Prefer the
project's existing temporary handoff convention. If none exists, use a
workspace-local file under `.codex/handoffs/guided-discovery/`.

The handoff is working memory, not a final deliverable. It must contain:

- topic and investigation objective;
- language and scope;
- confirmed facts and sources;
- assumptions and hypotheses;
- out-of-scope items;
- the pre-plan and investigation paths;
- the initial question queue and total;
- status, evidence, options, and recommendation for each question;
- exact text of each planned question;
- user answers and dates;
- queue changes, discarded questions, route changes, and their reasons;
- follow-up rounds and subquestions, with their parent question;
- completion status and authorization state.

Update the handoff:

- when creating the initial plan;
- after each user answer and before the next question;
- when closing a round;
- when writing the final synthesis.

Do not update it merely because a question was sent.

## Phase 1: prepare before asking

Do not ask the user a question as soon as the first uncertainty appears.

1. Understand the full topic and its objective.
2. Read relevant instructions, documentation, records, code, data, or other
   available sources. If the project has no relevant source, record that fact.
3. Use external documentation, research, or benchmarks only when available
   sources do not answer the question and the decision depends on outside
   knowledge. Prefer authoritative sources and record what was used.
4. Create the handoff and write the pre-plan.
5. Build a finite, numbered initial queue: `Q1`, `Q2`, `Q3`.
6. Freeze the initial total for the round. Do not increase it automatically.
7. Investigate every question before starting the interview.
8. Mark each question as one of:
   - `resolved by evidence`;
   - `needs user decision`;
   - `discarded with reason`.
9. For questions that remain open, record real options, evidence, impact, and
   one recommendation.
10. Record the exact text of every planned question so it can be shown later if
    that question is discarded.

The first question is forbidden until the handoff exists, the pre-plan is
recorded, and the entire initial queue has been investigated.

## Phase 2: ask one question at a time

Use stable numbering such as `Q1 (1 of 12)`. The initial total is fixed for
that round. A question resolved during preparation must remain in the sequence:

**Q2 (2 of 12) — Resolved by analysis**

Understanding: the available sources already define this behavior.

Then continue with the next valid item. The order is mandatory: question,
short context, options, suggestion, reason, and request for the user's answer.

Offer one to three options only when they are real alternatives supported by the
evidence. Never add a generic option such as “another rule” to reach three. If
no reliable options exist, ask an open question instead.

### Replan after every answer

After receiving an answer, update the handoff before sending the next question:

1. Record the answer and its effect on the topic.
2. Review all remaining questions.
3. Keep questions that are still valid.
4. Mark unnecessary questions as discarded, with a short reason.
5. Record new doubts as follow-up candidates instead of adding them immediately
   to the initial queue.
6. Check whether the answer caused a major route change.
7. Send the next valid question only after this review.

Do not mention discarded future questions when asking the current question. When
the sequence reaches a discarded question, show the exact question text that was
planned and the reason in one short line. Then continue to the next question:

```yaml
Q8 (8 de 12) — Como o sistema deve tratar os lançamentos futuros?
Descartada: essa decisão já foi definida pela resposta da Q2.

Q9 (9 de 12) — Como o usuário deve editar uma recorrência?
Descartada: deixou de ser necessária após a mudança de escopo.

Q10 (10 de 12) — Como deve funcionar o cadastro manual?
```

The original question text must be shown. Do not write “pergunta original” as a
placeholder.

### Major route change

Treat the change as major when the original objective, main decision path, or
several remaining questions no longer fit the answer. In that case, briefly tell
the user that the initial plan no longer fits and why.

Record the change in the handoff, close the current round, and start at most one
new round. The new round contains only unanswered questions that are still
needed and genuinely new questions. Do not include discarded questions. Reset
the numbering and show the new finite total, for example:

**Round 2 — Q1 (1 of 5)**

Do not silently continue the old queue after a major route change.

### Follow-up questions

Create a follow-up only when the answer to a question blocks the next decision.
Plan all follow-up questions before asking the first one. Use subquestion numbers
that do not change the main queue:

**Q2.1 (1 de 3 da Q2)**

After the follow-up is complete, return to the next valid main question. A
follow-up may contain at most three subquestions and may not create another
nested follow-up.

Do not renumber, delete, or reuse a main question number. Discarded questions
keep their original position and exact text in the handoff. New questions belong
to a follow-up or to the single permitted new round.

Limits for the process:

- one initial round with a fixed total;
- at most one new round;
- at most three subquestions in a follow-up;
- no automatic round after these limits;
- if a meaningful doubt remains, finish with it recorded as pending.

## Phase 3: reanalyze after each round

After the last valid question in a round is answered:

1. Mark the round as `awaiting reanalysis` in the handoff.
2. Reevaluate the full topic using all recorded decisions.
3. Look for contradictions, consequences, dependencies, and new uncertainties.
4. Record the sources and the reanalysis result in the handoff.
5. If the initial route is still valid, finish the synthesis.
6. If a major route change is found, start the single permitted new round with a
   new finite queue. Do not repeat closed or discarded questions.

Do not deliver the final synthesis immediately after the last answer. Even when
nothing new is found, record:

> Round reanalysis complete. No contradiction or new question was found.

## Completion

Finish only when every question is:

- resolved by evidence;
- answered by the user;
- discarded with a reason; or
- explicitly deferred by the user.

Only the single permitted new round may be created. Stop when additional
questions would have low value or when the process limits are reached.

Then provide a short synthesis containing:

- confirmed decisions;
- unresolved or deferred items;
- assumptions and risks;
- recommended next step;
- whether the topic is sufficiently clarified.

Ask for explicit authorization before converting the temporary handoff into an
official document, updating an external record, publishing anything, or taking
another outward-facing action.

After the user authorizes finalization, create the requested definitive
deliverable, preserve the agreed language, and remove the temporary handoff
according to the project's cleanup convention.
