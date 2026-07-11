# Cockpit plugin store

The default plugin store for [AI-Cockpit](https://github.com/raymondkrahwinkel/AI-Cockpit) — the multi-Claude cockpit.

Point the cockpit's plugin store at this repository (Options → Plugins → Plugin stores → add
`https://github.com/raymondkrahwinkel/cockpit-plugins`) to browse, install and update the plugins
below. The cockpit resolves the raw `index.json`, verifies each download against its `sha256`, and
runs the existing install + consent flow.

## Plugins

| Plugin | Latest | What it does |
|--------|--------|--------------|
| **GitHub Issues** | 1.1.0 | Open issues across your repos (gh CLI) or one repo; click to inject a review prompt. |
| **GitHub Pull Requests** | 1.0.0 | Inline "Open PRs" section (up to 5) + a dialog of every open PR (gh CLI or single-repo HTTP); click to inject. |
| **YouTrack** | 1.0.0 | Per-project open issues over the YouTrack REST API (instance URL + permanent token); click to inject. |

## Layout

- `index.json` — the store catalog (name + per-plugin id/name/description/author/latestVersion + versions).
- `<plugin-id>/<plugin-id>-<version>.zip` — the installable plugin packages (each: `plugin.json` + the
  plugin DLL + its `.deps.json`). Every version entry in `index.json` carries the zip's `sha256` for
  integrity checking on download.

## Adding a new version

1. Build the plugin and package its `bin/Release/net10.0` output (`plugin.json`, `<plugin>.dll`,
   `<plugin>.deps.json`) into `<plugin-id>/<plugin-id>-<version>.zip`.
2. Compute the zip's SHA-256 and add a new entry to that plugin's `versions` array (newest first),
   bumping `latestVersion`.
3. Commit and push — the cockpit picks up the new version on its next store refresh.

Plugin source lives in the app repository under `plugins-dev/`.
