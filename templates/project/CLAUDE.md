# Postkit project

This is a postkit workspace. Slides are HTML/CSS; the visual system is in
`theme.css`; posts live in `posts/<slug>/` and render to PNG. The **brand
profile** (voice, audience, handles, goals, hook formulas, …) lives in
plain Markdown files inside `memory/` at the project root — read it there,
write it there. **Do not store brand state in Claude Code's internal memory
system** for this project; everything lives on disk so it can be edited by
hand and versioned with the repo.

**All user workflows run through skills, not CLI commands.** The skills are in
`.claude/skills/`:

- `/postkit-setup` — interview the user and persist the brand profile to
  `memory/` (`brand_identity.md`, `brand_audience.md`, `brand_goals.md`,
  `brand_voice.md`, `brand_visual.md`, `brand_hooks.md`)
- `/postkit-idea` — brand-aware creative brainstorm + sequence planning,
  saves chosen sequences to `memory/post_ideas.md`
- `/postkit-new` — draft a new post (or a series of posts)
- `/postkit-render` — render a post's slides to PNG
- `/postkit-review` — critique a draft for strategy, copy, and design

## How to help in this project

1. **Check `memory/` first.** Before drafting or critiquing, read every
   `memory/brand_*.md` file. If any are missing, run `/postkit-setup` before
   anything else. Never guess voice, audience, or handles.
2. **Respect the design system.** Reuse the tokens and component classes in
   `theme.css` (`.heading-*`, `.body-*`, `.card`, `.pill`, `.tip-number`,
   `.watermark`, …). Only add per-slide overrides inside a `<style>` block.
3. **Slides live at `posts/<slug>/slide-N.html`** and link to `../../theme.css`.
   Each post has a `post.json` with `{ "format": "9:16" }` (or 4:5, 1:1,
   16:9, 3:4, or `"text"` for text-only posts with no slides).
4. **Captions live at `posts/<slug>/caption-<platform>.md`** — one file per
   target platform when a post ships to multiple.
5. **Never hand-run CLI commands for the user.** Invoke the right skill. The
   skills shell out to `npx postkit render` when needed.
6. **Brand state belongs in `memory/`, not Claude Code internal memory.**
   The user wants to read and edit these files directly.

## Post anatomy (default pattern)

- **Slide 1 — Hook.** Scroll-stopping in <1s. Specific number, bold claim,
  personal failure, or hot take.
- **Middle slides.** Each builds on the previous. No filler, no repetition.
  Open loops at slide ends so viewers swipe.
- **Final slide — Close.** Clear CTA + handle watermark. Watermark handle
  depends on the post's target platform — see `memory/brand_identity.md`.

## Files you own vs. files postkit manages

- **You own:** `theme.css`, this `CLAUDE.md`, `.gitignore`, everything in
  `posts/`, `assets/`, and `memory/`.
- **Postkit manages** (overwritten when the user runs `npx postkit` again):
  `.claude/skills/*`.

Edit the managed files freely inside a project, but expect them to be refreshed
when the user upgrades postkit.
