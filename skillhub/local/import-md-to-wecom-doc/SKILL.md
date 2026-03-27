---
name: import-md-to-wecom-doc
description: Import local Markdown content into WeCom (Tencent) Docs with high format fidelity. Use when users ask to migrate/copy/sync `.md` into `doc.weixin.qq.com` and require code blocks, line breaks, and tables to render correctly, or when post-import table width/code formatting needs repair.
---

# Import Md To Wecom Doc

## Overview

Import Markdown into WeCom Docs in two phases: structured content import, then native layout correction.
Prioritize three outcomes: code blocks stay as code blocks, line breaks remain consistent, and tables stay readable.

## Required Inputs

1. Source markdown absolute path (for example: `开发部分/DDD规范/DDD开发实践规范.md`)
2. Target WeCom Doc URL (`https://doc.weixin.qq.com/doc/...`)
3. User preference for table layout when table readability is poor (default: fit to page width)

## Workflow

### 1. Inspect Source Markdown

1. Read markdown content fully.
2. Confirm key structures exist and should be preserved:
- headings
- lists
- fenced code blocks
- tables
- blank-line based paragraph breaks
3. Record approximate table count to plan post-import verification.

### 2. Import with Structured Paste (not manual plain paste)

1. Open target WeCom Doc in editable mode.
2. Convert markdown to structured HTML plus plain text fallback.
3. Paste through editor clipboard manager (preferred) to maximize structure retention.

Reference snippet:

```js
window.workbench.editor.clipboardManager.pasteFromClipboard(html, plainText)
```

Do not rely on manual Ctrl/Cmd+V as the primary method when format fidelity is required.

### 3. First-Pass Verification After Import

1. Confirm code sections are rendered as code blocks (monospace/highlighted block), not plain paragraph text.
2. Confirm major list and paragraph breaks are visually consistent.
3. Identify tables with narrow columns or vertical text wrapping.

### 4. Fix Table Readability with Native Table Tools (critical)

When any table is too narrow, apply WeCom native command:

`表格布局 -> 自动调整 -> 自适应页面宽度`

Rules:
1. Fix visible problematic tables first (user screenshot/table section priority).
2. Then run full-document pass and apply the same adjustment table-by-table.
3. Prefer native adjustment over CSS-only attempts; imported width styles are often overridden by WeCom rendering.

### 5. Whole-Document Table Pass

For reliable coverage:
1. Scroll from top to bottom.
2. For each table encountered, execute `自适应页面宽度`.
3. Track processed tables to avoid duplicates.
4. Re-check at least one previously problematic section after batch pass.

### 6. Final Validation and User Handoff

1. Validate:
- code blocks display correctly
- no obvious line-break corruption
- no narrow unreadable tables in key sections
2. Report:
- import method used
- number of tables adjusted (if counted)
- any known residual issue and exact section location
3. If user still reports one bad table, do targeted re-fix on that specific section, then re-verify.

## Failure Handling

1. If structured paste succeeds but table widths are still poor:
- do not re-import immediately
- apply native table layout fixes first
2. If code blocks degrade after a re-import attempt:
- revert by re-importing once with structured paste only
- avoid mixed manual paste edits before verification
3. If only a subset of tables remain problematic:
- repair those tables in-place
- avoid global re-import to prevent regressions in already-correct sections

## Definition of Done

1. Markdown content is in target WeCom Doc.
2. Code blocks are rendered as code blocks.
3. Line breaks are visually consistent with source intent.
4. Tables are readable, with problematic narrow tables corrected via native table layout tools.
