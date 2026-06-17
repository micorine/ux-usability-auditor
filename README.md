# ux-usability-auditor

Automated UX/usability audit skill for OpenClaw. Navigates target web app pages via Playwright, captures screenshots and UI metadata, then generates a structured audit report mapped to a configurable checklist.

 Supports Chinese and English enterprise web apps (Element UI, Ant Design, custom frameworks).

---

## What It Does

1. **Login** — automatically fills and submits the login form
2. **Discover pages** — auto-detects sidebar/menu pages, or accepts an explicit page list
3. **Inspect each page** — takes screenshot, counts buttons/inputs/tables, detects empty states, breadcrumbs, search areas
4. **Generate report** — maps inspection data to a 28-item usability checklist, outputs a prioritized markdown report

---

## Quick Start

### 1. Install the Skill

```bash
# Via OpenClaw (recommended)
skillhub_install install_skill ux-usability-auditor

# Or manually: download ux-usability-auditor.skill, then in OpenClaw:
# /skill install path/to/ux-usability-auditor.skill
```

### 2. Create a config file

Save as `config.json` in your working directory:

```json
{
  "name": "My App Usability Audit",
  "targetUrl": "https://example.com/#/login",
  "login": {
    "username": "admin",
    "password": "123456"
  },
  "pages": [],
  "outputDir": "./results",
  "screenshotDir": "./results/screenshots",
  "headless": false
}
```

> **Tip:** Leave `pages` as `[]` to auto-discover pages from the sidebar after login.

### 3. Run the inspection

```bash
node scripts/inspect.js config.json
```

Output: `results/inspection_results.json` + `results/screenshots/*.png`

### 4. Generate the report

```bash
node scripts/generate_report.js config.json
```

Output: `results/usability_audit_report.md`

---

## Configuration Reference

| Field | Required | Description |
|---|---|---|
| `name` | ✅ | Audit report title |
| `targetUrl` | ✅ | Login page URL |
| `login.username` | ✅ | Login username |
| `login.password` | ✅ | Login password |
| `login.usernameSelector` | ❌ | CSS selector for username input (auto-detected if omitted) |
| `login.passwordSelector` | ❌ | CSS selector for password input (auto-detected if omitted) |
| `login.submitSelector` | ❌ | CSS selector for submit button (auto-detected if omitted) |
| `pages` | ❌ | Explicit page list ` [{"name":"Dashboard","url":"/#/dashboard"}]`; `[]` = auto-discover |
| `checklist` | ❌ | Path to custom checklist markdown file |
| `outputDir` | ❌ | Output directory (default: `./results`) |
| `screenshotDir` | ❌ | Screenshot directory (default: `./results/screenshots`) |
| `headless` | ❌ | Run browser headlessly (default: `false`) |
| `maxPages` | ❌ | Max pages to inspect (default: `80`) |
| `pageWaitMs` | ❌ | Wait time per page in ms (default: `2500`) |

---

## Default Checklist

The built-in checklist (`references/checklist-default.md`) covers 28 items across 6 categories:

| # | Category | Items |
|---|----------|-------|
| 1 | Navigation & Page Layout | 5 items — menu hierarchy, global search, visual prominence |
| 2 | Operation Flow Efficiency | 5 items — step minimization, quick fill, validation proactiveness |
| 3 | Prompt & Copy Interaction | 5 items — error precision, empty state guidance, copy uniformity |
| 4 | Learnability & Help | 4 items — new user guidance, help system, config thresholds |
| 5 | Cross-Device Consistency | 3 items — field consistency, mobile adaptation |
| 6 | Fault Tolerance & Security | 4 items — undo, high-risk confirmation, upload feedback |

To customize: copy `references/checklist-default.md`, modify items, and point `checklist` in your config to the new file.


## Requirements

- **Node.js** ≥ 16
- **Playwright** (auto-installed on first run: `npm install playwright`)
- **Chromium** (auto-downloaded by Playwright)

No Python required. Works on Windows / macOS / Linux.

---

## Manual Steps (After Auto-Inspection)

The automated inspection covers ~70% of checklist items. These items require manual verification:

- 4.1 New user guidance (first-login onboarding)
- 4.2 Help system completeness
- 6.2 High-risk operation confirmation dialog
- 6.1 Misoperation undo / draft recovery
- 2.1 Core flow step count (manual trace)

The report marks these as `❓ 无法评估` — fill in manually after verification.

---

## License

MIT — free to use, modify, and share.

---

## Contributing

PRs welcome! Especially:
- Additional checklist templates (WCAG accessibility, mobile-specific, etc.)
- Improved auto-discovery for different UI frameworks
- Better evaluation heuristics

---

## 中文说明

自动化的 Web 应用易用性巡检 Skill。基于 Playwright 自动登录、遍历页面、截图并提取 UI 元素数据，对照易用性检查项清单生成结构化巡检报告。

适用于：企业内部 Web 系统、SaaS 产品、中后台系统的易用性评估。

详细中文使用说明见 `SKILL.md`。
