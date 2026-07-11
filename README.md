# Cockpit plugin store

The default plugin store for [AI-Cockpit](https://github.com/raymondkrahwinkel/AI-Cockpit) — the multi-Claude cockpit.

Point the cockpit's plugin store at this repository (Options → Plugins → Plugin stores → add
`https://github.com/raymondkrahwinkel/AI-Cockpit-Plugins`) to browse, install and update the plugins
below. The cockpit resolves the raw `index.json`, verifies each download against its `sha256`, and
runs the existing install + consent flow.

## Plugins

### Issue trackers

| Plugin | Latest | What it does |
|--------|--------|--------------|
| **GitHub Issues** 🐛 | 1.1.0 | Open issues across your repos (gh CLI) or one repo, in a searchable/sortable dialog with a repository filter; click one to inject a review prompt. |
| **GitHub Pull Requests** 🔀 | 1.0.0 | Inline "Open PRs" section (up to 5) + a dialog of every open PR (gh CLI or single-repo HTTP); click to inject a review prompt. |
| **YouTrack** 🎫 | 1.2.0 | Per-instance/project open issues over the YouTrack REST API (instance URL + permanent token), with instance/project/state filters; click to inject a prompt. Also registers each instance's JetBrains remote MCP server so sessions get YouTrack tools directly. |

### AI providers

| Plugin | Latest | What it does |
|--------|--------|--------------|
| **Gemini / OpenAI Provider** ✨ | 0.1.0 | Adds Gemini and OpenAI as selectable session providers over an OpenAI-compatible chat-completions endpoint (Microsoft.Extensions.AI). Experimental. |
| **CLI Agent Provider (Codex)** 🤖 | 0.1.0 | Adds Codex CLI as a selectable session provider, driven as a subprocess per turn (`codex exec --json`, resumed for follow-up turns). Requires the `codex` CLI installed and authenticated. Experimental. |
| **GitHub Models** 🐙 | 0.1.0 | Adds GitHub Models as a selectable session provider over its OpenAI-compatible endpoint (`models.github.ai/inference`), authenticated with a GitHub PAT (`models:read`). |

## Layout

- `index.json` — the store catalog: top-level `name` + `plugins[]`, each with
  `id`/`name`/`description`/`author`/`category`/`icon`/`homepage`/`repository`/`featured`/`published`/
  `latestVersion`/`versions[]` (per version: `version`/`path`/`abstractionsVersion`/`minHostVersion`/
  `sha256`/`notes`). Full field-by-field reference:
  [Plugin SDK guide → publishing a plugin store](https://github.com/raymondkrahwinkel/AI-Cockpit/blob/main/docs/plugins/PLUGIN-SDK.md#publishing-a-plugin-store).
- `<plugin-id>/<plugin-id>-<version>.zip` — the installable plugin packages (each: `plugin.json` + the
  plugin DLL + its `.deps.json`). Every version entry in `index.json` carries the zip's `sha256` for
  integrity checking on download.

## Adding a new version

1. Build the plugin and package its `bin/Release/net10.0` output (`plugin.json`, `<plugin>.dll`,
   `<plugin>.deps.json`) into `<plugin-id>/<plugin-id>-<version>.zip`.
2. Compute the zip's SHA-256 (`(Get-FileHash plugin.zip -Algorithm SHA256).Hash.ToLower()` or
   `sha256sum plugin.zip`) and add a new entry to that plugin's `versions` array (newest first), bumping
   `latestVersion`.
3. Commit and push — the cockpit picks up the new version on its next store refresh.

## Authoring your own plugin or store

Plugin source for everything listed here lives in the app repository under
[`plugins-dev/`](https://github.com/raymondkrahwinkel/AI-Cockpit/tree/main/plugins-dev). To build your own
plugin, or to set up your own store instead of (or alongside) this one, see the
**[Plugin SDK guide](https://github.com/raymondkrahwinkel/AI-Cockpit/blob/main/docs/plugins/PLUGIN-SDK.md)**
and the **[API reference](https://github.com/raymondkrahwinkel/AI-Cockpit/blob/main/docs/plugins/API-REFERENCE.md)**.
