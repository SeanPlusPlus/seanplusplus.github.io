---
layout: post
title: "The Half-Chiclet Problem"
date: 2026-11-27
---

This one came up on vacation this week. A buddy of mine has a math PhD, and his brother, who I'll call Joe, keeps a bag of Chiclets in his car and only ever eats half a piece at a time. He reaches in without looking, bites a whole one in two, and drops the other half back in. Joe turned that habit into a question for his brother: how many grabs until the bag holds exactly as many wholes as halves? My buddy worked it out, then told me the story over a couple of beers.

Alright, so my math buddy, being the absolute monster he is, went online, found a TI-84 emulator, programmed the problem into it, and got the answer. Then he kept going with an LLM and found that a differential equation describes it too.

I walked away inspired. I don't have a math PhD, but I do have tools. This post is me working the problem with those tools, to understand it better instead of just knowing the answer.

## The Setup

Joe keeps a bag of Chiclets in his car. When we start, the bag holds `n` whole pieces and nothing else, with `n = 100` in our case. Joe only ever eats half a piece at a time. Whenever he wants gum, he reaches into the bag without looking and pulls out one item. He can't tell a whole piece from a half by feel, so every item in the bag is equally likely to be the one he grabs. If he pulls out a whole piece, he bites it in two, eats one half, and drops the other half back in. If he pulls out a half, he eats it.

We can describe the bag at any moment by two numbers: `w`, the count of whole pieces, and `h`, the count of halves. The bag starts at `(n, 0)`. On any draw, Joe picks a whole with probability `w / (w + h)` and a half with probability `h / (w + h)`. Picking a whole moves the bag to `(w - 1, h + 1)`, and picking a half moves it to `(w, h - 1)`. His first draw is always a whole, since no halves exist yet. Each whole piece takes exactly two draws to finish, one to split it and one to eat the leftover half, so the bag is empty after exactly `2n` draws however the luck falls. The number of wholes never goes up, the number of halves rises or falls by one with each draw, and the bag always ends at `(0, 0)`.

## The Question

At some point the bag might hold exactly as many wholes as halves. That's the 50/50 moment, when Joe's next grab is a coin flip. The question is how many draws it takes to get there for the first time.

The fastest it can happen: Joe pulls a whole 50 times in a row, and on draw 50 the bag sits at `(50, 50)`. The slowest: the bag dodges every balance until it's down to `(1, 1)`, on draw 197. Every bag that balances lands somewhere in that window. Some bags never balance at all. A whole draw shrinks the gap between wholes and halves by two, so the gap can jump from 1 straight to -1 and skip zero.

So the answer comes as a distribution. Run a lot of bags, record the draw where each one first balances, and plot it. The tallest bar is our answer.

## Brute Force First

Before hearing how my buddy cracked it, here's my instinct: brute force the thing. Simulate Joe emptying a bag, note the draw where it first balances, do that for 1,000 bags, and look at the pile.

```python
import random
from collections import Counter

N = 100      # whole pieces to start
RUNS = 1_000

random.seed(27)


def first_balance(n: int) -> int | None:
    """Draws until wholes == halves, or None."""
    whole, half = n, 0
    for draw in range(1, 2 * n + 1):
        if random.randrange(whole + half) < whole:
            whole, half = whole - 1, half + 1
        else:
            half -= 1
        if whole == half > 0:
            return draw
    return None


results = [first_balance(N) for _ in range(RUNS)]
hits = [r for r in results if r is not None]
top, _ = Counter(hits).most_common(1)[0]

print(f"balanced: {len(hits)} of {RUNS} bags")
print(f"most common draw: {top}")
print(f"average draw: {sum(hits) / len(hits):.1f}")
```

Each call to `first_balance` is one bag. The `randrange` line is Joe's blind grab: pick a random item, and if it lands among the wholes, he got a whole. After every draw, check whether wholes equal halves with something still in the bag. The seed is fixed, so rerunning it gives the same numbers.

```text
$ python3 chiclets.py
balanced: 928 of 1000 bags
most common draw: 83
average draw: 92.7
```

<div><svg viewBox="0 0 640 300" role="img" aria-label="Histogram of 1000 simulated bags. The most common first balance is draw 83; 72 bags never balance." style="width:100%;height:auto;display:block;font-family:var(--mono)"><line x1="44" x2="612" y1="246.0" y2="246.0" stroke="#ececef" stroke-width="1"/><text x="38.0" y="251.0" text-anchor="end" fill="#6b7280" font-size="15" font-weight="400">0%</text><line x1="44" x2="612" y1="203.6" y2="203.6" stroke="#ececef" stroke-width="1"/><text x="38.0" y="208.6" text-anchor="end" fill="#6b7280" font-size="15" font-weight="400">2%</text><line x1="44" x2="612" y1="161.2" y2="161.2" stroke="#ececef" stroke-width="1"/><text x="38.0" y="166.2" text-anchor="end" fill="#6b7280" font-size="15" font-weight="400">4%</text><line x1="44" x2="612" y1="118.8" y2="118.8" stroke="#ececef" stroke-width="1"/><text x="38.0" y="123.8" text-anchor="end" fill="#6b7280" font-size="15" font-weight="400">6%</text><line x1="44" x2="612" y1="76.4" y2="76.4" stroke="#ececef" stroke-width="1"/><text x="38.0" y="81.4" text-anchor="end" fill="#6b7280" font-size="15" font-weight="400">8%</text><line x1="44" x2="612" y1="34.0" y2="34.0" stroke="#ececef" stroke-width="1"/><text x="38.0" y="39.0" text-anchor="end" fill="#6b7280" font-size="15" font-weight="400">10%</text><text x="49.3" y="267.0" text-anchor="middle" fill="#6b7280" font-size="15" font-weight="400">50</text><path d="M76.6,246.0V243.9Q76.6,241.8 78.7,241.8H83.0Q85.1,241.8 85.1,243.9V246.0Z" fill="#a9c1f5"><title>draw 59: 2 bags (0.2%)</title></path><path d="M87.1,246.0V240.5Q87.1,237.5 90.1,237.5H92.6Q95.6,237.5 95.6,240.5V246.0Z" fill="#a9c1f5"><title>draw 62: 4 bags (0.4%)</title></path><path d="M97.6,246.0V227.8Q97.6,224.8 100.6,224.8H103.1Q106.1,224.8 106.1,227.8V246.0Z" fill="#a9c1f5"><title>draw 65: 10 bags (1.0%)</title></path><path d="M108.1,246.0V174.8Q108.1,171.8 111.1,171.8H113.6Q116.6,171.8 116.6,174.8V246.0Z" fill="#a9c1f5"><title>draw 68: 35 bags (3.5%)</title></path><path d="M118.6,246.0V162.1Q118.6,159.1 121.6,159.1H124.1Q127.1,159.1 127.1,162.1V246.0Z" fill="#a9c1f5"><title>draw 71: 41 bags (4.1%)</title></path><path d="M129.1,246.0V128.2Q129.1,125.2 132.1,125.2H134.7Q137.7,125.2 137.7,128.2V246.0Z" fill="#a9c1f5"><title>draw 74: 57 bags (5.7%)</title></path><path d="M139.7,246.0V117.6Q139.7,114.6 142.7,114.6H145.2Q148.2,114.6 148.2,117.6V246.0Z" fill="#a9c1f5"><title>draw 77: 62 bags (6.2%)</title></path><path d="M150.2,246.0V68.8Q150.2,65.8 153.2,65.8H155.7Q158.7,65.8 158.7,68.8V246.0Z" fill="#a9c1f5"><title>draw 80: 85 bags (8.5%)</title></path><text x="154.4" y="267.0" text-anchor="middle" fill="#6b7280" font-size="15" font-weight="400">80</text><path d="M160.7,246.0V56.1Q160.7,53.1 163.7,53.1H166.2Q169.2,53.1 169.2,56.1V246.0Z" fill="#2563eb"><title>draw 83: 91 bags (9.1%)</title></path><text x="165.0" y="45.1" text-anchor="middle" fill="#1c1c1e" font-size="15" font-weight="600">draw 83 · 9.1%</text><path d="M171.2,246.0V60.3Q171.2,57.3 174.2,57.3H176.7Q179.7,57.3 179.7,60.3V246.0Z" fill="#a9c1f5"><title>draw 86: 89 bags (8.9%)</title></path><path d="M181.7,246.0V90.0Q181.7,87.0 184.7,87.0H187.3Q190.3,87.0 190.3,90.0V246.0Z" fill="#a9c1f5"><title>draw 89: 75 bags (7.5%)</title></path><path d="M192.3,246.0V102.7Q192.3,99.7 195.3,99.7H197.8Q200.8,99.7 200.8,102.7V246.0Z" fill="#a9c1f5"><title>draw 92: 69 bags (6.9%)</title></path><path d="M202.8,246.0V126.0Q202.8,123.0 205.8,123.0H208.3Q211.3,123.0 211.3,126.0V246.0Z" fill="#a9c1f5"><title>draw 95: 58 bags (5.8%)</title></path><path d="M213.3,246.0V132.4Q213.3,129.4 216.3,129.4H218.8Q221.8,129.4 221.8,132.4V246.0Z" fill="#a9c1f5"><title>draw 98: 55 bags (5.5%)</title></path><path d="M223.8,246.0V179.0Q223.8,176.0 226.8,176.0H229.3Q232.3,176.0 232.3,179.0V246.0Z" fill="#a9c1f5"><title>draw 101: 33 bags (3.3%)</title></path><path d="M234.3,246.0V193.9Q234.3,190.9 237.3,190.9H239.9Q242.9,190.9 242.9,193.9V246.0Z" fill="#a9c1f5"><title>draw 104: 26 bags (2.6%)</title></path><path d="M244.9,246.0V208.7Q244.9,205.7 247.9,205.7H250.4Q253.4,205.7 253.4,208.7V246.0Z" fill="#a9c1f5"><title>draw 107: 19 bags (1.9%)</title></path><path d="M255.4,246.0V206.6Q255.4,203.6 258.4,203.6H260.9Q263.9,203.6 263.9,206.6V246.0Z" fill="#a9c1f5"><title>draw 110: 20 bags (2.0%)</title></path><text x="259.6" y="267.0" text-anchor="middle" fill="#6b7280" font-size="15" font-weight="400">110</text><path d="M265.9,246.0V210.8Q265.9,207.8 268.9,207.8H271.4Q274.4,207.8 274.4,210.8V246.0Z" fill="#a9c1f5"><title>draw 113: 18 bags (1.8%)</title></path><path d="M276.4,246.0V219.3Q276.4,216.3 279.4,216.3H281.9Q284.9,216.3 284.9,219.3V246.0Z" fill="#a9c1f5"><title>draw 116: 14 bags (1.4%)</title></path><path d="M286.9,246.0V232.0Q286.9,229.0 289.9,229.0H292.4Q295.4,229.0 295.4,232.0V246.0Z" fill="#a9c1f5"><title>draw 119: 8 bags (0.8%)</title></path><path d="M297.4,246.0V238.4Q297.4,235.4 300.4,235.4H303.0Q306.0,235.4 306.0,238.4V246.0Z" fill="#a9c1f5"><title>draw 122: 5 bags (0.5%)</title></path><path d="M308.0,246.0V238.4Q308.0,235.4 311.0,235.4H313.5Q316.5,235.4 316.5,238.4V246.0Z" fill="#a9c1f5"><title>draw 125: 5 bags (0.5%)</title></path><path d="M318.5,246.0V243.9Q318.5,241.8 320.6,241.8H324.9Q327.0,241.8 327.0,243.9V246.0Z" fill="#a9c1f5"><title>draw 128: 2 bags (0.2%)</title></path><path d="M329.0,246.0V243.9Q329.0,241.8 331.1,241.8H335.4Q337.5,241.8 337.5,243.9V246.0Z" fill="#a9c1f5"><title>draw 131: 2 bags (0.2%)</title></path><path d="M350.0,246.0V244.9Q350.0,243.9 351.1,243.9H357.5Q358.6,243.9 358.6,244.9V246.0Z" fill="#a9c1f5"><title>draw 137: 1 bag (0.1%)</title></path><text x="364.8" y="267.0" text-anchor="middle" fill="#6b7280" font-size="15" font-weight="400">140</text><path d="M423.7,246.0V244.9Q423.7,243.9 424.7,243.9H431.1Q432.2,243.9 432.2,244.9V246.0Z" fill="#a9c1f5"><title>draw 158: 1 bag (0.1%)</title></path><path d="M465.7,246.0V244.9Q465.7,243.9 466.8,243.9H473.2Q474.3,243.9 474.3,244.9V246.0Z" fill="#a9c1f5"><title>draw 170: 1 bag (0.1%)</title></path><text x="470.0" y="267.0" text-anchor="middle" fill="#6b7280" font-size="15" font-weight="400">170</text><path d="M486.8,246.0V242.6Q486.8,239.6 489.8,239.6H492.3Q495.3,239.6 495.3,242.6V246.0Z" fill="#a9c1f5"><title>draw 176: 3 bags (0.3%)</title></path><path d="M497.3,246.0V244.9Q497.3,243.9 498.4,243.9H504.8Q505.8,243.9 505.8,244.9V246.0Z" fill="#a9c1f5"><title>draw 179: 1 bag (0.1%)</title></path><path d="M507.8,246.0V243.9Q507.8,241.8 509.9,241.8H514.2Q516.3,241.8 516.3,243.9V246.0Z" fill="#a9c1f5"><title>draw 182: 2 bags (0.2%)</title></path><path d="M518.3,246.0V244.9Q518.3,243.9 519.4,243.9H525.8Q526.9,243.9 526.9,244.9V246.0Z" fill="#a9c1f5"><title>draw 185: 1 bag (0.1%)</title></path><path d="M528.9,246.0V242.6Q528.9,239.6 531.9,239.6H534.4Q537.4,239.6 537.4,242.6V246.0Z" fill="#a9c1f5"><title>draw 188: 3 bags (0.3%)</title></path><path d="M539.4,246.0V242.6Q539.4,239.6 542.4,239.6H544.9Q547.9,239.6 547.9,242.6V246.0Z" fill="#a9c1f5"><title>draw 191: 3 bags (0.3%)</title></path><path d="M549.9,246.0V227.8Q549.9,224.8 552.9,224.8H555.4Q558.4,224.8 558.4,227.8V246.0Z" fill="#a9c1f5"><title>draw 194: 10 bags (1.0%)</title></path><path d="M560.4,246.0V213.0Q560.4,210.0 563.4,210.0H565.9Q568.9,210.0 568.9,213.0V246.0Z" fill="#a9c1f5"><title>draw 197: 17 bags (1.7%)</title></path><text x="564.7" y="267.0" text-anchor="middle" fill="#6b7280" font-size="15" font-weight="400">197</text><path d="M602.5,246.0V96.4Q602.5,93.4 605.5,93.4H608.0Q611.0,93.4 611.0,96.4V246.0Z" fill="#c4c8cf"><title>never balanced: 72 bags (7.2%)</title></path><text x="606.7" y="267.0" text-anchor="middle" fill="#6b7280" font-size="15" font-weight="400">never</text><text x="328.0" y="294.0" text-anchor="middle" fill="#6b7280" font-size="15" font-weight="400">draw on which the bag first balances</text></svg></div>

Draw 83 wins, edging out 86 by two bags and 80 by six. The pile climbs fast off the 50-draw floor, peaks in the low 80s, and trails off through the 100s. The average, 92.7, gets dragged right by that tail and by a little cluster of late bloomers at 191 to 197, bags that dodged balance almost to the end. Another 72 bags never balanced at all.

The bars only sit on every third draw, and that's just bookkeeping. Getting to `(k, k)` takes `n - k` wholes split plus `n - 2k` halves eaten, which is `2n - 3k` draws. Each step down in `k` adds three.

## Brute Force: Pros and Cons

This is the programmer move. It works, and it's also kind of meathead.

The good part is how little it asks of you. It's about twenty lines, and the only math in it is "pick a random item." The code follows the story line for line, so if the story is right, the simulation is right, and that makes it hard to screw up. It also shows you the shape of the thing. I wouldn't have guessed the lopsided pile, the late cluster near draw 197, or the bags that never balance at all. And whatever the math says later has to agree with this chart, so the simulation doubles as a referee.

The bad part is everything it can't tell you. It's noisy: with 1,000 bags, draws 80, 83, and 86 sit within a few bags of each other, and a different seed could crown a different bar. It answers one `n` at a time, so the result for 100 pieces says nothing about 1,000 without rerunning. It tells you where the peak is but not why it's there. And at the end you're holding a pile of samples, not a formula or a proof.

Brute force answers what happens. The rest of this post is about why.

## The Math

_Coming soon._
