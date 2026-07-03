# Changelog

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
