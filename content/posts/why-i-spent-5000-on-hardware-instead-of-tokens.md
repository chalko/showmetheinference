---
title: "Why I Spent $5,000 on Hardware Instead of Tokens"
date: 2026-09-17T09:38:00-07:00
draft: false
summary:
  "Why I bought a $5,000 local inference box instead of burning a monthly cloud
  token budget, and how local hardware changes the way you build AI agents."
tags: ["AI", "Local AI", "Inference", "Hardware", "Multi-Agent", "Essays"]
categories: ["Essays"]
cover:
  image: "/images/suitcase-ai-minirack-gx10.jpg"
  alt:
    "The Suitcase AI 10-inch minirack with Unit 01 (control) and Unit 02 (ASUS
    Ascent GX10 inference), sitting next to the retail ASUS Ascent GX10 box."
  relative: false
  hidden: true
  hiddenInList: false
showtoc: false
---

When you leave Google, the first thing you lose is the illusion of infinite,
free compute. Suddenly, every token has an invoice attached to it. Watching
[Steve Yegge](https://yegge.ai) burn \$5,000 a month on Claude tokens for
[Wyvern](https://yegge.ai/wyvern.html) was inspiring. I—alas—am not Steve Yegge,
burning \$5,000 a month on a video game with active players that I've been
working on for 30 years. However, I did drop almost \$5,000 on an ASUS Ascent
GX10 to enable me to continue my AI learning without blowing my monthly budget
on tokens.

I wanted a self-contained, portable, sovereign compute node rather than a giant
rack in a server closet. Thus was born **Suitcase AI**—because the name reminds
me of a suitcase nuke, so why not?

The general idea is a 10" minirack packed with the inference, storage, and
general compute needed to handle real problems with a predictable cost: **Unit
02** (the ASUS GX10 on top) dedicated to sovereign GPU inference, and **Unit
01** (below) handling the control plane, hypervisor, and utility services.

<figure>
  <img src="/images/scai20260916.jpg" alt="A 10-inch minirack housing an ASUS Ascent GX10, custom patch panel, Netgear switch, and a local compute node for Suitcase AI." />
  <figcaption>
    <em>The Suitcase AI rig: A 10-inch minirack with the ASUS Ascent GX10 inference unit (Unit 02) on top and the local control plane node (Unit 01) below.</em>
  </figcaption>
</figure>

I spent several weeks just playing with different models and learning all kinds
of things. For example, oh my gosh, the models are huge and take a long time to
download—then as much as 10 minutes just to get the model loaded into memory!
This means I really need to plan more instead of just trying something new
because it's there.

After three weeks of conflicting environments, orphaned model weight caches, and
half-baked shell scripts, I finally decided I had made enough of a mess and
started over with a clean, reproducible setup for Suitcase AI. My
[suitcase-ai repository](https://github.com/chalko/suitcase-ai) is where all my
infrastructure as code lives. Of course, I couldn't resist using Kubernetes,
Vault, and all the other tools required for a reasonable homelab. But I was
putting more time into the infrastructure than actually learning about AI. The
infrastructure is up, and I can now turn my attention back to AI.

## The First Experiment: Hybrid Gas City

For my first experiment, I'm going to look at how I can use the
[Gas City](https://github.com/gastownhall/gascity) harness with frontier models
for human interaction, but rely on local models for the actual coding and other
smaller tasks. I experimented with this previously and got stuck mostly on the
watchdogs and other mechanisms needed to push things along; the smaller models
seemed to get stuck on the simplest tasks.

In ["Fences, Not Sandboxes,"](https://yegge.ai/essays/fences-not-sandboxes/)
Steve talked about how his agents wound up creating a large legal system to try
and codify answers to previous mistakes. That kind of overhead will completely
overwhelm local AI.

Steve was using all frontier models that have 200k+ context windows and
unlimited cloud compute; they can act like medieval parliamentarians drafting
bylaws. But a local model running on a maximum of 80GB of memory needs lean,
deterministic instructions. If you bury a local model under a mountain of
procedural "fences" and legal rules, it stalls under its own weight.

In fact, [Steve later found](https://yegge.ai/essays/seats-and-sunsets/) that
even Fable seemed to legislate itself into a corner.

Next week I will be back at it, and I'll let you know which model I selected and
how it worked on a sample task assigned to Gas City.
