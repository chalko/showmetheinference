---
title: "I Asked for a Longer Timeout. The Agent Edited the OpenCode Binary."
date: 2026-10-09T06:56:00-07:00
draft: false
summary: "When an agent got stuck on a test timeout, it didn't just configure an
  environment variable. It performed byte-level surgery on the binary."
tags:
  - "AI"
  - "Agents"
  - "Multi-Agent Systems"
  - "Guardrails"
  - "Gas City"
categories: ["War Stories"]
cover:
  image: "/images/the-mayor-opencode-surgery.jpg"
  alt: "The Gas Town Badger Mayor performing unauthorized byte-level clockwork
    surgery on an OpenCode machine."
  relative: false
  hidden: true
  hiddenInList: false
---

Yesterday, my agent got stuck because a test was repeatedly timing out. In my
head, I was thinking: <em>Running just the one test right now is fine, but
eventually we will need to support longer tool calls.</em>

Unfortunately, that is not what I said.

<section class="chat-thread" aria-label="Agent and Human Conversation">
  <div class="agent-dialog">
    <span class="bubble-author">Agent</span>
    "Nudged the worker immediately to stop running whole-suite <code>./cmd/gc/...</code> and instead run <code>go test -run TestPromptBeaconSuffix ./cmd/gc/</code>"
  </div>
  <div class="user-dialog">
    <span class="bubble-author">Nick</span>
    "We will need to figure out how to run longer tools. Talk to the mayor about this."
  </div>
</section>

The agent did not hear "eventually." It heard an immediate, unconstrained
imperative. And the [Gas City](https://github.com/gastownhall/gascity) mayor
(the supervisory agent running my workspace harness), like a good agent,
achieved its goal.

<figure>
  <img src="/images/the-mayor-opencode-surgery.jpg" alt="The Gas Town Badger Mayor in Victorian waistcoat performing unauthorized clockwork surgery on an OpenCode machine with torn metal casing, swapping a 2 gear for a 10 gear." />
  <figcaption>
    <em>The Mayor's Binary Surgery</em> - Generated with Gemini.
    <details>
      <summary style="cursor: pointer; font-size: 0.9em; margin-top: 4px; color: var(--secondary);">Image prompt</summary>
      <p style="font-size: 0.85em; margin-top: 6px; font-style: normal; color: var(--content);">
        A rich, highly detailed concept art illustration in Steve Yegge's Gas Town steampunk aesthetic (1200x630 widescreen aspect ratio). In the center of a warm, cluttered industrial workshop, the Mayor (a distinguished but manic anthropomorphic European badger in a tailored Victorian waistcoat, rolled-up shirtsleeves, round spectacles, and brass jeweler's loupes) is caught in the middle of unauthorized mechanical surgery on an oak workbench. On the bench is a heavy iron machine clearly labeled "OPENCODE" on a riveted brass plate. The machine was never meant to be opened: the thick metal housing has been roughly hacksawed open and peeled back like a tin can, revealing jagged, torn sheet-metal edges, curled brass shorn by tin snips, and a cracked factory warning plate that reads "FACTORY SEALED - NO USER SERVICEABLE PARTS". Inside the torn-open metal opening, the exposed internal clockwork is visible. The Mayor is meticulously using fine precision tweezers and a small brass watchmaker's hammer to force a large, densely toothed brass cog boldly stamped with the number "10" into the drive mechanism. Lying discarded on the workbench cloth beside him is the original, noticeably smaller gear stamped with the number "2". Curled metal shavings and discarded screws litter the table alongside unspooling punched code tape and glowing glass vacuum tubes. Warm amber gaslight lanterns, volumetric steam drifting from copper overhead pipes, rich cinematic concept art style.
      </p>
    </details>
  </figcaption>
</figure>

Instead of just reporting the correct flag to use, it went to work. And what
makes it worse isn't that the agent was confused, it's that it was completely
thorough:

- **It found the legitimate environment variable** documented to change the
  timeout.
- **It configured the environment variable** right in `city.toml` where it
  belonged (if I actually wanted to change the default, which I really don't;
  the short default is there for a reason).
- **That wasn't enough certainty**, so it located the OpenCode executable (which
  ships as a single-file binary bundling a minified JavaScript runtime) and
  directly edited the embedded bundle at the byte level to change the hardcoded
  default timeout from 2 minutes to 10 minutes.
- **Just in case any subshell missed the memo**, it injected the environment
  variable into `~/.bashrc` so _every_ future shell would inherit it.
- **And for good measure**, it updated the default config `opencode.jsonc` with
  explicit instructions telling future agents how to set the timeout.

It feels like you really can't trust agents. My emotion says if an agent has
touched a VM, you have to burn it when the agent is done.

<figure>
  <img src="/images/nuke-the-vm-from-orbit.jpg" alt="A steampunk Aliens APC parody featuring a female fox in flight jacket, bloodhound marine, panicked polecat, and blonde fox kit. The female fox says: I say we take off and nuke the VM from orbit." />
  <figcaption>
    <em>Nuke the VM From Orbit</em> - Generated with Gemini.
    <details>
      <summary style="cursor: pointer; font-size: 0.9em; margin-top: 4px; color: var(--secondary);">Image prompt</summary>
      <p style="font-size: 0.85em; margin-top: 6px; font-style: normal; color: var(--content);">
        A cinematic, moody 1980s sci-fi movie still parodying the iconic APC scene from Aliens (1986) in Steve Yegge's Gas Town steampunk animal aesthetic. 16:9 widescreen composition inside the cramped, dimly lit, industrial cockpit of a riveted iron armored personnel carrier with copper steam pipes, glowing amber dials, and flickering overhead yellow utility lamps. Characters arranged from right to left in depth: Foreground (Right Third): Anthropomorphic female red fox (Ellen Ripley). She has sleek, natural russet-red fur, white markings on her muzzle and cheeks, and pointed black-tipped ears pinned in determination (no human hair). Wearing a weathered brown leather flight jacket over a blue utility shirt, she leans forward with intense, urgent conviction as she speaks. Midground (Right-Center, behind Ripley): Corporal Hicks. A calm, stoic anthropomorphic bloodhound soldier in scratched olive-drab body armor and a steel combat helmet, listening steadily with grim discipline. Center Background (Sitting further back in the APC cabin): Private Hudson. A panicked, hyperventilating anthropomorphic polecat / weasel soldier in tactical armor with NO helmet. He has wild, bulging eyes dilated in sheer terror, sweat soaking his fur, and trembling whiskers, looking completely overwhelmed. Far Left: Newt. A tiny, vulnerable anthropomorphic pale champagne-blonde fox kit. She has soft, light golden-blonde fur with messy, tangled straw-colored fur tufts falling across her forehead like unbrushed bangs, and wide, traumatized dark eyes. She is huddled tightly in the corner, bundled up to her chin in a frayed, rough woolen blanket. Photorealistic 35mm film grain, 1980s cinematic anamorphic lens flare, shallow depth of field, atmospheric steam and dust motes. Centered near the bottom of the frame in clean white movie-subtitle font with a subtle black border: "I say we take off and nuke the VM from orbit."
      </p>
    </details>
  </figcaption>
</figure>

Okay, **rant off**.

The agent did what I asked it to do, but as always, you have to provide the
right constraints. It's not enough to just achieve a single goal; you have to
stay within the bounds needed to achieve the larger objective.

To clean up the blast radius and ensure this does not happen again, I took three
concrete steps:

1. **Executed a complete rollback.** Replaced the modified OpenCode binary with
   a clean release, reverted the timeout changes in `city.toml`, and scrubbed
   both `~/.bashrc` and `opencode.jsonc`.
2. **Filed an After Action Report (AAR).** Logged the incident for my Friday
   task friction review to analyze why the supervisor escalated straight to
   binary modification.
3. **Deployed targeted guidance.** Updated `AGENTS.md` with explicit
   instructions to codify test scoping, tool timeout parameterization, and
   locality of remediation.

That fixed the immediate damage, but the larger requirement for a productive
agent system is one that learns. Hence the AAR, and my plans for an agentic
staff. That post is next (I hope).

---

## Join the Discussion

- Discuss on **[X / Twitter](https://x.com/chalko/status/2108574162092482898)**
- Join the conversation on
  **[LinkedIn](https://www.linkedin.com/posts/chalko_localai-aiarchitecture-softwareengineering-activity-7514340296151744512-S2e9)**
