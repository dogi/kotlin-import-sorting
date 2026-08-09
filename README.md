# dogi — Claude Code plugin marketplace

A personal marketplace hosting the `kotlin-importing` (a Kotlin import
sorter / unused-import remover). Maintain the skill here once; opt any project
into it — including **Claude Code on the web / cloud** sessions.

## Structure

```
.claude-plugin/marketplace.json          # marketplace catalog
plugins/kotlin-importing/
├── .claude-plugin/plugin.json           # plugin manifest
└── skills/importing/
    ├── SKILL.md                         # skill definition
    └── kotlin-importing.py                  # the tool
```

## Hosting

This marketplace is hosted at `dogi/kotlin-importing`. The
`.claude-plugin/marketplace.json` catalog lives at the repo root so Claude Code
can discover it when the repo is added as a marketplace.

## Use it in the terminal (CLI)

```
/plugin marketplace add dogi/kotlin-importing
/plugin install kotlin-importing@dogi
/reload-plugins
```

Then invoke: `/kotlin-importing:sort` (or just ask to "sort the
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
    "dogi": {
      "source": {
        "source": "github",
        "repo": "dogi/kotlin-importing"
      }
    }
  },
  "enabledPlugins": {
    "kotlin-importing@dogi": true
  }
}
```

Commit that to each repo where you want the skill available in web sessions.
The skill itself stays maintained here — bump `version` in `plugin.json` on each
release so installs pick up updates.
