# PR screenshots (hosted on a side branch)

Captured from the **built extension** (`pnpm build` → `.output/chrome-mv3`) for
[eigenlux-ai/interline-translator#1](https://github.com/eigenlux-ai/interline-translator/pull/1).
They live on this `pr-assets` branch so binary files do not enter the PR diff.

## How the captures were made

- Chromium **131.0.6778.33** (Playwright 1.49 full `chromium` channel) in a
  persistent context with
  `--disable-extensions-except` + `--load-extension=.output/chrome-mv3`,
  headed under Xvfb, viewport 1440×900, `locale=zh-CN`.
- The extension **service worker** was asserted non-empty before capturing
  (`chrome-extension://<id>/background.js`), i.e. the extension really loaded.
- Content-script page: `test-pages/article-sample.html` served over HTTP.
  The first paragraph was selected, the pill clicked, and the card dragged by its
  header with real mouse events (before `(461,157)` → after `(201,397)`).
- Settings: the options page opens in a tab (this extension has no toolbar
  popup), so `03`–`05` are the real options UI with a seeded config.
- **Console errors and page errors were collected; the capture run finished with
  zero.** (`summary.json` of the run is not committed; the harness lives outside
  this branch.)
- No screenshot was edited or composed; only the viewport was cropped to the
  screenshot.

### Dark run

A second pass repeated the whole set with the extension in dark appearance
(`appearance.colorScheme: 'dark'`, `emulateMedia({ colorScheme: 'dark' })`) on
`test-pages/host-theme-nav.html` — a fixture whose **own** dark mode is a class
on `<html>`, toggled through its button. Same Chromium/extension/service-worker
assertion, same zero console/page-error gate. The card drag moved the header
from `(461,219)` to `(201,459)` in that run.

## One disclosure

The translation text in the selection-card screenshots comes from a **local mock
OpenAI-compatible provider** (127.0.0.1). The built-in free engine (Google) rate
limits the capture host with HTTP 429, so live MT was not usable here. Everything
else — the extension build, the shadow-DOM surface, the card, the drag
interaction, the options UI, the confirmation popover — is the real built
extension.

## Files

| File | What it shows |
| --- | --- |
| `01-selection-card.png` | Selection card anchored under the selected paragraph (streamed translation). |
| `02-selection-card-dragged.png` | The same card after dragging it by its header to a new spot; a new selection would re-anchor it. |
| `03-options-general.png` | Options page (general section) with the seeded engines. |
| `04-options-providers.png` | Engine list: **「第三方服务」** badge on the third-party `anthropic-compatible` endpoint. |
| `05-options-export-confirm.png` | The "export with keys" confirmation stating the JSON will contain plaintext API keys. |
| `01-selection-card-dark.png` | Dark appearance: card anchored under the selected paragraph on a dark host page. |
| `02-selection-card-dragged-dark.png` | Dark appearance: the same card after dragging it by its header. |
| `03-options-general-dark.png` | Dark appearance: options page, general section. |
| `04-options-providers-dark.png` | Dark appearance: engine list with the 「第三方服务」 badge. |
| `05-options-export-confirm-dark.png` | Dark appearance: the export-with-keys confirmation. |
