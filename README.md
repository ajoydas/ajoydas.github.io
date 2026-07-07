# ajoydas.github.io

Personal portfolio and blog built with [Jekyll](https://jekyllrb.com/) and the [portfolYOU](https://github.com/yousinix/portfolYOU) theme, hosted on [GitHub Pages](https://pages.github.com/).

**Live site:** [ajoydas.com](https://ajoydas.com)

## Tech Stack

- **Jekyll** — static site generator
- **GitHub Pages** — hosting & deployment (auto-deploys on push to `main`)
- **portfolYOU** — remote Jekyll theme (Bootstrap 4.6)
- **GLightbox** — photo lightbox with zoom/touch support
- **Ruby / Bundler** — build dependencies

## Project Structure

```
_config.yml          # Jekyll configuration & site settings
_data/
  photos.yml         # Photo gallery data (drives the photography page)
  social-media.yml   # Social media links
  timeline.yml       # Experience timeline data
_includes/           # Reusable HTML partials (navbar, footer, head, etc.)
_layouts/            # Page layouts (default, page)
_posts/              # Blog posts (Markdown, date-prefixed filenames)
_projects/           # Project pages (Markdown)
_sass/               # SCSS partials
assets/
  css/style.scss     # Main stylesheet
  img/               # Images (photography/, blog covers, etc.)
  js/                # JavaScript files
pages/               # Site pages (about, blog, skills, photography, etc.)
scripts/             # Utility scripts (photo processing)
```

## Local Development
### Prerequisites

- Ruby (3.x recommended)
- Bundler (`gem install bundler`)
- macOS `sips` (built-in, needed for photo processing only)

### Setup

```bash
git clone https://github.com/ajoydas/ajoydas.github.io.git
cd ajoydas.github.io
bundle install
```

### Run locally

```bash
bundle exec jekyll serve --watch
```

The site will be available at `http://localhost:4000`. Changes to most files auto-reload; `_config.yml` changes require a restart.

## Common Tasks

### Add a new blog post

1. Create a file in `_posts/` with the format `YYYY-MM-DD-title-slug.md`
2. Add front matter:
   ```yaml
   ---
   title: "Your Post Title 🚀"
   tags: [tag1, tag2, tag3]
   style: border
   color: primary  # success = achievements, info = event recaps, primary = technical, secondary = deep-dives, warning = retrospectives
   description:    # left empty by convention
   ---
   ```
3. Write content in Markdown below the front matter
4. To add a cover image, include an `<img>` tag early in the post — the first image is automatically used as the blog card cover

See the **Content Conventions** section in `AGENTS.md` for the full blog-post house style (voice, emoji, image naming, structure).

### Update an existing page

Pages live in `pages/`. Edit the Markdown file directly (e.g., `pages/about.md`, `pages/skills.md`). Most pages use `layout: page` or `layout: default`.

### Add a new project

Create a Markdown file in `_projects/` with the format `(NN) ProjectName.md` where `NN` controls sort order (lower = shown later). Add front matter with `name`, `tools`, `image`, and `description`.

### Update experience / timeline

Edit `_data/timeline.yml` — entries appear on the About page.

### Update social links

Edit `_data/social-media.yml`.

## Photography Page

The photography page at `/photography/` is data-driven — all photo metadata lives in `_data/photos.yml`.

### Process new photos

Use the included script to batch-convert and generate thumbnails:

```bash
# 1. Create a folder with images named: "category - description.ext"
#    e.g., "mountains - Moraine Lake.heic", "europe - Venice Bridge.jpg"

# 2. Run the processor
./scripts/process-photos.sh /path/to/your/photos/

# This will:
#   - Convert all images to JPEG (1600px max, 80% quality) → assets/img/photography/converted/
#   - Generate thumbnails (400px max) → assets/img/photography/thumbs/
#   - Regenerate _data/photos.yml with all photo metadata
```

**Supported input formats:** HEIC, JPG, JPEG, PNG, WEBP  
**Categories:** mountains, europe, cities, coasts, nature (custom categories are supported)

### Add photos manually

If you prefer not to use the script, you can:

1. Place full-size JPEGs in `assets/img/photography/converted/`
2. Place thumbnails in `assets/img/photography/thumbs/` (same filename)
3. Add entries to `_data/photos.yml` under the appropriate category:
   ```yaml
   - file: "your-photo.jpg"
     caption: "Description of the photo"
   ```

## Deployment

Push to the `main` branch — GitHub Pages automatically builds and deploys the site. No CI/CD configuration needed.

## License

Blog content and photographs © Ajoy Das. All rights reserved.  
The portfolYOU theme is licensed under [MIT](https://github.com/yousinix/portfolYOU/blob/master/LICENSE).

