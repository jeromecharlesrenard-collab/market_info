# Market Info

## Purpose
Static website of market reference pages, published with GitHub Pages. It is a duplicate of the JCR Knowledge Bank site (`C:\Users\Jerome\PYTHON_PROJECTS\knowledge_bank`): same style, script, tools, and conventions.
- Live site: https://jeromecharlesrenard-collab.github.io/market_info/
- Repo: https://github.com/jeromecharlesrenard-collab/market_info (branch `main`, remote `origin`)

`index.html` is the home page ("MARKET INFO") with an alphabetical list of pages. Every other page sits in `html_pages/` (e.g. `html_pages/Volume_Info.html`) and reaches shared files with `../` (see Conventions).

## Folder Structure
```
market_info/
├── index.html               home page (alphabetical list; sorted/grouped by assets/kb_common.js)
├── html_pages/              ALL pages except index.html (Volume_Info.html: "SPY Volume" box); links use ../
├── kb_images/               ALL page images (create when the first image is added), named <page>_<item>.png
├── kb_config.js             page settings: return arrow size (%), top/left margins (pt)
├── assets/
│   ├── kb_style.css         shared stylesheet - ALL page styling lives here
│   ├── kb_common.js         applies kb_config.js, sorts the home page list, expand buttons
│   ├── return_arrow.svg     top-left return icon
│   ├── top_arrow.svg        top-right "back to top" icon
│   ├── market_info_mi.ico   "MI" icon (shortcuts, preview window)
│   └── market_info.png      "MI" icon (browser tab)
├── templates/page_template.html   starting point for new pages
├── main.py                  entry point: logging + config + local preview window
├── tools/                   kb_core.py (paths, config, logging), new_page.py, site_check.py, preview.py, add_data.py
├── tests/                   pytest tests (logic only, no GUI)
├── docs/market_info NOTES.md
├── data/                    private source documents (git-ignored, never published), e.g. SPY Average Volume.docx
├── logs/app.log             rotating log (git-ignored)
├── config.json              tool settings: log_level, preview_port, preview_idle_minutes, site_url, repo_url, default_source
├── requirements.txt         pinned dev dependencies
├── pytest.ini, .editorconfig, .gitignore, .gitattributes, .nojekyll
└── .vscode/                 settings, recommended extensions, tasks
```
The `kb_` file prefix is kept from the Knowledge Bank so both sites share identical code.

## Environment
- venv: `C:\Users\Jerome\PYTHON_PROJECTS\market_info\.venv` (Python 3.14 from `C:\Users\Jerome\AppData\Local\Programs\Python\Python314\python.exe`)
- Dependencies (requirements.txt, pinned): pytest, pillow, tkinterdnd2 (drag and drop in the ADD window). The website itself needs no Python.

## Commands (run from the project folder)
- Shortcuts (in the project folder and in `PYTHON_PROJECTS\1 PYTHON APPS SHORTCUP`; git-ignored, *.lnk):
  - `MARKET INFO L.lnk`: open the site locally, no window (GitHub-like server; it stops by itself after `preview_idle_minutes` in config.json, default 60, without page requests; launching again while it runs just reopens the browser). Same as `.venv\Scripts\pythonw.exe main.py`.
  - `MARKET INFO W.lnk`: open the live website.
  - `MARKET INFO PREVIEW.lnk`: control window (OPEN SITE / CHECK SITE / EXIT), same as `main.py --window` or VS Code task "KB: Preview window".
  - `MARKET INFO ADD.lnk` (shortcut folder copy: `4 MI ADD.lnk`): MARKET INFO ADD DATA window (`tools/add_data.py`, same as `main.py --add-data` or task "KB: Add data"). Same app as KNOWLEDGE BANK ADD: pick a target page (or "New Page"), a source file (.docx, .pdf, .txt, .xlsx, .jpg, .png, .gif, .bmp, .html; Browse, drag and drop onto the window, or Ctrl+V / "Paste image" for a copied image or screenshot) and/or pasted text, and/or Instructions, "Update GitHub" (checked by default), then PROCESS; Claude (`claude -p` in this folder) places the data in the page style, runs the checks, and publishes. Unchecked: local update only, then a pop-up offers to publish just the files that job changed. "Match GitHub": commits and pushes every local change so GitHub matches the local site exactly (refuses if GitHub has newer commits). Timeout: `add_data_timeout_minutes` in config.json.
- New page: `.venv\Scripts\python.exe -m tools.new_page --title "Name" --description "One line"`
- Check site (links, images, upper/lowercase, arrow, listing): `.venv\Scripts\python.exe -m tools.site_check`
- Tests: `.venv\Scripts\python.exe -m pytest`
- VS Code: Terminal > Run Task > "KB: Preview site" / "KB: Preview window" / "KB: Add data" / "KB: New page" / "KB: Check site" / "KB: Run tests"
- Publish: commit and push to `origin main`; GitHub Pages updates in 1-2 minutes

## Conventions
- **Every page uses the shared style**: pages live in `html_pages/` and link `../assets/kb_style.css`, `../kb_config.js`, `../assets/kb_common.js` in `<head>`; return arrow `<a class="return-arrow" href="../index.html">` right after `<body>`; images `../kb_images/...`; home page entries link `html_pages/<Page>.html`. Only index.html stays in the root (GitHub Pages home page). The site check reports any page outside `html_pages/`. Do not put `<style>` blocks in pages; add shared classes to kb_style.css.
- White background, black Arial text, bold where needed; cards, stat boxes, striped tables.
- **No round numbering badges** on cards or boxes.
- **Boxed sections** ("title boxes"): `<article class="card row-card title-box">` with a one-line `<div class="title-line"><h3>Title</h3></div>` and all content in `<div class="more">`, shown with the expand button (see Volume_Info.html "SPY Volume").
- Long lists of items: full-width row cards (`.rows` / `.row-card`): one header line, max 2 text lines, extra details in `<div class="more">`.
- **Top arrow** and **Expand all** button are added automatically by kb_common.js.
- **Recap**: Put `<div class="recap" hidden>` with a `<table class="recap-table">` (columns Item | Remember; key words and numbers in `<strong>`, short memory aids in `<em>`) inside a box, or right after a section `<h2>` when the section holds many boxes or a table. kb_common.js adds a round "R" button next to that title or heading; it opens the recap in a window (3/4 of the page width; click anywhere or Esc to close). Recaps are short: general-knowledge facts only (nothing fringe or intricate), rounded easy numbers, strong words, about 3 to 8 rows. Search ignores recap text.
- **Bookmarks**: kb_common.js gives every section heading (`<h2>`) and every box/row card an address (`<page>.html#id`): an `id` already in the markup is kept, otherwise one is made from the title ("The Press" -> `#the-press`). A link icon (on hover, top right of a box, after an h2 title) copies the full address. Opening an address with `#id` expands that box and scrolls to it. To keep a bookmark stable when a title may change, give the box or h2 a fixed `id` in the markup.
- **Search**: kb_common.js adds a search box to every page (right of the title; centered under the subtitle on the home page), same code as the Knowledge Bank. It reads the pages listed on index.html on first use (no index file to rebuild), searches page titles and descriptions, section headings, and every box and paragraph (all words, case and accents ignored). A result opens `<page>?q=<words>&b=<block>`: matches are highlighted, their boxes expanded, and the page scrolls to them. Placeholder and local-preview name: `searchPlaceholder` / `localPreviewName` in kb_config.js. Needs http(s) (live site or MARKET INFO L), not file://.
- Home page entries: name on top, description + "Added" date on one line.
- Create new pages with `tools/new_page.py` (creates them in `html_pages/` and adds the home page entry automatically).
- **All page images go in `kb_images/`** (published), lowercase page prefix. Source files go in `data/` (never published; pages must not link to it). File and folder names are case-sensitive on GitHub.
- Market data times are in EDT. American spelling. Python: PEP 8, type hints, docstrings, pathlib.
- After every change, run the site check and tests, then commit and push to `origin main`. Do not start the preview window without the user's authorization.
