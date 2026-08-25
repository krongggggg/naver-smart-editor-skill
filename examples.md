# Naver Smart Editor — Examples

Set project root once:

```bash
SE="${SMART_EDITOR_ROOT:?clone naver-smart-editor-cli and export SMART_EDITOR_ROOT}"
```

## Draft post (no publish)

```bash
node "$SE/bin/smart-editor.js" title set "오늘의 메모"
node "$SE/bin/smart-editor.js" text write "본문 첫 줄\n"
node "$SE/bin/smart-editor.js" format bold
node "$SE/bin/smart-editor.js" text write "강조할 문장"
node "$SE/bin/smart-editor.js" save
```

## Publish setup without confirming

```bash
node "$SE/bin/smart-editor.js" publish config --json '{
  "category": "일상",
  "tags": ["일상", "메모"],
  "openType": "public",
  "options": { "comment": true, "search": true },
  "confirm": false
}'
node "$SE/bin/smart-editor.js" publish state
node "$SE/bin/smart-editor.js" publish close
```

## JSON script: full draft

```json
{
  "stopOnError": true,
  "steps": [
    { "action": "setTitle", "value": "테스트 글" },
    { "action": "focus", "compType": "text" },
    { "action": "writeText", "value": "첫 문단입니다.\n" },
    { "action": "lineBreak" },
    { "action": "writeText", "value": "두 번째 문단" },
    { "action": "format", "style": "bold" },
    { "action": "writeText", "value": "굵은 텍스트" },
    { "action": "save" }
  ]
}
```

Run: `node "$SE/scripts/run-script.js" draft.json`

## Agent handoff snippet

1. `SMART_EDITOR_ROOT` = path to `naver-smart-editor-cli` clone
2. CDP 9223 connected + Naver write page open
3. Use `node "$SE/bin/smart-editor.js"` or `run-script.js`
4. No `publish confirm` without user OK

## Scheduled publish workflow

For cron or task runners:

```bash
curl -s http://127.0.0.1:9223/json/version   # CDP up?
# Write tab: https://blog.naver.com/{blogId}?Redirect=Write&categoryNo=...
node "$SE/scripts/run-script.js" "$SE/examples/sample-script.json"
```

Replace `{blogId}` and `categoryNo` with your blog values. Keep the write page open in the CDP-connected browser before running scripts.
