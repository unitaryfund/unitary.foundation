---
title: "Quantum Open Source in the Age of Coding Agents: unitaryHACK26"
author: Alessandro Cosentino, Brad Chase, Veena Vijayakumar
day: 3
month: 9
year: 2026
tags:
  - unitaryHACK
  - open source
  - community
---

**The sixth edition of Unitary Foundation’s annual hackathon was very different from any previous edition.**

[unitaryHACK](https://unitaryhack.dev) is a two-week, bounty-based hackathon that brings together hackers and maintainers from across the quantum open-source ecosystem. It takes place on GitHub and GitLab: maintainers propose concrete issues they would like help with, and those issues become bounties, typically with around $500 available per project.

The event has always had goals beyond getting code merged. For maintainers, it is a chance to get neglected work over the line and, more importantly, meet potential long-term contributors. For hackers, it provides a structured way to explore unfamiliar projects, build experience, make connections, and enter quantum open source regardless of their starting skill level. For us as organizers, it is a way to bring these groups together and strengthen the community around the projects.

The headline numbers from 2026 looked healthy:

- **165 bounties closed** across **46 [projects](https://unitaryhack.dev/projects/)**
- **$17,870 awarded** directly to hackers
- **85 hackers** received at least one bounty
- **17 in-person HACKdays** across four continents
- **86 countries** represented around the world
- **10K+ visitors** to the unitaryHACK website

While the number of closed bounties didn’t follow the growing trend of the last few years (in fact it went down slightly from 172 in [2025](https://unitary.foundation/posts/2025_uhack/)), our geographic reach and the number of in-person events shot up.

By the usual measures, unitaryHACK26 was another successful hackathon. In practice, it ended up being a good stress test for what open-source quantum software development looks like with generative AI in the mix even with a human signal built in.

During unitaryHACK26, hackers could give a well-scoped GitHub issue to Claude, Codex, or another coding agent and get back something that looked like a complete pull request: implementation, tests, documentation, and a polished description. 

One obvious benefit: hackers could get up to speed on unfamiliar dense (and sometimes “intimidatingly quantum”) codebases faster, and people with less experience could tackle issues that might previously have looked inaccessible. Seeing this actually unfold over those two weeks was pretty cool.

While providing cash bounties attracts new contributors from outside quantum open source software (QOSS), we recognize that the format does create an incentive to move quickly; hackers competing for the same bounty is not new. What changed this year was the speed and scale at which plausible submissions could be produced. The compressed format made that shift particularly easy to see.

That is why, beyond celebrating the usual wins, we want to look directly at the challenges that coding agents created during unitaryHACK26—and at what those challenges might mean for quantum open source more broadly.


### The Phase Shift

We still wanted to run unitaryHACK in 2026 with the belief that the format isn’t obsolete, but rather needs to adapt to this new environment. The best way to find out how was simply to run the HACK. 

Maintainers still had issues they wanted help with; otherwise those issues would not have been sitting in their backlogs. Contributors still wanted ways to enter open-source projects, as we could see from activity in Unitary Foundation repositories and on our Discord server. The question was whether the hackathon still helped these two groups meet, and how much of the existing format still worked after coding agents had changed what producing a pull request looked like.

Not all AI-assisted pull requests were bad, and not all bad pull requests were AI-assisted. Some hackers used coding agents well: to understand unfamiliar abstractions, ask better questions, navigate documentation, write tests, and iterate more quickly after receiving feedback.

We were not interested in asking participants to pretend these tools didn’t exist. The question was whether the hacker remained involved in the contribution: could they explain the approach, answer questions, respond to review, and take responsibility for the result?


### Review Became the Bottleneck

At the same time, coding agents created a very visible volume-over-depth dynamic.

Some issues received clusters of parallel pull requests shortly after being released. A maintainer could find several implementations waiting for review before having had a design conversation with any of the hackers. In a few cases, later submissions also appeared to incorporate guidance given during the review of earlier ones without clearly acknowledging where that guidance came from.

And yes, we saw plenty of AI slop.

By slop, we don’t simply mean code written with AI. Many of this year’s bounty solutions arrived as one-shot PRs, with no prior issue discussion, no visible development process, and little evidence that the hacker had tested the change outside the generated test suite. Maintainers also saw clusters of similar PRs appear during review, sometimes incorporating guidance given on other submissions without clearly acknowledging it.

The review fatigue was real. An agent could produce an implementation quickly, but a maintainer still had to understand the change line by line, check whether it matched the project’s architecture, verify the assumptions behind it, and decide whether the hacker would be able to maintain or revise it.

Maintainers therefore spent less time helping newcomers understand quantum concepts and more time triaging submissions. As one maintainer noted in their post-event survey:

> In previous years, contributions trickled in at a steady pace, giving us time to engage thoughtfully with each hacker. This year, the sheer volume of immediate submissions made review cycles overwhelming—making it hard to focus on the deep, educational interactions that make open source so rewarding.

Hackers experienced the other side of the same problem. Someone could spend hours reading a paper, learning a project’s architecture, and thinking carefully about an implementation, only to discover that several pull requests had already appeared within hours of the issue being published.

That visible rush risked discouraging slower and more deliberate work, even when that work might ultimately have produced a better contribution.

### What We Tried to Keep Human

<div class="grid min-w-0 grid-cols-1 gap-4 sm:grid-cols-2">
  <img class="m-0 block aspect-[4/3] min-w-0 w-full max-w-full object-cover" src="/images/2026_unitaryhack_recap/UnitaryHack_Aalto.jpg" alt="unitaryHACK participants at Aalto" />
  <img class="m-0 block aspect-[4/3] min-w-0 w-full max-w-full object-cover object-bottom" src="/images/2026_unitaryhack_recap/UnitaryHack_GDGOAU.jpg" alt="unitaryHACK participants at GDG OAU" />
</div>

We had anticipated part of this problem, so we did not run exactly the same HACK as in previous years. We started using **human signal** as shorthand for evidence of real engagement between a hacker and project maintainers: asking questions, discussing an approach, explaining design choices, responding to feedback, and interacting with the people maintaining the software.
This did not mean banning AI; maintainers were using these tools too. It meant trying to prevent the pull request from becoming the first and only interaction between a hacker and a project.

Before the event, we introduced a public [AI Policy](https://unitaryhack.dev/ai-guide/). It gave maintainers and hackers a common set of expectations around meaningful AI assistance and provided organizers with a basis for intervening when submissions did not meet those expectations.

To boost the human signal, we introduced new programming to unitaryHACK26 as opportunities for maintainers, community organizers, and hackers to gain face-to-face interactions, keeping participants grounded as real peers rather than anonymous GitHub handles.

On Day 1, our organizing team kicked off the 2-week HACK with a **live virtual gathering** that welcomed more than 100 participants. It was a good venue for maintainers to exchange ideas and tips and for participants to get clarifications on the hackathon’s rules.

We introduced **project office hours** on Discord. Maintainers spent an hour in a voice channel where hackers could ask about a project, an issue, or a pull request already in progress. Some office hours became live review sessions. Others turned into wider discussions about the projects and where QOSS might be heading.

We also ran **17 in-person HACKdays across four continents**. These gave hackers a chance to work alongside one another, speak directly with maintainers, and learn about projects before choosing an issue. This was also the first year in which unitaryHACK developed a substantial in-person community in Africa.

A massive shoutout goes to the maintainers across the stack. Many spent hours reviewing code, explaining project context, answering questions on Discord, and helping hackers revise their work. That effort has always been central to unitaryHACK, but it was more demanding this year because so much maintainer time had shifted from mentoring to triage.

We also expanded enforcement of the **4-open-pull-request limit** introduced in 2025. Previously, organizers monitored the limit manually. This year, a bot regularly checked hackers’ open pull requests and notified organizers, maintainers, and hackers when someone exceeded the limit. Most people responded positively after a warning. The limit helped prevent one participant from filling several review queues at once, but it did not solve the problem of several participants opening parallel pull requests for the same issue.

Maintainers also began adding **project-specific guidance** to issue templates and files such as `CLAUDE.md` and `AGENTS.md`. This made local expectations easier to find: whether contributors should discuss a design first, how AI use should be disclosed, and what maintainers expected someone to understand before submitting a change.


> unitaryHACK26 showed me that open source is about much more than getting a PR merged—it's about learning, collaborating, and growing as a developer. Learning from the people behind these projects was one of the most rewarding parts of the experience. 

-- anonymous response from the hacker survey


### Is This the Right Format?

unitaryHACK26 felt like a transitional year. Many bounty issues had been written for a time when the code implementation itself was a substantial obstacle. As maintainers bring coding agents into their normal workflows, issues that can be solved reliably from a GitHub description and a repository may simply stop sitting in backlogs for very long. Future bounties may therefore involve more ambiguous design work, domain knowledge, coordination, hardware access, or project context that is not captured in the issue. 

Putting this theory into practice, during unitaryHACK26, Deltakit and Metriq experimented with requiring contributors to propose a design or architecture before a pull request would be reviewed. In previous years, giving a newcomer an implementation plan was often a good way to help them get started. In 2026, that same plan could become an effective prompt for a coding agent. Asking contributors to develop and discuss the proposal themselves moved more of the reasoning to the beginning of the process. We may push this further next year. One thing we want to avoid in future iterations is ten hackers opening ten parallel pull requests for the same issue before any of them has had a real interaction with the maintainer. One possible format would be to release issues in smaller daily batches and limit each issue to one or two active implementation slots. To reserve a slot, a contributor would first discuss the proposed approach with the maintainer: what they think the issue requires, how they plan to solve it, and which trade-offs they see. Only after that conversation would the implementation slot be unlocked. Once the available slots were taken, hackers could wait for the next batch of issues.

This is only an idea. It would introduce more coordination work and could make spontaneous contributions harder, so it needs to be tested rather than treated as an obvious fix. But we would prefer the design discussion to happen before several versions of the same pull request are already waiting in the review queue.

Review could also become a more visible and communal part of the event. Similar to what some maintainers tried this year during project office hours, selected submissions could be reviewed live on stream. Maintainers could demonstrate their review process, use coding agents themselves, and explain what they are checking, where the tools are useful, and why a contribution is accepted, rejected, or sent back for changes. No matter the outcome, this could become useful for the community, rather than limiting the interaction to a short exchange about a bounty solution.

We are also questioning why all this concentration of maintainer and contributor attention should be limited to two weeks. If coding agents make it easier to work through issues, why wait for one annual sprint? Our [DeltaKit Community Fund pilot](https://unitary.foundation/posts/2026_deltakit_community_fund_announcement/) is one attempt to test a more continuous model. The annual hackathon could remain a period of concentrated activity without being the only time when supported contributions happen.

Maybe there is a world in which unitaryHACK lives alongside other heavily-AI-focused competitions to help our community better understand LLMs’ influence on QOSS. Projects like [ECDSA.fail](https://ecdsa.fail) and our very own [QLDPC Challenge](https://unitaryfoundation.github.io/qldpc-challenge/) are ways to promote more focused AI-driven research.


### Could a “Lower-Stack” Quantum Hackathon be the Next Iteration?

A [recent Hacker News discussion](https://news.ycombinator.com/item?id=48468766) argued that hackathons may need to move towards hardware as software becomes increasingly cheap to produce. This resonates strongly with those of us in quantum computing as we are in a particularly interesting phase of building and rewriting the full computational stack. From bare-metal experiments and control firmware, to FPGA programming, instrument integration, calibration, and device characterization, many important problems still live close to the hardware.

These are also areas where producing code is only one part of the work. Someone still needs to connect the instruments, understand what the device is doing, validate the experiment, and debug the messy interfaces between software and a physical laboratory.

We can imagine a version of unitaryHACK with bounties around this kind of work. Software would still be involved, but a repository diff would no longer necessarily be the final deliverable. The output might instead be a working experiment, a calibration procedure, an instrument integration, a reproducible hardware result, or improved remote access to a device.

The practical challenges are substantial. unitaryHACK is remote and distributed, while most hardware is not. There are still relatively few open-source hardware platforms that contributors can access remotely and without substantial cost. But creating more remotely hackable hardware infrastructure could itself be a useful outcome of such an event.


### Share your experience

If you participated in unitaryHACK26, [let us know](https://airtable.com/appfhkqSH4zVtjha0/pag3wYF9zB2hPjuJD/form) whether this reflection matches your experience and what we may have missed.

We would also like to [hear from](mailto:info@unitary.foundation) people involved in other hackathons and open-source sprints. Are coding agents creating similar dynamics in your events?

Finally, if you maintain or contribute to quantum open-source software, please share any policies, workflows, or tools that have actually reduced review noise or made maintaining software easier—without closing the door to newcomers.
