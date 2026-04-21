---
title: "CTR is not the story. It's the first sentence."
description: "Optimizing search ranking for a sponsored ads product taught me something about proxy metrics: CTR tells you one thing. It doesn't tell you everything — and the gap is where the interesting problems live."
pubDate: 2026-04-16
tags: ["search", "ranking", "product-management", "ads"]
hasMermaid: false
---
Working on search ranking for a sponsored ads product taught me something I didn't expect to learn.

It wasn't about the algorithm. It was about what the algorithm was measuring.

---

## The proxy problem

Search ranking at scale needs fast, available signals. Click-through rate is one. It's measurable in real time, accumulates quickly, correlates with relevance in most cases.

Most cases.

CTR measures willingness to click on a specific ad in a specific context. That's it. It is not conversion. The gap between the two is small for high-intent searches — someone who knows what they want, types it in, sees it, clicks it. The gap widens for discovery categories. Products people don't know they want until they see them.

That gap matters more than it looks on a dashboard.

---

## The disambiguation problem

Some words carry two lives.

In any language, a single term can belong to completely different semantic fields depending on context. Take a campaign for an advertiser selling premium collectibles. One of their keywords perfectly described what they sold — and also, in an entirely unrelated context, referred to something automotive.

Their ads appeared at high volume on queries from people looking for luxury cars.

High impressions. Near-zero CTR. Exactly what a relevancy cutoff analysis would flag for removal.

---

## Why the cut didn't work

The analysis ran. Cut the low-CTR, low-impression bucket. Lose roughly 10% of clicks. Gain around 1% improvement in conversion rate.

The trade looked poor. But the more interesting finding was why.

The hypothesis going in: accidental clicks don't convert. Remove the irrelevant placements, conversion improves.

The data didn't support it. The conversion rate of those low-CTR clicks wasn't measurably worse than the high-CTR clicks.

Think about what that means for a discovery advertiser.

Someone searching for luxury cars sees an ad for a vintage pocket watch. They weren't looking for it. They stopped. They thought — actually, I've been meaning to look at one of these. They clicked. They bought.

That conversion happened because of a bad placement.

The low CTR was the signature of this dynamic, not evidence against it. Most people scrolled past. That was expected. The few who didn't were reachable precisely because they found something where they didn't expect it.

Cut the placement, and you don't cut waste. You cut the discovery surface.

---

## Why this is structural, not an edge case

An intent-based advertiser — someone selling sofas to people searching "buy sofa" — has a simple relationship between CTR and conversion. The signal is clean. Optimize on it.

A discovery advertiser had the opposite dynamic. Their best customers might not have been looking for them. CTR was low everywhere except where intent already existed. Treating both the same and cutting on CTR removed exactly the inventory that discovery advertisers depended on.

Any marketplace with heterogeneous advertiser types carries this tension. It doesn't resolve cleanly. And it doesn't show up in aggregate metrics — only when you trace individual campaigns back to the user searching on the other end.

The right signal was downstream conversion. But conversion data is hard — slow to accumulate, dependent on advertiser cooperation, noisy enough that acting on it prematurely creates more problems than it solves.

The bucketing was built on the best available signal. It just wasn't the right one for this problem.

---

## What this means for anyone building these systems

The cutoff analysis wasn't wrong. It was optimized for the majority case.

The majority case is always where the gap hides.

Any ranking system built on engagement signals will, over time, systematically underserve advertisers whose value to users is non-obvious at the moment of search. Discovery categories. Niche specialists. Anything where purchase intent develops after exposure rather than before.

The answer isn't to abandon CTR. It's to know when CTR is the right proxy and when it isn't — and to build the conversion signal infrastructure that eventually makes that distinction measurable rather than guesswork.

That's a harder problem than the algorithm. It's also the more important one.
