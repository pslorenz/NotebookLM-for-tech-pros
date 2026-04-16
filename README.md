# Learning Loops

A workshop on using NotebookLM and Claude to study IT and cybersecurity topics the way a practitioner actually needs to study them. Companion repository to the YouTube series of the same name.

## What this is

Most of us learned to study the way school taught us to study. Read the chapter, take notes, answer the questions at the end. That method still works, but it does not scale to the volume of material a modern security practitioner has to absorb. A single CVE writeup, a vendor whitepaper, three conference talks, a twelve-part YouTube walkthrough, a vendor documentation site, and a blog post from someone who actually deployed the control you are looking at in production. That is one afternoon of research on one control, and the working professional cannot spend days re-deriving what someone already wrote down.

Two tools change the economics of that problem. NotebookLM turns a pile of sources into a searchable, queryable knowledge base with a consistent voice and a proper citation trail. Claude turns the same pile, plus your own questions, into a Socratic tutor that will check your understanding, refuse to let you bluff past gaps, and build lab artifacts when you are ready to put hands on keys.

Neither tool replaces the work of learning. Both accelerate it by a measurable factor if you use them deliberately. You still need to RTFM.

This repository contains the full walkthrough guide, the video series scripts, a prompt library you can lift wholesale, and a few worked examples. The intent is that a technician, an analyst, or a consulting architect can open the guide, pick a topic they actually need to learn, and be productive inside of an hour.

## How to use this repo

Read the guide first. Everything else supports it.

```
GUIDE.md                       The walkthrough. Start here.
video-scripts/                 Full scripts for the four-episode YouTube series. (That I still need to record)
prompts/                       Reusable prompts for NotebookLM and Claude.
examples/                      Fully worked study sessions, end to end.
templates/                     Blank study-plan and notebook-starter templates.
```

The prompts are written to be copy-pasted without editing. Where a prompt needs you to insert your own topic, the placeholder is in angle brackets like `<topic>`.

## Who this is for

Working IT and security professionals studying for certification, onboarding to a new platform, evaluating a vendor, or deepening expertise in a control they already deploy. The examples use Microsoft 365 identity, Windows endpoint, and detection engineering, because that is the lane I work in. The method generalizes cleanly to networking, cloud, SRE, application security, and anything else where you read more than you can remember.

## What this is not

Not a tutorial on NotebookLM's or Claude's full feature set. The vendors do that job adequately. This guide is specifically about using them to learn technical material faster and retain it longer.

Not a claim that AI tools replace reading primary sources, testing in a lab, or writing your own notes. They change the ratio of time you spend on each of those activities, not whether you do them. Still RTFM.

Not a prompt-engineering course. The prompts here are good enough to get you moving. If you want to tune them, tune them.

## License and contributions

Content is released under CC BY 4.0. Code samples and prompts are MIT. Pull requests welcome, particularly for additional worked examples in domains I do not cover.

## Companion video series (in Progress)

Four episodes on YouTube. Full scripts live in `video-scripts/`. Watch them in order the first time.

1. Why studying stopped scaling, and what to do about it
2. Building a notebook that actually teaches you something
3. Claude as tutor, Claude as lab partner
4. Combining both, and turning study into deliverables
