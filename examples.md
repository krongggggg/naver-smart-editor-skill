# Naver Smart Editor — Examples

Set project root once:

```bash
SE="${SMART_EDITOR_ROOT:-/Users/macmini/orca/projects/스마트 에디터 cli}"
```

## Draft post (no publish)

```bash
node "$SE/bin/smart-editor.js" title set "8월 앱테크 퀴즈 정답"
node "$SE/bin/smart-editor.js" text write "오늘자 정리\n"
node "$SE/bin/smart-editor.js" format bold
node "$SE/bin/smart-editor.js" text write "중요: 오후껀 바뀔 수 있음"
node "$SE/bin/smart-editor.js" save
```

## Publish setup without confirming

```bash
node "$SE/bin/smart-editor.js" publish config --json '{
  "category": "앱테크",
  "tags": ["앱테크", "퀴즈"],
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

1. `SMART_EDITOR_ROOT` = `/Users/macmini/orca/projects/스마트 에디터 cli`
2. CDP 9223 connected + Naver write page open
3. Use `node "$SE/bin/smart-editor.js"` or `run-script.js`
4. No `publish confirm` without user OK

## Daily apptech promo (08:00 KST)

Loop: `앱테크 퀴즈 정답사이트/scripts/promo-daily-loop.sh`

When the loop fires, read `naver-smart-editor` skill then:

```bash
curl -s http://127.0.0.1:9223/json/version   # CDP up?
# Write tab: https://blog.naver.com/cury8282?Redirect=Write&categoryNo=18
node "/Users/macmini/orca/projects/스마트 에디터 cli/scripts/publish-apptech.js"
```

Script pulls today's answers from production D1 and publishes plain-text body (same format as `lib/blog-html.ts` `buildNaverBlogPlainText`).
