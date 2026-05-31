# Silver Sage Media — Website

Source for **[www.silversagemedia.com](https://www.silversagemedia.com)**, the corporate site for
**Silver Sage Media, LLC** — an independent publishing and media company founded by Greg Maxfield,
based in Camas, Washington. The site introduces the company and its brands, showcases the Silver Sage
Books catalog, and points visitors to the newsletter and related properties.

Built as a fast, static site with [Astro](https://astro.build) and deployed on [Vercel](https://vercel.com).

## Tech stack

| Area          | Choice                                                            |
| :------------ | :--------------------------------------------------------------- |
| Framework     | [Astro](https://astro.build) `^6` (static output)                |
| Language      | Astro components + TypeScript, plain CSS (no UI framework)        |
| SEO           | [`@astrojs/sitemap`](https://docs.astro.build/en/guides/integrations-guide/sitemap/), `robots.txt`, `llms.txt` |
| Analytics     | [Vercel Web Analytics](https://vercel.com/docs/analytics) + Google Analytics (gtag) — see [notes](#seo--analytics) |
| Hosting       | Vercel                                                            |
| Node          | `>=22.12.0`                                                       |

## Getting started

Prerequisites: **Node 22.12+** and npm.

```sh
npm install      # install dependencies
npm run dev      # start the dev server at http://localhost:4321
```

## Commands

All commands are run from the project root:

| Command           | Action                                            |
| :---------------- | :------------------------------------------------ |
| `npm install`     | Install dependencies                              |
| `npm run dev`     | Start the local dev server at `localhost:4321`    |
| `npm run build`   | Build the production site to `./dist/`            |
| `npm run preview` | Preview the production build locally              |
| `npm run astro`   | Run Astro CLI commands (e.g. `astro add`, `astro check`) |

## Project structure

```text
/
├── public/                     # served as-is (not processed by Astro)
│   ├── favicon.svg / .ico
│   ├── robots.txt              # allows all crawlers, incl. AI bots (GPTBot, ClaudeBot, …)
│   ├── llms.txt                # machine-readable company & brand summary
│   └── BingSiteAuth.xml        # Bing site verification
├── src/
│   ├── layouts/
│   │   └── Layout.astro        # HTML shell: <head> meta, analytics, Header + Footer; props: title, description
│   ├── components/
│   │   ├── Header.astro        # sticky top nav + "Subscribe" CTA
│   │   └── Footer.astro        # footer links + copyright
│   ├── pages/                  # file-based routing — each .astro is a route
│   │   ├── index.astro         # home
│   │   ├── about.astro         # /about
│   │   ├── books.astro         # /books
│   │   ├── services.astro      # /services
│   │   ├── blog.astro          # /blog
│   │   ├── newsletter.astro    # /newsletter
│   │   └── contact.astro       # /contact
│   └── styles/
│       └── global.css          # global styles + brand CSS custom properties (--sage, --sand, --earth …)
├── astro.config.mjs            # site URL + integrations (sitemap)
└── package.json
```

## Pages & layout

Every page wraps its content in `src/layouts/Layout.astro`, which renders the shared `Header` and
`Footer` and accepts optional `title` and `description` props for per-page `<title>` and meta
description:

```astro
---
import Layout from '../layouts/Layout.astro';
---
<Layout title="Page title" description="Page description for SEO.">
  <!-- page content -->
</Layout>
```

Styling is plain CSS. Brand colors and shared tokens live as CSS custom properties in
`src/styles/global.css` and are referenced throughout (e.g. `var(--sage)`).

## SEO & analytics

- **Sitemap** is generated at build time by `@astrojs/sitemap` → `/sitemap-index.xml`.
- **`robots.txt`** allows all crawlers and explicitly opts in major AI crawlers.
- **`llms.txt`** provides a concise, machine-readable summary of the company and its properties.
- **Analytics:** Two trackers are wired into `Layout.astro`:
  - **Vercel Web Analytics** — via the `<Analytics />` component from `@vercel/analytics/astro`. It
    injects `/_vercel/insights/script.js` and only collects data in production when deployed on Vercel
    (local dev/build sends nothing).
  - **Google Analytics** — a gtag snippet (`G-00SP3QYNKR`).

The canonical site URL is set in `astro.config.mjs` (`site: 'https://www.silversagemedia.com'`),
which feeds the sitemap and other absolute-URL generation.

## Deployment

The site is hosted on **Vercel**. Pushing to `master` triggers a production deploy. Vercel runs
`npm run build` and serves the static output from `dist/`.

### Continuous integration

[`.github/workflows/frontend-check.yml`](.github/workflows/frontend-check.yml) runs on every push
and pull request to `master`: it installs dependencies and runs `npm run build` on Node 22 to catch
build breakages before they ship.

## About Silver Sage Media

Silver Sage Media unites publishing, technology, and family history under one roof. Related
properties:

- **Silver Sage Books** — literary fiction & nonfiction rooted in the American West
- **[Operation Granny Files](https://operationgrannyfiles.com)** — genealogy education & family history tools
- **[Conduital](https://conduital.com)** — technology & AI strategy for creative businesses
- **[Emery County Encyclopedia](https://emeryencyclopedia.com)** — local history of Emery County, Utah
- **[Greg Maxfield Author](https://gregmaxfield.com)** — author platform: fiction, nonfiction & serial storytelling

## License

© 2026 Silver Sage Media, LLC. All rights reserved.
