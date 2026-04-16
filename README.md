# Carousel-kit

[![npm version](https://img.shields.io/npm/v/social-carousel-bot-creator.svg)](https://www.npmjs.com/package/social-carousel-bot-creator)
[![npm downloads](https://img.shields.io/npm/dm/social-carousel-bot-creator.svg)](https://www.npmjs.com/package/social-carousel-bot-creator)
[![license](https://img.shields.io/npm/l/social-carousel-bot-creator.svg)](https://github.com/Trystan-SA/social-carousel-bot-creator/blob/main/LICENSE)
[![node](https://img.shields.io/node/v/social-carousel-bot-creator.svg)](https://www.npmjs.com/package/social-carousel-bot-creator)
[![GitHub stars](https://img.shields.io/github/stars/Trystan-SA/social-carousel-bot-creator?style=social)](https://github.com/Trystan-SA/social-carousel-bot-creator)

**Generate Social media images and carousel for TikTok, Instagram, X.com from HTML/CSS.** Themeable, scriptable, and designed to pair with [Claude Code](https://claude.com/claude-code) for AI-assisted content creation.

```bash
npx social-carousel-bot-creator init
npx social-carousel-bot-creator new intro-post --format 9:16
npx social-carousel-bot-creator render posts/intro-post
```

That's it — open `posts/intro-post/output/` and upload the PNGs.

> After a global install the short command `carousel-kit` is available too (`npm i -g social-carousel-bot-creator && carousel-kit render posts/intro-post`).

## Why carousel-kit?

Most social-media carousel tools are locked-in SaaS with rigid templates. If you care about design quality or want your post pipeline in version control, you're stuck.

carousel-kit is the opposite:

- **HTML/CSS is the source of truth**, full control of every pixel.
- **Theme once, reuse everywhere**, your palette and type live in `theme.css`.
- **Native export dimensions** for every major platform (9:16, 4:5, 1:1, 16:9, 3:4).
- **Watch mode** for live reload while you design.
- **Claude-Code-friendly**, optional AI review pipeline (strategist, copywriter, designer).
- **Docker fallback** when you'd rather not install Chrome locally.

## Install

carousel-kit needs **Node 20+** and a Chrome/Chromium binary (Puppeteer downloads one on first install; a system install works too).

```bash
npm install -g social-carousel-bot-creator
# or run ad-hoc with npx
npx social-carousel-bot-creator <command>
```

Once installed globally, the CLI binary is `carousel-kit` (shorter to type). The rest of this README uses that short form.

On minimal Linux systems you may also need native libs for Chromium:

```bash
sudo ./setup.sh   # apt-based
```

Don't want to touch your system? See [Docker](#docker).

## Quickstart

```bash
mkdir my-brand && cd my-brand
carousel-kit init
```

This creates:

```
my-brand/
├── CLAUDE.md         # Your brand / voice guide for Claude Code
├── theme.css         # Your palette, fonts, radii — edit freely
├── agents/           # (optional) AI review agents for Claude Code
├── posts/            # Your posts go here
└── assets/           # Images, photos, backgrounds
```

Create your first post:

```bash
carousel-kit new my-first-post
```

Edit the generated HTML in `posts/my-first-post/`, then render:

```bash
carousel-kit render posts/my-first-post
# → posts/my-first-post/output/slide-1.png ... slide-N.png
```

Or iterate with hot-reload:

```bash
carousel-kit watch posts/my-first-post
```

## Formats

Set the format per post in `carousel.json`:

```json
{ "format": "9:16" }
```

| Key    | Dimensions  | Platforms                        |
| ------ | ----------- | -------------------------------- |
| `9:16` | 1080 × 1920 | TikTok, Instagram Reels, Stories |
| `4:5`  | 1080 × 1350 | Instagram feed (portrait)        |
| `1:1`  | 1080 × 1080 | Instagram feed, X posts          |
| `16:9` | 1920 × 1080 | X, YouTube thumbnails, LinkedIn  |
| `3:4`  | 1080 × 1440 | LinkedIn carousels               |

Override on the fly:

```bash
carousel-kit render posts/my-post --format 1:1
```

List them anytime:

```bash
carousel-kit formats
```

## Theming

Everything visual is driven by CSS custom properties in `theme.css` at your project root:

```css
:root {
  --bg: #ffffff;
  --primary: #2563eb;
  --accent: #f59e0b;
  --text: #111827;
  --font-display: "Inter", system-ui, sans-serif;
  /* …etc */
}
```

Slides link to `../../theme.css` and can layer per-slide overrides in a `<style>` block. See [`examples/quickstart/`](examples/quickstart/) for a complete dark-mode theme.

## AI review (optional)

carousel-kit ships three Claude Code sub-agents that review your drafts sequentially:

1. **social-media-strategist** — hook, angle, format fit, value arc, audience, goal alignment
2. **social-media-copywriter** — CTA, word economy, slide transitions, emotional triggers, tone
3. **social-media-designer** — layout, whitespace, rhythm, typography, text density, color

Each returns HIGH / MEDIUM / LOW findings. You apply HIGH impact items; the rest are logged. `carousel-kit init` installs them into `agents/`, and the scaffolded `CLAUDE.md` explains how to trigger the pipeline in Claude Code.

Not a Claude Code user? The agents are plain Markdown specs — the review logic works as prompts with any LLM.

## Docker

Prefer not to install Chrome? The repo ships a `Dockerfile` that bakes in everything:

```bash
docker build -t carousel-kit .
docker run --rm -v "$PWD":/work -w /work carousel-kit render posts/my-post
```

## CLI reference

```
carousel-kit init                          Scaffold a project in the current dir
carousel-kit new <slug> [--format 9:16]    Create a new post folder
carousel-kit render <post>  [--format …]   Render slides to PNG
carousel-kit watch  <post>  [--format …]   Re-render on file change
carousel-kit formats                       List supported aspect ratios
carousel-kit version                       Print version
```

## Roadmap

- [ ] `carousel-kit export` → MP4 / GIF for animated carousels
- [ ] Built-in asset optimizer
- [ ] Template gallery (community themes)
- [ ] Direct upload hooks (Buffer, Typefully, …)
- [ ] Figma import

Have an idea? Open an issue.

## Contributing

PRs welcome — see [`CONTRIBUTING.md`](CONTRIBUTING.md).

## License

[MIT](LICENSE)
