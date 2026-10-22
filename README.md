# MaLGa academic page template

The starting point for MaLGa Vision paper pages. Each page is one static `index.html` in the
MaLGa visual identity: no build step, no third-party scripts, and a check that runs before every deploy.

Live pages built on it:
[PACER](https://malga-vision.github.io/PACER-wacv27-AcademicPage/) ·
[CDPM-Align](https://malga-vision.github.io/CDPM-Align_AcademicPage/) ·
[Hip landmarks](https://malga-vision.github.io/Landmarks-Hip-Conditions-AcademicPage/) ·
[Diffusing DeBias](https://malga-vision.github.io/DiffusingDeBias-AcademicPage/) ·
[FairerXRay](https://malga-vision.github.io/FairerXRay-LandmarkDetection-AcademicPage/) ·
[DiffusionXray](https://malga-vision.github.io/DiffusionXray-FewShot-LandmarkDetection-AcademicPage/) ·
[In-domain transfer](https://malga-vision.github.io/InDomain-TransferLearning-LandmarkDetection-AcademicPage/)

## Start a page for a new paper

1. Click **Use this template** and create `Malga-Vision/<Paper>-AcademicPage` (public).
2. In the new repository, open **Settings > Pages** and set **Source** to **GitHub Actions**.
3. Edit `index.html`. Every placeholder is marked in comments; replace the canonical URL,
   metadata, JSON-LD, hero, sections and BibTeX. Delete sections you do not need and the matching
   links in `site-nav`. Put figures in `static/images/`, PDFs in `static/pdfs/`, videos in `static/videos/`.
4. Add the paper to `tools/projects.json` in **this** template repository (newest first), then render the
   shared blocks on the new page:

   ```
   python tools/render_shared.py index.html --page-repo <Paper>-AcademicPage --code-repo <CodeRepo>
   ```

   Re-run the same command on the other pages so their "Related work from our lab" lists the new paper.
5. Check the page and preview it:

   ```
   python tools/page_guard.py
   python -m http.server     # then open http://localhost:8000
   ```

6. Push. The **Check and deploy page** workflow runs the guard and publishes only if it passes.
7. Update `llms.txt` and `sitemap.xml`, and add the URL to Google Search Console
   (URL-prefix property, HTML tag method; the verification tag is already in the template).

## What is in the box

| Path | What it is | Edit it? |
|---|---|---|
| `index.html` | The page. Head metadata, sections, BibTeX. | Yes |
| `static/malga/` | Shared stylesheet, script, fonts, logos, icons. Identical on every page. | Only here, then copy to the pages |
| `static/images/`, `static/pdfs/`, `static/videos/` | The paper's figures and files. | Yes |
| `static/css/page.css` | Optional page-only styles, linked after `malga.css`. Avoid when possible. | Rarely |
| `tools/render_shared.py` | Writes "Related work from our lab" and the footer from `tools/projects.json`. | No |
| `tools/page_guard.py` | Security and integrity checks. | No |
| `tools/allowed-domains.txt` | Domains the page may link to. | Add real domains only |
| `.github/workflows/pages.yml` | Runs the guard, then deploys to GitHub Pages. | No |

## Components

All classes live in `static/malga/malga.css`; `index.html` shows each one in use.

- **Hero**: venue line, acronym and title, authors, affiliations, link buttons.
  Unavailable links stay visible as disabled buttons marked "soon".
- **Teaser** figure with an optional TL;DR.
- **Sections** with ring-marked titles, alternating white and light grey backgrounds.
- **Figures** (`figure.figure`), side-by-side grids (`figure-grid`), equations (`p.eq`).
- **Tables** (`data-table`): booktabs-style rules, dataset row groups, `best` / `second` cells, highlighted `ours` rows, horizontal scroll on phones.
- **Highlights** (spot cards) and **stats** (key numbers).
- **Media**: video, poster and slides cards, with a "Coming soon" placeholder.
- **FAQ** (`details`), kept identical to the FAQPage JSON-LD.
- **Citation** block with a copy button.
- **Related work**: the lab's papers as a connected timeline, the current one marked.

## Design

It follows the MaLGa identity used on [malga.unige.it](https://malga.unige.it/):

- coral `#FE6767` as the only accent (links use the darker `#B84545` so body text stays readable),
  UniGe night blue `#0F0E36` for the footer and primary buttons, neutral greys;
- Roboto Slab for headings (the open stand-in for Adelle) and Fira Sans for text, self-hosted;
- square corners, thin `#E2E2E2` rules, uppercase buttons;
- the logo's node-and-arc graph as the one decorative motif, in the hero and the related-work timeline.

## Security

The pages are static, so the realistic threats are a compromised third-party script, injected
spam links ("SEO injection"), doorway pages added to the repository, and clickjacking-style tricks.
The template closes them off:

- **Content Security Policy** in a meta tag: scripts, styles, fonts and media only from the page itself;
  no inline scripts or styles, no frames, forms, plugins or `<base>`.
- **No third-party code**: fonts, icons and the small script are in the repository.
- **Page guard** (`tools/page_guard.py`) fails the deploy when it finds inline or external scripts,
  event-handler attributes, `javascript:` URLs, inline styles, iframes or forms, links to domains outside
  `tools/allowed-domains.txt`, links inside hidden elements, extra HTML pages, spam keywords, scripts in SVGs,
  sitemap URLs outside the page, broken local references, files over 25 MB, a missing or weakened CSP,
  or leftover template placeholders.
- **Deploy gate**: GitHub Pages publishes from the workflow, so a commit that fails the guard is never served.
  Workflow actions are pinned to commit SHAs and run with read-only permissions.
- External links use `rel="noopener noreferrer"`; the referrer policy is `strict-origin-when-cross-origin`.

Recommended repository settings: protect the default branch against force pushes and deletion, and
require the `guard` check on pull requests. See [SECURITY.md](SECURITY.md) to report a problem.

## Updating the shared files on existing pages

Change `static/malga/` or `tools/` here, then copy them into each page repository and re-run
`tools/render_shared.py` and `tools/page_guard.py` there. The files are plain copies, so a page keeps
working even if this template changes.

## Licenses

Code (HTML, CSS, JavaScript, Python): MIT, see [LICENSE](LICENSE).
Fira Sans: SIL Open Font License 1.1. Roboto Slab: Apache License 2.0 (license files in `static/malga/fonts/`).
The MaLGa and University of Genoa logos belong to their owners and are included only for MaLGa pages.
