# ebookcleaner-plugins

Community parser plugins for [EbookCleaner](https://github.com/lcslouis/ebookcleaner).

The `PluginManager` class (`src/plugin_manager.py`) in the main app fetches `registry.json` from this repository and loads each entry as a `SiteConfig`-driven parser, slotting it between the dedicated parsers and the default fallback.

---

## Plugin schema

Each object in the `plugins` array of `registry.json` must follow this structure:

```json
{
  "site_name": "My Site",
  "domains": ["mysite.com"],
  "toc_selectors": ["ul.chapter-list a"],
  "content_selectors": ["div.chapter-content"],
  "title_selectors": ["h1"],
  "author_selectors": ["a.author"],
  "description_selectors": ["div.synopsis"],
  "cover_selectors": ["img.cover"],
  "single_chapter_url": false
}
```

### Field reference

| Field | Type | Required | Description |
|---|---|---|---|
| `site_name` | string | yes | Human-readable name shown in the UI |
| `domains` | string[] | yes | Substring(s) matched against the URL — e.g. `"mysite.com"` matches `https://mysite.com/novel/123` |
| `toc_selectors` | string[] | yes | CSS selectors tried in order to find `<a>` chapter links on the table-of-contents page |
| `content_selectors` | string[] | yes | CSS selectors tried in order to find the chapter body element |
| `title_selectors` | string[] | no | CSS selectors for the book title (falls back to `<h1>`) |
| `author_selectors` | string[] | no | CSS selectors for the author name |
| `description_selectors` | string[] | no | CSS selectors for the synopsis / description |
| `cover_selectors` | string[] | no | CSS selectors for the cover image (`src`, `data-src`, or `content` on `<meta>` tags) |
| `single_chapter_url` | boolean | no | Set `true` when the URL points directly to a chapter rather than a table of contents (default: `false`) |

### Selector resolution

- Selectors are tried **in order**; the first one that returns a non-empty result wins.
- `<meta>` tags expose their `content` attribute automatically.
- Cover selectors also check `data-src`, `data-lazy-src`, and `data-original` in addition to `src`.
- Chapter links with `href` starting with `#` are ignored.

---

## Contributing a plugin

1. Fork this repository.
2. Add your entry to the `plugins` array in `registry.json`.
3. Test it locally by pointing `PluginManager` at your fork.
4. Open a pull request with a brief description of the site and a sample URL.

---

## Directory layout

```
ebookcleaner-plugins/
├── registry.json   # master plugin list consumed by PluginManager
└── plugins/        # reserved for future per-plugin asset files
```
