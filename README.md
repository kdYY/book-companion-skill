# book-companion-skill

**Same book, different roles see different value.**

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) Skill that generates role-specific reading guides for enterprise book clubs. Feed it a book's table of contents, pick your industry and role, and get tailored reading paths, deep-dive guides, Socratic discussions, and book club operations — all grounded in the original text.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)
Made with ❤️

---

## Quick Demo

```
$ claude
> /bc init
  Book: Genius Makers
  Industry: 互联网
  Roles: tech, product-ops, marketing

  ✅ Project created at ~/.claude/books/genius-makers/
     _config.json  _glossary.md  _progress.json
     chapters/  guides/  discussions/  club/

> /bc guide
  Role: tech (Senior)
  → guides/tech-path.md          # Role-specific reading path

> /bc explore
  Chapter: 1  Role: tech  Level: senior
  → guides/1-tech-senior.md      # 9-section deep-dive guide

> /bc discuss
  Chapter: 1  Depth: deep
  🗣️ Socratic discussion starts (interactive, not one-shot)

> /bc club
  Size: 12 people  Duration: 6 weeks
  → club/plan.md  club/scoring.md  club/week-1.md ~ week-6.md
```

## Key Features

- **Role-specific, not generic** — Every guide, question, and action item is tailored to a specific job role. Swap the role and the entire output changes.
- **Grounded in original text** — "Anchor Triangle" technique (WHO + VERBATIM QUOTE + SCENE) ensures every insight traces back to the book.
- **Methodology hidden, logic embedded** — SQ3R, Feynman, Bloom's taxonomy are baked into the flow but never named. The reader feels guided, not lectured.
- **Self-checking quality gate** — Every generated output is scored on 5 dimensions (1-5 scale) and iteratively revised until all dimensions pass ≥ 4.
- **End-to-end book club engine** — From reading plans and scoring systems to weekly discussion topics and cross-role collision sessions.

## Quick Start

### Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed and configured

### Install

```bash
# Copy the skill to your Claude Code skills directory
cp -r book-companion-skill ~/.claude/skills/

# Or clone directly
git clone https://github.com/YOUR_USERNAME/book-companion-skill.git ~/.claude/skills/book-companion-skill
```

### Use

Launch Claude Code and type any `/bc` command:

```
claude
> /bc init          # Create a new book project
> /bc guide         # Generate role-specific reading path
> /bc explore       # Deep-dive chapter guide (core command)
> /bc discuss       # Socratic discussion
> /bc club          # Book club operations
```

## Command Reference

| Command | Purpose | Key References |
|---------|---------|---------------|
| `/bc init` | Create a new book project with directory skeleton | `role-profiles.md` |
| `/bc guide` | Generate role-specific reading path (must-read vs. optional chapters) | `role-profiles.md`, `methodology.md` |
| `/bc explore` | Generate 9-section deep-dive chapter guide (core command) | `v4-template.md`, `patterns.md`, `quality-gate.md` |
| `/bc discuss` | Interactive Socratic discussion with 6 questioning types | `quality-gate.md` |
| `/bc club` | Generate 6-week book club plan, scoring, and weekly topics | `club-engine.md` |

## Design Principles (The Three Iron Rules)

1. **Methodology stays invisible** — No SQ3R / Feynman / Bloom's names in any output. The logic is embedded in the flow.
2. **Criticism must carry equal evidence** — Every guiding question must cite an original-text anchor (person + quote + scene). No anchor, no output.
3. **Every action must be concrete** — Each action item contains: a specific task + a deliverable + a time frame.

## Quality Gate (5-Point Checklist)

Every `guide` and `explore` output must pass all five checks before delivery:

| # | Dimension | What It Checks |
|---|-----------|---------------|
| 1 | **Anchor Density** | Every core claim has a verbatim quote from the original text |
| 2 | **Role Specificity** | The guide becomes invalid if you swap the role |
| 3 | **Action Executability** | Action items have concrete task + deliverable + time frame |
| 4 | **Methodology Invisibility** | No methodology names appear anywhere |
| 5 | **Physical Book Nudge** | At least one natural nudge to go back to the printed book |

The generation loop runs two mandatory rounds: draft → self-score → locate weak sections → targeted revision → re-score. All five dimensions must reach ≥ 4/5.

## Project Structure

```
book-companion-skill/
├── SKILL.md                  # Skill definition (commands, flows, routing)
├── README.md                 # This file
└── references/               # Engine files (read-only at runtime)
    ├── role-profiles.md      # Industry × role profiles (7 base + industry extensions)
    ├── methodology.md        # Role × level → reading method mapping
    ├── v4-template.md        # 9-section guide document template
    ├── patterns.md           # Content patterns (15 good + 9 anti-patterns)
    ├── quality-gate.md       # 5-point quality checklist + scoring rubric
    ├── club-engine.md        # Book club operations framework
    ├── wiki-hierarchy.md     # Feishu Wiki hierarchy template (optional publish)
    └── examples.md           # Few-shot anchor examples
```

Generated book project structure:

```
~/.claude/books/{slug}/
├── _config.json              # Book metadata, chapters, roles
├── _glossary.md              # Glossary (auto-accumulated)
├── _progress.json            # Reading progress tracker
├── chapters/{id}.md          # Chapter source text / summaries
├── guides/
│   ├── {role}-path.md        # Role reading path
│   └── {chapter}-{role}-{level}.md  # Chapter deep-dive guides
├── discussions/
│   └── {chapter}-{ts}.md     # Discussion records
└── club/
    ├── plan.md               # Book club plan
    ├── scoring.md            # Scoring rules
    └── week-{N}.md           # Weekly discussion topics
```

## Configuration

### Industry and Role Customization

Edit `references/role-profiles.md` to add your industry and roles:

```markdown
| Industry   | slug          | Roles                                              |
|------------|---------------|----------------------------------------------------|
| 通用       | general       | tech, product-ops, cs, finance, marketing, biz, management |
| 互联网     | internet      | tech, product-ops, cs, finance, ..., sre           |
| 金融       | finance       | tech, ..., risk-control, compliance, investment-research |
| 制造       | manufacturing | tech, ..., supply-chain, quality-mgmt              |
| 医疗       | healthcare    | tech, ..., doctor, nursing-mgmt, pharma-rd         |
```

Each role has a profile with: reading goals, core concerns, thinking patterns, and typical questions. The guide engine uses these profiles to generate role-specific content.

### Methodology Mapping

`references/methodology.md` maps roles × experience levels to reading methods. New hires get structural overviews; seniors get paradox-driven analysis. The mapping is invisible to the end user.

## Technical Highlights

Standing on the shoulders of giants:

### 1. Verbatim Anchor Triangle

Every claim is grounded with three elements: **WHO** (person name) + **WHERE** (scene) + **VERBATIM QUOTE** (exact words). This triangulation anchors abstract ideas to concrete text, making each guide impossible to apply to a different book.

### 2. CoT Quote-then-Interpretation Pairing

Following Chain-of-Thought prompting research (AAAI 2024), every interpretation is preceded by its source quote. The reader sees evidence first, then the analysis — never the reverse. This is enforced structurally in the generation template.

### 3. Few-Shot Anchor Examples

`references/examples.md` provides demonstrated anchor patterns for the model to emulate. Each example shows a verbatim quote followed by a role-specific interpretation, establishing the expected output format.

### 4. Self-Check Counter + Iterative Loop

The quality gate doesn't just check — it **scores** (1-5 scale), **locates weak sections**, and forces a **targeted revision round**. The generation loop runs a minimum of two passes. Sections scoring < 4/5 are identified by paragraph and revised in isolation.

### 5. Genre-Aware Generation

The system adapts generation parameters based on book genre (narrative non-fiction vs. textbook vs. biography). Narrative books get more scene-based anchors; textbooks get more concept mapping.

## Acknowledgments

### Research and Methods

- **Mortimer Adler** — *How to Read a Book* (four levels of reading)
- **Bloom's Taxonomy** — Six levels of cognitive depth for discussion design
- **SQ3R Method** — Survey-Question-Read-Recite-Review reading framework
- **Feynman Technique** — Explain-it-simple understanding verification
- **Socratic Questioning** — Six questioning types for discussion engine
- **Chain-of-Thought Prompting** — Wei et al., AAAI 2024

### Tools and Platforms

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — AI coding agent that hosts the skill
- [Feishu / Lark](https://www.feishu.cn/) — Optional knowledge base publishing target

## License

[MIT](LICENSE)
