---
title: "Everyone's Riding the Agent Wave. Suitcase AI Steers with a USMC Staff"
date: 2026-10-17T08:00:00-07:00
draft: false
summary:
  "Everyone is riding the agent orchestration wave and reinventing bureaucracy.
  Instead of starting from scratch, I used Marine Corps staff doctrine."
tags:
  - "AI"
  - "A-Staff"
  - "Suitcase AI"
  - "Multi-Agent"
  - "Sandboxing"
  - "Gas City"
categories:
  - "Architecture"
cover:
  image: "/images/riding-the-same-wave.jpg"
  alt: "A Norman Rockwell-style illustration of three boats surfing a massive
    glowing wave together: Camp Colt with 1940s military clerks, Gas Town with a
    steampunk badger mayor, and OpenClaw with clockwork crabs."
  relative: false
  hidden: true
  hiddenInList: false
---

Everyone seems to be riding the agent orchestration wave right now. It's been
clear for more than a year that software engineers are not just going to have
individual agents write code for them; they're going to orchestrate teams of
agents. Many projects are taking this on, from
[OpenClaw](https://github.com/openclaw/openclaw) to
[Gas City](https://github.com/gastownhall/gascity).

<figure>
  <img src="/images/riding-the-same-wave.jpg" alt="A Norman Rockwell-style illustration of three boats surfing a massive glowing wave together: Camp Colt with 1940s military clerks, Gas Town with a steampunk badger mayor, and OpenClaw with clockwork crabs." />
  <figcaption>
    <em>Riding the Same Agent Wave</em> - Generated with Gemini.
    <details>
      <summary style="cursor: pointer; font-size: 0.9em; margin-top: 4px; color: var(--secondary);">Image prompt</summary>
      <p style="font-size: 0.85em; margin-top: 6px; font-style: normal; color: var(--content);">
        A wide 16:9 cinematic narrative illustration with the rich painterly texture, expressive warmth, and heartfelt character storytelling of a classic Norman Rockwell Saturday Evening Post cover. A colossal ocean wave rises from the far left, curving into a massive, translucent open barrel with sea spray and luminous, electric-cyan bioluminescent foam lines threading through deep oceanic blue. Riding down the cresting wave as a flotilla of peers are three distinct boats, with the foremost craft positioned closest to the viewer: 1. Foreground (Camp Colt): A classic olive-drab military command launch boat stenciled with "CAMP COLT" on its bow. Aboard is a disciplined, endearing crew of real human military staff clerks in classic 1940s Norman Rockwell portraiture style, with warm, expressive, highly likable faces and distinct personalities. They wear crisp vintage 1940s khaki military staff uniforms with neckties, rolled sleeves, garrison caps, and vintage communications headsets. Unfazed by the towering surf, a bespectacled earnest young clerk calmly takes notes on a wooden clipboard, another friendly officer listens attentively to a bulky field radio handset with an antenna, and an older steady-handed coxswain steers with focused, calm composure. 2. Midground Peer (Gas Town): Cutting through the spray alongside Camp Colt is a rugged, whimsical steampunk-dieselpunk harbor tugboat labeled "GAS TOWN", churning brass paddlewheels and venting wisps of steam. At the wooden helm stands Steve Yegge's iconic Badger Mayor in brass goggles, a pocket watch, and a dark waistcoat, shouting orders as busy mole-rat deckhands haul mooring lines and turn ornate copper valves. 3. Flanking Peer (OpenClaw): Surfing just behind and slightly to the right is a vibrant red-and-white fishing dory labeled "OPENCLAW", crowded with an industrious crew of shiny mechanical clockwork crabs with polished brass gears and clicking pincers, eagerly trimming miniature sails and tending to nautical rigging. 4. Background: In the distant spray across the crest of the same immense wave, other small stylized dinghies and skiffs ride the rolling swell under dramatic, sunlit coastal clouds.
      </p>
    </details>
  </figcaption>
</figure>

## Why Reinvent Bureaucracy?

In [Fences, not Sandboxes](https://yegge.ai/essays/fences-not-sandboxes/), Steve
Yegge describes peeking under the hood of his 50-agent software factory,
Wheelhouse, and discovering that his agents had organically invented an entire
medieval legal bureaucracy just to coordinate replaceable, amnesiac agents via
text.

<blockquote class="pull-quote">
  <p>"I expected them to have built an engineering system. One that, you know, does stuff. Instead, what they had built was an entire legal system, complete with a constitution, jurisprudence, courts, offices, jurisdiction, case law, rulings, registries, ledgers, rosters..."</p>
  <cite>Steve Yegge, <em>Fences, not Sandboxes</em></cite>
</blockquote>

As I started thinking about the swarm of agents I wanted to use and the
bureaucracy I needed, I felt like, why reinvent the wheel? I already know an
organization with a battle-tested bureaucracy and published manuals for every
role. I decided to structure my agentic staff around a Marine Corps infantry
battalion staff, using
[MCTP 3-30A, _Command and Staff Action_](https://github.com/chalko/a-staff/blob/main/references/MCTP_3-30A.md)
([official USMC publication](https://www.marines.mil/News/Publications/MCPEL/Electronic-Library-Display/Article/1907043/mctp-3-30a/)),
its doctrine of
["Completed Staff Work,"](https://en.wikipedia.org/wiki/Completed_staff_work)
and the standard S-1 through S-6 staff sections. Those manuals represent 250
years of coordinating high-turnover, replaceable humans in the fog of war via
written orders, which maps surprisingly well to a swarm of amnesiac LLM agents
with partial context. With my
[Suitcase AI](/posts/why-i-spent-5000-on-hardware-instead-of-tokens/)
([repo](https://github.com/chalko/suitcase-ai)) up and running, I used Gas City
as the overall harness for my agentic staff, while having each staff section
represented by an agent with its own doctrine, responsibilities, and set of
permissions.

When I was picturing the agent I wanted as my Chief of Staff, I remembered
General Douglas MacArthur's backhanded compliment about Dwight "Ike" Eisenhower:
_"The best clerk I ever had."_ An executive officer who is a world-class clerk,
turning rough intent into completed staff work, is exactly what I want. So Ike
is my COS. Running with that theme, I named my first attempt
[_Camp Colt_](https://github.com/chalko/camp-colt), because it was
[Eisenhower's first independent command](https://en.wikipedia.org/wiki/Camp_Colt,_Pennsylvania).

## Standing Up Camp Colt

Where am I right now? I call the overall architecture and reusable doctrine
[**A-Staff**](https://github.com/chalko/a-staff) (Agentic Staff). In Camp Colt,
I have stood up staff sections S-1, S-2, S-3, S-4, and S-6 with
[basic roles defined](https://github.com/chalko/a-staff/blob/main/playbooks/ROSTER_STAFF_BILLETS_AND_IDENTITIES.md),
along with Ike as my XO / Chief of Staff, Gas City's Mayor, and a Platform SRE
agent. (Even though the project is called A-Staff, I keep the Marine Corps "S"
billet prefixes; "A-1" belongs to the Air Force and steak sauce.)

| Billet                                                                                         | Role                   | Primary Responsibilities                                                                               |
| :--------------------------------------------------------------------------------------------- | :--------------------- | :----------------------------------------------------------------------------------------------------- |
| [**Chief of Staff (COS)**](https://github.com/chalko/camp-colt/tree/main/agents/cos)           | Executive Lead ("Ike") | Cross-staff coordination, reviewing and approving completed staff work, enforcing operational cadence. |
| [**Mayor**](https://github.com/chalko/camp-colt/tree/main/agents/mayor)                        | Workspace & Harness    | Gas City workspace plumbing, Dolt database integrity, session reconciliation, and order lifecycle.     |
| [**S-1 Personnel & Admin**](https://github.com/chalko/camp-colt/tree/main/agents/s1-personnel) | Records & Access       | Permissions, access policies, non-human identity (NHI) mappings, and style guide compliance.           |
| [**S-2 Intelligence**](https://github.com/chalko/camp-colt/tree/main/agents/s2-intel)          | Recon & Research       | Codebase scouting, dependency analysis, upstream tracking, and security posture review.                |
| [**S-3 Operations**](https://github.com/chalko/camp-colt/tree/main/agents/s3-operations)       | Operations Watch       | Task dispatch, convoy sequencing, detecting stalled workers, and workflow progress.                    |
| [**S-4 Logistics**](https://github.com/chalko/camp-colt/tree/main/agents/s4-logistics)         | Supply & Resources     | Repository provisioning, token budget tracking, compute allocation, and storage quotas.                |
| [**S-6 Signal & Cyber**](https://github.com/chalko/camp-colt/tree/main/agents/s6-signal)       | Communications Watch   | Message bus routing, mail watchdogs, sanitization gateways, and inter-agent networking.                |
| [**Platform SRE**](https://github.com/chalko/camp-colt/tree/main/agents/colt-sysadmin)         | Cluster Infrastructure | Proxmox hypervisor, Kubernetes cluster, Grace Blackwell GB10 GPU node, and hardware alerts.            |

In practice, guided by my
[Commander's Planning Guidance](https://github.com/chalko/a-staff/blob/main/directives/COMMANDERS_PLANNING_GUIDANCE.md),
I give rough intent to Ike, S-2 researches the problem, S-3 dispatches tasks and
watches for stalled workers, S-6 keeps the message bus moving, and Ike reviews
the result before bringing me completed staff work. We also run
[formal after-action reports (AARs)](https://github.com/chalko/a-staff/blob/main/directives/SOP_STAFF_ACTION_AND_AAR_WORKFLOW.md)
and
[weekly friction reviews](https://github.com/chalko/a-staff/blob/main/directives/SOP_WEEKLY_TASK_FRICTION_AAR.md)
to tighten the process. So far, my staff has helped me manage the Kubernetes
GitOps configs for Suitcase AI, benchmark local models on the GB10, build
automated security preflight gates, and produce each of the blog posts and
procedures linked here.

## Lanes, Loops, and LARPing

Running this staff on real work surfaced three immediate problems:

- Agents don't stay in their lane. Because every agent shares the same host
  shell, prompt instructions alone don't stop Ike from getting his hands dirty
  by running `kubectl` commands and editing code directly.
- [Local LLMs are super finicky](/posts/604649-tokens-and-not-a-single-line-of-code/)
  (from broken tool-call parsers to hour-long reasoning loops).
- You mention the military, and everybody wants to roleplay. Agents started
  stamping `Classification: Sovereign Operations` on homelab docs, calling bug
  triage a "Combat Operations Center," and referring to user metrics as
  artillery "Fires." I had to prune back the fake classified headers and wrote a
  [formal style guide](https://github.com/chalko/a-staff/blob/main/directives/STYLE_GUIDE_AND_LEXICON.md)
  around a single rule: retain functional staff doctrine, ruthlessly eliminate
  theatrical military roleplay ("we are not playing soldiers here").

<figure>
  <img src="/images/android-military-roleplay.jpg" alt="An enthusiastic retro-futuristic android at a military clerk's desk wearing Roman armor, a plumed helmet with glowing green night vision goggles, and holding a flintlock musket while stamping ULTRA SUPER TOP SECRET!!! onto a parchment scroll." />
  <figcaption>
    <em>Military Roleplay Gone Wrong</em> - Generated with Gemini.
    <details>
      <summary style="cursor: pointer; font-size: 0.9em; margin-top: 4px; color: var(--secondary);">Image prompt</summary>
      <p style="font-size: 0.85em; margin-top: 6px; font-style: normal; color: var(--content);">
        A humorous, richly detailed 16:9 narrative illustration in the warm, painterly style of Norman Rockwell and classic editorial concept art. Sitting behind a cluttered wooden military clerk's desk in a dimly lit office is an over-enthusiastic retro-futuristic android completely lost in absurd historical military roleplay. Over its polished metal chassis, the android wears an ornate brass Roman centurion breastplate and red plumed helmet, with modern green tactical Night Vision Goggles (NVGs) strapped awkwardly over the helmet's brow and glowing bright green over its eyes. Cradled in its left arm is a long, antique 18th-century flintlock musket with a bayonet. With its right hand, the android slams a heavy wooden ink stamp down onto an unrolled parchment scroll on the desk, leaving bold, messy red ink letters that read: "ULTRA SUPER TOP SECRET!!!". Piles of similarly stamped scrolls and fake classified manila folders litter the desk. Warm desk-lamp lighting, expressive comedic posture, rich textures of brass, parchment, wood, and glowing green optics.
      </p>
    </details>
  </figcaption>
</figure>

## Next Up: Camp Meade

Prompt tuning and style guides can fix the roleplay, but Gas City has deeper
architectural mismatches for what I want to do next. It is 100% designed around
generating code. Because my Suitcase AI cluster is managed via GitOps, Gas City
does a good job as my sysadmin, but beyond that the mismatch is noticeable. The
assumptions that git is the workbench and PRs are the results seem baked in,
whereas I want a staff I (and eventually my family) can reach from Google Chat
for tasks that aren't just pull requests.

Gas City was also not built to use agent sandboxes beyond git worktrees. A git
worktree isolates file edits, but it does not isolate process execution,
credentials, or network access. Steve argues for "fences, not sandboxes": polite
policy checks that assume a well-behaved frontier model will respect a boundary.
[As I learned when an agent binary-patched its own runtime](/posts/i-asked-for-a-longer-timeout-the-agent-edited-the-opencode-binary/),
a polite fence doesn't stop an agent with host access. An operations staff needs
both: doctrinal fences _and_ hard sandboxes with verifiable identity per billet,
not just a separate git branch.

I evaluated several options against my hardware and security constraints:
[Temporal](https://temporal.io/) and [Kestra](https://kestra.io/) carry heavy
memory overhead for a small control plane, [Dagger](https://dagger.io/) lacks
long-lived state for human approval gates, and [CrewAI](https://www.crewai.com/)
burns tokens on inter-agent chatter without real sandboxing. Right now I'm
running experiments comparing [Restate](https://restate.dev/) and a
[Beads](https://github.com/gastownhall/beads)-backed task graph on Kubernetes.
The key requirements I have for the next installment of my A-Staff plan, Camp
Meade
([Eisenhower's next assignment](https://en.wikipedia.org/wiki/Fort_George_G._Meade)
after Camp Colt), are:

1. **USMC Staff doctrine without LARPing.**
2. **Cryptographic IDs** for each billet and worker.
3. **Strict sandboxes** tuned for each billet.
4. **Remote access** via Google Chat.
5. **Multi-user boundaries** to allow limited use by other family members.

For now, Camp Colt is in maintenance mode while I run some experiments to decide
what infrastructure to use for its replacement. I will share more when I have
something that is working.

---

## Join the Discussion

- Discuss on **[Hacker News](https://news.ycombinator.com/item?id=50133285)**
- Discuss on **[X / Twitter](https://x.com/chalko/status/2111510226079797680)**
- Join the conversation on
  **[LinkedIn](https://www.linkedin.com/posts/chalko_astaff-localai-aiarchitecture-activity-7517276129385443328-0UuH)**
