# wildfire-map.org

Public GitHub Pages site for [US Wildfire Tracker](https://wildfire-map.org).

The root of this repository holds the **compiled web app** (`index.html`, `assets/`, icons, `welcome.html`). Do not edit those by hand — republish them from the app repository.

The `blog/` folder is different: it is **generated in this repository** and is not produced by the app build.

## ⚠️ Republishing the app must not delete `blog/`

When you copy a new app build into this repository, copy **only** the app's own files. If you wipe the repository root first, you delete the blog, `sitemap.xml` and `llms.txt` with it.

Files owned by this repository, not by the app build:

```
blog/                 generated guide pages, blog index and the shared CSS
fires/                generated state pages and the by-state hub
about/                hand-written methodology page
sitemap.xml           generated — the home page and every content page
llms.txt              generated — markdown site map for AI crawlers
robots.txt            hand-maintained — allows AI crawlers
seo-workspace/        page sources and the generator (git-ignored, keep a local copy)
```

## The blog pipeline

Article sources live in `seo-workspace/`, which is git-ignored — the repository publishes only the built output. Keep that folder backed up locally; without it the articles cannot be rebuilt.

```
seo-workspace/
  WRITER-RULES.md      the specification every article follows
  VERIFIED-FACTS.md    shared verified statistics and research technique
  cluster-plan.json    the keyword cluster: one entry per article
  competitors/         competitor research notes
  SEO-PLAN.md          the ranking strategy and its phases
  build.py             validator and site generator
  STATE-WRITER-RULES.md  what differs for a state page
  articles/<slug>/     body.html, meta.json, sources.json per guide
  states/<slug>/       the same three files per state
  homepage-patch/      the home page SEO change, to apply in the app repository
```

Commands, run from the repository root:

```bash
python3 seo-workspace/build.py check <slug>          # validate one guide
python3 seo-workspace/build.py check-all             # validate every guide
python3 seo-workspace/build.py check-state <state>   # validate one state page
python3 seo-workspace/build.py check-states          # validate every state page
python3 seo-workspace/build.py build                 # regenerate blog/, fires/, sitemap.xml, llms.txt
```

`check` enforces the guide rules: a 10,000-word floor (1,800 for a state page), one `h1` with only `h2`/`h3` below it, headings under 60 characters, table-of-contents anchors that match section ids, tables with captions and header scopes, at least five FAQ entries, the trust badge and call-to-action blocks, no competitor names, no emoji outside the meta line, and no unknown CSS classes. `build` skips any article that fails, so a broken draft can never reach the site.

Each generated page carries a JSON-LD graph. Guides use Organization, WebSite, Article, BreadcrumbList, FAQPage and SoftwareApplication, plus HowTo where the page documents a procedure. State pages use Article, State, BreadcrumbList and FAQPage, and carry a `geo.region` meta tag.

A state that already has a full-length guide, California and South Carolina, gets no state page. The hub links to the guide instead, so the two never compete for the same query.

## The outstanding SEO change

`seo-workspace/homepage-patch/` holds the one change that cannot be made in this repository, because the app build overwrites `index.html`. The home page currently serves crawlers an empty `<div id="root">`, so it ranks for nothing and passes almost no authority to the 65 content pages below it. The patch contains the static content block, the CSS needed to let the home page scroll past the map, and the structured data for its head. Apply it in the app source repository.

## GitHub Pages

1. Open **Settings → Pages**.
2. Set **Source** to **Deploy from a branch**.
3. Branch: `main`, folder: `/ (root)`.
4. Custom domain: `wildfire-map.org` (this repo includes a `CNAME` file).
5. After DNS is green, enable **Enforce HTTPS**.
