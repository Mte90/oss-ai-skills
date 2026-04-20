# OSS AI Skills Collection
[![License](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)   

A comprehensive collection of AI assistant skills (SKILL.md files) following the [skillreg.dev](https://skillreg.dev/docs/skill-md-reference) specification, designed to help contribute to and extend open-source projects (for plugins).
*Note: we test the majority of them and improving based on asking to the agents to extend the skills based on their experience.*

## What is this?

This repository provides reusable skill definitions that can be loaded into AI coding assistants. Each skill encapsulates domain knowledge, best practices, and workflows for specific tasks in open-source development. 

## Available Skills

### Contribute

| Skill | Description | Lines | Tags |
|-------|-------------|-------|------|
| [WordPress](contribute/wordpress/SKILL.md) | Contribute to WordPress core - hooks system, coding standards, database API, REST API, testing, Trac workflow | 1,387 | wordpress, php, cms, open-source, core-contribution |

### Extend

| Skill | Description | Lines | Tags |
|-------|-------------|-------|------|
| [Firefox Extension](extend/firefox-extension/SKILL.md) | Build WebExtensions for Firefox - MV2/MV3, APIs, web-ext, AMO publishing | 796 | firefox, webextension, browser-extension, mozilla, amo, manifest-v3 |
| [GIMP Plugin](extend/gimp-plugin/SKILL.md) | GIMP 3.0+ plugins with Python 3 - GEGL operations, image manipulation, UI dialogs | 1,156 | python, gimp, image-processing, graphics, plugin, gegl |
| [Kate Plugin](extend/kate-plugin/SKILL.md) | Develop C++ plugins for Kate text editor - KTextEditor interface, CMake, Qt widgets | ~441 | kate, kde, text-editor, plugin, c++, qt, kde-frameworks |
| [KDE Plasmoid](extend/kde-plasmoid/SKILL.md) | Build Plasma widgets - Python backend, QML UI, Plasma 6+ | 749 | kde, plasma, plasmoid, widget, qml, qt, desktop |
| [mGBA Scripting](extend/mgba/SKILL.md) | Lua scripting for mGBA emulator - game automation, memory hacking, cheats, callbacks | ~800 | lua, emulator, gba, gameboy-advance, scripting, memory-hacking |
| [OpenCode Plugin](extend/opencode/SKILL.md) | Develop plugins, tools, and extensions for OpenCode AI agent - MCP, tools, SDK | 1346 | opencode, plugin, ai-agent, mcp, tool-development |
| [OpenRCT2 Plugin](extend/openrct2/SKILL.md) | JavaScript/TypeScript plugins for OpenRCT2 - game actions, UI windows, hooks, multiplayer | 877 | openrct2, plugin, javascript, typescript, game-modding, rollercoaster-tycoon |
| [Playwright Visual Regression](frameworks/playwright-visual-regression/SKILL.md) | Visual regression testing with Playwright - toHaveScreenshot(), Python/TS, VUDA MCP, masking, thresholds, CI/CD | 1,056 | playwright, visual-regression, screenshot, testing, e2e, vuda, vrt |
| [Thunderbird Extension](extend/thunderbird-extension/SKILL.md) | Build MailExtensions for Thunderbird - messenger.* APIs, compose, ATN | 1,067 | thunderbird, mailextension, email-extension, mozilla, atn, messenger-api |
| [Vagrant](extend/vagrant/SKILL.md) | Development environments - providers, provisioners, multi-machine, plugins, troubleshooting | 1,097 | vagrant, virtualization, devops, virtualbox, infrastructure |
| [Zed Editor](extend/zed-editor/SKILL.md) | Zed extensions - Rust/Wasm plugins, LSP, Tree-sitter, themes, MCP servers, slash commands | 1,012 | zed, editor, extension, rust, wasm, tree-sitter, lsp, plugin |

### Frameworks

| Skill | Description | Lines | Tags |
|-------|-------------|-------|------|
| [aiohttp](frameworks/aiohttp/SKILL.md) | Async HTTP server/client - WebSocket, SSE, middleware, streaming | ~1,700 | python, http, async, server, websocket, sse |
| [BPCore Engine](frameworks/bpcore-engine/SKILL.md) | Lua game framework for GBA - sprites, entities, collision, audio, multiplayer | ~1,100 | lua, gba, game-engine, gameboy-advance |
| [Celery](frameworks/celery/SKILL.md) | Distributed task queue - Redis/RabbitMQ brokers, periodic tasks, Django integration | 1,250 | python, task-queue, async, distributed, celery |
| [Django](frameworks/django/SKILL.md) | Web applications - ORM, views, DRF, middleware, caching, async views, deployment, security, Django 6.0 | 1,499 | python, django, web-framework, orm, rest-api, drf, async, hub |
| [Django Celery](frameworks/django-celery/SKILL.md) | Django Celery integration - distributed tasks, django-celery-beat scheduling, monitoring | ~700 | python, django, celery, task-queue, periodic-tasks, django-celery-beat |
| [Django Bolt](frameworks/django-bolt/SKILL.md) | Rust-powered high-performance API framework - 60k+ RPS, decorator routing, built-in auth, async ORM | 2,503 | python, django, bolt, api, rust, performance, async |
| [Django Ninja](frameworks/django-ninja/SKILL.md) | Fast REST APIs with Pydantic - schemas, authentication, pagination, OpenAPI docs | 1,869 | python, django, rest-api, pydantic, openapi, type-safe |
| [Django Storages](frameworks/django-storages/SKILL.md) | Django cloud storage - S3, Azure, Google Cloud, boto3, file storage backends | ~800 | python, django, storage, s3, azure, gcs, cloud |
| [Django Unfold](frameworks/django-unfold/SKILL.md) | Modern Django admin theme - customization, components, actions, filters, integrations | 653 | python, django, admin, unfold, theme, dashboard |
| [django-allauth](frameworks/django-allauth/SKILL.md) | django-allauth package - local accounts, OAuth, email verification, MFA | ~700 | python, django, authentication, oauth, mfa, allauth |
| [httpx](frameworks/httpx/SKILL.md) | Modern async HTTP client - sync/async API, HTTP/2, connection pooling, retries | ~1,500 | python, http, async, client, network |
| [LlamaIndex](frameworks/llama-index/SKILL.md) | LLM applications - RAG, retrievers, agents, vector stores, streaming, evaluation | 1,130 | python, llm, rag, llamaindex, ai, vector-store, agents |
| [Pydantic](frameworks/pydantic/SKILL.md) | Data validation - type hints, models, serialization, settings, FastAPI | ~1,700 | python, validation, pydantic, serialization, settings |
| [pygame](frameworks/pygame/SKILL.md) | Python 2D game development - sprites, surfaces, events, sound, fonts, game loops, collision | 549 | python, game-development, 2d-games, pygame, graphics |
| [PyQt](frameworks/pyqt/SKILL.md) | Desktop applications - PyQt5/PyQt6/PySide6, widgets, signals, layouts, threading, testing | 1,941 | python, qt, pyqt, pyside, gui, desktop, hub |
|   ↳ [pyqt-core](frameworks/pyqt/core/SKILL.md) | QtCore fundamentals - signals, slots, properties, timers, settings, file I/O | 487 | python, qt, pyqt, core, signals |
|   ↳ [pyqt-dialogs](frameworks/pyqt/dialogs/SKILL.md) | Dialogs - QFileDialog, QMessageBox, custom dialogs, Qt 6 | 518 | python, qt, pyqt, dialogs, ui |
|   ↳ [pyqt-multimedia](frameworks/pyqt/multimedia/SKILL.md) | Audio, video, camera, recording | ~700 | python, qt, pyqt, multimedia, audio, video |
|   ↳ [pyqt-styling](frameworks/pyqt/styling/SKILL.md) | QSS styling - selectors, properties, dark theme | 668 | python, qt, pyqt, styling, qss, css |
|   ↳ [pyqt-testing](frameworks/pyqt/testing/SKILL.md) | Testing - pytest-qt, qtbot, waitSignal, fixtures | 444 | python, qt, pyqt, testing, pytest |
|   ↳ [pyqt-threading](frameworks/pyqt/threading/SKILL.md) | Threading - QThread, QThreadPool, QTimer, thread safety | 439 | python, qt, pyqt, threading, concurrency |
|   ↳ [pyqt-widgets](frameworks/pyqt/widgets/SKILL.md) | QtWidgets - buttons, inputs, containers, item views, layouts | 610 | python, qt, pyqt, widgets, gui |
| [pytest](frameworks/pytest/SKILL.md) | Python testing framework - fixtures, parametrization, asyncio, Django, mocking, coverage, parallel | 1,481 | python, testing, tdd, fixtures, unit-test |
| [Qt C++](frameworks/qt-cpp/SKILL.md) | Cross-platform desktop apps - signals/slots, QML, threading, CMake, deployment | 1,204 | qt, c++, gui, desktop, qt6, cmake, cross-platform, qml |
| [ratatui](frameworks/ratatui/SKILL.md) | Rust TUI framework - terminal UI, widgets, layouts, event handling, cross-platform | 651 | rust, tui, terminal, cli, user-interface |
| [SQLAlchemy](frameworks/sqlalchemy/SKILL.md) | Python SQL toolkit and ORM - queries, relationships, async, Alembic migrations | 1,179 | python, orm, database, sql, alembic, async |
| [Tailwind CSS](frameworks/tailwind/SKILL.md) | Utility-first CSS framework v4 - CSS-first config, @theme directive, Oxide engine | 540 | css, frontend, responsive, design-system, utility-first |
| [TurboDRF](frameworks/turbodrf/SKILL.md) | Fast Django REST framework - automatic OpenAPI, caching, JWT auth | ~600 | python, django, rest-api, openapi, fast |
| [Django HTMX](frameworks/django-htmx/SKILL.md) | Build modern dynamic web apps with Django and htmx - partial rendering, HTMX-specific responses, template tags, middleware, CSRF protection | 415 | django, htmx, python, web, frontend, partial-rendering |
| [django-filter](frameworks/django-filter/SKILL.md) | Django filtering library - querysets, DRF integration, custom filters, FilterSet | 451 | django, django-filter, filtering, django-rest-framework, queryset |
### Tool

| Skill | Description | Lines | Tags |
|-------|-------------|-------|------|
| [ast-grep](tool/ast-grep/SKILL.md) | AST-based code search and rewriting - structural patterns, linting, refactoring, multi-language | 531 | ast-grep, code-search, linting, refactoring, cli, ast |
| [Docker](tool/docker/SKILL.md) | Containers - Dockerfile, docker-compose, BuildKit, multi-stage builds, production, CI/CD, security | 432 | docker, docker-compose, containerization, deployment, ci-cd |
| [Redis](tool/redis/SKILL.md) | In-memory database - caching, pub/sub, sessions, rate limiting, data structures | 315 | redis, database, caching, pub-sub, sessions, rate-limiting |
| [SQLite](tool/sqlite/SKILL.md) | Embedded database - SQL queries, schema design, Python integration, FTS5, optimization, concurrent access | 814 | sqlite, database, sql, embedded, python |
| [Waydroid](tool/waydroid/SKILL.md) | Android on Linux - container-based Android with Wayland, GPU acceleration, GAPPS | ~657 | waydroid, android, container, linux, wayland, gapps |

### Languages

| Skill | Description | Lines | Tags |
|-------|-------------|-------|------|
| [Rust Common Pitfalls](languages/rust-common-pitfalls/SKILL.md) | Common Rust development pitfalls - compiler errors, struct constructors, test organization, coverage enforcement | 730 | rust, pitfalls, best-practices, common-errors, testing |

## Skill Format

Each skill follows the [skillreg.dev specification](https://skillreg.dev/docs/skill-md-reference):

```yaml
---
name: skill-name
description: Short description of the skill
metadata:
  author: Author Name
  version: 1.0.0
  tags:
    - tag1
    - tag2
---

# Skill Title

[Markdown content with guidelines, examples, references...]
```

## How to Use

1. **Browse available skills** in the `contribute/`, `extend/`, or `frameworks/` directories
2. **Copy the SKILL.md** to your project or load it into your AI assistant
3. **Reference the skill** when asking for help with that domain
4. **Customize** as needed for your specific project
