# stefan-tools — Claude Code plugin marketplace

A personal marketplace hosting the `kotlin-imports-plugin` (a Kotlin import
sorter / unused-import remover). Maintain the skill here once; opt any project
into it — including **Claude Code on the web / cloud** sessions.

## Structure

```
.claude-plugin/marketplace.json          # marketplace catalog
plugins/kotlin-imports-plugin/
├── .claude-plugin/plugin.json           # plugin manifest
└── skills/kotlin-imports/
    ├── SKILL.md                         # skill definition
    └── sort_imports.py                  # the tool
```

## One-time: host it

Push this directory to a GitHub repo you own, e.g. `stefan/claude-plugins`:

```bash
git init && git add -A && git commit -m "kotlin-imports marketplace"
git branch -M main
git remote add origin git@github.com:stefan/claude-plugins.git
git push -u origin main
```

## Use it in the terminal (CLI)

```
/plugin marketplace add stefan/claude-plugins
/plugin install kotlin-imports-plugin@stefan-tools
/reload-plugins
```

Then invoke: `/kotlin-imports-plugin:kotlin-imports` (or just ask to "sort the
Kotlin imports" — the description auto-triggers it).

## Use it on Claude Code web / cloud

Cloud sessions can't see your local `~/.claude`, and user-scoped `enabledPlugins`
does **not** carry over. Declare the marketplace + plugin in the target repo's
`.claude/settings.json` (this file is part of the clone, so the cloud VM installs
the plugin at session start — needs network access to GitHub, which the default
allowlist covers):

```json
{
  "extraKnownMarketplaces": {
    "stefan-tools": {
      "source": {
        "source": "github",
        "repo": "stefan/claude-plugins"
      }
    }
  },
  "enabledPlugins": {
    "kotlin-imports-plugin@stefan-tools": true
  }
}
```

Commit that to each repo where you want the skill available in web sessions.
The skill itself stays maintained here — bump `version` in `plugin.json` on each
release so installs pick up updates.
