---
layout: post
title: "Claude Code as Career Coach"
date: 2026-03-03
---

Shit happens. Reorgs hit your team. The AI tool you just got comfortable with is already yesterday's news. The macro landscape shifts and suddenly the five-year plan you had feels like fiction.

The stress is real. And if you're in tech right now, you're feeling it — the anxiety of an industry that won't sit still long enough for you to catch your breath.

I live all of this. I'm a dad, I've got 15+ years in this industry, and it weighs on me viscerally. But instead of letting the anxiety dictate my day and getting constantly sidetracked, I've built a daily accountability loop with Claude Code that keeps me oriented toward my north star, even when the HN front page and my Twitter timeline are doing everything they can to knock me off course.

The fuel is simple: I use Bear to track what I do each day. Quick bullets — the task, the meeting, the PR, whatever. I used to half-ass this with two or three items. Now that I'm running it through the system below, I track a dozen or more things daily. Each one gets a `[x]` when done, and anything that doesn't gets carried forward.

The coach sees it all, grades it against my frameworks, and tells me where my time actually went versus where I said it should go. Carry-forwards that would have quietly died now have a system holding me to them.

Five weeks in and 20+ daily logs later, I can see patterns I never would have caught on my own. Days that felt unproductive turn out to be fine. Days that felt great sometimes aren't. The data doesn't lie, and having something review it honestly every single day changes how you show up the next morning.

## The System

Drop this into a fresh repo, fire up Claude Code, and start coaching yourself.

`OVERVIEW.md`

````markdown
# Claude Code Career Coaching System

A structured approach to career coaching using Claude Code's persistent context system. No custom tooling — just markdown files, a `CLAUDE.md` role definition, and a structured accountability loop.

**This doc is the blueprint.** It explains the architecture and patterns that make the system work. You don't need to write all the frameworks yourself — start a Claude Code session, describe your situation and goals, and have it help you build out `CLAUDE.md`, your frameworks, strategy docs, and log structure. The coach builds the coaching system with you.

## Why This Exists

Let's be real: you're not reading this if everything is fine. Nobody sets up a structured coaching system with daily accountability loops because they have zero notes on how their days are going. Something's off — or something could be better — and you want a system that helps you close that gap.

The gap doesn't have to be a promotion. It could be: you want to stop overworking, you want to shift from IC to management, you're trying to figure out what you even want next, you want to get better at the thing you already do, or you just want to spend less time on bullshit and more time on work that matters to you. All of those are valid. All of them benefit from a framework and an accountability loop.

This system assumes you want *something* to change. It doesn't assume what.

## What This Is

Claude Code loads `CLAUDE.md` automatically at session start. By defining a coaching role, decision frameworks, and a log structure in a git repo, you get a persistent career coach that:

- Knows your situation, goals, and patterns across sessions
- Grades your daily work against explicit frameworks (not vibes)
- Tracks trends over weeks, not daily pass/fail
- Calls out backsliding with specific evidence
- Evolves its coaching as your situation changes

The key insight: **structured context beats conversational memory.** Frameworks in files produce consistent, calibrated coaching. Frameworks in chat history drift and decay.

## Repo Structure

```
CLAUDE.md               # Role definition + coaching protocol
frameworks/             # Reusable decision frameworks
  daily-review.md       # The core accountability rubric
  leveling.md           # Level requirements (current vs target)
  ceremonies.md         # Meeting participation decision matrix
  prototyping.md        # Build philosophy and tradeoff patterns
strategy/               # Point-in-time situation analysis
  transition.md         # Current career transition plan
  org-leadership.md     # Org chart and power dynamics
  finding-your-edge.md  # Differentiation strategy
  the-hard-part.md      # Honest reckoning with what's difficult right now
log/                    # Daily/weekly/quarterly/EOY logs
  YYYY/MM/DD.md         # Daily: raw task list + coach feedback
  YYYY/WNN.md           # Weekly: pattern trends, weekly report signal
  YYYY/QN.md            # Quarterly: direction check, trajectory evidence
  YYYY/EOY.md           # EOY: full year review, feeds annual review
skills/                 # Skills inventory and gap analysis
  inventory.md          # Current strengths, gaps, quarterly targets
meta/                   # Coaching meta-docs
  conversation-style.md # What tone works, what doesn't
context/                # Git submodules (optional external context)
```

### Why Each Directory Exists

| Directory | Purpose | Key Design Choice |
|-----------|---------|-------------------|
| `frameworks/` | Stable rubrics the coach grades against | Frameworks, not prose — produces consistent evaluations |
| `strategy/` | Mutable situation docs that change with your career | Separated from frameworks so rubrics stay stable |
| `log/` | Daily/weekly/quarterly/EOY reviews | Git history = trend analysis over any timeframe |
| `skills/` | Gap analysis between where you are and where you want to be | Quarterly targets make abstract goals concrete |
| `meta/` | How the coach should behave | Explicit tone calibration prevents generic advice |

## The Review Cadence

The daily review is the core mechanism. But daily alone isn't enough — patterns emerge at different timescales. This system uses four tiers that build on each other. You don't need all four from day one. Start with daily, add the others when they'd be useful for your situation.

### Daily: The Accountability Loop

Dump what you did. Raw task list, links to artifacts, whatever format. No fixed time — morning, evening, whenever works for you. The coach categorizes each item against `frameworks/daily-review.md`:

- **Good**: Work that maps to your stated goals — whatever those are
- **Questionable**: Could go either way — coach applies context (time cost, phase, priorities)
- **Flag**: Work that pulls you away from what you said matters

Your goals don't have to be "get promoted." They could be: ship a side project, get better at system design, transition from IC to management, stop working 60-hour weeks, get your first engineering job, or just spend more time on the work that energizes you. The framework grades against *your* definition of good, not a universal one.

Then the coach gives an honest percentage breakdown with specific evidence, identifies what's missing, and looks forward: what should tomorrow look like? What's the weekly trend?

**Missed days**: Life happens. If you skip a day, the coach notes it lightly and moves on. No guilt trips — the system should make you *want* to check in, not dread it.

### Weekly: Pattern Recognition

If you do weekly write-ups (for your boss, for yourself, for a journal — doesn't matter), point the coach at them. Weekly reports are more polished than daily dumps and often surface the real wins and themes. The coach uses these as additional signal — not to re-grade the week, but to spot patterns worth tracking for longer-term reviews.

### Quarterly: Direction Check

Step back. Review accumulated dailies and weeklies from the quarter. Is the trajectory on track? What shifted? What patterns emerged? What should change for next quarter? This is where the daily noise resolves into actual signal about whether your time allocation matches your goals.

### EOY: The Full Picture

A beefy version of quarterly that covers the full year. If you have an annual review process at work, this is the prep doc. Pull from all the quarterly reviews + weeklies + dailies. Focus on: what you actually accomplished, where you grew, what evidence supports your goals, and the narrative for whatever comes next.

### Calibration Rules

These prevent the coach from being useless:

- **Phase-grading**: Grade against where you are now, not where you'll be in 12 months
- **Time-weighting**: A 5-minute fix is not the same as a 3-hour deep dive. Don't penalize speed.
- **Trend over score**: Track patterns over weeks. One bad day is noise. A bad week is signal.
- **Emotional honesty**: "You're holding onto this because letting go is hard" is valid coaching. Generic advice is not.

## How to Set Up Your Own

### Step 1: Define the role in CLAUDE.md

```markdown
# Career Coaching Context

## Role
You are a career coach for [name] — [current title/situation].
Coaching style: [direct/supportive/framework-driven/etc.]

## Goal
[What you're working toward — promotion, skill change, work-life balance,
 first job, side project, management transition, whatever it is]

## Review Cadence
[Daily/weekly/quarterly/EOY — define what works for you]

## Key Context
- Current role and responsibilities
- What "good" looks like for you right now
- Current situation (new job, transition, stable and optimizing, etc.)
- Priorities — yours, your boss's, or self-directed
```

The role definition is the most important file. Be specific about:
- The gap you're trying to close — what's different between now and where you want to be
- What "good work" and "wasted time" mean for *your* situation
- What you're trying to stop doing, start doing, or do differently

### Step 2: Build your daily review framework

Create `frameworks/daily-review.md` with:

- **Explicit categories**: What counts as goal-aligned work? What's a distraction? What's ambiguous?
- **Time allocation targets**: What % should go where? Adjusted for your current phase.
- **Test questions**: Before doing any work, ask these questions to categorize it
- **Calibration rules**: What's a real red flag vs. noise?
- **Division of responsibilities**: If you're transitioning work to others, who owns what and when?

### Step 3: Document where you are vs. where you want to be

Create `frameworks/leveling.md` — or whatever makes sense for your goal. If you're targeting a promotion, use your company's actual leveling framework. If you're building a skill, define what beginner/intermediate/advanced looks like. If you're optimizing for sustainability, define what a healthy week looks like. The point is an explicit rubric the coach can grade against.

### Step 4: Write the strategy docs

`strategy/` captures your specific situation — a reorg, a job search, a career pivot, a new role, or just "I'm stable and optimizing." These docs change as your situation changes. Frameworks don't (much).

### Step 5: Calibrate the conversation style

`meta/conversation-style.md` tells the coach how to talk to you. This matters more than you think. Generic coaching advice is useless. Coaching that matches your energy, uses your language, and meets your emotional reality actually changes behavior.

### Step 6: Start logging

Start with daily logs. Send your day, get feedback, course-correct. Add weekly reviews when you want pattern recognition across days. Add quarterly when you need a direction check. Add EOY when annual review season hits. The git history becomes your career trajectory over time.

## Getting Started

You don't need all of this on day one. Here's the minimum viable setup:

1. **Create a fresh repo** — private, just for you
2. **Drop in a `CLAUDE.md`** with your role, goals, and how often you want to check in
3. **Start a Claude Code session** — the coach helps you build the rest (frameworks, strategy docs, log structure)
4. **Send your first day** — raw task list, whatever format

That's it. You can also drop in this `OVERVIEW.md` as a reference and a short `README.md` explaining what the repo is. Add `frameworks/`, `strategy/`, `skills/`, `meta/` as they become useful — not before. The system should serve you, not the other way around. Adapt the structure to what works for your situation.

## Key Design Decisions

### Frameworks over prose

Prose coaching drifts. "Focus on strategic work" means nothing after week 2. A framework with explicit categories, test questions, and phase-adjusted targets produces consistent, calibrated feedback every session.

### Phase-grading

If your goal has phases (month 1 of a job search vs. month 6, or week 1 of learning Rust vs. week 12), grading against end-state standards is demoralizing and inaccurate. The daily review framework can define explicit phases with different targets for each. The coach grades against where you are, not where you'll be. Even if your goal is stable ("spend 60% of my time on deep work"), phase-grading helps during the adjustment period.

### Time-weighting

Not all tactical work is equal. A 5-minute AI-assisted fix that ships the same day is not the same as a 3-hour debugging session. The framework distinguishes between these and prevents false flags.

### Separation of frameworks and strategy

Frameworks are stable rubrics — they change maybe quarterly. Strategy docs are mutable — they change when your situation changes (new role, new boss, new project, new priorities). Keeping them separate means your coaching rubric stays consistent even as context evolves.

### Conversation style as explicit config

Without `meta/conversation-style.md`, the coach defaults to generic helpful-assistant mode. With it, the coach knows: be direct not hedging, use frameworks not lectures, acknowledge emotional reality without minimizing it, match energy level.

### Git-native logging

Daily logs in git mean:
- Full history of your trajectory
- Diffable progress over any timeframe
- Weekly rollups aggregate daily logs (or pull from external sources like boss reports)
- No external tools needed

## What This Doesn't Do

- **Not therapy**: Strategic career coaching with emotional honesty. Not a replacement for actual support.
- **Not a planner**: Doesn't manage your calendar or tasks. You decide what to work on; the coach evaluates whether those choices serve your goals.
- **Not a resume builder**: Focused on daily behavior change and trajectory, not artifacts.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (loads `CLAUDE.md` automatically)
- A git repo with the structure above
- Willingness to send your day and hear honest feedback

## The Meta Point

The value isn't the AI. It's the **structured accountability loop**. The frameworks force clarity about what matters. The daily log forces honesty about where time actually goes. The coach applies the frameworks consistently and catches patterns you can't see yourself.

Claude Code is the delivery mechanism. The system is the structure.
````

That's it. Go build yours.
