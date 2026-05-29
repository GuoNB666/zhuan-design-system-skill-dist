# Harness Workflow For 转转 Design Work

This reference distills `harness-for-design.md` into a compact workflow for Open Design.

## Purpose

Harness thinking helps prevent AI style drift by using:

- project rules
- plan-first decomposition
- self-review loops
- rule updates after repeated failures

For 转转, use it as a guardrail around the existing design system, not as a new style source.

## Project Rule Pattern

Before execution, make the agent state:

- business context
- target user and main anxiety
- existing design-system components to reuse
- hard rules
- forbidden outputs
- objective review items

For 转转, typical hard rules are:

- use Simplified Chinese commerce labels
- preserve compact mobile marketplace density
- use existing tokens and components
- prioritize product image, price, condition, trust, filter, and search hierarchy

Typical forbidden outputs are:

- new visual system
- generic ecommerce theme
- decorative marketing hero
- unsourced fonts/colors/icons
- duplicated component variants

## Plan Mode

For non-trivial tasks:

1. Ask for or infer the information architecture.
2. Choose existing 转转 components and previews.
3. Execute the smallest useful artifact.
4. Self-review against preservation and UX checks.

Do not jump directly to detailed visuals for broad requests.

## Self-Review Loop

Separate review into:

- A class: objective checks the AI can verify, such as token usage, component reuse, no new fonts, no duplicate icons.
- B class: professional judgment, such as hierarchy clarity, density, commerce trust, and interaction clarity.
- C class: subjective preference. Do not force the model to hard-code these unless the user explicitly states them.

If A class fails, fix before final delivery.

## What To Report

Report deviations plainly:

- what drifted
- why it matters
- whether fixing it changes component internals
- the smallest safe correction
