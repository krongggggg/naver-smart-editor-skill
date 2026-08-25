---
name: naver-smart-editor
description: Controls Naver Blog Smart Editor ONE via CDP (port 9223). Use when writing or publishing Naver blog posts programmatically, automating the smart editor toolbar, setting title/body/tags, or when the user mentions smart-editor CLI, agent-browser 9223, or blog.naver.com write page.
---

# Naver Smart Editor CLI

Automates **Smart Editor ONE** on an already-open Naver blog write page through Chrome DevTools Protocol.

## CLI project root

Clone the repo and set `SMART_EDITOR_ROOT` to your checkout path:

```bash
git clone https://github.com/YOUR_ORG/naver-smart-editor-cli.git
cd naver-smart-editor-cli
npm install
export SMART_EDITOR_ROOT="$(pwd)"
```

All commands below use:

```bash
SE="${SMART_EDITOR_ROOT:?set SMART_EDITOR_ROOT to your clone path}"
```

## Prerequisites (check first)

1. **CDP** on `127.0.0.1:9223` — `agent-browser autoconnect 9223`
2. **Write page open** — `https://blog.naver.com/{blogId}?Redirect=Write&categoryNo=...`
3. **Editor loaded** — PostWriteForm iframe ready
4. **Dependencies** — `npm install` in `$SE`

Verify:

```bash
curl -s http://127.0.0.1:9223/json/version
node "$SE/bin/smart-editor.js" connect
node "$SE/bin/smart-editor.js" info
```

Set `CDP_PORT` if not 9223.

## Standard workflow

```
- [ ] connect / info
- [ ] title set
- [ ] text write (+ format / image url as needed)
- [ ] save (optional)
- [ ] publish open → category / tags / visibility
- [ ] publish confirm — ONLY with explicit user approval
```

## Method 1: CLI

```bash
SE="${SMART_EDITOR_ROOT:?}"

node "$SE/bin/smart-editor.js" title set "글 제목"
node "$SE/bin/smart-editor.js" text write "본문"
node "$SE/bin/smart-editor.js" format bold
node "$SE/bin/smart-editor.js" module table
node "$SE/bin/smart-editor.js" image url https://example.com/a.jpg
node "$SE/bin/smart-editor.js" save
node "$SE/bin/smart-editor.js" publish config --json '{"category":"일상","tags":["메모"],"openType":"public","confirm":false}'
```

Help: `node "$SE/bin/smart-editor.js" help`

## Method 2: JSON script

```bash
node "$SE/scripts/run-script.js" path/to/script.json
```

See [examples.md](examples.md) for templates.

## Method 3: Node library

```javascript
import { connect } from './src/index.js'; // from project root

const { editor, modules, publish, disconnect } = await connect({ port: 9223 });
await editor.setTitle('제목');
await modules.writeText('본문');
await publish.save();
await disconnect();
```

## API objects

| Object | Use for |
|--------|---------|
| `editor` | title, body, formatting, document JSON |
| `modules` | toolbar (photo, table, bold, …) |
| `publish` | save, publish popup |

## Module aliases

`photo` `video` `table` `quote`/`quotation` `link`/`oglink` `code` `map` `sticker` `file` `schedule` `formula` `line` `shopping` `search`

## Safety rules

- One agent per CDP 9223 session
- **`publish confirm` = live publish** — user approval required
- `module photo`/`video` opens file dialog — use `image url` for URLs
- Prefer `save` when testing

## More detail

- [reference.md](reference.md) — all CLI & JSON actions
- [examples.md](examples.md) — copy-paste workflows
