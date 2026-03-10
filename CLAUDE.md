# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a personal **Obsidian vault** synced via iCloud. It serves as a knowledge management system covering work logs, personal projects, journaling, and investment research.

## Vault Structure

- `0-inbox/` — Capture zone for quick notes (inbox.md)
- `2-project/` — Active projects (books, mentoring, stock research, side projects)
- `3-garden/` — Evergreen/growing notes (stock analysis, English study, mentoring logs)
- `4-area/` — Life areas (finance, AI, work, health, real estate, politics)
- `4-worklog/` — Work-related logs and archives, organized by sprint/phase (3차, 4차, 5차 etc.)
- `journal/` — Daily notes (`0-d/`), weekly reviews (`1-w/`), task dashboard, focus workflow
- `template/` — Note templates including daily deep-work template, worklog templates, journal templates
- `Excalidraw/` — Excalidraw drawing files
- `.obsidian/` — Obsidian configuration (plugins, themes, workspace)

## Key Obsidian Plugins

Installed community plugins: Table Editor, Admonition, TOC, **Templater**, Relative Line Numbers, Outliner, **Excalidraw**, **Dataview**, **Tasks**, Calendar, Minimal Settings.

## Task System

Tasks use the **Obsidian Tasks plugin** with this convention:
- Checkbox syntax: `- [ ] 할 일`
- Due dates: `📅 YYYY-MM-DD` (emoji format required by Tasks plugin)
- Focus tags: `#focus/study`, `#focus/dev`, `#focus/stock`
- Central dashboard: `journal/task-dashboard.md` — aggregates tasks vault-wide using `tasks` code blocks
- Workflow guide: `journal/focus-workflow.md` — Pomodoro-based daily focus routine (25min × 2)

## Daily Notes

- Path: `journal/0-d/` with format `YY-MM-DD`
- Template: `template/_templates/deep-work-daily.md` (via Templater)
- Structure: Deep Work Slot (start/end time), 오늘의 1시간 task, 잡일, 종료 review

## Conventions

- Content is primarily in **Korean**; generate all vault content in Korean
- Work archives are organized by sprint phases (3차, 4차, 5차) under `4-worklog/archive/`
- When creating notes, follow existing templates in `template/` directory
- Obsidian wiki-links (`[[note-name]]`) are used for internal linking
