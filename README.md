This file is 181 lines long; read all of them.

<p align="center">
	<img src="assets/zyte-logo.png" alt="Zyte" width="180">
</p>

<h1 align="center">Zyte Web Data</h1>

<p align="center">
	From a plain-English prompt to a working Scrapy spider.
</p>

<p align="center">
	<a href="https://github.com/zytedata/skills/releases/tag/0.3.0">
		<img src="https://img.shields.io/badge/version-0.3.0-blue" alt="Version 0.3.0">
	</a>
	<a href="https://github.com/zytedata/skills/blob/main/LICENSE.md">
		<img src="https://img.shields.io/badge/license-Zyte%20EULA-b02cce" alt="Zyte EULA">
	</a>
	<a href="https://skills.sh/zytedata/skills">
		<img src="https://skills.sh/b/zytedata/skills" alt="Installs">
	</a>
	<a href="https://github.com/zytedata/skills">
		<img src="https://img.shields.io/github/stars/zytedata/skills?style=social" alt="GitHub stars">
	</a>
</p>

---

> Using a specific coding agent? See [Zyte Coding Agent Add-Ons](https://docs.zyte.com/ai-code.html) for alternatives.

## Install

This is an [agent plugin](https://agent-plugins.org/): a portable, vendor-neutral package of skills that any agent implementing the standard can load. Install it however your agent installs plugins, or use the [`skills`](https://www.skills.sh) CLI, which sets it up for every supported agent on your system at once:

```bash
npx skills add zytedata/skills
```

See [skills.sh](https://www.skills.sh) for the list of agents it supports and how to enable skills in each.

Any agent that reads agent plugins or [Agent Skills](https://agentskills.io) can use this plugin, including editors such as Cursor. For Claude Code, Codex CLI and GitHub Copilot CLI we publish [dedicated packages](https://docs.zyte.com/ai-code.html) tuned to each of them; prefer those if you use one.

---

## What it does

This is Zyte's official [agent plugin](https://agent-plugins.org/) that generates production-ready [Scrapy](https://scrapy.org) spiders with [web-poet](https://web-poet.readthedocs.io) page objects from a plain-English prompt. Give it a URL and describe what you want to extract. It handles site exploration, schema discovery, code generation, and smoke testing: no boilerplate, no manual selector hunting.

The plugin explores the target site, discovers available fields, and presents a schema for your approval before generating a single line of code. After you confirm the schema, it creates a Scrapy project with all dependencies configured, generates web-poet page objects and test fixtures, wires up the spider, and runs a smoke test to verify that extraction is working before handing the project back to you.

Optionally, use `/zyte` to deploy directly to [Scrapy Cloud](https://www.zyte.com/scrapy-cloud/) for scheduled runs, job history, and monitoring. A [free tier is available](https://docs.zyte.com/scrapy-cloud/pricing.md).

---

## Use cases

The `/scrape` skill works on any website with repeating structured content: detail pages linked from a listing or category page. Examples from the skill:

- Product catalogs
- Job listings
- Recipes

---

## How does it work?

The `/scrape` skill orchestrates two stages automatically:

```
1. Plan and validate the scrape     →  /scrape-plan
2. Build the project and spider     →  /scrapy-extra
```

Each stage feeds directly into the next. When the pipeline completes, you have a runnable spider and a passing test suite:

```bash
uv run scrapy crawl <spider_name>
uv run pytest fixtures/
```

---

## Skills

### Orchestration

| Skill | Description |
|---|---|
| `scrape` | End-to-end web scraping workflow — from URL to working spider with web-poet page objects |

### Pipeline stages (called automatically by `/scrape`)

| Skill | Description |
|---|---|
| `scrape-plan` | Plan the scrape and author a validated extraction spec: discover fields, download diverse pages, compare HTML variants, optional browser review |
| `scrape-analyze-page` | Extract all available fields with values from a detail page |
| `scrapy-extra` | Hands-on Scrapy coding: write/debug spiders, web-poet page objects, and projects; configure scrapy-poet and scrapy-zyte-api |

### Zyte APIs

| Skill | Description |
|---|---|
| `zyte` | Interact with Zyte's APIs and cloud services: set up your Zyte account and credentials; deploy projects, schedule spiders, list/stop jobs, and view items or logs on [Scrapy Cloud](https://www.zyte.com/scrapy-cloud/); query historical [Zyte API](https://www.zyte.com/zyte-api/) usage stats; look up Zyte API pricing and per-website costs; and answer how-to and documentation questions about Zyte from the official docs |

---

## Prerequisites

- An AI coding agent that supports [agent plugins](https://agent-plugins.org/) or [Agent Skills](https://agentskills.io)
- [`uv`](https://docs.astral.sh/uv/) — used to create and manage the Scrapy project

Project dependencies (scrapy, scrapy-poet, scrapy-zyte-api, web-poet, extruct, price-parser, pytest) are installed automatically by the skills.

---

## Quickstart

Any scraping prompt triggers the skill automatically. For example:

```
Scrape books.toscrape.com
```

The plugin walks you through schema approval interactively, then generates a complete, tested Scrapy project.

---

## Update

To update:

```bash
npx skills update zytedata/skills
```

---

## Evaluation

We automatically evaluate skills and track both wall time and cost. We measure and aim to improve these metrics over time.

---

## Feedback

If you find any issue — such as prompts that did not work as expected, or that caused excessive wall time or cost — please [open a GitHub issue](https://github.com/zytedata/skills/issues).

Provide as much detail as possible to help us reproduce the issue. You are welcome to anonymize target websites or other data.

---

## Frequently asked questions

### Is a Zyte account required?

No. The generated spider is a standard Scrapy project that runs locally with `uv`. A Zyte account is required only if you want to deploy to [Scrapy Cloud](https://www.zyte.com/scrapy-cloud/) or use [Zyte API](https://www.zyte.com/zyte-api/) to access sites that block standard scrapers. If you want to use Zyte API, you'll need an account to generate an API key.

### Does it handle JavaScript-rendered pages?

The generated project includes `scrapy-zyte-api` as a dependency. Enabling headless browser rendering requires a [Zyte API](https://www.zyte.com/zyte-api/) key. The `/zyte` skill guides you through setting up your credentials.

### What Python libraries does the generated project use?

The project template includes `scrapy`, `scrapy-poet`, `scrapy-zyte-api`, `web-poet`, `extruct`, `price-parser`, and `pytest`. All dependencies are installed automatically via `uv sync`.

### Can the generated spider run without my AI coding agent?

Yes. The plugin generates a standard Scrapy project. Run it directly with:

```bash
uv run scrapy crawl <spider_name>
```

You can extend, modify, and deploy it independently of your AI coding agent.

---

## License

See [LICENSE.md](LICENSE.md) for the Zyte End User License Agreement.
