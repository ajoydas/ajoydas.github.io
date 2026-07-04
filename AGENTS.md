# Agent Instructions for ajoydas.github.io

## Repository Summary

This is a **personal portfolio website** for Ajoy Das, built as a **Jekyll-based GitHub Pages site** using the [portfolYOU](https://github.com/yousinix/portfolYOU) remote theme. It is deployed at [https://ajoydas.com](https://ajoydas.com). The site includes a blog, professional/hobby/coursework project showcases, certifications, skills, research publications, activities, photography gallery, and an about/experience timeline.

**Type:** Static site (Jekyll)  
**Language:** Markdown, HTML, Liquid, SCSS, YAML  
**Theme:** `yousinix/portfolYOU` (remote theme via `jekyll-remote-theme`)  
**Hosting:** GitHub Pages  
**Domain:** `ajoydas.com` (configured via `CNAME` file)

---

## Build & Development

### Prerequisites
- **Ruby** >= 3.x (tested with 3.4.1)
- **Bundler** >= 2.x (tested with 2.6.2)

### Bootstrap (first time)
```bash
bundle install
```
> Always run `bundle install` before building if gems have not been installed.

### Build
```bash
bundle exec jekyll build
```
Generates the static site into `_site/`. Build completes in ~4 seconds.

### Serve Locally (development)
```bash
bundle exec jekyll serve --watch
```
Serves at `http://localhost:4000`. The `--watch` flag enables auto-regeneration on file changes. Note: `_config.yml` changes require a restart.

### Known Build Notes
- A warning `To use retry middleware with Faraday v2.0+, install faraday-retry gem` may appear. It is benign and does not affect the build.
- The remote theme is fetched on each build. Builds require internet access.
- The `_site/` directory is the build output and is gitignored. Do not edit files inside `_site/` directly.

---

## Project Layout

### Root Files
| File | Purpose |
|------|---------|
| `_config.yml` | Main Jekyll configuration: site title, URL, description, author info, analytics, navbar, collections, plugins |
| `CNAME` | Custom domain (`ajoydas.com`) |
| `Gemfile` | Ruby dependencies (`github-pages` gem) |
| `README.md` | Project overview, development guide, and content management instructions |
| `robots.txt` | SEO: allows all crawlers, points to sitemap |

### Key Directories

| Directory | Purpose |
|-----------|---------|
| `_posts/` | Blog posts in `YYYY-MM-DD-slug.md` format. Front matter: `title`, `tags`, `style`, `color`, `description` |
| `_projects/` | Project cards with `(NN) Name.md` naming. Front matter: `name`, `type` (professional/hobby/coursework), `tools`, `image`, `description`, `external_url` |
| `_data/` | YAML data files: `timeline.yml` (work experience), `social-media.yml` (social icons), `photos.yml` (photography gallery data) |
| `_includes/` | Liquid partials: `navbar.html`, `head.html` (SEO meta tags, JSON-LD schema), `footer.html`, `scripts.html` (GLightbox, gallery toggle JS), `landing.html` (homepage), `about/timeline.html`, `about/skills.html`, `projects/index.html`, `blog/post-card.html` |
| `_layouts/` | `default.html` (main layout), `page.html` (content pages) |
| `_sass/` | SCSS stylesheets including `portfolYOU.scss`, `_variables.scss`, `_blog.scss`, `_timeline.scss`, etc. |
| `pages/` | Top-level pages: `about.md`, `blog.html`, `skills.md`, `certifications.md`, `activities.md`, `research.md`, `photography.md`, `index.md` |
| `pages/projects/` | Project listing pages by type: `professional.md`, `hobby.md`, `coursework.md` |
| `scripts/` | `process-photos.sh` — image processing script for the photography page |
| `assets/css/` | `style.scss` — main stylesheet that imports theme and custom styles (blog cards, photography gallery, show-more pagination) |
| `assets/js/` | `theme.js` — light/dark mode toggle |
| `assets/img/` | Images organized by: `posts/`, `projects/`, `certifications/`, `photography/` (with `converted/`, `thumbs/`, and source directories) |

### Content Conventions

**Blog Posts** (`_posts/`):
```yaml
---
title: "Post Title 🚀"       # double-quote if it contains a colon or emoji; titles usually end with an emoji
tags: [tag1, tag2, tag3]     # 3-5 short tags, lowercase except proper nouns (e.g., ADK, AI agents, LangGraph)
style: border                # always `border` (existing posts keep a trailing space after the value)
color: primary               # see color semantics below
description:                 # left empty by convention
---
```
- File naming: `YYYY-MM-DD-slug.md`
- Color semantics: `success` = achievements/certifications, `primary` = technical/AI posts, `info` = conference/event recaps, `secondary` = conceptual deep-dives/syntheses, `warning` = retrospectives/reflections
- Body house style:
  - Open with a cover image block: `<br/>` + `<img src="/assets/img/posts/<name>.png" alt="..." style="width: 100%; height: auto;"/>` + `<br/>`
  - First-person voice with generous emoji use; `####` headers structure longer posts; `**bold**` for names of people, companies, and products
  - Close with a gratitude line (🙏) and/or a forward-looking sentence
  - No `#hashtag` lines in the body — use front-matter `tags` instead; no "share on LinkedIn" CTA links (older posts have them; new posts should not)
  - When adapting LinkedIn drafts: keep the emojis, drop hashtag lines and `<aside>` publishing-metadata blocks
- Cover images go in `assets/img/posts/` and are referenced as `/assets/img/posts/filename.png`; naming: `<topic>-cover.png` for designed covers, `<topic>-1.jpeg`/`<topic>-2.jpeg` for photos
- The first `<img>` tag in a post is automatically extracted and used as the blog card cover image (via `_includes/blog/post-card.html`)

**Projects** (`_projects/`):
```yaml
---
name: Project Name
type: professional  # professional|hobby|coursework
tools: [Tool1, Tool2]
image: ../assets/img/projects/image.png
description: Description text (HTML supported)
external_url: https://example.com
---
```
- File naming: `(NN) ProjectName.md` where NN is a sort-weight number (lower = higher priority)

**Pages** (`pages/`):
- Front matter includes `weight` for navbar ordering (lower weight = appears earlier)
- Pages listed in `nav_exclude` in `_config.yml` are hidden from the navbar
- `activities.md` sections are ordered: Workshops & Events (first), then Books, then Courses

**Photography** (`_data/photos.yml`):
- Data-driven page: all photo metadata is in `_data/photos.yml`, template is in `pages/photography.md`
- Photos are organized into categories (mountains, europe, cities, coasts, nature)
- Each category shows 8 photos initially; a "Show All" button reveals the rest
- Photos use GLightbox for lightbox viewing with zoom/touch/drag support (loaded conditionally via `glightbox: true` front matter)
- Images: full-size in `assets/img/photography/converted/`, thumbnails in `assets/img/photography/thumbs/`
- Use `scripts/process-photos.sh <input_dir>` to batch-process new photos (HEIC/JPG/PNG → JPEG, generates converted + thumbs + YAML)
- Input files must be named `category - description.ext` (e.g., `mountains - Moraine Lake.heic`)
- Photos can also be added manually: place JPEGs in `converted/` and `thumbs/`, then add entries to `photos.yml`
- Copyright notice is displayed at the bottom of the photography page

### Navigation
The navbar is generated dynamically from pages sorted by `weight`. Projects have a dropdown menu (Professional, Coursework, Hobby) hardcoded in `_includes/navbar.html`. The `AGENTS.md` file is excluded from the navbar via `nav_exclude` in `_config.yml`.

---

## SEO & Social Sharing

- **Open Graph tags**: `og:type`, `og:title`, `og:description`, `og:image` (absolute URL), `og:url`, `og:site_name` — in `_includes/head.html`
- **Twitter cards**: `summary_large_image` with title, description, and image — in `_includes/head.html`
- **JSON-LD Person schema**: Structured data with name, URL, image, jobTitle, worksFor, and sameAs (LinkedIn, GitHub, Google Scholar) — in `_includes/head.html`
- **Sitemap**: Auto-generated by `jekyll-sitemap` plugin at `/sitemap.xml`
- **robots.txt**: Allows all crawlers, references sitemap
- **Google Analytics**: Enabled via GA4 tracking ID `G-3BD4D1PDK3` in `_config.yml`
- **`site.url`**: Set to `https://ajoydas.com` in `_config.yml` for absolute URLs

---

## Custom Overrides from Remote Theme

These files override the remote theme's defaults:

| File | What it overrides |
|------|-------------------|
| `_includes/landing.html` | Homepage layout — larger name/title, certification badges, quick-link buttons |
| `_includes/head.html` | Adds JSON-LD schema, improved OG tags, Twitter cards, GLightbox CSS |
| `_includes/scripts.html` | Adds GLightbox JS initialization and gallery toggle function |
| `_includes/blog/post-card.html` | Blog card with cover image extraction from first `<img>` in post content |
| `_includes/navbar.html` | Custom project dropdown navigation |
| `assets/css/style.scss` | Blog card images, photography gallery grid, show-more pagination styling |

---

## Validation

After making changes:
1. Run `bundle exec jekyll build` and verify it completes without errors
2. Run `bundle exec jekyll serve --watch` and check the page at `http://localhost:4000`
3. Verify new blog posts appear on the `/blog/` page
4. Verify new projects appear on the correct project listing page
5. Check that the navbar displays all expected pages
6. For photography changes: verify photo count, show-more buttons, and lightbox functionality
7. For SEO changes: check `_site/sitemap.xml` and inspect `<head>` for OG/JSON-LD tags

There is **no CI/CD pipeline** configured. The site is deployed automatically by GitHub Pages from the default branch. There are no linters, test suites, or pre-commit hooks.

---

## Important Notes

- The site uses a **remote theme** (`yousinix/portfolYOU`). If you need to override theme includes or layouts, copy the file from the theme repo into the local `_includes/` or `_layouts/` directory with the same name.
- The landing page (`pages/index.md`) uses `{% include landing.html %}` — this is now a **local override** in `_includes/landing.html` (not the remote theme version).
- Google Analytics is enabled (`G-3BD4D1PDK3`). The tracking ID is in `_config.yml`.
- The `jemoji` plugin is enabled for GitHub-style emoji support in markdown.
- The `github-pages` gem builds with `future: true`, so future-dated posts render immediately on the next push-triggered build — post dates never delay publication.
- The `jekyll-sitemap` plugin generates `/sitemap.xml` automatically.
- Custom CSS for the photography gallery, blog cards, and show-more pagination is in `assets/css/style.scss`.
- The photography page uses **CSS specificity** `a.photo-item.photo-hidden` to override the base `a.photo-item { display: block }` rule for the show-more feature.
- Blog cards in two-column layout extract the first `<img src>` from post content for cover images while using `strip_html` on the text excerpt.
- The `scripts/process-photos.sh` script is Bash 3.x compatible (macOS default) and uses `sips` for image conversion — no extra dependencies needed.

Trust these instructions. Only perform exploratory searches if the information above is incomplete or found to be incorrect.
