# The Guide

A walkthrough on using NotebookLM and Claude to learn IT and cybersecurity topics properly. Read this end to end the first time. After that, skim the table of contents and drop into whichever section matches what you are doing today.

## Contents

1. The problem we are actually solving
2. Before you open either tool
3. Sourcing: picking the right inputs
4. NotebookLM: the workflow
5. Claude: the workflow
6. Combining the two
7. Turning study into deliverables
8. Pitfalls, limits, and failure modes
9. A 90-minute session template

## 1. The problem we are actually solving

The gap most practitioners hit is not a lack of material. It is the opposite. On any security topic worth learning, there are more primary sources, vendor docs, conference talks, and practitioner writeups than a full-time reader could finish in a month. What we lack is a way to compress that volume into understanding without skipping the steps that produce real retention.

Traditional techniques still work. Read, take notes, build a lab, try the configuration, break something, fix it, write up what you learned. The technique is sound. The problem is the first step, which in a modern environment means sitting through six hours of YouTube, a hundred pages of vendor documentation, and three conflicting practitioner blogs before you can even begin the lab.

NotebookLM compresses the read-and-synthesize step. You hand it a curated set of sources and it produces summaries, briefings, study guides, mind maps, and audio overviews that are grounded in the material you provided rather than in the open web. Every claim traces back to a specific sentence in a specific source, which matters in a field where being wrong in the right direction can still mean a misconfiguration on a production tenant.

Claude compresses the check your understanding step and the build the artifact step. You can walk through a topic with it Socratically, have it quiz you, have it roleplay a reviewer, and then have it draft the policy, the detection rule, the PowerShell, or the lab guide once you actually understand what you are asking for and can validate the end result.

Neither tool makes you a better practitioner on its own. Both change the economics of study enough that a working professional can realistically keep current across more domains than before.

## 2. Before you open either tool

Decide what you are trying to learn, and why, before you open a browser tab. This sounds obvious and it is the step most people skip.

Write down the following, in plain prose, before touching NotebookLM or Claude.

- The topic, narrowed to a scope you could teach to a peer in forty-five minutes. "Conditional Access" is too broad. "Designing a Conditional Access policy set for a hybrid-joined workforce with a BYOD stipend" is specific enough to study.
- The operational outcome you want. A passed exam item, a deployed control, a written policy, a detection rule, a briefing for a client. The outcome shapes which tool you lean on and how deep you need to go.
- Your current level. Honestly. If you have never touched the subject, the sourcing will lean toward primers and conceptual videos. If you are brushing up, the sourcing will lean toward current edge cases and recent changes.
- Your time budget. Ninety minutes of focused study produces more retention than eight hours of distracted consumption. Pick a realistic block and defend it.

This becomes the brief you feed both tools. Skipping it produces notebooks full of tangentially related material and Claude sessions that wander.

## 3. Sourcing: picking the right inputs

NotebookLM is grounded in what you give it. Claude is grounded in what you tell it. Both are only as good as the sources you curate. Treat sourcing as the skill it is.

**Primary over secondary.** Vendor documentation, RFCs, NIST publications, CIS benchmarks, the SANS reading room, MITRE ATT&CK technique pages, and official security advisories outrank blog posts and YouTube explainers for factual accuracy. Practitioner content is still valuable, especially for the "here is what nobody warned me about" angle, but it rides on top of a foundation of primary sources.

**Recency matters, and varies by topic.** A piece on kerberoasting from 2016 is still accurate. A piece on Conditional Access from 2021 is almost certainly stale. Check the publication date on every source. NotebookLM cannot always tell you when a technique it is describing was superseded.

**Diversity of perspective, not quantity.** Ten sources that all copy from the same original do not teach you more than that one original. Deliberately pick sources that disagree, or that cover the topic from different angles. A vendor doc, a practitioner writeup that criticizes the vendor, a threat actor's perspective from a red team report, and a compliance framework's take on the same control will teach you more in four sources than thirty shallow reposts.

**Respect the source limit.** NotebookLM free caps you at fifty sources per notebook. Plus raises that cap but not infinitely. Tight curation produces better answers than an overstuffed notebook. If you are tempted to dump an entire YouTube playlist, stop and ask whether every video earns its slot.

**Formats that work well as NotebookLM sources.**

- YouTube videos (NotebookLM ingests the transcript)
- Public webpages pasted as URLs
- PDF files (vendor whitepapers, NIST publications, academic papers)
- Google Docs you own
- Pasted text for anything else, including your own existing notes and prior findings

For Claude, treat sourcing differently. Claude works best when you paste or attach the primary source directly and then ask it to reason against that source, or when you describe your understanding and let it probe the gaps. Claude is not grounded in a source set the way NotebookLM is; it is grounded in whatever you tell it in the current conversation.

## 4. NotebookLM: the workflow

What follows is a deliberate sequence. Run it in order the first few times. Once you have the muscle memory, adapt it.

### 4.1 Create the notebook with intent

Name the notebook specifically. "Conditional Access for Hybrid-Join BYOD" is useful six weeks from now. "CA Study" is not. You will accumulate notebooks quickly, and the naming convention is the difference between a searchable knowledge base and a landfill.

### 4.2 Seed with the foundational source

Start with the single best primary source you found. Usually this is vendor documentation or a NIST publication. Feed it alone, ask NotebookLM for a briefing doc, and read the briefing. This gives you a baseline vocabulary and a sense of the topic's shape before you layer on complexity.

### 4.3 Layer in the supporting sources

Add the remaining sources in order of decreasing authority. Primary vendor documentation first, practitioner writeups next, conference talks and tutorials last. NotebookLM weighs all sources equally when answering, so the mix matters.

### 4.4 Generate the study artifacts

NotebookLM's Studio panel produces several artifact types. Use them in this order for a new topic:

1. **Briefing doc.** A two-page synthesis. Read this before anything else. If the briefing misrepresents your topic, your sources are wrong, not NotebookLM.
2. **Study guide.** Outline with key terms and questions. Print it or paste it somewhere you can mark up.
3. **Mind map.** Visual hierarchy of the concepts. Useful for seeing which branches of the topic you have sourced thinly and need to fill in.
4. **Audio and video overviews.** Covered in the next section because the audio modalities deserve treatment of their own.

### 4.4.1 Audio overviews and the auditory learning channel

Not every practitioner learns best by reading. A meaningful fraction of people retain technical material better when they hear it explained in conversation, and NotebookLM's audio modalities make that channel available at the same level of depth as the written artifacts. If you have ever listened to a security podcast on a commute and retained the discussion better than you retained the same material from a written brief, this section matters to you specifically.

NotebookLM produces several audio formats from your sources, and they serve different purposes.

**Two-host podcast overview.** The flagship feature. NotebookLM generates a conversational podcast between two synthetic hosts who discuss the material in the notebook. Length is configurable, typically twenty to thirty minutes in the default setting. The hosts ask each other questions, build on each other's answers, and occasionally surface tensions in the source material the way a real podcast would. The conversational format works because explanation through dialogue tends to surface assumptions that monologue glosses over.

The two-host podcast is genuinely useful for commute time, gym time, or any block where you cannot read but you can listen attentively. Use it as a first pass to establish the shape of the topic before you sit down with the written artifacts, or as a reinforcement pass after you have studied the material in depth. Do not use it as your primary source of truth on technical specifics. The conversational format necessarily compresses and simplifies, and the hosts will occasionally state something with more confidence than the underlying sources support.

**Custom-prompted audio overviews.** This is the feature most practitioners overlook and the one with the highest leverage for technical study. You can prompt the audio overview with specific instructions before generation. "Focus the discussion on the operational tradeoffs between the on-premises and cloud storage backends, and skip the introductory material on what the platform does." That instruction produces a podcast tuned to your study session rather than the generic overview.

For exam preparation specifically, prompt the audio overview to focus on application-level scenarios and the reasoning behind them rather than memorization material. For platform evaluation, prompt it to surface the disagreements between sources and the unresolved questions. The custom prompt is the difference between a podcast that fills time and a podcast that compresses an hour of reading into a thirty-minute walk.

**Single-host explainer.** Recently added option for a single-presenter audio format. Useful when the dialogue format feels artificial for the topic, or when you want a more lecture-style delivery. Less engaging than the two-host format, more efficient for pure information density.

**Video overviews.** NotebookLM also produces video versions of the audio overviews, with visual elements drawn from the sources. The video format adds value when the source material is visual to begin with, such as architecture diagrams, screenshots, or conference slides. It adds little for purely textual sources, where the same audio with no video is just as useful.

A few practical patterns that work for technical study.

Use audio for the first pass on topics where you want orientation before you commit to a deep session. A twenty-five minute podcast on the morning commute followed by a focused notebook session in the evening produces better retention than the notebook session alone, because the audio establishes the vocabulary and the rough shape of the topic before the written material has to build it from scratch.

Use audio for the reinforcement pass three to seven days after the original session. Generate a fresh audio overview from the same notebook, prompted to focus on the areas you flagged as weak in the original Claude session. The spacing matters; auditory reinforcement at a delay produces measurably better long-term retention than immediate review.

Use audio with the custom prompt for exam preparation. Generate one audio overview per exam domain, each prompted to focus on application-level scenarios. Listen through them in rotation in the days before the exam.

A real warning on the audio modalities specifically. Because the format is conversational and pleasant to listen to, it produces a stronger sense of having learned the material than the actual depth justifies. The two-host format in particular is engineered for engagement, and engagement and retention are not the same thing. If you study only through audio, you will feel competent and test poorly. Audio is a complement to the written artifacts and the Claude tutoring loop, not a substitute. Use it to extend study into time you could not otherwise study, not to replace study you would have done anyway.

### 4.5 Run targeted queries

This is where most of the value sits, and where most users stop too early. Ask specific questions against the notebook. The prompt library in `prompts/notebooklm-prompts.md` has a starting set. A few that consistently earn their keep:

- "Compare how source A and source C describe the failure mode of this control. Where do they disagree, and which is better supported by the underlying evidence?"
- "Extract every specific configuration value or command mentioned across the sources, grouped by which source mentioned it."
- "Generate ten exam-style questions at the application level, not recall level, with answer keys that cite the specific source sentences."
- "What is present in the sources that I have not asked about yet? List ideas the material covers that I have not queried."

That last prompt, the "what have I not asked," is the one that repeatedly surfaces blind spots. Use it near the end of every session.

### 4.6 Convert answers to notes, then to sources

NotebookLM lets you save answers as notes, and convert notes back into sources. When the tool produces a synthesis you want to build on, save it, then promote it. The next round of queries can then reason against your own synthesized understanding alongside the primary material. This is the mechanism that turns a notebook from a pile of inputs into a structured knowledge base.

## 5. Claude: the workflow

Claude sits in a different slot. Where NotebookLM synthesizes and summarizes grounded in your sources, Claude reasons, tutors, and produces artifacts. The two workflows complement each other, and a good study session uses both.

### 5.1 Set the frame

Open every Claude study session by telling it who you are, what you are studying, what level you are at, and what you want from this conversation. "I am an IT practitioner studying Conditional Access policy design. I have deployed basic policies for three years. I am trying to understand the interaction between sign-in frequency controls and session controls in the context of a BYOD program. Tutor me Socratically and push back when I am wrong."

This framing shapes every subsequent response. Without it, you get generic content. With it, you get a tutor calibrated to your level.

### 5.2 Explain the topic to Claude, not the other way around

The highest-leverage study technique with any tutor, human or otherwise, is the Feynman method. You explain the topic as if teaching a peer. They listen for gaps and point them out. Claude is extraordinary at this specific task because it has infinite patience and will not let you hand-wave past a weak point if you ask it not to.

Tell it explicitly: "I am going to explain this topic to you. Listen for gaps, inconsistencies, and places where my mental model is probably wrong. Do not fill in the gaps yourself on the first pass. Just name them."

Then explain. When it flags a gap, go back to NotebookLM or your primary sources and fill it. Return and explain again.

### 5.3 Use Claude to generate adversarial questions

Once you think you understand the topic, have Claude quiz you at the application level. Not "define Conditional Access." The useful question is "design a policy set for the following hypothetical environment, and justify each policy with respect to the threat it mitigates and the user experience tradeoff it imposes." Then let Claude critique your answer as a skeptical reviewer.

The prompt library has a dozen of these adversarial question patterns. The "roleplay as a skeptical peer reviewer" pattern is the most useful one in the set.

### 5.4 Use Claude to build the artifact

At the end of study, you usually need something real. A policy document, a detection rule, a PowerShell script, a client briefing, a lab exercise for your team. Claude is genuinely good at this, with one important discipline: it must have your sources in front of it, or your synthesized understanding in front of it, to produce something accurate.

The pattern that works: you explain your requirements, you paste the relevant primary source material, and you ask for the artifact with explicit constraints. "Produce a Conditional Access policy definition for the following scenario, structured as a JSON template, assuming Entra ID P2 licensing. Flag any control that requires a license level higher than P2. Flag any control where the underlying documentation has changed within the last six months and recommend verification."

That last clause matters. It tells Claude to be honest about what it is uncertain of, which is the behavior you want.

### 5.5 Critique what Claude produces

Never ship what Claude produces without reviewing it against a primary source. The failure mode of AI-assisted study is not that the model is lazy. The failure mode is that the model is confident, you are tired, and the small error it made at hour three of study ends up in production at hour four. Budget time to review the output the same way you would review a junior engineer's work.

## 6. Combining the two

The pattern that works for nontrivial topics looks like this:

You source and build the notebook first. You generate the briefing and the study guide. You read them, and you run the targeted queries until the topic's shape is clear. Then you open Claude, explain the topic back to it Socratically, and use it to find the gaps in your own understanding. When Claude flags a gap, you go back to NotebookLM to fill it, either from existing sources or by adding a new source. When you can explain the topic cleanly end to end, you move to Claude for artifact production. When the artifact is produced, you verify it against the NotebookLM-grounded source set and against one or two primary sources directly.

The roles are not interchangeable. NotebookLM is the library and the research assistant. Claude is the tutor and the drafter. Using NotebookLM as a tutor produces shallower results than Claude does. Using Claude as a library produces hallucinations. Respect the division of labor.

## 7. Turning study into deliverables

If you study a topic and produce only notes, you left most of the value on the table. Every study session should terminate in something reusable.

For a working practitioner, the reusable outputs typically include:

- A written synthesis you could hand to a junior engineer or a client. Two to five pages of prose, in your voice, that explains the topic and the decisions you would make.
- A configuration artifact. A policy definition, a PowerShell script, a detection rule, a template. Something that actually runs or applies.
- A lab guide. Ninety minutes of guided exercise that lets someone else internalize what you just learned.
- A decision log. The tradeoffs you weighed, the options you rejected, and why. Future-you will thank present-you.

Produce at least the synthesis at the end of every session. The other three are topic-dependent.

The examples folder contains a complete worked session on studying Windows LAPS, showing all four artifacts produced from a single notebook.

## 8. Pitfalls, limits, and failure modes

**NotebookLM will confidently summarize stale material as if it were current.** If your sources are old, the briefing will be old. Check dates, both on individual sources and on the consensus position in the material.

**NotebookLM citations tell you where a claim came from, not whether the source is right.** A bad blog post still gets cited. Source quality is your job.

**Claude's confidence is not calibrated to accuracy on narrow technical topics.** It will answer fluently about a feature that was deprecated two years ago. Verify technical specifics against vendor documentation, always.

**Studio-generated audio overviews skew toward accessibility and away from depth.** Useful for passive review. Dangerous as a primary source of truth. Listen on the commute; do not quote from it.

**Neither tool is a substitute for building a lab.** Watching, reading, querying, and explaining are all consumption modes. Retention comes from breaking and fixing the thing. Use AI to compress study time so you have more time left over for the lab, not to replace the lab.

**Context contamination.** If you study three unrelated topics in the same Claude conversation, Claude's answers on the third will be subtly shaped by the first two. Start a new conversation per topic.

**Source drift.** NotebookLM's source list is not re-fetched automatically. If a primary source changes, you need to remove and re-add it.

## 9. A 90-minute session template

Block this on the calendar. Defend it. Treat it as billable time against your own development.

- Minute 0 to 10. Write the brief. Topic, outcome, level, time. In plain prose, by hand or in a doc.
- Minute 10 to 30. Source and seed the notebook. Five to ten sources, primary-weighted, diverse.
- Minute 30 to 45. Generate briefing doc and study guide. Read both. Mark up the study guide with the questions you cannot yet answer.
- Minute 45 to 65. Claude session. Explain the topic to Claude, have it flag gaps, return to the notebook to fill them, repeat until you can explain cleanly.
- Minute 65 to 80. Adversarial quiz with Claude. Application-level questions, reviewer roleplay, critique.
- Minute 80 to 90. Produce the synthesis. Two to five pages in your own words. This is the artifact that proves you learned the thing.

Then schedule the lab session separately, for another day. Trying to study and lab in the same ninety-minute block produces shallow versions of both.

---

That is the method. The rest of this repository is prompts, examples, and templates to help you run it. Start with the worked example in `examples/laps-study.md` if you want to see the whole loop executed on a single topic before you attempt it on one of your own.
