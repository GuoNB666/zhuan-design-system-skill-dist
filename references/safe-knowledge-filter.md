# Safe Knowledge Filter

This reference distills the `design-knowledge-base` folder into safe and unsafe uses for the existing 转转 design system.

## Safe To Use As Review Signals

Use these as checks, not as style sources:

- UX hierarchy
- mobile usability
- accessibility basics
- form clarity
- search and filtering ergonomics
- performance awareness
- overlap and responsive layout checks
- anti-pattern detection
- industry reasoning when it aligns with second-hand marketplace flows

Useful files in the source knowledge base:

- `01-ux-guidelines.md`
- `02-anti-patterns.md`
- targeted parts of `06-industry-rules.md` for ecommerce and mobile apps
- targeted parts of `07-tech-stacks.md` only when the user asks about implementation stack
- `08-prompt-templates.md` only for structuring prompts/checklists

## Unsafe To Apply Directly

Do not apply these to 转转 artifacts unless the user explicitly asks to create a new visual direction:

- color palettes from `04-color-palettes.md`
- Google font pairings from `05-typography.md`
- UI styles from `03-ui-styles.md`
- generic industry recipes that conflict with the existing 转转 system
- shadcn token blocks, Tailwind palette recipes, or unrelated typography imports

## Conflict Rule

When generic knowledge conflicts with 转转 source files:

1. `DESIGN.md` wins.
2. `tokens.css` and `colors_and_type.css` win.
3. Existing `preview/` and `ui_kits/zhuan-app/` patterns win.
4. Generic knowledge may only explain a risk or propose a review question.

## Review Questions

Ask these when checking a generated artifact:

- Did the agent use the 转转 design system instead of a generic ecommerce system?
- Did it preserve the original component density and hierarchy?
- Did it avoid changing component internals?
- Did it avoid importing new colors, fonts, shadows, gradients, or icon styles?
- Did it keep all source-backed component states and content hierarchy?
- Are mobile hit areas, text wrapping, and visual hierarchy acceptable?
