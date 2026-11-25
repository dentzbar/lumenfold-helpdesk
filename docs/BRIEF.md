# Take-home exercise: where should the helpdesk agent improve, and how do you know?

Thank you for taking the time to do this. Please budget about three to four hours. The scope below is sized for
that; we would rather see three findings argued well than ten listed.

## The product problem

Lumenfold Software runs its IT helpdesk through an AI agent in Slack. Employees ask for access, report that
something broke, or just complain. The agent answers from a knowledge base, runs a few automated flows (password
reset, license assignment, approvals), and escalates to the IT team when it cannot help.

After every conversation an enrichment pipeline summarizes what happened: what was asked, which systems were
involved, how it ended, how the employee felt. Separately, the ticketing system records whether a human took
over and whether the employee came back within a week. Occasionally the employee leaves a thumbs up or down.

We want to use this stream to improve the agent. Sometimes the right intervention is new knowledge: an article the
agent can answer from. Sometimes it is a correction to knowledge the agent already has, a change in how the agent
decides what to do, an integration it is missing, or nothing at all because the problem was a one-off or is
already handled. Every intervention costs IT time and a wrong one makes the agent worse. So the question is not
"what topics appear" but "where would a specific change improve outcomes, and what is the evidence".

## The data

`dataset/conversations.csv` (also as `conversations.jsonl`) holds about 1,000 conversations from 90 days,
one row per conversation, with the full transcript in `conversation_content`. Column meanings are in
`DATA_DICTIONARY.md`.

Two things you should know:

- The enrichment fields (`category`, `sub_category`, `entities`, `resolution`, `sentiment`, and the free-text
  summaries) are produced by an LLM pipeline. They are useful and imperfect. Decide for yourself how far to trust
  each one, and show how you decided.
- The dataset is synthetic. Names, emails and systems are invented. Treat the patterns in it as real.

## What we are asking for

**1. Identify and prioritize improvement opportunities (the main task).**
Find recurring issues where a specific intervention would improve outcomes. Present your **top three**. For each:

- Quantify size and impact: how many conversations, how they end today, who is affected (everyone, one office,
  one platform).
- Validate against conversation evidence, not only the enrichment labels.
- Name the likely root cause and the intervention you recommend: new knowledge, a correction to existing
  behavior or knowledge, an integration, or something else.
- State which of your findings you consider robust, which are sensitive to data quality or sampling, and what
  additional evidence would change your ranking.

Also describe how you identified recurring issues across conversations: how you handled different descriptions
of the same underlying problem, and how you judged whether the resulting groups are reliable.

**2. Challenge your findings.**
Show **two** patterns that look important at first and that you would not act on. Support the reasoning with
numbers and transcript evidence.

**3. Design the continuous version, in one to two pages.**
Describe how you would turn this analysis into a recurring production process. Focus on the three hard parts
and the trade-offs in each, not on infrastructure:

- Detecting and validating candidate improvements when the enrichment labels are unreliable.
- Where LLMs and where human review belong in that loop.
- Measuring whether an intervention improved outcomes, and distinguishing real impact from unrelated changes.

## What to hand in

- A notebook (or script plus rendered output) with your analysis. We will read it, so keep it legible: short
  sections, a sentence of interpretation under each table or chart.
- The design as a Markdown document.
- A short note on any tools you used, including LLM assistants. They are allowed. We are interested in your
  judgment, not in whether you typed every line.

## What we are looking at

Whether your numbers would survive a skeptical reader. How you separate a recurring gap from noise, duplicates,
one-off incidents and things that already work. Whether you check the labels against the transcripts. Whether the
intervention you propose fits the cause, the person and the situation. Whether your process design closes the loop
back to outcomes. Clarity.

If something in the data looks wrong to you, say so and explain how you handled it. That is part of the work.
