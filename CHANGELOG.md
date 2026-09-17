This file is 101 lines long; read all of them.

# Changelog

## 0.3.0 (2026-09-17)

### Changed

- The skill lineup has been consolidated: the skills are now `/scrape`,
  `/scrape-plan`, `/scrape-analyze-page`, `/scrapy-extra` and `/zyte`.
  - The new `/scrape-plan` skill replaces `/scrape-define`, `/scrape-spec`,
    `/scrape-explore-site` and `/scrape-review-schema`: it plans a web scrape
    end to end — explores a detail page, discovers fields, confirms the
    schema, validates it against more pages and presents the finalized plan
    for approval.
  - The new `/zyte` skill replaces `/scrape-scrapy-cloud`,
    `/scrape-zyte-api-stats` and `/scrape-zyte-login`: one skill for anything
    about Zyte.
  - `/scrape` now drives spider and page-object generation directly; the
    internal `/scrape-codegen`, `/scrape-create-spider`,
    `/scrape-add-page-object` and `/scrape-ensure-project` subskills are gone.

### Added

- The new `/scrapy-extra` skill provides up-to-date guidance for writing,
  modifying, debugging and explaining Scrapy, scrapy-poet, scrapy-zyte-api and
  web-poet code. It complements the `scrapy` skill of the official
  [Scrapy agent plugin](https://github.com/scrapy/scrapy-agent-plugin).
- `/zyte` also answers Zyte API pricing and cost questions (plans, tiers,
  discounts, cost estimates) and how-to questions about any Zyte product,
  sourced from docs.zyte.com.

### Improved

- `/scrape`: generated code now prefers browser dependencies declared by page
  objects over the `browserHtml` automap, producing more reliable spiders for
  JS-rendered sites.
- `/scrape`: now also works when run inside an existing Scrapy project,
  instead of always creating a new one.
- `/zyte`: guidance reorganized into targeted how-to references loaded on
  demand, improving accuracy while using less context.
- Skill instructions were tuned for Codex CLI.
- Page analysis now produces compact summaries, reducing context passed back
  to the orchestrating skill.
- The plugin now follows the [Agent Plugins](https://agent-plugins.org/)
  standard and ships a `plugin.json` manifest.

### Fixed

- Improved Windows compatibility through portable credential handling, `uv`
  invocations and command guidance that no longer assume a POSIX shell.

## 0.2.3 (2026-07-16)

### Improved

- `/scrape-scrapy-cloud`: if a Scrapy Cloud job was started the skill now waits
  for it to finish and reports the results. If the results look wrong (errors,
  wrong items, low coverage) it tries to fix the problems and rerun the job.
- `/scrape`, `/scrape-spec` and other related skills: if all required fields
  can be extracted from list pages, the skills will generate code that does
  that and doesn't request detail pages.
- `scrape-zyte-login`: the login flow now uses OAuth and stores credentials in
  the project's `.env` file for reuse by the scraping skills.

### Fixed

- `/scrape-review-schema`: fixed problems when running the review on Windows.

## 0.2.2 (2026-07-03)

### Improved

- `/scrape-scrapy-cloud`: requirements handling reworked. It now always
  generates a frozen `requirements.txt` (dependencies pinned with `==`) — from
  whatever dependency specification the project already uses, or, when none
  exists, from the third-party packages inferred from the project source — and
  always points `scrapinghub.yml` at it. When it generates the file, it reports
  the exact command used and how to refresh it when dependencies change.
- `/scrape-scrapy-cloud`: smarter Scrapy stack selection — it now also falls
  back to the latest stack when no stack matches the Scrapy version pinned in
  `requirements.txt`, recommends a test job when the stack's Scrapy version is
  older than the pinned one, and adds troubleshooting guidance for `sh_scrapy`
  errors caused by the stack's `scrapinghub-entrypoint-scrapy` lagging the
  pinned Scrapy version.
- `/scrape-zyte-api-stats`: refined triggering — it now also covers spend and
  usage projections that explicitly extrapolate from recorded usage, while
  staying out of purely hypothetical "what would this spider generate"
  estimates.
- `/scrape-define` and `/scrape-spec`: clearer descriptions that better convey
  each skill's role in the workflow (create a spec from a URL; expand a spec
  created by `/scrape-define`), improving skill selection.

### Fixed

- `/scrape-add-page-object`: no longer crashes when adding a page object in a
  project that has Twisted installed.

## 0.2.1 (2026-06-26)

Initial release.
