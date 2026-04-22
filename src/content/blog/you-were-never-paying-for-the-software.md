---
title: "You were never paying for the software."
description: "The AI-kills-SaaS debate is asking the wrong question. SaaS was always a data product. AI doesn't change that — it makes it more true."
pubDate: 2026-04-22
tags: ["saas", "data", "ai", "product-management", "reflection"]
cover: "/images/smoobu-header.png"
---

![It's all about the data](/images/smoobu-header.png)

The AI-kills-SaaS debate is asking the wrong question.

People are watching agents write code, Claude build dashboards, and ChatGPT generate UIs in minutes, and drawing a logical conclusion: if the software layer is getting commoditised, SaaS is in trouble. Reasonable inference. Wrong conclusion.

Because it assumes SaaS was about selling software.

I sat in a room of Airbnb hosts last week — thirty people, startup office in Berlin, wine and pizza, one shared piece of software — and watched them accidentally make the case for the opposite.

---

## The stack nobody planned

These were not product managers or startup founders. They were a student monetising her empty nights, a couple covering the cost of family visits, a local manager running other people's properties, and a woman who had, without ever planning to, built a small hospitality company. Completely different people. Completely different use cases.

Every one of them had an identical structural problem.

Dynamic pricing through PriceLabs or Beyond. Automated guest messaging fired on booking status. Smart locks triggered by check-in windows. Multiple channels running in parallel — Airbnb, Booking.com, VRBO, some with direct booking sites. A few already running AI for listing copy and review responses.

Not one of these tools is native to any single platform. Every one is a third-party integration bolted onto a channel manager. And the channel manager — Smoobu — is the one node in this network that every other tool has agreed to treat as the source of truth.

Not because the interface is beautiful. Because the data is reliable.

---

## Reliability is the product, not the UI

Here's the thing nobody in that room said explicitly, but all of them understood:

They are not paying for software. They are paying for a canonical record of bookings they can trust.

The guarantee looks like this: when Booking.com and Airbnb and the direct booking widget all receive a reservation on the same Saturday night, exactly one guest shows up.

That sounds mundane. It is not mundane.

A pricing tool reading stale availability data sells a night that was already taken. A messaging automation fires against a cancelled booking. A revenue report pulls from one API and misses transactions on another. Every automation in the stack has the same single dependency: a central record it can trust. Break that record and the whole stack breaks with it — just at the worst possible moment, usually at 11pm, usually involving an angry guest at the door.

Strip away the interface and what Smoobu actually sells is data integrity as a service.

---

## I've seen this structural position before

I spent 3 years at 3YOURMIND building software for industrial manufacturers — Ford, the US Navy, SNCF — running additive manufacturing operations across SAP, ERP systems, machine APIs, and the inevitable spreadsheets.

Same problem, different industry. The moment you try to build analytics or scheduling or cross-site coordination on top of fragmented systems, you hit the same wall. You have to answer one question first: *which system is right?*

The most valuable thing I shipped was a reporting layer — one place where data had been reconciled and could be trusted. Ford valued the data layer more than the tooling it sat on top of. Not because building dashboards is hard. Because getting to data you would stake a real decision on is hard.

3YOURMIND: SAP + machine APIs + ERP → one canonical record of manufacturing operations.  
Smoobu: Airbnb + Booking.com + VRBO → one canonical record of bookings.

Different industry. Same structural position. Same actual product.

A missing part in a 3D printing order and a missing booking — different consequences, same root cause: something upstream was wrong, and every system downstream acted on it with confidence.

---

## AI doesn't diminish this. It amplifies it.

Here is where the AI discourse gets it precisely backwards.

The argument goes: AI commoditises the UI, therefore SaaS value erodes. Agents can hit APIs. LLMs can generate dashboards. The interface layer, once the expensive part, is now nearly free. So why pay for it?

You shouldn't. And you were never really paying for it.

You were paying for the data underneath. And that just got more important.

AI is a multiplier. It amplifies signal and noise equally — what's underneath determines whether you get speed or catastrophe.

**Clean data + AI = better decisions at speed.**  
**Dirty data + AI = worse decisions, faster, at scale.**

This is just GIGO — garbage in, garbage out — with a bigger blast radius. It is one of the oldest principles in computing, coined in 1963, and it has never been more relevant than right now.

Every new AI agent that reads your booking data, synthesises your pricing signals, or automates your guest communications adds another consumer that breaks when the canonical record is wrong. The more AI you layer onto a stack, the more the integrity of the data layer compounds in value. More automations mean more dependents. More dependents mean more failures when the source is corrupted.

The interface is free now. The guarantee underneath it is not.

---

## The gap will widen, not narrow

There's a simple bet you can make about the next five years of SaaS.

The companies with clean, trustworthy, well-structured data at their core will extract more value from AI than anyone else — because AI can actually act on what they have. Every model, every agent, every automation plugged into a reliable data layer performs. The same tools plugged into fragmented, stale, or inconsistent data confidently do the wrong thing — a double-booking at 11pm, or a cleaning team arriving to make up a bed for two when four guests are checking in. Faster. At scale.

The SaaS products that survive the AI transition are not the ones with the best interface — those are genuinely becoming free. They're the ones that have earned the right to sit at the centre of the data graph. The trust anchors. The canonical records. The systems that thirty independent operators, with no corporate mandate and no lock-in clause, all agreed to treat as the source of truth.

That agreement is not a product feature. It's years of conflict-resolution logic, webhook retry handling, and API versioning discipline — mapping the quirks of Booking.com's cancellation events, Airbnb's availability model, VRBO's pricing flags, and every other platform's idiosyncrasies into a single coherent record. It's the monitoring layer that catches a sync failure at 2am before a guest does. None of this shows up in a demo. All of it took years to get right. That's the moat.

---

The room in Berlin was not a room of software users. It was a room of operators who had each, independently, made a rational bet: that the data layer is the last thing you want to rebuild from scratch.

They're right.

---

*We list our flat on [Seumi Direct](https://seumi.vanderlore.de) — built on Smoobu's API. Direct bookings, no platform fee. It works because Smoobu's availability data is reliable enough to build on.*
