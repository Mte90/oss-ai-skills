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

### Frameworks

| Skill | Description | Lines | Tags |
|-------|-------------|-------|------|
| [Qt C++](frameworks/qt-cpp/SKILL.md) | Cross-platform desktop apps - signals/slots, QML, threading, CMake, deployment | 1,204 | qt, c++, gui, desktop, qt6, cmake, cross-platform, qml |
| [PyQt](frameworks/pyqt/SKILL.md) | Desktop applications - PyQt5/PyQt6/PySide6, widgets, signals, layouts, packaging | 1,104 | python, qt, pyqt, pyside, gui, desktop |
| [Django](frameworks/django/SKILL.md) | Web applications - ORM, views, DRF, middleware, caching, async views, deployment | 1,464 | python, django, web-framework, orm, rest-api, drf, async |
| [LlamaIndex](frameworks/llama-index/SKILL.md) | LLM applications - RAG, retrievers, agents, vector stores, streaming, evaluation | 1,130 | python, llm, rag, llamaindex, ai, vector-store, agents |

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
