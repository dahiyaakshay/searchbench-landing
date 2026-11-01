# Searchbench, the project page

A single-file portfolio page for Searchbench, a local SEO workbench.
One HTML file, about 79 kB, with every style, script and illustration
inside it. The only thing it fetches is the Urbanist font from Google
Fonts.

Live at **https://dahiyaakshay.github.io/searchbench-page/** once the
steps below are done.

---

## Put it online

GitHub Pages serves a static file straight from a repository, free,
with HTTPS. The whole job is four steps.

### 1. Make the repository

On GitHub, **New repository**. Call it something like
`searchbench-page`. Public, and no README, since this file is it.

### 2. Add the file

The page has to be named `index.html` for Pages to serve it as the
root of the site. Rename it if it arrived as `searchbench.html`.

Through the browser, which is the quickest way for one file:

1. **Add file → Upload files** on the new repository
2. Drag in `index.html` and this `README.md`
3. **Commit changes**

Or from a terminal:

```bash
git init
git add index.html README.md
git commit -m "The Searchbench project page"
git branch -M main
git remote add origin https://github.com/dahiyaakshay/searchbench-page.git
git push -u origin main
```

### 3. Turn Pages on

**Settings → Pages** in the repository.

- **Source**: Deploy from a branch
- **Branch**: `main`, folder `/ (root)`
- **Save**

### 4. Wait a minute

The first build takes thirty seconds to a couple of minutes. The
Pages settings page shows the URL once it is live. Every push after
that republishes within about a minute.

---

## Your own domain

If you want `searchbench.akshaydahiya.site` rather than the
github.io address:

**At your DNS provider**, add a CNAME record:

| Field | Value |
| --- | --- |
| Type | CNAME |
| Name | `searchbench` |
| Value | `dahiyaakshay.github.io` |

**On GitHub**, in Settings → Pages, put `searchbench.akshaydahiya.site`
in the Custom domain box and save. Tick **Enforce HTTPS** once the
certificate is issued, which usually takes a few minutes and
occasionally an hour.

GitHub writes a `CNAME` file into the repository when you do this.
Leave it there; deleting it unsets the domain.

---

## Editing it

Everything is in `index.html`. There is no build step, no
`node_modules`, and nothing to install. Open it in a browser to see
changes, and in any editor to make them.

**The colours** are CSS custom properties at the very top of the
`<style>` block. Changing `--lime` recolours every button, tile,
glow and tick on the page, because nothing hardcodes the brand
colour.

**The shadows** are also variables: `--up` for raised surfaces,
`--in` for pressed ones, `--up-dark` and `--up-lime` for the dark and
lime tiles. They are deliberately eight layers deep; three layers
reads as flat.

**The sections** each have an `id` and appear in the page in the
order they appear in the file. Moving one is cut and paste. The nav
links point at those ids, so update both or the link goes nowhere.

**The animations** need nothing. Anything with `class="rv"` fades up
when it scrolls into view, any `[data-to]` counts up to that number,
and any `.fill` grows to its `data-w` percentage. All three respect
`prefers-reduced-motion`.

---

## What is on the page

| Section | What it covers |
| --- | --- |
| Overview | The project, the role, the problem, the stack |
| Numbers | 345 pages, 30+ checks, 439 tests, $0 to read |
| Walkthrough | One page followed from CSV to published, with the arithmetic |
| Capabilities | Twelve things the tool does |
| How it runs | The three phases |
| The crawl | AI retrievability, per-bot access, extractability |
| Where it fits | SEO, GEO, technical, programmatic |
| Decisions | Three choices, as problem, decision and outcome |
| Performance | 21.7s to 0.1s, 1,340 kB to 55 kB |
| Hard parts | The four problems that took longest |
| The checks | Twenty-five of them, as chips |
| Built | Backend, frontend, analysis, writing |
| Cost | A worked sum for fifty pages |
| The brief | What a page brief contains |
| Does it work | Four questions with evidence |
| Compare | Against a hosted platform, honestly in both directions |
| Questions | Six, answered |

---

## Worth knowing

**The social preview has no image.** Open Graph tags are in place, so
a shared link shows the right title and description, but no picture.
Add a 1200x630 PNG and one `<meta property="og:image">` line if you
post the link anywhere that shows cards.

**Two links need you.** The hero and the close both point at GitHub,
your portfolio, LinkedIn, AI Spider and MarAI. Check them after you
publish, in case a URL has moved.

**The cost figures are a worked example**, labelled as such on the
page. If you run the tool against real pages at scale, replace them
with what it actually cost.

**One claim is deliberately absent.** The page does not say the
generated content needs little editing, because that has not been
tested against a live model at scale. If you test it, that is a
section worth adding.

---

## Licence

Yours. Do what you like with it.
