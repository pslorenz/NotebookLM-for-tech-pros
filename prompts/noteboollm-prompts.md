# NotebookLM Prompts

A working library of prompts for studying IT and cybersecurity topics in NotebookLM. Copy them wholesale. Substitute your topic where you see angle brackets.

The prompts are grouped by phase of the study session. Run them roughly in order the first few times you follow the method. After that, mix and match as the topic demands.

## Phase 1: Orientation

Use these after seeding the notebook with your sources, before generating any studio artifacts. They establish the shape of what you have.

### Source inventory

```
For each source in this notebook, produce a one-sentence description of what
the source covers, its apparent authority (vendor primary, standards body,
practitioner, academic, adversary perspective), and its publication date if
available. Flag any source that appears to be derivative of another source in
the notebook.
```

### Concept extraction

```
Extract the top ten concepts, terms, or controls that appear across multiple
sources in this notebook. For each, give the shortest accurate definition
the sources support, and list which sources discuss it. Do not include a
concept that appears in only one source.
```

### Disagreement map

```
Identify points where sources in this notebook disagree with each other,
explicitly or implicitly. For each disagreement, describe the positions, cite
which sources take which position, and assess which position is better
supported by specific technical evidence in the material.
```

## Phase 2: Depth

Use these after you have read the auto-generated briefing and study guide. They push past the surface.

### Configuration extraction

```
Extract every specific configuration value, command, registry path, cmdlet,
GPO setting, API endpoint, or technical parameter mentioned across the
sources. Group them by the source that mentioned them. Produce the result as
a table with columns for item, value, source, and a one-line description.
```

### Tradeoff analysis

```
For <topic>, identify the three most important design tradeoffs a practitioner
must make when implementing it. For each tradeoff, describe the competing
considerations, the factors that would lead you to favor one side or the
other, and cite the sources that inform the analysis.
```

### Adversary perspective

```
Based on the sources in this notebook, describe how an adversary would attack
or abuse <topic>. Include the specific techniques, the prerequisites for each
technique, and the detection or prevention controls that the sources
recommend. Cite specifically.
```

### Failure modes

```
Describe the failure modes of <topic>. What goes wrong in practice, what are
the warning signs, and what are the remediation steps? Distinguish between
configuration failures, operational failures, and failures caused by changes
to the underlying platform or vendor.
```

## Phase 3: Application

Use these when you are moving from understanding to deciding or deploying.

### Decision framework

```
Design a decision framework for whether to deploy <topic> in a given
environment. The framework should produce a recommendation as output, and it
should work for a practitioner who has not studied the underlying material.
Base the framework on the criteria the sources identify as important, and
cite the sources for each criterion.
```

### Design exercise

```
Design a deployment of <topic> for the following environment: <describe
environment>. Justify each architectural decision with reference to the
sources. For decisions where the sources do not give clear guidance, name
the gap and recommend how to resolve it.
```

### Compliance mapping

```
Map <topic> to the relevant controls in CIS Controls v8, NIST CSF 2.0,
and <any other framework you work with>. For each mapping, cite the specific
control language and indicate whether <topic> fully satisfies, partially
satisfies, or supports the control.
```

## Phase 4: Gap identification

Use these near the end of every session. They surface what you did not know to ask.

### Unasked questions

```
What is present in the sources that I have not asked about yet? List topics,
concepts, or details the material covers that my queries have not touched.
Prioritize items that seem operationally important based on how prominently
the sources treat them.
```

### Thin coverage

```
Based on my queries so far, identify topics where my sources are thin or
where the coverage is inconsistent. For each thin area, suggest what
additional source types would strengthen the notebook.
```

### Assumption check

```
Based on the questions I have asked so far, what assumptions about <topic>
am I apparently operating under? List the assumptions explicitly. For each,
assess whether the sources support, contradict, or are silent on the
assumption.
```

## Phase 4.5: Audio overview customization

Use these as customization prompts when generating the two-host podcast or single-host audio overview. Paste into the customization field before generation. The default audio overview is generic; the custom-prompted version is tuned to your study session.

### First-pass orientation

```
This is a first-pass orientation listen for someone who has not yet read
the source material in depth. Establish the vocabulary, the shape of the
topic, and the major distinctions a practitioner needs to hold in mind.
Skip background that a working IT or security professional would already
know. Spend more time on the parts where the sources disagree or where
the topic is genuinely tricky than on the parts where the sources align.
```

### Reinforcement pass

```
This is a reinforcement listen for someone who has already studied the
material in depth. Skip introductory definitions. Focus on application,
edge cases, failure modes, and the specific operational decisions a
practitioner has to make. Surface tensions in the source material rather
than smoothing them over.
```

### Tradeoff focus

```
Focus the discussion on the operational tradeoffs in <topic>. For each
tradeoff, identify the competing considerations, the conditions under
which each side is preferable, and the position the strongest sources
take. Spend less time on what the technology is and more time on the
decisions a practitioner has to make about it.
```

### Adversary perspective focus

```
Frame the discussion around the adversary's perspective on <topic>. What
does an attacker want from this control or technology? Where are the
weak points? What does the red team material in the sources say that the
vendor material does not? End with the defensive priorities the sources
support.
```

### Exam preparation focus

```
This is an exam preparation listen. Focus on application-level scenarios
where a practitioner has to make a decision and justify it, not on
recall material. For each major concept, present a scenario, walk
through the reasoning, and arrive at the recommended decision. Skip
trivia. Spend time on the topics most commonly tested at the
application level.
```

### Disagreement spotlight

```
Focus this discussion on the points where sources in the notebook
disagree with each other. Identify each disagreement, present both
positions, discuss what evidence each side brings, and where possible
identify which position is better supported. Where the disagreement is
unresolved, say so explicitly.
```

### Custom for a specific study question

```
The listener is studying <specific narrow question> as part of a larger
project. Focus the entire discussion on that question. Use the broader
material in the notebook only as background and only as needed to
support the focused discussion. Do not survey topics tangential to the
question.
```



## Phase 5: Artifact seeds

Use these to generate raw material that you will refine in Claude or by hand.

### Briefing seed

```
Produce a two-page practitioner briefing on <topic>, grounded exclusively in
the sources in this notebook. Lead with the recommendation or the key point.
Use full paragraphs, not bullets. Cite specifically. Do not use marketing
language. Do not hedge where the sources are clear.
```

### Runbook seed

```
Produce a deployment runbook for <topic>, grounded in the specific
configuration values and commands the sources provide. Structure as numbered
operational steps. Flag any step where the sources disagree or where
verification against current vendor documentation is recommended.
```

### Detection seed

```
Based on the sources in this notebook, list the observable indicators that
would suggest <topic> is being misused, misconfigured, or attacked. For each
indicator, name the log source, the event type, and the specific field
values that would constitute a detection signal. Do not invent indicators
that the sources do not support.
```

### Lab seed

```
Design a ninety-minute lab exercise that teaches a learner the core operational
skills of <topic>. The lab must include learning objectives, setup steps,
guided walkthrough, independent challenge with a verifiable success condition,
debrief questions, and cleanup steps. The independent challenge must force
the learner to make a design decision, not follow a procedure.
```

## Meta-prompts

A few prompts about your own study process.

### Self-assessment

```
Based on the queries I have run in this notebook, assess where my
understanding of <topic> appears strong, where it appears shallow, and where
I seem to be avoiding difficult material. Do not flatter. Be specific about
which queries led to which assessment.
```

### Study plan

```
Based on the sources in this notebook and the queries I have run, produce a
study plan for the next three sessions. For each session, name the specific
aspect of <topic> to focus on, the sources most relevant to that aspect, and
the deliverable the session should produce.
```

## Notes on prompt use

Prompts in this library assume your notebook has at least five sources. With fewer, the synthesis prompts produce weak results.

Prompts that ask for "specific" output are designed to surface hallucination. If NotebookLM cannot ground a claim in a source, it will either cite weakly or decline. Both are signals you can act on.

When a prompt produces output you want to keep, convert it to a note and then to a source. The next round of queries reasons against your own synthesis alongside the primary material, which compounds.
