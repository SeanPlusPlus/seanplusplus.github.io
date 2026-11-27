---
layout: post
title: "Half a Chiclet at a Time"
date: 2026-11-27
---

A buddy told me his brother Joe keeps a bag of Chiclets in his car and only ever eats half a piece at a time. That's a fun habit, and it also turns out to be a probability problem.

## The Setup

Joe keeps a bag of Chiclets in his car. When we start, the bag holds `n` whole pieces and nothing else, with `n = 100` in our case. Joe only ever eats half a piece at a time. Whenever he wants gum, he reaches into the bag without looking and pulls out one item. He can't tell a whole piece from a half by feel, so every item in the bag is equally likely to be the one he grabs. If he pulls out a whole piece, he bites it in two, eats one half, and drops the other half back in. If he pulls out a half, he eats it.

We can describe the bag at any moment by two numbers: `w`, the count of whole pieces, and `h`, the count of halves. The bag starts at `(n, 0)`. On any draw, Joe picks a whole with probability `w / (w + h)` and a half with probability `h / (w + h)`. Picking a whole moves the bag to `(w - 1, h + 1)`, and picking a half moves it to `(w, h - 1)`. His first draw is always a whole, since no halves exist yet. Each whole piece takes exactly two draws to finish, one to split it and one to eat the leftover half, so the bag is empty after exactly `2n` draws however the luck falls. The number of wholes never goes up, the number of halves rises or falls by one with each draw, and the bag always ends at `(0, 0)`.

## The Question

How often, over the life of the bag, does Joe find it holding the same number of wholes and halves, meaning `w = h` with both above zero? At those moments his next draw is an even coin flip between a whole and a half. That count varies from bag to bag, so we want its expected value: averaged over many bags, how many times does Joe meet an equal bag before it runs out? The final empty bag doesn't count. We want the answer as a formula in `n`, then evaluated at `n = 100`.

## Working It Out

_Coming soon._
