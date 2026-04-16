# Worked Example: Studying Conditional Access Architecture for Hybrid BYOD

A second full study session, on a different topic, to demonstrate that the method generalizes. The first worked example used Windows LAPS as the topic. This one uses Conditional Access policy architecture for an environment with a hybrid-joined workforce and a BYOD stipend program.

The two examples are deliberately different in shape. LAPS is a discrete feature with a defined configuration surface, and the deliverables were mostly documentation of a specific technology. Conditional Access is an architecture pattern, not a feature. The deliverables are correspondingly different. The study produces a policy architecture, a set of policy definitions, a validation plan, and an operational runbook; together they form a deployable posture rather than a product deployment.

If you have studied this topic before, you will probably disagree with some of the conclusions this session reaches. That is the point. Conditional Access policy design is the kind of topic where reasonable practitioners land in different places, and a worked example is more useful when it shows the reasoning than when it presents a canonical answer. Read this against your own prior thinking and note where you would have pushed back on my reasoning. That note is itself a useful output.

## The brief

Written before opening any tool.

**Topic:** Conditional Access policy architecture for an organization with a mixed device population consisting of corporate-owned hybrid-joined Windows endpoints, corporate-owned Entra-joined endpoints, and personally-owned devices participating in a BYOD stipend program with mobile access to mail, Teams, and selected SharePoint content.

**Outcome:** Four deliverables. A policy architecture document explaining the overall posture. A set of specific Conditional Access policy definitions suitable for deployment. A validation plan for rolling out the policy set from report-only through enforcement. An operational runbook for the ongoing management of the policy set, including the breakage triage playbook and the change management discipline.

**Current level:** Deployed Conditional Access in many client environments. Comfortable with the policy engine at a technical level. The specific gap: I tend to design policy sets by adapting a template I have refined over time, and I want to test whether the template holds when stress-tested against current Microsoft guidance and recent changes in the feature surface. I also want to sharpen the BYOD portion, which is the part of the template that has accumulated the most patches over the years and is probably the part most overdue for clean rethinking.

**Time budget:** Ninety minutes for study, followed by drafting time in a second session. The topic is wide enough that one session produces the architecture and the first pass at the policy set; the second session produces the validation plan and the operational runbook.

## Sourcing

Nine sources, selected with a deliberate balance between Microsoft primary material, implementer perspective, and adversarial perspective.

1. Microsoft Learn, Conditional Access overview and architecture guidance. Primary vendor documentation on the policy engine itself.
2. Microsoft Learn, Zero Trust deployment guide for identity. Primary vendor documentation on the intended posture the policy engine is designed to support.
3. Microsoft Learn, documentation for each of the specific controls that will appear in the policy set: compliance requirements, session controls, Authentication Strengths, and the Conditional Access for workload identities capability. Grouped as one source bundle because Microsoft's documentation site treats them as related pages.
4. The Conditional Access documentation on Microsoft's "common policies" templates. Primary vendor documentation with pre-built policy recommendations that serve as a reference baseline.
5. Practitioner writeup from a consultancy that specializes in Entra ID implementations, documenting their current recommended baseline for mid-market clients. Secondary but high-quality implementer perspective.
6. Second practitioner writeup, deliberately from a different implementer with a different client profile, focused specifically on BYOD scenarios. The BYOD angle is underrepresented in Microsoft's own material.
7. Red team writeup on attacker techniques against Conditional Access, including bypass patterns involving device registration abuse, OAuth consent abuse, and the limitations of specific control types against Adversary-in-the-Middle attacks. Adversarial perspective.
8. Recent Microsoft Security blog post on the current recommended posture for phishing-resistant authentication, because the interaction between Conditional Access and Authentication Strengths has changed meaningfully over the past year.
9. Notes from my own prior Conditional Access work, specifically the design decisions from three recent client deployments and what I learned from each. Pasted as text sources, which is how NotebookLM ingests unstructured personal notes.

Sources deliberately not included: vendor blog posts from Conditional Access-adjacent identity products (Okta, Duo, CyberArk), because this study is specifically about the Microsoft engine rather than a comparative analysis. A comparative study would deliberately include those.

## Movement one: NotebookLM synthesis

Notebook created, named "Conditional Access Hybrid BYOD 2026-04". Sources ingested.

### Initial briefing (generated from all sources)

The briefing surfaces three things worth noting at the outset.

First, the Microsoft-primary material and the practitioner material agree on more than they disagree about at the architecture level. The disagreements are at the policy-definition level, particularly around the handling of edge cases like service accounts, break-glass accounts, and device registration flows. This is useful because it tells me I do not need to spend study time on the question of whether my overall architecture is defensible; the architecture is settled. I need to spend study time on the specific policy definitions and their interactions.

Second, the red team source identifies a category of bypass attacks the Microsoft material does not adequately address. Specifically, the red team writeup walks through several patterns where a policy set that looks comprehensive on paper can be bypassed by abusing workload identities, device registration from an attacker-controlled environment, or OAuth consent to an attacker-controlled application. The Microsoft material addresses these individually in separate documentation pages but does not connect them as a threat model the way the red team source does. This is a genuine gap in the primary source material, and my notebook is better for having both perspectives.

Third, my own prior notes, once ingested as a source, produce interesting results when NotebookLM queries across them alongside the primary material. Specifically, patterns I had intuited from multiple client deployments are visible in the synthesis in a way they were not visible when I held them only in my head. This is a good argument for treating personal notes as a first-class source type; the synthesis across your own prior work and the vendor material produces insights that neither alone would produce.

### Study guide review

Generated, reviewed, marked up. The study guide emphasizes policy design patterns (risk-based, identity-based, device-based, session-based) and the control interactions across them. My markup: seven concepts I know cold, four I understand but have not expressed clearly, two where my mental model may be out of date due to recent feature changes.

The four I understand but have not expressed clearly are worth listing explicitly, because they become the focus of the Claude session. The interaction between sign-in frequency controls and session controls under the new continuous access evaluation model. The precise semantics of the "or require compliant device" compound grant combined with Authentication Strengths. The scope and limits of the workload identities Conditional Access capability, which has matured meaningfully since my last deep engagement with it. The operational reality of device filters in Conditional Access versus the older trusted locations approach.

### Mind map review

The branch on exclusions is thin, with only three leaf items. This is suspicious because exclusions are a major operational consideration in any Conditional Access deployment and the thin branch suggests either my sources underweight them or the sources treat them implicitly rather than explicitly. I flag for a targeted query.

The branch on BYOD is adequate but organized around the Microsoft-primary framing, which is device-centric. The practitioner source on BYOD is organized around the user experience, which is a meaningfully different frame and produces different design choices. I make a note to test both frames against each other in the Claude session.

### Targeted queries

Four queries run, in order. This is one more than the LAPS session; the topic is wider and the first few queries surface more things that need their own follow-up.

**Query 1: Exclusion practice extraction.**

> Extract every recommendation across the sources about account and group exclusions from Conditional Access policies. Include service accounts, break-glass accounts, synchronization accounts, and any other exclusion pattern mentioned. For each, note the rationale given and any operational cautions.

The answer assembles a synthesis of exclusion practices that is more complete than any single source provides. Notably, it surfaces the fact that Microsoft's own documentation and the practitioner sources treat the break-glass account exclusion pattern somewhat differently, with Microsoft recommending two break-glass accounts excluded from specific named policies and the practitioners recommending a tier-two exclusion posture where the break-glass accounts are excluded from all Conditional Access and protected instead by heavily restricted session controls and aggressive audit monitoring. Both approaches have merit; the difference tracks to the underlying assumption about what scenario the break-glass account is meant for.

**Query 2: Control interaction mapping.**

> Describe how Conditional Access control types interact with each other when multiple policies apply to the same sign-in. Specifically: how do grant controls compose across policies? How do session controls compose? How do policies targeting different app scopes combine? What happens when a user matches multiple policies with different device requirements?

The answer clarifies a subtle point that I had been informally clear about but not precisely clear about: when multiple Conditional Access policies apply to a single sign-in, grant controls are combined as a logical AND, while session controls apply cumulatively with the most restrictive taking precedence. This affects policy design in a specific way: a comprehensive policy set can be composed of many narrow policies rather than a few broad ones, as long as the designer reasons carefully about the combined effect. This is the preferred pattern in current Microsoft guidance but is not uniformly followed in older practitioner templates.

**Query 3: BYOD pattern comparison.**

> Compare how the Microsoft-primary material and the practitioner source on BYOD describe the appropriate Conditional Access posture for personally-owned devices. Where do they agree, where do they disagree, and which approach is better supported by the current feature surface?

The answer surfaces a meaningful difference. Microsoft's posture is oriented around App Protection Policies applied through Conditional Access session controls, keeping BYOD devices unenrolled and managing data at the application layer. The practitioner source takes a similar position but adds a specific recommendation around requiring compliant or Entra-registered (not hybrid-joined) device for SharePoint and OneDrive specifically, on the grounds that data-at-rest on the device is the actual risk for those workloads and application-layer controls are insufficient. The reasoning is sound and the recommendation is more aggressive than Microsoft's default. It also imposes a real user-experience cost on BYOD, which is exactly the tension the topic surfaces.

**Query 4: Unasked questions.**

> What is present in the sources that I have not asked about yet? List topics the material covers that my queries have not touched. Prioritize items that seem operationally important.

The answer flags three items. First, the interaction between Conditional Access and the newer continuous access evaluation (CAE) capability, which changes the semantics of sign-in frequency controls in ways that matter for policy design. Second, the Conditional Access for workload identities capability, which was in preview when my template was last substantially updated and is now generally available with additional features. Third, the recommended posture for the Entra ID guest user population, which my current template treats as an edge case but which the current Microsoft guidance treats as a first-class consideration.

I run follow-up queries on each of the three items. The CAE query reveals that my current policy set's sign-in frequency configurations are not invalid under CAE but are less useful than they could be; CAE provides event-driven reauthentication that reduces the operational value of time-based sign-in frequency for some scenarios. The workload identities query reveals that the current feature surface genuinely warrants inclusion in the policy architecture rather than being an afterthought. The guest user query reveals that my current template's guest handling is probably acceptable but should be made explicit rather than being implicit in the default configuration.

I save the answers as notes and promote them to sources. Subsequent queries benefit from the synthesized understanding.

### State at the end of Movement One

I have the shape of the topic and enough detail to identify where my prior template needs updating. The areas needing update: the CAE interaction, the workload identities handling, and the explicit guest user posture. The areas where my prior template is probably correct but should be tested: the break-glass exclusion approach and the BYOD SharePoint posture. The areas where my prior template is probably wrong: some of the session control configurations that predated CAE.

## Movement two: Claude testing

### Opening frame

```
I am a Chief Security Architect at a managed security provider. I have
designed and deployed Conditional Access policy sets for many client
environments over several years. I have developed a template I adapt for
each environment, and I am doing this study to stress-test the template
against current Microsoft guidance and recent feature changes.

I want you to act as a Socratic tutor on Conditional Access architecture.
The specific areas I want to pressure-test:

1. The interaction between sign-in frequency controls and continuous access
evaluation (CAE) in the current feature surface, and whether my existing
time-based SIF configurations are appropriate under CAE.

2. The appropriate handling of workload identities, which I have historically
treated as a separate topic but which current guidance integrates into the
main Conditional Access architecture.

3. The BYOD posture for SharePoint and OneDrive specifically, where
application-layer controls and device-layer controls produce different
tradeoffs.

Rules: listen for gaps in my reasoning, name them without filling them on
the first pass. Do not flatter me; I have been doing this long enough that
flattery is counterproductive. Push back hard when I am wrong. If my
position is correct but I have not defended it well, tell me I have not
defended it well and make me do it again. When I use imprecise language,
call it out.

I will explain my current thinking on the first pressure-test area in my
next message.
```

### Feynman pass on the CAE interaction

I explain my current understanding. Sign-in frequency controls force reauthentication at a configured interval, which has historically been useful for high-risk scenarios (privileged access) and for BYOD where the device is untrusted. CAE provides event-driven reauthentication in response to specific signals like user disablement, password change, high user risk, and network location change, which reduces the need for aggressive time-based SIF for routine scenarios but does not eliminate it for scenarios where the CAE signals are insufficient.

Claude's response flags two gaps.

First, my explanation did not distinguish between SIF applied to the initial sign-in and SIF applied to session refresh. These are configured by the same control but behave differently under CAE, and the behavioral difference matters for policy design. I return to the notebook, query specifically on the configuration semantics, and correct my explanation.

Second, my explanation treated CAE as a feature that applies uniformly across the affected workloads. This is not quite right. CAE coverage varies by workload and by the specific signal; some workloads and some signals have reached general availability, others are still rolling out, and a policy design that assumes uniform CAE coverage will have unexpected behavior at the edges. I return to the notebook to query the current CAE coverage status and correct my explanation.

The corrected explanation holds. Claude accepts it with one clarification on the interaction between CAE and the specific Authentication Strength grant, which I note for the policy design phase.

### Feynman pass on workload identities

I explain that workload identities in Entra ID include service principals and managed identities, that Conditional Access for workload identities is a separate feature that applies policies to these non-human identities, and that the current feature surface includes the ability to require the workload identity to sign in from specific IP ranges, require specific risk levels, and block specific workload identities from certain resource access.

Claude flags one gap and one error. The gap: my explanation treated the workload identities capability as symmetric with user Conditional Access, which it is not. Workload identities have a narrower set of available conditions and grant controls, and the mental model of "Conditional Access for service accounts" produces incorrect assumptions about what is configurable. The error: I stated that the feature requires a specific Entra ID licensing tier that is not actually correct under the current licensing. I verify against Microsoft documentation and the correct licensing tier is different. This is a licensing detail I would have been embarrassed to get wrong in front of a client, and catching it here is exactly what the study process is for.

I note both corrections and move on.

### Feynman pass on BYOD SharePoint

I explain the tension. Application-layer controls through App Protection Policies prevent data exfiltration from within approved applications on unmanaged devices, which is appropriate for email and Teams, where the threat model is primarily data in flight and the operational cost of MDM enrollment on personal devices is prohibitive. For SharePoint and OneDrive, the threat model includes data at rest on the device, and the application-layer controls alone are insufficient because users can still sync files to the device through the OneDrive client or through browser download.

Claude engages this position and pushes back. Not on the factual claim, which is correct, but on the conclusion I had drawn from it. I had stated that the appropriate posture is therefore to require compliant device for SharePoint and OneDrive on BYOD, imposing the device enrollment cost for those specific workloads. Claude's pushback: this is one defensible posture, but the alternative posture, which the Microsoft guidance favors, is to allow application-only access to SharePoint and OneDrive from unmanaged devices, with session controls configured to prevent download and to restrict the access pattern to the browser rather than the sync client. This alternative has different operational characteristics and different failure modes, and choosing between them is a real architectural decision that depends on the specific organizational context.

I had argued for my position without engaging the alternative carefully. Claude's pushback is the right one; I had defaulted to my template's choice without re-examining the assumption.

I revise my thinking. The honest answer is that either posture is defensible, the choice depends on factors like the organization's data sensitivity profile and their BYOD user population's tolerance for friction, and the architecture document should present both options and recommend one while documenting the criteria for choosing the other. This is a more honest answer than the one my template produces, and it is better for the client.

### Application-level questions

Three scenario questions generated by Claude. One of interest:

"Design the Conditional Access policy set for an organization with three hundred employees, forty of whom are C-suite or senior leadership with extensive mobile device usage on both corporate-owned and personal devices, one hundred-eighty of whom are knowledge workers with hybrid-joined corporate laptops and optional BYOD mobile, and eighty of whom are production floor workers with shared kiosk devices and no personal devices. Produce the policy architecture and explain how you handle the overlap between the three populations, particularly for leadership members who sometimes use production floor kiosks during plant visits."

I answer this in prose and Claude reviews it. Two things the review catches that I am glad it caught. First, I did not address the session persistence question for the kiosk scenario adequately; a senior leader signing into a shared kiosk and then walking away is exactly the scenario where CAE and aggressive SIF are valuable, and my initial answer did not make the design choice explicit. Second, I did not address how the policy set handles a leader authenticating from their own BYOD mobile device while physically present at a production floor location, which is a real scenario in this organization profile and has Conditional Access implications because the leader's device compliance posture and the location signals point in different directions.

The final version of my answer addresses both, and is meaningfully stronger than the first pass.

### Gap inventory

Closing query:

```
Based on this conversation, produce an honest inventory of my understanding
of the Conditional Access architecture topic. Where am I strong, where am I
thin, and where have I been avoiding difficult material?
```

Claude's response names three areas. First, my grasp of the CAE interaction is now adequate, but my prior template did not reflect CAE correctly and I have not yet thought through the migration path from my existing deployed policy sets to the updated approach. Second, the workload identities handling is now understood at the architectural level but I have not yet done the work of deciding which workload identities in a typical client environment warrant Conditional Access versus simpler controls. Third, the BYOD SharePoint question was resolved by accepting that both postures are defensible, but I did not develop strong criteria for which posture to recommend in which situation, which means I have traded a confident (but potentially wrong) default for a thoughtful (but operationally harder to use) multi-option framework.

All three are legitimate gaps. The first two go into the next session. The third is the kind of thing that resolves through using the framework on a few real client engagements and letting the criteria emerge from the use.

### State at the end of Movement Two

I can defend the architecture. I have identified specific updates to my template, a specific licensing detail I had wrong, and a specific BYOD architectural question where my template oversimplifies. I am ready to produce deliverables against the updated architecture.

## Movement three: deliverable production

Four deliverables. The drafting phase for this topic ran longer than the LAPS session because the architecture is more complex, and the total deliverable set was split across two drafting sessions rather than produced in one. The production-time estimates below reflect the actual time spent.

### Deliverable 1: Policy architecture document

A five to seven page document explaining the overall posture, the zones of the policy space (all users baseline, privileged access, guest users, workload identities), the principles that govern the design (least privilege, risk-proportionate friction, defense against specific attacker techniques, accommodating defined operational exceptions), and the explicit tradeoffs the design makes. This document is the foundation for the remaining three deliverables and for client conversations about the policy set.

Claude's first draft covered about sixty percent of what I needed. I rewrote substantially, particularly in the sections on BYOD posture (where I inserted the two-option framework) and on workload identities (where the first draft treated the topic at too high a level to be useful).

Time to draft and revise: approximately forty-five minutes.

### Deliverable 2: Conditional Access policy definitions

A set of discrete policy definitions, each suitable for deployment, covering the baseline policies for all users, the privileged access policies, the guest user policies, the workload identity policies, the session controls for BYOD, and the explicit exclusion patterns. Each policy definition includes the scope (users, groups, applications, conditions), the controls (grant, session), the expected behavior, the operational considerations, and the verification steps.

This is the deliverable where the verification discipline matters most. Every policy definition references specific Microsoft feature names, specific grant control types, specific Authentication Strength names, and specific session control types. Every reference is flagged for verification against the current Entra ID admin center UI, because Microsoft renames things at a rate that approximately matches how often practitioners update their templates, which is to say it creates constant low-grade drift.

Time to draft, verify, and correct: approximately forty minutes, across two sessions.

### Deliverable 3: Validation plan

A plan for rolling the policy set out, from the initial report-only deployment through enforcement, structured in waves. Each wave identifies the scope of enforcement, the monitoring criteria, the pause-and-investigate conditions, the expected operational impact, and the rollback procedure. The validation plan is where the theoretical architecture meets the practical reality that Conditional Access policies, when misconfigured, lock out the organization.

The drafting for this deliverable went quickly because the structure is the same pattern I have used for many deployments. Claude's draft was substantially usable.

Time to draft and revise: approximately twenty minutes.

### Deliverable 4: Operational runbook

A runbook for the ongoing management of the policy set, including the triage playbook for users reporting access problems, the change management discipline for policy modifications, the monthly and quarterly review cadence for the policy set as a whole, and the specific procedure for break-glass account use. This deliverable is the one that most practitioners skip and most organizations need; a Conditional Access deployment without an operational runbook drifts into chaos within a year of deployment.

Time to draft and revise: approximately thirty minutes.

## Summary

Inputs: nine sources, one focused brief, ninety minutes of synthesis and testing plus approximately two hours of drafting across a second session.

Outputs: one architecture document, one set of policy definitions, one validation plan, one operational runbook. Plus: specific updates to my template, identification of a licensing error in my prior material, and a shift in how I treat the BYOD SharePoint question.

Gaps identified and noted for next session: migration path from existing deployed policy sets to the updated template, decision framework for which workload identities warrant Conditional Access, criteria for choosing between the two defensible BYOD SharePoint postures.

Verification deferred to a separate session: every specific Microsoft feature name, Authentication Strength name, grant control type, and session control type in the policy definitions. Verification time estimated at ninety minutes.

A note on what this session accomplished beyond the deliverables. The topic is one I have worked in extensively, and the study session nevertheless surfaced a licensing error, a feature surface change I had not fully integrated, and an architectural assumption I had been making that did not survive careful examination. None of these would have been caught by another project or another client engagement; they were caught because the study process created the conditions for examining them. This is the argument for running the method on topics you already know well, not only on topics you are learning for the first time. The compounding value is real.

Compare this against the LAPS session. The LAPS session compressed a topic I did not know well into a deployable understanding in a single afternoon. The Conditional Access session refined a topic I know deeply and caught specific errors and gaps. The method serves both use cases with the same structure. The topic's complexity, not the method's complexity, determines how long the session takes and how many deliverables it produces.

## Artifacts

The four deliverables produced in this session are not included in this file, matching the structure of the LAPS example. The pattern is intentional: the worked example demonstrates the method, and the deliverables demonstrate the outputs. Keeping them separate means the method documentation does not balloon and the deliverables are located where a workshop participant would expect them, in a sibling directory specific to the example.

For the LAPS example, the deliverables live in `laps-deliverables/`. For this example, a production version of the repository would include a corresponding `ca-deliverables/` directory. This file does not include those deliverables because they would be substantially similar to the kind of CA architecture work I have produced for clients over the years, and reproducing that work here would be both lengthy and duplicative. If a workshop participant wanted to see what the deliverables would look like, the structure would be: a policy architecture document in the voice of the practitioner synthesis from the LAPS example, a set of policy definitions in the voice of the deployment runbook, a validation plan in the structure of a phased deployment, and an operational runbook covering the ongoing management. The existing LAPS deliverables serve as structural examples for how each of those artifacts is shaped.

If, on the other hand, a workshop participant disagrees with any of the architectural conclusions reached in the study session described above, the useful exercise is to run the method on this same topic in their own environment and see what conclusions their own session produces. The method is robust enough that two practitioners with different priors will produce meaningfully different outputs from the same sources, and the comparison across those outputs is in itself instructive.
