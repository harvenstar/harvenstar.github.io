---
layout: post
title: Your LLM Agent Has Knowledge, Tools, and Fine-Tuning. What's Still Missing?
date: 2026-03-12 15:11:00
description: Why agents fail at specialized workflows — and the missing procedural layer.
tags: agents llm agent-skills
categories:
giscus_comments: false
related_posts: false
---

There is a pattern that keeps appearing across LLM agent deployments. The model knows the facts. The tools are connected. The fine-tuning is done. And yet, when the agent faces a real specialized workflow — synthesizing a chemical compound, navigating a clinical diagnosis protocol, generating a well-structured slide deck — it still fails. Not because it lacks knowledge. Not because it lacks tools. But because it doesn't know the procedure.

This is the gap that motivated our paper, "[Recipes for Agents: Understanding Skills and Their Open Questions](https://www.researchgate.net/publication/402491994)."

## The Three Bottlenecks

We identified three structural problems that persist even as LLM capabilities improve.

The first is diminishing returns from scaling. On WebArena, the best agent went from 14.4% to 57.1% success in roughly a year — a burst of gains driven by architectural innovation. But the subsequent eighteen months added only about 15 percentage points. Raw scaling is hitting a ceiling on realistic agent tasks.

The second is context inefficiency. In a study of software engineering agent sessions, the average task consumed 48,400 tokens across 40 steps, with tool outputs accounting for 63% of all context. Agents waste an enormous share of their token budget on failed attempts and irrelevant outputs. As tasks grow more complex, this becomes a binding constraint.

The third is the reasoning-execution gap. Frontier models now exceed 90% on AIME 2025. Yet in medicine, LLMs score 84–90% on knowledge assessments but only 45–69% on practice-based tasks requiring multi-step procedural execution. High reasoning ability does not translate to reliable specialized execution.

The bottleneck, we argue, is epistemological rather than computational. Domain-specialized 8B models consistently outperform general-purpose 110B+ models on their respective tasks. What's missing is not scale — it's procedural knowledge.

## What Is an Agent Skill?

An agent skill is a modular package of domain-specific procedural knowledge that an agent loads at inference time. It minimally contains three things: a scope declaration (what the skill does and does not cover), procedural instructions (how to execute the task), and fallback conditions (what to do when something goes wrong).

Conceptually, a skill is like a cooking recipe. It doesn't add new ingredients or kitchen tools. It tells you how to combine what you already have to reliably produce a good outcome.

Consider a PPTX skill. Without it, an agent might use the wrong library, crash on malformed inputs, and waste dozens of tokens on trial and error. With the skill loaded, it knows to use PptxGenJS instead of python-pptx, set slide dimensions first, embed fonts, and fall back gracefully when image conversion fails. The agent's underlying capabilities haven't changed. Its procedural guidance has.

This is different from RAG (which retrieves facts), tools (which execute single functions), and fine-tuning (which modifies weights). Skills reshape the agent's execution behavior across an entire session — persisting across steps, specifying when not to act, and composing with other skills.

## Early Evidence

The skills ecosystem is already growing at remarkable speed. One major marketplace grew from 2,179 to over 40,000 public skills in just 20 days. The anthropics/skills repository accumulated over 62,000 GitHub stars within four months of launch.

On the efficiency side, reusing a learned skill library has been shown to reduce interaction steps by 26% and generated tokens by 59%. Agents equipped with accumulated skill libraries explore 2.3× farther and unlock 3.3× more unique outcomes in open-ended environments — task opportunities that agents without skills simply cannot access.

## Ten Open Questions

Despite this momentum, the fundamental questions about skills remain unanswered. We identify ten.

On runtime use: How should agents select among thousands of skills? Can skills be both reliable and varied, or does procedural specificity suppress creativity? How do you prevent a loaded skill from influencing behavior beyond its declared scope? Where exactly do skills help, and where do they fall short?

On construction: How granular should a skill be — too fine and it becomes brittle, too coarse and it can't be composed? How do you ensure a skill written for GPT-4 still works reliably on a different model six months later? Who governs a skills ecosystem where the most valuable skills depend on rare domain expertise that may disappear if the maintainer moves on?

On quality and trust: How do you evaluate a skill without the same domain expertise needed to build it? How do you secure skills against prompt injection and tool-poisoning attacks, which already exceed 70% success rates on some real MCP servers? And as skills become training data for future models, will models collapse toward a narrow procedural canon, losing the behavioral diversity needed for adaptation?

These are not implementation details. They are foundational questions for anyone building or depending on agent systems.

## A Call for Contribution

The skills ecosystem is growing faster than the research community can study it, and the work ahead is shared. Developers should follow emerging standards, scope their skills clearly, and share them openly — a well-documented skill in a public registry becomes reusable infrastructure for many agents. Users should treat skills as a first-class mechanism, learn to compose them effectively, and report failures. And researchers have the open questions above to solve: contract schemas, discovery systems, verification pipelines, and governance models — none of this infrastructure exists yet.

Whether skills become a lasting systems layer or a transitional engineering pattern remains open. What is already clear is that they force a serious question: how should procedural knowledge be represented, shared, evaluated, and governed in agentic AI?

<hr style="margin-top: 2.5rem; margin-bottom: 1rem; border: none; border-top: 1px solid var(--global-divider-color);">

<p style="font-size: 0.8rem; font-style: italic; color: var(--global-text-color-light); margin-bottom: 0.4rem;">Cite this work</p>

<p style="font-size: 0.85rem; font-style: italic; color: var(--global-text-color-light); margin-top: 0;">
Hanwen Xing, Haomin Zhuang, Xuandong Zhao, Yue Huang, Zhenheng Tang, and Xiangliang Zhang. 2026. Recipes for Agents: Understanding Skills and Their Open Questions. In Proceedings of the 32nd ACM SIGKDD Conference on Knowledge Discovery and Data Mining V.2 (KDD '26), August 09–13, 2026, Jeju Island, Republic of Korea. ACM, New York, NY, USA, 7 pages. <a href="https://doi.org/10.1145/3770855.3818652" target="_blank" rel="noopener">https://doi.org/10.1145/3770855.3818652</a>
</p>
