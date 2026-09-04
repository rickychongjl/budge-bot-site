# Budge Bot — marketing site

Static marketing site for Budge Bot. Built with Astro, no framework, no Tailwind,
plain CSS. Deploys as static HTML.

## Running it locally

You need Node 18 or newer. Check with `node --version`.

```bash
npm install     # once, downloads dependencies into node_modules/
npm run dev     # starts a local server, usually http://localhost:4321
```

Leave `npm run dev` running while you work. Save a file and the browser updates
by itself. Stop it with Ctrl+C.

```bash
npm run build   # writes the finished site into dist/
npm run preview # serves dist/ so you can check the built version
```

You never commit `dist/` or `node_modules/` — `.gitignore` already excludes both.

## What's where

```
src/
  pages/index.astro        the home page — all its copy lives here
  layouts/Base.astro       the html shell: head tags, fonts, nav, footer
  components/
    Nav.astro              header bar and logo
    Footer.astro           footer links and legal line
    PhoneHero.astro        the animated phone in the hero
    ChatDemo.astro         the interactive "try it" chat
  styles/global.css        every style on the site, including design tokens
public/
  favicon.svg              copied to the site root as-is
  robots.txt
```

Anything in `public/` is served unchanged at the site root, so `public/favicon.svg`
becomes `/favicon.svg`.

Astro files have two parts: JavaScript between the `---` fences at the top, which
runs at build time only, and markup below it. A `<script>` tag in the markup runs
in the visitor's browser as normal.

## Changing things

Copy, prices, FAQ questions — `src/pages/index.astro`. The steps and FAQ are
arrays near the top of that file; add an entry and it renders.

Colours and type — the `:root` block at the top of `src/styles/global.css`. Every
colour on the site is a variable there, so changing `--ochre` changes it everywhere.

The Telegram link — `TELEGRAM_URL` at the top of `src/pages/index.astro`. It's a
placeholder (`https://t.me/`) until the bot handle exists.

## Adding a page

Create `src/pages/pricing.astro` and it's live at `/pricing`. The filename is the
URL. Start it like this:

```astro
---
import Base from '../layouts/Base.astro';
---
<Base title="Pricing — Budge Bot">
  <section>
    <div class="shell">
      <h2>Pricing</h2>
    </div>
  </section>
</Base>
```

Still to build: `/pricing`, `/faq`, `/about`, `/support`, `/privacy`, `/terms`, and
a 404. The nav and footer currently link to sections on the home page instead.

## Deploying to Cloudflare Pages

Push this to GitHub first, then:

1. Sign in at dash.cloudflare.com and go to Workers & Pages
2. Create → Pages → Connect to Git, and authorise your GitHub account
3. Pick this repository
4. Set the build config:
   - Framework preset: **Astro**
   - Build command: `npm run build`
   - Build output directory: `dist`
5. Save and deploy

It builds and gives you a `.pages.dev` URL. After that, every push to `main`
redeploys automatically, and pushes to other branches get their own preview URL.

Nothing here needs environment variables or a server — it's static files.

## Notes

- No analytics is installed. Cloudflare Web Analytics is free and cookieless if
  you want numbers later.
- The chat demo is a front-end illusion. It parses the amount and matches a
  keyword list in `ChatDemo.astro`; it never contacts the real bot.
- Privacy and terms pages don't exist yet. The footer says so rather than linking
  to nothing.
