# Naver Smart Editor — Reference

## CLI commands

### Editor

| Command | Description |
|---------|-------------|
| `connect` | Test CDP + editor connection |
| `info` | Title preview, components, isEmpty |
| `modules` | List document + property toolbar modules |
| `commands` | List commandManager commands |
| `title get` / `title set <text>` | Document title |
| `text get` / `text write <text>` / `text break` | Body text |
| `format <bold\|italic\|underline\|strikethrough>` | Toggle style |
| `align <left\|center\|right>` | Alignment |
| `toolbar <data-name>` | Click any `[data-name]` button |
| `module <name> [variant]` | High-level toolbar alias |
| `image url <url> [...]` | Insert images by URL |
| `document` | Export full Smart Editor JSON |

### Save / Publish popup

| Command | Description |
|---------|-------------|
| `save` | Click 저장 |
| `save drafts` | Open 임시저장 list |
| `publish open` | Open 발행 popup |
| `publish state` | Read popup fields |
| `publish close` | Close popup |
| `publish categories` | List category names |
| `publish category <name>` | Select category |
| `publish tags <a,b,c>` | Set tags |
| `publish visibility <type>` | `public` `neighbor` `both_neighbor` `private` |
| `publish option <name> <on\|off>` | `comment` `sympathy` `search` `scrap` `outside` `notice` `setDefault` |
| `publish time <now\|schedule>` | Publish time radio |
| `publish confirm` | Click final 발행 (live!) |
| `publish config --json '{...}'` | Batch configure; `"confirm": true` to publish |

### `publish config` JSON keys

```json
{
  "category": "일상",
  "tags": ["a", "b"],
  "openType": "public",
  "time": "now",
  "options": {
    "comment": true,
    "sympathy": true,
    "search": true,
    "scrap": true,
    "outside": true,
    "notice": false,
    "setDefault": false
  },
  "confirm": false
}
```

## JSON script actions

| action | fields | notes |
|--------|--------|-------|
| `setTitle` | `value` | |
| `writeText` | `value` | appends to body |
| `lineBreak` | | new paragraph |
| `format` | `style` | bold, italic, … |
| `align` | `value` | left, center, right |
| `focus` | `compType` | default `text` |
| `toolbar` | `dataName` | raw data-name click |
| `module` | `name`, `variant?` | alias → modules.* |
| `imageUrl` | `urls` | array |
| `link` | `url` | text link |
| `wait` | `ms` | default 500 |
| `getTitle` / `getContent` / `getDocument` | | read-only |
| `save` | | |
| `publishOpen` / `publishClose` / `publishState` | | |
| `publishCategory` | `name` | |
| `publishTags` | `tags` | array |
| `publishVisibility` | `value` | openType |
| `publishOption` | `name`, `enabled` | boolean |
| `publishTime` | `value` | now, schedule |
| `publishConfirm` | | live publish |
| `publishConfig` | `config` | same as CLI config |

## Document toolbar `data-name`

`image` `social-media-image` `video` `sticker` `insert-quotation` `insert-horizontal-line` `oglink` `file` `schedule` `code` `table` `formula` `map` `shopping-connect` `search` `moment` `library` `template`

## Property toolbar `data-name`

`text-format` `font-family` `font-size` `bold` `italic` `underline` `strikethrough` `font-color` `background-color` `align` `line-height` `list` `drop-cap` `superscript` `subscript` `special-letter` `text-link` `speller`

## Environment

| Variable | Default | Purpose |
|----------|---------|---------|
| `CDP_PORT` | `9223` | Chrome DevTools port |
| `SMART_EDITOR_ROOT` | _(required)_ | Path to `naver-smart-editor-cli` clone |
