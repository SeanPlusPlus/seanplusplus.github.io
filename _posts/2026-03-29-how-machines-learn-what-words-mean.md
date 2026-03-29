---
layout: post
title: "How Machines Learn What Words Mean"
date: 2026-03-29
---

This started on a walk. Headphones in, group call with two buddies, roadie in hand, enjoying the night air on empty streets while nerding out about something that has no business being this interesting when you're 4 IPAs deep.

I'd been building RAG tooling for an AI agent at work and had gotten into embedding models (the thing that turns text into numbers so machines can reason about meaning). One of my buddies, a psych PhD, heard me describe how they work and said "that's just factor analysis." I didn't know what that was. He started explaining: compressing a bunch of measurements down to a few hidden factors. I kept pushing. How does compressing survey data connect to something like GPT? He kept going. The third guy kept asking good questions. And the more we talked, the more I realized the lineage was obvious once you saw it. I slept on it. The next morning I wrote code. He was right.

## The Core Insight

Imagine you survey 10,000 people and collect dozens of measurements: height, shoe size, steps walked per day, hours of sleep, screen time, resting heart rate. Most of these aren't independent. Height correlates with shoe size. People who walk more steps tend to sleep better.

There's hidden structure underneath — a smaller set of **latent factors** (physical build, activity level) that explain most of the variation. You can compress messy, high-dimensional data into a compact representation that captures what actually matters.

That compact representation is an **embedding**. And this is the whole game. Everything that follows is increasingly clever ways of doing this compression.

## The Psychologists Got There First

Charles Spearman noticed in 1904 that students who scored well on one type of test tended to score well on others. He proposed that a smaller set of hidden factors could explain the pattern. This was **factor analysis**: take observed measurements, extract the latent factors driving them.

**Principal Component Analysis (PCA)** is the mechanical version. Given a big spreadsheet, find the axes of maximum variance, project everything down. It's the "smoosh it into a dense space" technique — and it's still used constantly in data science today.

By mid-century, researchers could say "these 50 measurements are really driven by 5 underlying factors." Compress the 50 into 5, and you've built an embedding. They just didn't call it that.

## Words as Numbers

Here's where language enters the picture. In 1957, linguist J.R. Firth proposed that the meaning of a word is defined by the company it keeps — what words tend to appear nearby. "Cat" shows up near "fur," "purr," "pet." So does "kitten." "Spaceship" does not.

Researchers started counting co-occurrences across huge text corpora. Each word becomes a long row of numbers. Words in similar contexts get similar rows. The rows are enormous and sparse, so they applied the same compression trick: **Latent Semantic Analysis** used Singular Value Decomposition (a cousin of PCA) to compress each word down to a dense vector of ~300 numbers.

Then in 2013, a team at Google changed everything. **Word2Vec** trained a small neural network to predict a word from its neighbors. The byproduct was a dense vector for every word — and these vectors captured relationships so well you could do arithmetic with meaning:

> **Paris** − **France** + **Japan** ≈ **Tokyo**

Nobody programmed that. It emerged from the structure of language itself.

## Context Changes Everything

Every method up to this point had one big limitation: each word gets exactly one vector, forever. "Bank" the river and "bank" the financial institution get the same embedding.

The 2017 paper *Attention Is All You Need* introduced the **Transformer** — the architecture behind GPT, Claude, and basically everything in modern AI. The key innovation: every word's representation is informed by every other word in the sequence. "Bank" now gets a *different* vector depending on whether the sentence is about rivers or finance.

Modern embedding models are descendants of this. You feed in a sentence or document, and the model returns a single dense vector capturing the contextual meaning of the whole input.

## Seeing It Work

I've been experimenting with Amazon's Titan embedding model. The simplest possible test — embed three words and measure how "close" they are using cosine similarity (how much do the vectors point in the same direction):

```typescript
const terms = ["cat", "kitten", "spaceship"];
const embeddings = await Promise.all(terms.map(embed));
```

| Pair | Similarity |
|------|-----------|
| "cat" ↔ "kitten" | 0.44 |
| "cat" ↔ "spaceship" | 0.15 |
| "kitten" ↔ "spaceship" | 0.10 |

The model has never been told cats and kittens are related. It learned this from the structure of language — the same distributional hypothesis Firth described in 1957.

## Where It Gets Practical

The cat example is cute, but the real payoff is when you embed things that matter. I took 10 engineering tickets — a mix of performance work, React bugs, CI/CD tasks, and design updates — embedded each summary, and computed pairwise similarity:

```typescript
const tickets = [
  { id: "PROJ-101", summary: "Perf: Lazy-load hero image on landing page" },
  { id: "PROJ-201", summary: "Bug: useMemo dependency array missing in CartProvider" },
  { id: "PROJ-301", summary: "Integrate SonarQube static analysis into CI/CD pipeline" },
  // ... 10 tickets total
];

const embeddings = await Promise.all(tickets.map(t => embed(t.summary)));
```

The model instantly clusters them by meaning. Perf tickets land near each other. React hook bugs group together. CI/CD and monitoring pair off. No labels, no manual categorization — just the text.

Then I asked a plain English question and ranked tickets by relevance:

```typescript
const query = "performance optimization for web page loading";
const queryEmbedding = await embed(query);

const ranked = tickets
  .map((t, i) => ({ ...t, sim: cosineSimilarity(queryEmbedding, embeddings[i]) }))
  .sort((a, b) => b.sim - a.sim);
```

The perf tickets float to the top, even though none of them literally contain the phrase "performance optimization." This is **semantic search** — finding things by meaning, not keywords. It's how modern search engines and AI assistants retrieve relevant information.

## The Through-Line

| Era | Data | Method | Result |
|-----|------|--------|--------|
| 1904 | Test scores | Factor analysis | Latent psychological factors |
| 2000s | Word co-occurrences | SVD | Semantic word vectors |
| 2013 | Billions of words | Word2Vec | Meaning as arithmetic |
| 2017+ | Full documents | Transformers | Context-aware embeddings |

Every step is the same bet: **high-dimensional data has lower-dimensional structure, and you can learn to compress into that structure.** The psychologists had the right idea. It just took a century of better data, faster computers, and cleverer algorithms to apply it to language at scale.

My buddy was right. I owe him a beer.

---

*The code for all of this is at [github.com/SeanPlusPlus/embeddings-workshop](https://github.com/SeanPlusPlus/embeddings-workshop). Built with Bun, TypeScript, and Amazon Titan Embeddings v2 via AWS Bedrock.*
