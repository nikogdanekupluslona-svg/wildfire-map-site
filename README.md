# wildfire-map.org

Public GitHub Pages site for [US Wildfire Tracker](https://wildfire-map.org).

The root of this repository holds the **compiled web app** (`index.html`, `assets/`, icons, `welcome.html`). Do not edit those by hand — republish them from the app repository.

The `blog/` folder is different: it is **generated in this repository** and is not produced by the app build.

## ⚠️ Republishing the app must not delete `blog/`

When you copy a new app build into this repository, copy **only** the app's own files. If you wipe the repository root first, you delete the blog, `sitemap.xml` and `llms.txt` with it.

Files owned by this repository, not by the app build:

```
blog/                 generated article pages, blog index and CSS
sitemap.xml           generated — lists the home page and every article
llms.txt              generated — markdown site map for AI crawlers
robots.txt            hand-maintained — allows AI crawlers
seo-workspace/        article sources and the generator (git-ignored, keep a local copy)
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
  articles/<slug>/     body.html, meta.json, sources.json per article
```

Commands, run from the repository root:

```bash
python3 seo-workspace/build.py check <slug>   # validate one article
python3 seo-workspace/build.py check-all      # validate every article
python3 seo-workspace/build.py build          # regenerate blog/, sitemap.xml, llms.txt
```

`check` enforces the article rules: a 10,000-word floor, one `h1` with only `h2`/`h3` below it, headings under 60 characters, table-of-contents anchors that match section ids, tables with captions and header scopes, at least five FAQ entries, the trust badge and call-to-action blocks, no competitor names, no emoji outside the meta line, and no unknown CSS classes. `build` skips any article that fails, so a broken draft can never reach the site.

Each generated page carries a JSON-LD graph of Organization, WebSite, Article, BreadcrumbList, FAQPage and SoftwareApplication, plus HowTo where an article documents a procedure.

## GitHub Pages

1. Open **Settings → Pages**.
2. Set **Source** to **Deploy from a branch**.
3. Branch: `main`, folder: `/ (root)`.
4. Custom domain: `wildfire-map.org` (this repo includes a `CNAME` file).
5. After DNS is green, enable **Enforce HTTPS**.
