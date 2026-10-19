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
│   └── market_info.ico/.png icon for the preview window / browser tab
├── templates/page_template.html   starting point for new pages
├── main.py                  entry point: logging + config + local preview window
├── tools/                   kb_core.py (paths, config, logging), new_page.py, site_check.py, preview.py
├── tests/                   pytest tests (logic only, no GUI)
├── docs/market_info NOTES.md
├── data/                    private source documents (git-ignored, never published), e.g. SPY Average Volume.docx
├── logs/app.log             rotating log (git-ignored)
├── config.json              tool settings: log_level, preview_port, site_url, repo_url, default_source
├── requirements.txt         pinned dev dependencies
├── pytest.ini, .editorconfig, .gitignore, .gitattributes, .nojekyll
└── .vscode/                 settings, recommended extensions, tasks
```
The `kb_` file prefix is kept from the Knowledge Bank so both sites share identical code.

## Environment
- venv: `C:\Users\Jerome\PYTHON_PROJECTS\market_info\.venv` (Python 3.14 from `C:\Users\Jerome\AppData\Local\Programs\Python\Python314\python.exe`)
- Dependencies (requirements.txt, pinned): pytest, pillow. The website itself needs no Python.

## Commands (run from the project folder)
- Preview site (GitHub-like local server + control window): `.venv\Scripts\pythonw.exe main.py`
- New page: `.venv\Scripts\python.exe -m tools.new_page --title "Name" --description "One line"`
- Check site (links, images, upper/lowercase, arrow, listing): `.venv\Scripts\python.exe -m tools.site_check`
- Tests: `.venv\Scripts\python.exe -m pytest`
- VS Code: Terminal > Run Task > "KB: Preview site" / "KB: New page" / "KB: Check site" / "KB: Run tests"
- Publish: commit and push to `origin main`; GitHub Pages updates in 1-2 minutes

## Conventions
- **Every page uses the shared style**: pages live in `html_pages/` and link `../assets/kb_style.css`, `../kb_config.js`, `../assets/kb_common.js` in `<head>`; return arrow `<a class="return-arrow" href="../index.html">` right after `<body>`; images `../kb_images/...`; home page entries link `html_pages/<Page>.html`. Only index.html stays in the root (GitHub Pages home page). The site check reports any page outside `html_pages/`. Do not put `<style>` blocks in pages; add shared classes to kb_style.css.
- White background, black Arial text, bold where needed; cards, stat boxes, striped tables.
- **No round numbering badges** on cards or boxes.
- **Boxed sections** ("title boxes"): `<article class="card row-card title-box">` with a one-line `<div class="title-line"><h3>Title</h3></div>` and all content in `<div class="more">`, shown with the expand button (see Volume_Info.html "SPY Volume").
- Long lists of items: full-width row cards (`.rows` / `.row-card`): one header line, max 2 text lines, extra details in `<div class="more">`.
- **Top arrow** and **Expand all** button are added automatically by kb_common.js.
- Home page entries: name on top, description + "Added" date on one line.
- Create new pages with `tools/new_page.py` (creates them in `html_pages/` and adds the home page entry automatically).
- **All page images go in `kb_images/`** (published), lowercase page prefix. Source files go in `data/` (never published; pages must not link to it). File and folder names are case-sensitive on GitHub.
- Market data times are in EDT. American spelling. Python: PEP 8, type hints, docstrings, pathlib.
- After every change, run the site check and tests, then commit and push to `origin main`. Do not start the preview window without the user's authorization.
