# Claude Prompts

A working library of prompts for studying IT and cybersecurity topics with Claude. These are different in character from the NotebookLM prompts. NotebookLM prompts are queries against a grounded source set. Claude prompts are conversation starters, framing statements, and evaluation structures.

Copy them wholesale. Substitute your topic and your context where you see angle brackets.

## The opening frame

Every study session opens with one of these. The specifics matter; generic framing produces generic output.

### Study session frame

```
I am a <role> studying <topic>. My background is <brief background>. I have
been working with <adjacent domain> for <time>. I have studied <topic> to the
following depth: <honest assessment>.

I want you to act as a Socratic tutor for this topic.

Rules for this conversation:
- When I explain something, listen for gaps, inconsistencies, or places my
  mental model is probably wrong. Name them. Do not fill them on the first
  pass.
- When I ask a question, probe whether I am asking the right question before
  you answer the one I asked.
- Do not flatter me. Do not open responses with praise of my question.
- If my question or explanation is shallow, tell me and ask a better one.
- Push back when I am wrong.
- When I use imprecise language, call it out.

I will explain what I know in my next message. Start from there.
```

### Exam preparation frame

```
I am preparing for the <certification name> exam. The exam tests <domains>.
My current level is <honest assessment>. I have <time> until the exam.

I want you to act as a tough examiner on this topic.

Rules:
- Ask me application-level questions, not recall questions.
- When my answer is correct but thin, tell me it is thin and push deeper.
- When my answer is wrong, tell me it is wrong and explain why using the
  actual underlying technology, not just the exam objectives.
- Do not reveal the answer to a question before I have attempted it.
- At the end of each exchange, tell me what to study next.

I will name the domain I want to work on in my next message.
```

### Artifact production frame

```
I have completed a study session on <topic>. I can explain the topic at the
following level: <brief description>. The remaining gaps in my understanding
are: <list gaps honestly>.

I want to produce <specific deliverable>. The audience is <audience>. The
format is <format>. The length should be <length>. The voice should be
<voice, ideally with an example>.

I will provide you with my study notes and the relevant source material in
the next message. Draft the deliverable. Where a claim depends on information
I have not provided, name the gap rather than filling it with plausible
content. Where a technical specific could be verified against vendor
documentation, note that it should be verified.
```

## Explanation and gap-finding

### Feynman pass

```
Here is my current understanding of <specific aspect of topic>:

<your explanation in your own words, as complete as you can make it>

Do not fill in the gaps in this explanation. Instead, list the gaps,
inconsistencies, or places where I appear to be wrong or imprecise. For each,
tell me specifically what I said that prompted the observation.
```

### Revised pass

```
Here is my revised explanation based on your feedback:

<your revised explanation>

Evaluate this pass. Has the prior gap been closed? Are there new gaps that
appeared in the revision? Do not accept improved vocabulary in place of
improved understanding.
```

### Depth probe

```
I have been answering questions on this topic confidently. Look back at my
answers in this conversation and identify where my confidence appears
uncalibrated. Where have I been fluent but thin? Where have I pivoted away
from a difficult question rather than engaging it directly?
```

## Adversarial quizzing

### Application-level question generator

```
Generate five application-level questions on <topic>. Each question should
describe a scenario, include operational constraints that matter, and require
me to make a design decision and justify it.

Do not write recall or definition questions. If a question could be answered
from a definition, rewrite it to require synthesis.

After I answer each question, roleplay as a skeptical peer reviewer and
critique my answer. Hold me to operational constraints you embedded in the
scenario. Do not accept vague answers.
```

### Peer review roleplay

```
I am going to present an architectural decision I made on <topic>. Roleplay
as a senior engineer who disagrees with the decision and is known for being
technically sharp and direct. Your job is to find the weakest point in my
reasoning and press it.

If the decision is actually sound, you will eventually be convinced. Do not
be convinced too easily. If the decision is not sound, do not let me bluff
past the weak point.
```

### Threat modeler roleplay

```
I am going to describe a design for <topic>. Roleplay as a red team operator
who will be attacking this design. Your job is to identify the attack
surface, the assumptions the design makes that you can violate, and the
controls that would meaningfully slow you down versus controls that look
good on paper but do not.
```

## Deliverable drafting

### Written synthesis

```
I am attaching my study notes on <topic> and the primary sources I consulted.

Draft a <length> synthesis for <audience>. Lead with the recommendation or
key point. Use full paragraphs, not bullets. Measured technical voice. Cite
specifically where a claim comes from a named source. Where my notes and
the sources disagree, surface the disagreement rather than picking a side.
Where the sources are silent, say so.

After the draft, list five things you were uncertain about and either
checked or flagged for me to check.
```

### Configuration artifact

```
I am attaching my study notes on <topic> and the relevant vendor
documentation.

Produce <specific artifact: runbook, policy, script, template, rule> for
<specific scenario>. Every technical specific must be traceable to the
documentation I provided. Where a value is not in the documentation, name
it as a gap rather than guessing. Where a value has changed recently in
vendor documentation, flag it for verification.

The artifact should be immediately usable with the noted verification steps.
Produce the full artifact, not an outline.
```

### Lab exercise

```
Produce a ninety-minute lab exercise on <topic> for <audience>. The lab must
include:

- Three learning objectives, each with a verifiable success condition
- Prerequisites, both knowledge and environmental
- Setup steps with specific commands or configurations
- Guided walkthrough covering the baseline competency
- Independent challenge that forces the learner to make a design decision
  and justify it, not follow a procedure
- Debrief questions at the end
- Cleanup steps

The independent challenge must not be a harder version of the walkthrough.
It must be a scenario the learner has not seen during the walkthrough.

Produce the full lab, including an instructor key as a separate section at
the end. The instructor key should include expected answers for the
independent challenge with discussion of what a strong versus weak answer
looks like.
```

### Detection rule

```
I am attaching my study notes on <topic> and the relevant threat research.

Produce a detection rule for <specific behavior> targeting <SIEM or
detection platform>. The rule must include:

- The log source and specific event types
- The specific field values that constitute the detection signal
- The expected false positive profile
- A recommended alert severity and rationale
- A suggested response runbook outline

For every field reference, cite the specific source that documents it. Do
not invent field names. If you are uncertain whether a field exists in the
target platform, flag it for verification.
```

## Closing a session

### Gap inventory

```
Based on this entire conversation, produce an honest inventory of my
understanding of <topic>.

Three sections:
1. What I appear to understand well, with specific examples from my answers.
2. What I understand partially or shakily, with examples.
3. What I have not touched or appear to be avoiding.

Do not flatter. Specific citations from my answers carry more weight than
general assessments.
```

### Study plan for next session

```
Based on the gaps identified, produce a plan for my next study session.

Include:
- The specific aspect of <topic> to focus on
- Source types that would be most useful for filling the gaps
- One or two specific NotebookLM queries I should run to prepare
- The deliverable the next session should produce
```

## Notes on prompt use

The frames at the top of this file are the most important prompts in the library. Generic framing produces generic content, and the quality of the study session depends on the quality of the opening frame more than it depends on any later prompt.

Claude's default tone is polite and accommodating. For study, that is the wrong calibration. The "do not flatter me" line and "push back when I am wrong" line are not rhetorical. Claude responds to them.

Every prompt that asks for artifact production includes a clause about flagging gaps and verification. Keep those clauses. They are the difference between output you can ship and output that looks like you can ship it.

Start a new conversation per topic. Context bleed between unrelated topics is subtle and produces errors you will not notice until they matter.
