# Wiki Skill

This is my adaptation of droid wiki / Devin's deep wiki / OpenWiki from LangChain. Each lacked something I need:

- Droid / Devin are closed source
- OpenWiki is not a skill and forces a CLI

## Guiding principles

1. Invoke as a skill (a CLI can be bundled later) for a vendor-neutral transport
2. Build on [OKF (Open Knowledge Format)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) markdown concepts with YAML frontmatter, listing `index.md` files, and `log.md`, kept in-repo

## Triggers

| Trigger | Purpose |
| --- | --- |
| **init** | Create `wiki/` from scratch (only when missing) |
| **update** | Surgical refresh after code changes |
| **ask** | Answer questions using `wiki/` as primary source |

## Layout

```
wiki/
├── SKILL.md                          # router: init / ask / update
├── README.md
└── references/
    ├── init-and-update.md            # init + surgical update (no wipe)
    ├── ask.md                        # answer from wiki
    ├── agents-pointer.md             # AGENTS.md / CLAUDE.md pointer
    ├── structure-and-format.md       # index → toc-and-layout, page-writing, wiki-meta
    ├── toc-and-layout.md             # TOC, frontmatter/types, listings, layout, tiers
    ├── page-writing.md               # concept page templates and writing quality
    ├── wiki-meta.md                  # .wiki-meta.json (commitHash baseline)
    └── install.md                    # CI auto-refresh (GitHub Actions / GitLab)
```
