# Personal Academic Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Convert the cloned Academic Pages template into Danning Li's minimal English academic website with About, Publications, and CV pages, then deploy to GitHub Pages.

**Architecture:** Keep the Jekyll/Academic Pages theme. Fix configuration, replace template content with real content, add three publication entries, and remove demo collections/pages. Verify with a Dockerized Jekyll build and deploy by pushing `master`.

**Tech Stack:** Jekyll (GitHub Pages gem), Liquid, YAML, Markdown, Docker Compose, git.

**Spec:** `docs/superpowers/specs/2026-11-08-personal-academic-website-design.md`

---

## File Structure

**Modified:**
- `_config.yml` — site metadata, author bio, locale, publication categories
- `_data/navigation.yml` — About / Publications / CV
- `_pages/about.md` — homepage
- `_pages/cv.md` — education, research experience, publications, skills
- `_includes/archive-single.html` — author line + shared link renderer
- `_layouts/single.html` — author line + shared link renderer
- `.gitignore` — ignore Jekyll cache directories

**Created:**
- `_includes/publication-links.html` — single source of truth for paper links
- `_publications/2026-06-01-geoworld.md`
- `_publications/2025-12-01-flashmo.md`
- `_publications/2025-04-01-infant-cry-detection.md`
- `files/.gitkeep`

**Deleted:**
- Five template publication files in `_publications/`
- `_posts/`, `_portfolio/`, `_talks/`, `_teaching/`, `_drafts/`, `_data/comments/`
- Demo pages: `archive-layout-with-content.md`, `category-archive.html`, `collection-archive.html`, `cv-json.md`, `markdown.md`, `non-menu-page.md`, `page-archive.html`, `portfolio.html`, `tag-archive.html`, `talkmap.html`, `talks.html`, `teaching.html`, `terms.md`, `year-archive.html`
- `_data/cv.json`
- Template PDFs/BibTeX in `files/`

**Kept:**
- `_pages/404.md`, `_pages/sitemap.md`, `_pages/publications.html`
- `images/profile.png` (placeholder avatar)
- Unused demo images (leave in place to avoid breaking unknown references)

---

## Task 1: Fix site configuration and navigation

**Files:**
- Modify: `_config.yml:10,15,26-35,82-88`
- Modify: `_data/navigation.yml` (replace entire file)
- Modify: `.gitignore`

- [ ] **Step 1: Fix locale, description, author bio, and remove redundant `uri`**

In `_config.yml`, change line 10 from:

```yaml
locale                   : "en-CN"
```

to:

```yaml
locale                   : "en-US"
```

Change line 15 from:

```yaml
description              : &description "MPhil Student at HKUST(GZ) | AI + Finance & Healthcare"
```

to:

```yaml
description              : &description "MPhil Student in Financial Technology at HKUST(GZ) | AI for Finance & Healthcare"
```

Replace the broken `author.bio` and remove the redundant `uri` line. The current block is:

```yaml
  pronouns         : "he/him"  
  bio              : "Danning Li is a research assistant at HKUST (Guangzhou), working in the Thrust of Financial Technology under Dr. Chao Zhang."Red Bird MPhil Student at HKUST (Guangzhou), Thrust of Financial Technology. Research on Robust AI for Asset Pricing. Interested in AI + X including Medical VLMs and World Models."
"
  location         : "Guangzhou"
  employer         : "HKUST(GZ)"
  uri              : https://danninglee.github.io/
  email            : "eccsha256@outlook.com" 
```

Replace it with:

```yaml
  pronouns         : "he/him"
  bio              : "Red Bird MPhil Student at HKUST (Guangzhou), Thrust of Financial Technology. Research on Robust AI for Asset Pricing. Interested in AI + X, including Medical VLMs and World Models."
  location         : "Guangzhou"
  employer         : "HKUST(GZ)"
  email            : "eccsha256@outlook.com"
```

- [ ] **Step 2: Keep only the conference publication category**

Replace:

```yaml
publication_category:
  books:
    title: 'Books'
  manuscripts:
    title: 'Journal Articles'    
  conferences:
    title: 'Conference Papers'
```

with:

```yaml
publication_category:
  conferences:
    title: 'Conference Papers'
```

- [ ] **Step 3: Replace navigation**

Replace the entire contents of `_data/navigation.yml` with:

```yaml
# The following is the order of the links in the header of the website.
main:
  - title: "About"
    url: /

  - title: "Publications"
    url: /publications/

  - title: "CV"
    url: /cv/
```

- [ ] **Step 4: Ignore Jekyll cache directories**

Append to `.gitignore`:

```gitignore
# Jekyll cache and metadata
.jekyll-cache/
.jekyll-metadata
```

- [ ] **Step 5: Validate YAML**

Run:

```bash
python - <<'PY'
import yaml
for path in ["_config.yml", "_data/navigation.yml"]:
    with open(path, encoding="utf-8") as f:
        yaml.safe_load(f)
    print(f"{path}: OK")
PY
```

Expected output:

```text
_config.yml: OK
_data/navigation.yml: OK
```

- [ ] **Step 6: Commit**

```bash
git add _config.yml _data/navigation.yml .gitignore
git commit -m "fix: configure site metadata and navigation

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

---

## Task 2: Rewrite the About page

**Files:**
- Modify: `_pages/about.md` (replace entire file)

- [ ] **Step 1: Replace `_pages/about.md`**

Write exactly:

```markdown
---
permalink: /
title: "About Me"
author_profile: true
redirect_from:
  - /about/
  - /about.html
---
Hi! I'm **Danning Li** (李丹宁), a recent graduate from [**McGill University**](https://www.mcgill.ca/), where I majored in **Statistics** with a minor in **Computer Science**.

I am currently a **Red Bird MPhil Student** at the [**Hong Kong University of Science and Technology (Guangzhou)**](https://www.hkust-gz.edu.cn/) in the **Thrust of Financial Technology**, supervised by [**Dr. Chao Zhang**](https://facultyprofiles.hkust-gz.edu.cn/faculty-personal-page/ZHANG-Chao/chaoz). My MPhil research focuses on **Robust AI for Asset Pricing**. I also work as a **Research Assistant** in the same group.

Prior to this, I collaborated with researchers at the **West China Hospital, Sichuan University**, where I contributed to the development of a statistically reliable large model for **breast pathology** clinical case analysis, working alongside Postdoctoral Researcher **Ting Yin** and Professor **Yuhao Yi**.

## Research Interests

- **AI + X**: applying artificial intelligence and statistical methods to problems in finance, healthcare, and business
- **Robust AI for Asset Pricing**
- **Medical Vision-Language Models**
- **World Models**

---

See my [**CV**](/cv/) for education and research experience, or browse my [**Publications**](/publications/).
```

- [ ] **Step 2: Verify required content is present**

Run:

```bash
grep -q "Red Bird MPhil Student" _pages/about.md && \
grep -q "Research Interests" _pages/about.md && \
grep -q "Medical Vision-Language Models" _pages/about.md && \
grep -q "World Models" _pages/about.md && \
echo "About page content OK"
```

Expected output: `About page content OK`

- [ ] **Step 3: Commit**

```bash
git add _pages/about.md
git commit -m "feat: rewrite homepage with bio and research interests

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

---

## Task 3: Rewrite the CV page

**Files:**
- Modify: `_pages/cv.md` (replace entire file)

- [ ] **Step 1: Replace `_pages/cv.md`**

Write exactly:

```markdown
---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---
{% include base_path %}

Education
======
* **MPhil in Financial Technology**, The Hong Kong University of Science and Technology (Guangzhou), 2028 (Expected)
  * Thrust of Financial Technology
  * Supervisor: Dr. Chao Zhang
  * Research focus: Robust AI for Asset Pricing
* **B.A. in Statistics** (Minor: Computer Science), McGill University, 2025

Research Experience
======
* **Spring 2026 — Research Assistant**, The Hong Kong University of Science and Technology (Guangzhou)
  * Research on robust asset pricing under factor feature perturbations
  * Supervisor: Dr. Chao Zhang

* **Fall & Winter 2025 — Research Assistant**, West China Hospital, Sichuan University
  * Fine-tuning pathology vision-language models; research on model honesty in clinical settings
  * Collaborators: Postdoctoral Researcher Ting Yin, Professor Yuhao Yi

* **Spring 2025 — Research Assistant**, The Hong Kong University of Science and Technology (Guangzhou)
  * Research on asset pricing in financial market regime switching
  * Supervisor: Dr. Chao Zhang

* **Summer 2024 — Research Assistant**, Mohamed bin Zayed University of Artificial Intelligence (MBZUAI)
  * Deep learning algorithms for biological audio recognition and dataset construction
  * Focus: infant cry detection and causal temporal representation
  * Supervisor: Hanan Aldarmaki

Publications
======
* [**GeoWorld: Geometric World Models**](/publication/geoworld/), [CVPR 2026](https://cvpr.thecvf.com/)
* [**FlashMo: Geometric Interpolants and Frequency-Aware Sparsity for Scalable Efficient Motion Generation**](/publication/flashmo/), [NeurIPS 2025](https://neurips.cc/)
* [**Infant Cry Detection Using Causal Temporal Representation**](/publication/infant-cry-detection/), [ICASSP 2025](https://2025.ieeeicassp.org/)

Skills
======
* **Data Processing & Analysis**
  * Statistical modeling, data wrangling, experimental design
* **Statistical & Machine Learning**
  * Classical statistical methods, deep learning, causal inference
* **Multimodal & Vision-Language Models**
  * Medical VLM fine-tuning, honesty evaluation, multimodal alignment
* **Asset Pricing & Quantitative Finance**
  * Factor models, regime switching, robust optimization
```

- [ ] **Step 2: Verify required sections and advisor name**

Run:

```bash
grep -q "^Education" _pages/cv.md && \
grep -q "^Research Experience" _pages/cv.md && \
grep -q "Hanan Aldarmaki" _pages/cv.md && \
grep -q "^Skills" _pages/cv.md && \
echo "CV page content OK"
```

Expected output: `CV page content OK`

- [ ] **Step 3: Commit**

```bash
git add _pages/cv.md
git commit -m "feat: rewrite CV with education, research experience, and skills

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

---

## Task 4: Add shared publication link rendering and author display

**Files:**
- Create: `_includes/publication-links.html`
- Modify: `_includes/archive-single.html:35,55-81`
- Modify: `_layouts/single.html:29,50-74`

- [ ] **Step 1: Create `_includes/publication-links.html`**

Write exactly:

```liquid
{% assign item = include.item %}
{% assign has_links = false %}
{% if item.paperurl or item.arxivurl or item.codeurl or item.posterurl or item.bibtexurl %}
  {% assign has_links = true %}
{% endif %}
{% if item.citation or has_links %}
  <p class="publication-links">
    {% if item.citation %}<span class="publication-citation">Recommended citation: {{ item.citation }}</span><br />{% endif %}
    {% if item.paperurl %}<a href="{{ item.paperurl }}">Paper</a>{% endif %}
    {% if item.arxivurl %}{% if item.paperurl %} | {% endif %}<a href="{{ item.arxivurl }}">arXiv</a>{% endif %}
    {% if item.codeurl %}{% if item.paperurl or item.arxivurl %} | {% endif %}<a href="{{ item.codeurl }}">Code</a>{% endif %}
    {% if item.posterurl %}{% if item.paperurl or item.arxivurl or item.codeurl %} | {% endif %}<a href="{{ item.posterurl }}">Poster</a>{% endif %}
    {% if item.bibtexurl %}{% if item.paperurl or item.arxivurl or item.codeurl or item.posterurl %} | {% endif %}<a href="{{ item.bibtexurl }}">BibTeX</a>{% endif %}
  </p>
{% endif %}
```

- [ ] **Step 2: Show authors and use the shared link renderer in `_includes/archive-single.html`**

After the closing `</h2>` of the title, insert:

```liquid
    {% if post.collection == 'publications' and post.authors %}
      <p class="page__meta">{{ post.authors }}</p>
    {% endif %}
```

Replace the entire block from `{% if post.citation and post.paperurl and post.slidesurl and post.bibtexurl %}` through the final `{% endif %}` before `</article>` with:

```liquid
    {% include publication-links.html item=post %}
```

- [ ] **Step 3: Show authors and use the shared link renderer in `_layouts/single.html`**

After the `<h1 class="page__title" ...>` line, insert:

```liquid
          {% if page.collection == 'publications' and page.authors %}
            <p class="page__meta">{{ page.authors }}</p>
          {% endif %}
```

Replace the entire block from `{% if page.citation and page.paperurl and page.slidesurl and page.bibtexurl %}` through the final `{% endif %}` before `{% if page.link %}` with:

```liquid
        {% include publication-links.html item=page %}
```

- [ ] **Step 4: Check Liquid tags are balanced**

Run:

```bash
python - <<'PY'
from pathlib import Path
for path in ["_includes/archive-single.html", "_layouts/single.html", "_includes/publication-links.html"]:
    text = Path(path).read_text(encoding="utf-8")
    opens = text.count("{%")
    closes = text.count("%}")
    assert opens == closes, f"{path}: {opens} '{{%' vs {closes} '%}}'"
    print(f"{path}: {opens} Liquid tags, balanced")
PY
```

Expected output: three lines ending in `balanced`.

- [ ] **Step 5: Commit**

```bash
git add _includes/publication-links.html _includes/archive-single.html _layouts/single.html
git commit -m "feat: add shared publication links and author display

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

---

## Task 5: Replace demo publications with the three real papers

**Files:**
- Delete: `_publications/2009-10-01-paper-title-number-1.md`
- Delete: `_publications/2010-10-01-paper-title-number-2.md`
- Delete: `_publications/2015-10-01-paper-title-number-3.md`
- Delete: `_publications/2024-02-17-paper-title-number-4.md`
- Delete: `_publications/2025-06-08-paper-title-number-5.md`
- Create: `_publications/2026-06-01-geoworld.md`
- Create: `_publications/2025-12-01-flashmo.md`
- Create: `_publications/2025-04-01-infant-cry-detection.md`

- [ ] **Step 1: Delete the five template publications**

```bash
git rm _publications/2009-10-01-paper-title-number-1.md \
       _publications/2010-10-01-paper-title-number-2.md \
       _publications/2015-10-01-paper-title-number-3.md \
       _publications/2024-02-17-paper-title-number-4.md \
       _publications/2025-06-08-paper-title-number-5.md
```

- [ ] **Step 2: Create `_publications/2026-06-01-geoworld.md`**

Write exactly:

```markdown
---
title: "GeoWorld: Geometric World Models"
collection: publications
category: conferences
permalink: /publication/geoworld/
excerpt: "Conference paper at CVPR 2026."
date: 2026-06-01
venue: "CVPR"
---

Conference paper at CVPR 2026.
```

- [ ] **Step 3: Create `_publications/2025-12-01-flashmo.md`**

Write exactly:

```markdown
---
title: "FlashMo: Geometric Interpolants and Frequency-Aware Sparsity for Scalable Efficient Motion Generation"
collection: publications
category: conferences
permalink: /publication/flashmo/
excerpt: "Conference paper at NeurIPS 2025."
date: 2025-12-01
venue: "NeurIPS"
---

Conference paper at NeurIPS 2025.
```

- [ ] **Step 4: Create `_publications/2025-04-01-infant-cry-detection.md`**

Write exactly:

```markdown
---
title: "Infant Cry Detection Using Causal Temporal Representation"
collection: publications
category: conferences
permalink: /publication/infant-cry-detection/
excerpt: "Conference paper at ICASSP 2025."
date: 2025-04-01
venue: "ICASSP"
---

Conference paper at ICASSP 2025.
```

- [ ] **Step 5: Verify publication files**

Run:

```bash
ls _publications
if grep -L "category: conferences" _publications/*.md; then
  echo "ERROR: a publication is missing category: conferences"
  exit 1
else
  echo "All publications categorized"
fi
```

Expected: exactly three files, and the final line `All publications categorized`.

- [ ] **Step 6: Commit**

```bash
git add _publications
git commit -m "feat: add real conference publications

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

---

## Task 6: Remove template demo content

**Files:** see deletion list below.

- [ ] **Step 1: Remove demo collections and drafts**

```bash
git rm -r _posts _portfolio _talks _teaching _drafts _data/comments
```

- [ ] **Step 2: Remove demo pages and JSON CV data**

```bash
git rm _pages/archive-layout-with-content.md \
       _pages/category-archive.html \
       _pages/collection-archive.html \
       _pages/cv-json.md \
       _pages/markdown.md \
       _pages/non-menu-page.md \
       _pages/page-archive.html \
       _pages/portfolio.html \
       _pages/tag-archive.html \
       _pages/talkmap.html \
       _pages/talks.html \
       _pages/teaching.html \
       _pages/terms.md \
       _pages/year-archive.html \
       _data/cv.json
```

- [ ] **Step 3: Remove template PDFs and keep the files directory**

```bash
git rm files/bibtex1.bib files/paper1.pdf files/paper2.pdf files/paper3.pdf \
       files/slides1.pdf files/slides2.pdf files/slides3.pdf
touch files/.gitkeep
git add files/.gitkeep
```

- [ ] **Step 4: Verify no navigation references remain and only intended pages exist**

Run:

```bash
cat _data/navigation.yml
ls _pages
grep -rn "portfolio/\|/talks/\|/teaching/\|/markdown/\|/year-archive/\|/cv-json/" _data/navigation.yml _includes _layouts || true
```

Expected:
- Navigation contains only About, Publications, CV.
- `_pages` contains `404.md`, `about.md`, `cv.md`, `publications.html`, `sitemap.md`.
- No navigation/header references to removed pages.

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "chore: remove template demo content and pages

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

---

## Task 7: Build and verify the site locally with Docker

**Files:** none (verification only)

- [ ] **Step 1: Build and start the Jekyll container**

Run from the project root:

```bash
docker compose up --build -d
```

Expected: image builds and container starts. The first build may take several minutes.

- [ ] **Step 2: Watch the build log until the server is ready**

```bash
docker compose logs --tail=80 jekyll-site
```

Expected final lines include:

```text
Server running... press ctrl-c to stop.
```

There must be no `YAML Exception`, `Liquid Exception`, or `Build Warning` about missing layouts.

If the container exits or reports permission errors writing `_site`, stop it and use the root-user fallback:

```bash
docker compose down
docker compose run --rm -d --service-ports --user root --name danning-jekyll jekyll-site
docker logs -f danning-jekyll
```

- [ ] **Step 3: Run HTTP content checks**

Run:

```bash
set -e
base=http://localhost:4000
curl -sf "$base/" | grep -q "Red Bird MPhil Student"
curl -sf "$base/" | grep -q "Research Interests"
curl -sf "$base/" | grep -q "Medical Vision-Language Models"
curl -sf "$base/" | grep -q "World Models"
curl -sf "$base/publications/" | grep -q "GeoWorld"
curl -sf "$base/publications/" | grep -q "FlashMo"
curl -sf "$base/publications/" | grep -q "Infant Cry Detection"
curl -sf "$base/cv/" | grep -q "Hanan Aldarmaki"
curl -sf "$base/cv/" | grep -q "McGill University"
curl -sf "$base/publication/geoworld/" | grep -q "GeoWorld"
curl -sf "$base/publication/flashmo/" | grep -q "FlashMo"
curl -sf "$base/publication/infant-cry-detection/" | grep -q "Infant Cry Detection"
curl -sf "$base/" | grep -q 'href="/publications/"'
curl -sf "$base/" | grep -q 'href="/cv/"'
! curl -sf "$base/" | grep -q "Paper Title Number"
! curl -sf "$base/" | grep -q ">Portfolio<"
! curl -sf "$base/" | grep -q ">Blog Posts<"
! curl -sf "$base/" | grep -q ">Guide<"
echo "All site checks passed"
```

Expected final line: `All site checks passed`

- [ ] **Step 4: Stop the container**

```bash
docker compose down
```

If the fallback container was used:

```bash
docker stop danning-jekyll
```

- [ ] **Step 5: Commit any fixes made during verification**

If no files changed, skip. Otherwise:

```bash
git add -A
git commit -m "fix: resolve local build and rendering issues

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

---

## Task 8: Fill in publication metadata from user input

**Files:**
- Modify: `_publications/2026-06-01-geoworld.md`
- Modify: `_publications/2025-12-01-flashmo.md`
- Modify: `_publications/2025-04-01-infant-cry-detection.md`

**This task is blocked until the user provides the details below. Do not invent authors, links, dates, or abstracts.**

- [ ] **Step 1: Ask the user for each paper**

Use this exact template:

```text
Please provide, for each of the three papers (GeoWorld, FlashMo, Infant Cry Detection):

1. Full author list in publication order:
2. Full venue name (e.g., IEEE/CVF Conference on Computer Vision and Pattern Recognition):
3. Year:
4. Paper / arXiv / DOI URL:
5. Code URL (if any):
6. Poster URL (if any):
7. One-sentence summary:
8. BibTeX (if available):
```

- [ ] **Step 2: Update each publication file with the provided values**

For each file, add the fields that were provided:

```yaml
authors: "<author list exactly as provided>"
paperurl: "<paper URL>"
arxivurl: "<arXiv URL>"
codeurl: "<code URL>"
posterurl: "<poster URL>"
bibtexurl: "<BibTeX URL or /files/name.bib>"
citation: "<authors>. (<year>). &quot;<title>.&quot; <i><venue></i>."
```

Set `excerpt` and the Markdown body to the user's one-sentence summary. If a URL was not provided, omit that field entirely. Do not use `#` or empty strings as fake links.

If the user supplies a PDF file instead of a URL, place it in `files/` (for example `files/geoworld.pdf`) and set `paperurl: "/files/geoworld.pdf"`.

- [ ] **Step 3: Rebuild and verify the rendered links**

```bash
docker compose up --build -d
docker compose logs --tail=40 jekyll-site
curl -sf http://localhost:4000/publications/ | grep -q 'class="publication-links"'
curl -sf http://localhost:4000/publications/ | grep -q '>Paper<'
curl -sf http://localhost:4000/publications/ | grep -q '>arXiv<'
echo "Publication links OK"
docker compose down
```

Expected: `Publication links OK`

- [ ] **Step 4: Commit**

```bash
git add _publications
git commit -m "feat: add publication metadata and links

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

---

## Task 9: Deploy to GitHub Pages

**Files:** none

- [ ] **Step 1: Review the final diff and status**

```bash
git status
git log --oneline -8
```

Expected: clean working tree; commits for config, About, CV, publication rendering, publications, cleanup, and metadata.

- [ ] **Step 2: Push to `origin/master`**

```bash
git push origin master
```

Expected: push succeeds and the GitHub Pages build is triggered.

If the push fails with authentication errors, tell the user to run `! git push origin master` in the session so their stored credentials are used.

- [ ] **Step 3: Verify the live site**

After GitHub Pages has had time to build (typically 1–3 minutes), run:

```bash
curl -sf https://danninglee.github.io/ | grep -q "Red Bird MPhil Student" && \
curl -sf https://danninglee.github.io/publications/ | grep -q "GeoWorld" && \
curl -sf https://danninglee.github.io/cv/ | grep -q "Hanan Aldarmaki" && \
echo "Live site updated"
```

Expected: `Live site updated`

If the live site still shows the old content, wait and retry. If the URL returns 404, the repository's Pages source is not enabled; tell the user to open **Settings → Pages**, set **Source** to **Deploy from a branch**, branch **master**, folder **/ (root)**, and save.

---

## Self-Review Notes

- Spec §4.1 → Task 2
- Spec §4.2 → Tasks 4, 5, 8
- Spec §4.3 → Task 3
- Spec §5 → Tasks 1–4 (theme unchanged; sidebar reduced)
- Spec §6 → Tasks 1, 5, 6
- Spec §7 → Task 8 (explicitly blocked on user input, no fabricated data)
- Spec §8 → Task 7
- Spec §9 → Task 9
- Publication field names are consistent across `publication-links.html`, `archive-single.html`, `single.html`, and the three publication files: `authors`, `paperurl`, `arxivurl`, `codeurl`, `posterurl`, `bibtexurl`, `citation`.
