# REAA Standard Library — Skills for AI coding agents

Agent-consumable skills for the **REAA Standard Library** (SL) — the `@reaatech` collection of [MIT-licensed TypeScript packages for production AI](https://reaatech.com/products/packages).

Each `<skill-name>/SKILL.md` follows Anthropic's progressive-disclosure pattern:

- The **frontmatter `description`** always loads. Agents use it to decide whether the skill is relevant to the current task.
- The **body** loads on demand when the agent reaches for the skill. It lists the packages, install command, when to use them, and where to file issues.

42 skills shipping today, one per `@reaatech` repo with published packages. The catalog continues to grow; this repo regenerates on every catalog change.

## Quick start

The full per-agent installation guide will land at [reaatech.com/install](https://reaatech.com/install) (planned). For now:

### Claude Code (skill files)

```bash
# Clone alongside your project
git clone https://github.com/reaatech/sl-skills.git ~/.claude/skills/reaatech-sl
```

Claude Code auto-discovers SKILL.md files in `~/.claude/skills/`. Restart Claude Code to pick them up.

### MCP server (also in flight)

For agents that prefer live catalog access over local skill files, the REAA SL MCP server at `mcp.reaatech.com/sl` will expose the same catalog with `search_packages`, `get_package`, `list_categories`, and `report_issue` tools. See the brief in the [website repo](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) for status.

## File layout

```
.
├── README.md                                  # this file
├── LICENSE                                    # MIT
├── index.json                                 # machine-readable catalog of all skills
└── reaatech-<repo-slug>/
    └── SKILL.md                               # the skill itself
```

`index.json` lists every skill with its frontmatter description, repo, category, and package count — handy for skill-aggregator MCP servers that want the full catalog in one fetch.

## Source of truth

These files are **generated**, not hand-edited. The source of truth is the live catalog at [reaatech.com](https://reaatech.com), which in turn syncs from package READMEs and `package.json` keywords in the [github.com/reaatech](https://github.com/reaatech) org.

The generator lives in the [website repo](https://github.com/reaatech/website/tree/main/scripts/sl-skills-generate). Regeneration cadence:

- **Nightly** via GitHub Actions (planned).
- **On catalog change** via `repository_dispatch` webhook from the catalog-sync pipeline.

If you edit a SKILL.md by hand, the next regen will overwrite it. Edits belong upstream — in the package's README or the catalog's editorial fields.

## What's "skeleton" today vs. coming next

The opening paragraph, packages table, and metadata land directly from the catalog. Four editorial sections currently emit `_pending_` placeholders:

- "When to use this" (beyond the category hint)
- "Quick start" (the install line is there; the code example is pending)
- "Don't reach for this when"

These will be filled by **Phase 3.5** — DeepSeek-v4-Flash drafts the editorial copy from package READMEs, then drafts land in an admin review queue before publication. See [`SL_DISTRIBUTION.md`](https://github.com/reaatech/website/blob/main/docs/SL_DISTRIBUTION.md) Phase 3.5 for the spec.

## License

MIT. Every package linked from a SKILL.md is also MIT.

## Issues + feedback

- **Issues with a specific `@reaatech` package**: file in that package's repo (linked from each SKILL.md). The MCP server's `report_issue` tool will eventually file these on agents' behalf.
- **Issues with the skill files themselves** (wrong package list, broken link, generator bug): file in this repo.
- **Suggestions for new categories or refactors**: open a discussion in the [website repo](https://github.com/reaatech/website/discussions).
