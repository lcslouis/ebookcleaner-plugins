# ebookcleaner-plugins

Community parser plugins for [EbookCleaner](https://github.com/lcslouis/ebookcleaner).

The `PluginManager` class (`src/plugin_manager.py`) in the main app fetches `registry.json` from this repository and loads each listed plugin.

---

## registry.json format

```json
{
  "version": "1",
  "updated_at": "YYYY-MM-DD",
  "plugins": [
    {
      "id": "my_site",
      "name": "My Site",
      "version": "1.0.0",
      "author": "username",
      "description": "One-line description of the site",
      "domains": ["mysite.com"],
      "file": "plugins/my_site.json"
    }
  ]
}
```

### Registry fields

| Field | Description |
|---|---|
| `version` | Registry schema version (currently `"1"`) |
| `updated_at` | Date the registry was last updated (`YYYY-MM-DD`) |
| `plugins` | Array of plugin descriptor objects |

### Plugin descriptor fields

| Field | Required | Description |
|---|---|---|
| `id` | yes | Unique snake_case identifier for the plugin |
| `name` | yes | Human-readable site name shown in the UI |
| `version` | yes | Plugin version string (semver recommended) |
| `author` | yes | GitHub username or display name of the contributor |
| `description` | yes | One-line description of the site |
| `domains` | yes | List of domain substrings matched against the URL |
| `file` | yes | Relative path to the plugin's selector config JSON (e.g. `plugins/my_site.json`) |

---

## Plugin config file format

Each `file` path points to a JSON file in the `plugins/` directory with this structure:

```json
{
  "toc_selectors": ["ul.chapter-list a"],
  "content_selectors": ["div.chapter-content"],
  "title_selectors": ["h1"],
  "author_selectors": ["a.author"],
  "description_selectors": ["div.synopsis"],
  "cover_selectors": ["img.cover"]
}
```

### Selector fields

| Field | Description |
|---|---|
| `toc_selectors` | CSS selectors (tried in order) to find `<a>` chapter links on the table-of-contents page |
| `content_selectors` | CSS selectors to find the chapter body element |
| `title_selectors` | CSS selectors for the book title |
| `author_selectors` | CSS selectors for the author name |
| `description_selectors` | CSS selectors for the synopsis / description |
| `cover_selectors` | CSS selectors for the cover image (`src`, `data-src`, or `content` on `<meta>` tags) |

Selectors are tried **in order**; the first one returning a non-empty result wins.

---

## Contributing a plugin

1. Fork this repository.
2. Add a new file at `plugins/<id>.json` with the selector config.
3. Add an entry to the `plugins` array in `registry.json` and update `updated_at`.
4. Open a pull request with a brief description and a sample URL to test against.

---

## Directory layout

```
ebookcleaner-plugins/
├── registry.json        # master plugin list fetched by PluginManager
└── plugins/             # per-plugin selector config files
```
