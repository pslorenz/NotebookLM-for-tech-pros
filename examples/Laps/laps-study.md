# Worked Example: Studying Windows LAPS in a Single 90-Minute Session

A complete session, start to finish, on a real topic. The purpose of this file is to show the method executed end to end so you can see the cadence, the decisions, and the outputs before you attempt it on a topic of your own.

The topic is Windows LAPS, the modernized Local Administrator Password Solution that replaced the legacy tool. I picked it because it is narrow enough to finish in a single session, broad enough to exercise the full method, and because most practitioners have at least heard of it, which makes the example legible.

## The brief

Written before opening any tool.

**Topic:** Windows LAPS, with focus on the deployment model for hybrid-joined devices, the security posture tradeoffs against legacy LAPS, and operational considerations for MSP-managed environments.

**Outcome:** Four deliverables. A practitioner synthesis, a deployment runbook, a detection rule concept, and a ninety-minute lab guide for internal training.

**Current level:** Deployed legacy LAPS in multiple environments several years ago. Have not worked with the modern version in depth. Familiar with related Entra ID and Intune concepts.

**Time budget:** Ninety minutes for study and drafting. Verification against live vendor documentation happens in a separate session.

## Sourcing

Eight sources, selected with primary weighting and deliberate diversity.

1. Microsoft Learn, Windows LAPS overview page. Primary vendor documentation.
2. Microsoft Tech Community, announcement blog for modern Windows LAPS. Primary vendor perspective, secondary to the docs.
3. SANS reading room white paper comparing legacy and modern LAPS. Independent analysis from a standards-adjacent source.
4. Practitioner writeup from an MSP detailing their migration from legacy to modern LAPS. Operational perspective.
5. Second practitioner writeup, deliberately from a different MSP with a different environment profile. Diversity of practitioner experience.
6. Red team blog post on attacking Windows LAPS, including both the legacy tool and the modern version. Adversary perspective.
7. Microsoft Learn, Conditional Access integration guidance for LAPS. Primary vendor documentation on a specific integration that matters for the runbook.
8. MITRE ATT&CK technique pages for credential access techniques relevant to local admin compromise. Framework perspective.

Sources deliberately not included: the vendor marketing pages (redundant with the docs), Reddit threads (signal to noise too poor for a focused session), and a Gartner writeup behind a paywall (NotebookLM cannot read it, and if I wanted the perspective I would read it separately).

## Movement one: NotebookLM synthesis

Notebook created, named "Windows LAPS Study 2026-04". Sources ingested.

### Initial briefing (generated from all sources)

The briefing surfaces the key distinction I was expecting and one I was not.

Expected: modern LAPS supports two password storage backends, on-premises Active Directory and Entra ID, and the security posture and operational model differ between them.

Unexpected: the interaction between modern LAPS password retrieval and Entra ID role-based access is more nuanced than I had assumed, with implications for who can retrieve passwords and under what audit conditions. This becomes a focus area for the rest of the session.

### Study guide review

Generated, reviewed, marked up. Three concepts I know cold, five I need to work through, two I had not previously encountered. The two new ones: the specific CSP (Configuration Service Provider) used for Intune-managed deployment, and the Post-Authentication Actions feature that triggers local admin password rotation after a specified idle period following credential use.

### Mind map review

The branch on rollback and recovery is thin, with only two leaf items. My sources do not cover failure modes well. Flagged for a targeted query.

### Targeted queries

Three queries run, in order.

**Query 1: Source disagreement analysis.**

> Compare how Microsoft's documentation and the red team blog describe the attack surface of Windows LAPS. Where do they disagree, and what is each omitting that the other includes?

The answer reveals that Microsoft's documentation does not adequately address the scenario where an attacker has already achieved domain-level privilege, which is the scenario the red team post treats as the primary concern. The vendor documentation focuses on the attack surface of the LAPS solution itself, while the red team perspective focuses on LAPS as one element of a broader kill chain that assumes earlier compromise. Both perspectives are valid; together they form a more complete threat model than either alone.

**Query 2: Configuration extraction.**

> Extract every specific PowerShell cmdlet, registry path, GPO setting, CSP path, and configuration value mentioned across the sources, grouped by the source that mentioned them.

The result is a reference card of approximately thirty specific items. Some are cited by multiple sources and cross-checkable. Others appear in only one source and will need verification against live Microsoft documentation before they go into the runbook. This query alone saves roughly two hours of manual extraction from the sources.

**Query 3: Unasked questions.**

> What is present in the sources that I have not asked about yet? List topics the material covers that my queries have not touched. Prioritize items that seem operationally important.

The answer flags three items, the most important of which is the interaction between Windows LAPS and Entra ID role-based access control for password retrieval permissions. This is exactly the topic the initial briefing surfaced as more nuanced than I had assumed. I run a follow-up query specifically on this interaction, save the answer as a note, and promote the note to a source. Subsequent queries in the session now benefit from the synthesized understanding.

### State at the end of Movement One

I can describe modern Windows LAPS at a conceptual level. I understand the deployment modes, the storage options, the retrieval permission model at a high level, and the adversary's perspective. I have a reference card of specific configuration values. I have two or three areas where my understanding is thin and I know which ones they are.

## Movement two: Claude testing

### Opening frame

```
I am a Chief Security Architect at a managed security provider. I have
fifteen years in IT and five years running a security practice. I have
studied Windows LAPS through a NotebookLM notebook I built from eight
sources including Microsoft documentation and practitioner writeups.

I want you to act as a Socratic tutor on Windows LAPS.

Rules: when I explain something, listen for gaps and name them without
filling them on the first pass. Do not flatter. Push back when I am wrong.
If my question is shallow, tell me and ask a better one. When I use
imprecise language, call it out.

I will explain what I know in my next message.
```

### Feynman pass

I explain Windows LAPS to Claude in roughly a paragraph of prose, covering the deployment modes, the storage backends, the rotation mechanism, the retrieval permission model, and the auditing hooks.

Claude's response identifies three gaps.

First, my explanation conflated the password generation parameters with the rotation parameters. These are configured separately and have different operational implications. A correct explanation distinguishes them.

Second, I did not address the interaction with Intune's device-scope policy versus user-scope policy for LAPS configuration, and the implications for mixed-ownership devices. This is an operational gap that will matter for the runbook.

Third, I used the phrase "password retrieval audit" imprecisely. The audit events that fire on retrieval depend on the storage backend (AD versus Entra ID), and the event IDs and fields differ. A correct explanation names the distinction.

I go back to the notebook, query each gap specifically, and return with a revised explanation.

### Revised pass

The revised explanation addresses each flagged gap. Claude accepts the revision with one minor clarification on the audit event schema in the Entra ID case. I make a note of the clarification and move on.

### Application questions

I ask Claude to generate five application-level questions. Three notable ones:

- "Design a Windows LAPS deployment for a hybrid-joined environment with 400 devices, where 150 are BYOD participating in a company stipend program. Justify your choice of storage backend, retrieval permission model, and rotation policy, and describe the tradeoffs you are accepting."
- "An MSP technician needs emergency local admin access to a client device at 2 AM during an incident. The client's primary IT contact is unreachable. Design the process, the access controls, and the audit trail that permits the recovery without creating a standing access path that could be abused later."
- "A red team report finds that, post-LAPS deployment, an attacker who achieves domain admin can still retrieve LAPS-managed passwords at will. Explain why this is expected behavior rather than a bug, and describe what compensating controls would raise the bar for this specific attack."

I answer all three in prose, with one deliberately weak section in the second answer. Claude catches the weak section and presses it. The final version of my answer is meaningfully stronger than my first pass, which is the point of the exercise.

### Gap inventory

Closing query to Claude:

```
Based on this entire conversation, produce an honest inventory of my
understanding of Windows LAPS. Where am I strong, where am I thin, and
where have I been avoiding difficult material?
```

Claude's response names two remaining weak areas. First, my understanding of the Post-Authentication Actions feature is thin. I recognize it exists but have not reasoned about its operational implications or the specific threat model it addresses. Second, my treatment of the multi-tenant MSP deployment scenario was underdeveloped, specifically around how to structure retrieval permissions across tenants so that MSP staff have appropriate access without violating client isolation. Both go into the notes for the next session.

### State at the end of Movement Two

I can defend my understanding of Windows LAPS against pointed questions. I have identified two areas still needing work. I am ready to draft deliverables on everything except those two areas, which I will note as gaps in the output.

## Movement three: deliverable production

Four deliverables. Each draft produced in Claude with the appropriate framing prompt and my study notes pasted as context. Each reviewed and revised.

### Deliverable 1: Practitioner synthesis

Three-page document addressed to the technical team at my MSP. Measured voice, specific tradeoffs, explicit recommendations. Claude's first draft was usable at roughly seventy percent, with the remaining thirty percent rewritten by me primarily to strengthen the recommendations where the evidence supported a clear position. Default Claude voice hedges more than I do, and the document needed to sound like me.

Time to draft and revise: approximately twenty minutes.

### Deliverable 2: Deployment runbook

Structured as numbered operational steps with specific PowerShell commands, Intune configuration paths, and Entra ID settings. I pasted in the reference card from the NotebookLM configuration extraction query as context.

Claude produced a draft. I verified every specific command against Microsoft documentation before accepting. Two commands were subtly incorrect: one cmdlet name was from an older pre-release version of the module, and one registry path had been reorganized in a recent documentation update. Both corrected.

This verification step is the one that cannot be skipped on configuration artifacts. The failure mode is not that Claude invented a command out of nothing. The failure mode is that a command that was correct two years ago is no longer quite right, and the draft cannot tell.

Time to draft, verify, and correct: approximately twenty-five minutes.

### Deliverable 3: Detection rule concept

A detection rule concept, not a shippable rule. The prompt specified the target SIEM and asked for the log source, the specific event IDs, the detection signal, the false positive profile, and a response runbook outline.

Claude produced a draft for detecting unusual patterns of LAPS password retrieval. The draft identified the correct event IDs and fields, proposed a threshold-based detection logic, and flagged the false positive scenario of legitimate break-glass operational use. The draft is a strong starting point; the actual tuning and threshold selection has to happen against real telemetry in the SIEM authoring environment, which is separate work.

Time to draft and review: approximately ten minutes.

### Deliverable 4: Lab guide

A ninety-minute lab exercise for internal team training. The prompt specified learning objectives, setup, guided walkthrough, independent challenge with a verifiable success condition, debrief, and cleanup.

Claude produced a draft. The guided walkthrough and the independent challenge were too similar in the first pass, which is a common failure mode of AI-drafted lab content. I rewrote the independent challenge to force a design decision that the walkthrough had not made for the learner. The rest held.

I added an instructor key as a separate file, per the editorial standards I hold for lab content generally.

Time to draft and revise: approximately fifteen minutes.

## Summary

Inputs: eight sources, one focused brief, ninety minutes of study and drafting time.

Outputs: one three-page synthesis, one deployment runbook, one detection rule concept, one lab guide with instructor key.

Gaps identified and noted for next session: Post-Authentication Actions feature, multi-tenant MSP retrieval permission model.

Verification deferred to a separate session: all specific technical commands and values in the runbook and the detection rule. Verification time estimated at thirty to forty-five minutes.

Traditional equivalent: the same depth of output, produced by reading the same sources manually and drafting the deliverables by hand, would take a full weekend of focused work, and the reference-card equivalent of the NotebookLM configuration extraction query would not have been produced at all because the effort is disproportionate to the value without the tool.

This is what the method looks like when it works. Your first few sessions will be slower. After the third or fourth, the cadence becomes natural, and you will produce deliverables at a rate that was not possible before.

## Artifacts

The four deliverables produced in this session are not included in this file, to keep it focused on the method. In a real repository, they would live in a sibling directory and would be versioned alongside the brief and the source list. The brief, the source list, and the notes from the Claude session are the most important artifacts to preserve; they are what make the study reproducible and auditable.
