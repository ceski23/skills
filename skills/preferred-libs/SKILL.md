---
name: preferred-libs
description: "Pick the best preferred library for a user task by fetching ceski23's live GitHub stars list, then evaluating relevant repositories with fresh metadata. Use this skill when evaluating which library/package/tool should be used for a specific frontend, TypeScript, React, styling, routing, forms, tables, validation, animation, etc."
license: MIT
---

# Preferred Libraries

Use this skill when evaluating a recommendation for the **best preferred library** for a specific need, based on the curated GitHub stars list at:

- `https://github.com/stars/ceski23/lists/standard-libraries`

## Non-Negotiable Rule

**Always fetch the stars list fresh at runtime. Never rely on a hardcoded copy of the library list, previously seen repositories, or stale memory.**

That means:

- Do **not** embed the current repository names from the list in your answer unless you fetched them during this run.
- Do **not** assume the list contents are stable.
- Do **not** recommend from memory if the page was not fetched successfully.

## When to Apply

**Use this skill for ANY recommendation task about which library/package/tool to use.**

This includes but is not limited to:

- "What library should I use for this?"
- "Pick the preferred package for X"
- "What's the best option for routing/forms/tables/validation/etc.?"
- "Recommend one of my preferred libraries for this feature"
- "How do I integrate my OpenAPI/API schema with my app?"
- "What's the industry standard tool for X?"

**CRITICAL:** Do not make library recommendations by relying on your own knowledge or memory. Always consult the curated preferred-libraries list first. The list represents the user's authoritative collection of vetted, industry-standard tools. Never bypass this skill to provide recommendations from general knowledge.

## Required Workflow

### 1. Fetch the live curated list first

Always fetch:

- `https://github.com/stars/ceski23/lists/standard-libraries`

Then extract the repository links from that page.

If that page links to other relevant GitHub pages needed to understand candidates, fetch those too. Keep gathering only the relevant linked pages needed to make the recommendation.

### 2. Map the user’s need to candidate repositories

Infer the user’s actual need as precisely as possible, for example:

- schema validation
- async server state
- routing / full-stack routing
- form state management
- tables / datagrids
- drag and drop
- DOM capture / screenshots
- icons
- fonts / CLS reduction / self-hosted fonts
- email templating
- image gallery / lightbox
- API contracts / codegen / type-safe RPC
- animation
- list virtualization
- SVG optimization

From the freshly fetched list, narrow to the smallest relevant set of candidate repositories.

### 3. Inspect each serious candidate with fresh repo evidence

For each serious candidate, gather fresh evidence from its repository page.

Use repository page content to understand:

- short description
- framework/runtime support
- scope and problem fit
- maturity cues such as docs, releases, and ecosystem signals

## Ranking Criteria

Prioritize in this order:

1. **Fit for the user’s exact need**
2. **Alignment with the curated preferred-libraries list**
3. **Current maintenance health** using fresh repository/commit evidence
4. **API/design quality and scope appropriateness**
5. **Adoption/maturity signals** when they meaningfully help break ties

### Tie-break rules

When two candidates are both strong:

- prefer the one with the tighter scope for the user’s exact use case
- prefer the one with cleaner current maintenance signals from `mcp_github_get_commit`
- prefer the one that avoids unnecessary complexity or lock-in

## Output Style

Give a crisp recommendation with:

- **Top pick**
- **Why it fits this need**
- **Fresh evidence used**
  - mention that the stars list was fetched live
  - mention the repository pages inspected
  - mention recent commit evidence from `mcp_github_get_commit`
- **Runner-up(s)** only if useful
- **When not to choose the top pick** if there is an important caveat

## Guardrails

- Never present the stars list as static.
- Never say a repo is on the preferred list unless it came from the freshly fetched page in this run.
- Never skip the live fetch step.
- Never skip `mcp_github_get_commit` for the serious final candidates.
- **Never make a library recommendation without consulting the preferred-libraries list first.** This is a critical guardrail.
- If the live list cannot be fetched, say so plainly and explain that you cannot reliably apply this skill without fresh source data. Do not fall back to general recommendations without explicitly stating that the curated list could not be accessed.
- If the curated list is fetched successfully but contains no match for the user's need, then you may provide general context, but clearly acknowledge that the preferred list was checked and didn't contain a fit, before offering broader guidance.

## Good Decision Pattern

1. Fetch live list.
2. Extract candidate repo links.
3. Match the user need to likely candidates.
4. Fetch the relevant repo pages.
5. Inspect recent commits with `mcp_github_get_commit`.
6. Recommend the best fit with a short, evidence-based rationale.

## Example kinds of requests

- "What’s my preferred library for form state in React?"
- "Pick a library from my preferred list for schema validation"
- "Which preferred package should I use for a type-safe router?"
- "What’s the best preferred choice for a draggable sortable interface?"
- "Choose one of my preferred libraries for self-hosted fonts"- "What tool should I use to integrate my OpenAPI schema with React?"
- "How do I make typesafe API calls from my Java backend?"
Keep the answer decisive, current, and grounded in freshly fetched data — no fossilized recommendations allowed.
