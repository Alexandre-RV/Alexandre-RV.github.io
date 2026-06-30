# Alexandre Hervé — Portfolio

Single-page personal portfolio, hosted on **GitHub Pages**.

- **Live site:** https://alexandre-rv.github.io
- **Repo:** `Alexandre-RV/Alexandre-RV.github.io`
- **Stack:** one static `index.html` (inline CSS + a small vanilla-JS block). No build step, no dependencies.

---

## Project structure

```
.
├── index.html          # The entire site: markup + CSS (in <style>) + JS (in <script>)
├── README.md           # This file
└── assets/
    ├── CV.pdf
    ├── certs/                      # Certifications (image of each certificate)
    │   └── toefl-itp.png
    ├── hexaforce/                  # Thales — Hexaforce internship (LLM / agentic)
    │   ├── report.pdf
    │   ├── supervisor-recommendation.pdf
    │   ├── recommendation.png          # image preview shown on the page
    │   ├── llm-critical-contexts-paper.pdf
    │   ├── comparison-window.png
    │   ├── evaluator-window.png
    │   ├── main-pipeline-overview.webm  # screen recording (VP9, compressed)
    │   └── turn-loop-overview.webm      # screen recording (VP9, compressed)
    ├── ncop/                       # Thales — NCOP internship (NLP)
    │   ├── report.pdf
    │   ├── map-all-items.png            # carousel: "no filter"
    │   ├── map-aviation-line.png        # carousel: aviation line filter
    │   ├── map-italian-infantry.png     # carousel: Italian infantry filter
    │   └── map-hostile.png              # carousel: hostile units filter
    ├── games/                      # Game-dev project
    │   ├── schmup-gameplay.mp4
    │   ├── dungeon-textured.jpg
    │   ├── dungeon-wireframe.jpg
    │   └── dungeon-flat.jpg
    └── irl/                        # DIY / hardware project
        ├── laser-cutter-built-1.jpg
        ├── laser-cutter-built-2.jpg
        └── laser-cutting-example.jpg
```

The page is organised top-to-bottom as: **Hero → Experience → Research → Certifications → Projects → About → Contact**, each a `<section>` with an `id` the nav links to.

---

## How to edit

Everything lives in `index.html`. Editable spots are flagged with `<!-- EDIT ... -->` comments. Common changes:

- **Add an experience:** copy a `.experience` card block in `#experience`.
- **Add a project:** copy a `.project` block in `#projects`.
- **Add a certification:** copy a `.cert` block in `#certifications`. Put the certificate image in `assets/certs/` and point the `<img src>` at it.
- **NCOP carousel:** the slide captions live in the `slides` array inside the `<script>` at the bottom; the matching `<img>` slides are in the `#ncop-carousel` markup. Keep the two in the same order.
- **Re-skin:** edit the CSS custom properties in `:root` (colors, fonts) at the top of the `<style>` block.

Interactive bits, all vanilla JS at the bottom of the file:
- **Carousel** auto-loops the NCOP maps (pauses on hover, prev/next + dots).
- **Lightbox** enlarges any image in `.gallery`, `.doc-preview`, or `.cert-img` on click.

---

## Good practices for this repo

### Asset naming
Keep every filename **web-safe**: lowercase, ASCII, hyphens instead of spaces, no accents/parentheses/commas/colons. GitHub Pages serves files by exact path, and spaces/accents/odd characters cause broken links. Always rename uploads before linking them.

### Images
- Export screenshots/scans at a sensible size; a full-width image rarely needs to be wider than ~1600 px.
- Prefer `.png` for UI/screenshots and document scans, `.jpg` for photos.

### Video — compress before committing
Raw screen recordings are huge (a 36 s 1080p capture can be ~80 MB). Re-encode to **VP9 `.webm`** before committing — for static/screen content it shrinks ~90 % with no visible quality loss:

```bash
ffmpeg -i input.webm -c:v libvpx-vp9 -crf 34 -b:v 0 \
       -row-mt 1 -cpu-used 4 -deadline good -pix_fmt yuv420p -an output.webm
```

- `-crf` 30–36 trades size vs quality (lower = bigger/sharper). 34 is a good default for screen captures.
- `-an` drops the audio track (screen demos usually have none).
- For camera/gameplay footage (lots of motion) keep H.264 `.mp4`; it's already efficient and re-encoding mostly just degrades it.
- **Keep individual files well under 100 MB** (GitHub's hard limit) — ideally a few MB so the page loads fast.

### Git hygiene
- Commit compressed assets, never the raw originals — a large blob stays in history forever even after you replace it, bloating every clone.
- If a heavy file does slip in, scrub it from history with [`git filter-repo`](https://github.com/newren/git-filter-repo) (`--strip-blobs-with-ids`) and force-push, rather than just deleting it in a new commit.

### Deploy
GitHub Pages auto-publishes the `main` branch. To ship a change: commit and `git push origin main`; the site rebuilds in ~1 minute. Hard-refresh (Ctrl/Cmd+Shift+R) to bypass the browser cache.
