# OSS AI Skills Collection
[![License](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)   

A comprehensive collection of AI assistant skills (SKILL.md files) following the [skillreg.dev](https://skillreg.dev/docs/skill-md-reference) specification, designed to help contribute to and extend open-source projects (for plugins).

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
| [Thunderbird Extension](extend/thunderbird-extension/SKILL.md) | Build MailExtensions for Thunderbird - messenger.* APIs, compose, ATN | 1,067 | thunderbird, mailextension, email-extension, mozilla, atn, messenger-api |
| [KDE Plasmoid](extend/kde-plasmoid/SKILL.md) | Build Plasma widgets - Python backend, QML UI, Plasma 6+ | 749 | kde, plasma, plasmoid, widget, qml, qt, desktop |
| [Vagrant](extend/vagrant/SKILL.md) | Development environments - providers, provisioners, multi-machine, plugins, troubleshooting | 1,097 | vagrant, virtualization, devops, virtualbox, infrastructure |
| [OpenCode Plugin](extend/opencode/SKILL.md) | Develop plugins, tools, and extensions for OpenCode AI agent - MCP, tools, SDK | 976 | opencode, plugin, ai-agent, mcp, tool-development |
| [GIMP Plugin](extend/gimp-plugin/SKILL.md) | GIMP 3.0+ plugins with Python 3 - GEGL operations, image manipulation, UI dialogs | 1,156 | python, gimp, image-processing, graphics, plugin, gegl |
| [mGBA Scripting](extend/mgba/SKILL.md) | Lua scripting for mGBA emulator - game automation, memory hacking, cheats, callbacks | ~800 | lua, emulator, gba, gameboy-advance, scripting, memory-hacking |
| [OpenRCT2 Plugin](extend/openrct2/SKILL.md) | JavaScript/TypeScript plugins for OpenRCT2 - game actions, UI windows, hooks, multiplayer | 877 | openrct2, plugin, javascript, typescript, game-modding, rollercoaster-tycoon |
| [Kate Plugin](extend/kate-plugin/SKILL.md) | Develop C++ plugins for Kate text editor - KTextEditor interface, CMake, Qt widgets | ~600 | kate, kde, text-editor, plugin, c++, qt, kde-frameworks |
| [Playwright Visual Regression](extend/playwright-visual-regression/SKILL.md) | Visual regression testing with Playwright - screenshot comparison, baselines, CI/CD (based on maxrihter/claude-skill-visual-regression) | 715 | playwright, visual-regression, screenshot, testing, e2e, snapshot |

### Frameworks

| Skill | Description | Lines | Tags |
|-------|-------------|-------|------|
| [Qt C++](frameworks/qt-cpp/SKILL.md) | Cross-platform desktop apps - signals/slots, QML, threading, CMake, deployment | 1,204 | qt, c++, gui, desktop, qt6, cmake, cross-platform, qml |
| [PyQt](frameworks/pyqt/SKILL.md) | Desktop applications - PyQt5/PyQt6/PySide6, widgets, signals, layouts, threading, testing | 1,941 | python, qt, pyqt, pyside, gui, desktop, hub |
|   ↳ [pyqt-core](frameworks/pyqt/core/SKILL.md) | QtCore fundamentals - signals, slots, properties, timers, settings, file I/O | 487 | python, qt, pyqt, core, signals |
|   ↳ [pyqt-widgets](frameworks/pyqt/widgets/SKILL.md) | QtWidgets - buttons, inputs, containers, item views, layouts | 610 | python, qt, pyqt, widgets, gui |
|   ↳ [pyqt-threading](frameworks/pyqt/threading/SKILL.md) | Threading - QThread, QThreadPool, QTimer, thread safety | 439 | python, qt, pyqt, threading, concurrency |
|   ↳ [pyqt-dialogs](frameworks/pyqt/dialogs/SKILL.md) | Dialogs - QFileDialog, QMessageBox, custom dialogs | 409 | python, qt, pyqt, dialogs, ui |
|   ↳ [pyqt-testing](frameworks/pyqt/testing/SKILL.md) | Testing - pytest-qt, qtbot, waitSignal, fixtures | 444 | python, qt, pyqt, testing, pytest |
|   ↳ [pyqt-styling](frameworks/pyqt/styling/SKILL.md) | QSS styling - selectors, properties, dark theme | 668 | python, qt, pyqt, styling, qss, css |
| [pygame](frameworks/pygame/SKILL.md) | Python 2D game development - sprites, surfaces, events, sound, fonts, game loops, collision | 549 | python, game-development, 2d-games, pygame, graphics |
| [ratatui](frameworks/ratatui/SKILL.md) | Rust TUI framework - terminal UI, widgets, layouts, event handling, cross-platform | 651 | rust, tui, terminal, cli, user-interface |
| [Django](frameworks/django/SKILL.md) | Web applications - ORM, views, DRF, middleware, caching, async views, deployment | 1,464 | python, django, web-framework, orm, rest-api, drf, async |
| [Django Ninja](frameworks/django-ninja/SKILL.md) | Fast REST APIs with Pydantic - schemas, authentication, pagination, OpenAPI docs | 1,869 | python, django, rest-api, pydantic, openapi, type-safe |
| [Tailwind CSS](frameworks/tailwind/SKILL.md) | Utility-first CSS framework - responsive design, dark mode, component patterns | 1,751 | css, frontend, responsive, design-system, utility-first |
| [Celery](frameworks/celery/SKILL.md) | Distributed task queue - Redis/RabbitMQ brokers, periodic tasks, Django integration | 1,250 | python, task-queue, async, distributed, celery |
| [pytest](frameworks/pytest/SKILL.md) | Python testing framework - fixtures, parametrization, asyncio, Django, mocking, coverage, parallel | 1,481 | python, testing, tdd, fixtures, unit-test |
| [httpx](frameworks/httpx/SKILL.md) | Modern async HTTP client - sync/async API, HTTP/2, connection pooling, retries | ~1,500 | python, http, async, client, network |
| [aiohttp](frameworks/aiohttp/SKILL.md) | Async HTTP server/client - WebSocket, SSE, middleware, streaming | ~1,700 | python, http, async, server, websocket, sse |
| [Pydantic](frameworks/pydantic/SKILL.md) | Data validation - type hints, models, serialization, settings, FastAPI | ~1,700 | python, validation, pydantic, serialization, settings |
| [SQLAlchemy](frameworks/sqlalchemy/SKILL.md) | Python SQL toolkit and ORM - queries, relationships, async, Alembic migrations | 1,179 | python, orm, database, sql, alembic, async |
| [LlamaIndex](frameworks/llama-index/SKILL.md) | LLM applications - RAG, retrievers, agents, vector stores, streaming, evaluation | 1,130 | python, llm, rag, llamaindex, ai, vector-store, agents |
| [TurboDRF](frameworks/turbodrf/SKILL.md) | Fast Django REST framework - automatic OpenAPI, caching, JWT auth | ~600 | python, django, rest-api, openapi, fast |
| [django-allauth](frameworks/django-allauth/SKILL.md) | Django authentication - local accounts, OAuth, email verification, MFA | ~700 | python, django, authentication, oauth, mfa |
| [BPCore Engine](frameworks/bpcore-engine/SKILL.md) | Lua game framework for GBA - sprites, entities, collision, audio, multiplayer | ~1,100 | lua, gba, game-engine, gameboy-advance |

### Contribute

| Skill | Description | Tags |
|-------|-------------|------|
| [WordPress](contribute/wordpress/SKILL.md) | Contribute to WordPress core (hooks, coding standards, Trac workflow) | wordpress, php, cms, open-source, core-contribution |

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
