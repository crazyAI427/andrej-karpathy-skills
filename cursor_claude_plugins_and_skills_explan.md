# Claude plugins and skills explanation
_Exported on 6/14/2026 at 21:16:04 PDT from Cursor (3.7.36)_

---

**User**

 I am going to use claude is it all need @.claude-plugin @skills @CLAUDE.md 

what they do?

---

**Cursor**

I'll explore the repo structure and read how `.claude-plugin`, `skills`, and `CLAUDE.md` are documented so I can explain what each does and what you actually need.


You do **not** need all three. They deliver the **same four guidelines** in different ways. Pick one install path.

## Short answer

| Piece | Do you need it? | What it does |
|-------|-----------------|--------------|
| **`CLAUDE.md`** | Optional (per-project) | Project instructions Claude Code reads in that repo |
| **`skills/`** | Only if using the plugin | Reusable skill content the plugin installs |
| **`.claude-plugin/`** | Only if publishing/installing as a plugin | Packaging so Claude Code can install from a marketplace |

Same content, different delivery mechanism.

---

## What each one does

### `CLAUDE.md` — per-project instructions

A file at the project root that **Claude Code** loads when you work in that folder.

- Contains the four principles: Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution
- **Scope:** one project at a time
- **Setup:** copy or append into each repo

Best if you want guidelines in specific projects without a global plugin.

---

### `skills/` — the skill definition

Contains `skills/karpathy-guidelines/SKILL.md` — the guidelines formatted as a **Claude Code skill**.

- Frontmatter (`name`, `description`) tells Claude when to use it
- Body is the same rules as `CLAUDE.md`
- Normally you **don’t copy this yourself**; the plugin installs it

Best as the **content** behind the plugin, not something most users manage manually.

---

### `.claude-plugin/` — plugin packaging

Metadata for distributing via Claude Code’s plugin system:

- **`plugin.json`** — plugin name, version, and pointer to `./skills/karpathy-guidelines`
- **`marketplace.json`** — marketplace listing (how others find/install it)

You need this only if you’re **installing from the marketplace** or **publishing** your own plugin. End users run:

```
/plugin marketplace add forrestchang/andrej-karpathy-skills
/plugin install andrej-karpathy-skills@karpathy-skills
```

That installs the skill **globally** across projects.

---

## What to use with Claude Code

**Option A — Plugin (recommended in README)**  
→ Uses `.claude-plugin/` + `skills/`  
→ Guidelines available everywhere  
→ You do **not** also need `CLAUDE.md` unless you want extra per-project rules

**Option B — `CLAUDE.md` only**  
→ Copy into each project  
→ No plugin, no skills folder needed  
→ Good for one repo or when you can’t use plugins

**Don’t do both** unless you want the global skill **plus** project-specific additions in `CLAUDE.md` (merge extra sections under the same guidelines).

---

## If you’re using Cursor instead

From `CURSOR.md`:

- Cursor uses **`.cursor/rules/karpathy-guidelines.mdc`**, not `.claude-plugin/` or `CLAUDE.md` by default
- You can optionally copy `skills/karpathy-guidelines/SKILL.md` into `~/.cursor/skills` for Cursor Agent Skills

---

## Mental model

```mermaid
flowchart LR
  subgraph content [Same guidelines]
    CLAUDE[CLAUDE.md]
    SKILL[skills/.../SKILL.md]
  end

  subgraph delivery [How they reach the AI]
    CLAUDE -->|per-project| CC1[Claude Code in that repo]
    SKILL -->|via plugin| CC2[Claude Code globally]
    MDC[.cursor/rules/...] -->|Cursor| Cursor[Cursor IDE]
  end

  PLUGIN[.claude-plugin/] -->|packages| SKILL
```

**Bottom line:** For Claude Code, choose **plugin** (global) **or** **`CLAUDE.md`** (per project). `.claude-plugin/` and `skills/` are for the plugin path; you don’t manually wire all three unless you’re maintaining or extending this repo.

If you say whether you mean **Claude Code CLI** or **Claude in the browser**, I can narrow this to exact setup steps for your case.

---

**User**

@CLAUDE.md 

I think it is global config, not not specific project?

---

**Cursor**

Checking how this repo documents `CLAUDE.md` and whether Claude Code treats it as global or per-project.


You're right to question that — **`CLAUDE.md` can be global or project-specific**. It depends on **where** you put it, not the filename itself.

## `CLAUDE.md` has multiple scopes

In **Claude Code**, the same filename works at different levels:

| Scope | Location | Applies to |
|-------|----------|------------|
| **Global (user)** | `~/.claude/CLAUDE.md` | Every project on your machine |
| **Project** | `./CLAUDE.md` or `./.claude/CLAUDE.md` | That repo / folder (often committed for the team) |
| **Local (personal)** | `./CLAUDE.local.md` | Just you, in that project (usually gitignored) |

Claude Code can load **more than one** of these in a session. It walks up from your current directory and combines what it finds. So global + project instructions can both apply.

Official docs: [How Claude remembers your project](https://code.claude.com/docs/en/memory)

---

## What this repo means by `CLAUDE.md`

In **this** repo, the root `CLAUDE.md` is the **content/template** for the Karpathy guidelines.

The README labels **Option B** as **per-project**:

```115:126:c:\Users\Phoenix\Videos\andrej-karpathy-skills-main\README.md
**Option B: CLAUDE.md (per-project)**

New project:
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md
```

Existing project (append):
```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md >> CLAUDE.md
```
```

So here, “use `CLAUDE.md`” means: **copy this file into each project** — not “it’s only ever project-scoped.”

---

## Global vs project — which path to use

**Want guidelines everywhere (global)?** You have two main options:

1. **Plugin (recommended in README)** — installs a skill globally via `/plugin install ...`
2. **Global `CLAUDE.md`** — copy the content to `~/.claude/CLAUDE.md` on your machine

**Want guidelines only in some repos?**

- Put `CLAUDE.md` in each project root (what this repo’s Option B describes)

**Want global defaults + project-specific extras?**

- Put shared rules in `~/.claude/CLAUDE.md`
- Put project-only rules in that repo’s `CLAUDE.md`
- Both can load together

---

## Quick mental model

```text
Global config     →  ~/.claude/CLAUDE.md          (all projects)
Project config    →  ./CLAUDE.md                  (this repo only)
Plugin/skills     →  ~/.claude/... via plugin     (all projects, different mechanism)
```

So: **`CLAUDE.md` is not “only project” or “only global.”** It’s a format that works at both levels. This repo’s README just recommends the **per-project copy** as the simple alternative to the **global plugin**.

If you tell me whether you want these rules on **every** project or **one** project, I can point you to the single cleanest setup.

---

**User**

is it OK, only use one @CLAUDE.md , not others?

---

**Cursor**

Yes — **using only `CLAUDE.md` is fine.**

In this repo, that’s **Option B** in the README: copy `CLAUDE.md` into a project (or put the same content in `~/.claude/CLAUDE.md` if you want it global). You do **not** need:

- `.claude-plugin/`
- `skills/`
- the plugin marketplace install

Those are just another way to deliver the **same guidelines**.

## When `CLAUDE.md` alone is enough

- You’re using **Claude Code**
- You want the four Karpathy rules (think first, keep it simple, surgical edits, goal-driven)
- You’re fine managing instructions in one markdown file

## Tradeoffs vs the plugin

| Only `CLAUDE.md` | Plugin + skills |
|------------------|-----------------|
| Simple — one file | Install once via `/plugin` |
| Per-project unless you use `~/.claude/CLAUDE.md` | Works across projects automatically |
| No marketplace/plugin setup | Slightly more setup upfront |

Functionally, for these guidelines, **one `CLAUDE.md` is enough**. The other files exist for distribution and reuse, not because you need all of them.

**Note:** If you use **Cursor** instead of Claude Code, Cursor won’t read `CLAUDE.md` by default — you’d use `.cursor/rules/` instead. For Claude Code only, `CLAUDE.md` alone is perfectly OK.

