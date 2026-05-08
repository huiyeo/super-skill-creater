---
name: super-skill-creator
description: "Super skill creator that combines the best of skill-creator-agent and skill-creator-pro. Create, edit, improve, audit, validate, package, test, and iterate on skills with eval-driven development. Always use this skill when the user mentions: create a skill, optimize a skill, write a skill for something, improve a skill, build a skill, skill creation, write a SKILL.md, refactor skill, audit skill, validate skill, test skill, or any mention of creating, modifying, improving, auditing, packaging, or iterating on skills. Do not attempt to create skills manually - use this skill EVERY SINGLE TIME skill work needs to be done."
---

# Super Skill Creator

> Combines the best of skill-creator-agent + skill-creator-pro for professional skill development.

## Overview

This is the one-stop shop for all skill-related work:
- **Create** new skills from scratch with proper structure
- **Edit/improve** existing skills
- **Audit/review** skills for quality and security
- **Validate** against AgentSkills spec
- **Package** into distributable .skill files
- **Test/evaluate** with eval-driven iteration
- **Optimize** trigger descriptions for better triggering

## Core Principles

### Concise is Key
The context window is a public good. Skills share the context window with everything else: system prompt, conversation history, other skills' metadata, and the actual user request.

**Default assumption: AI is already very smart.** Only add context AI doesn't already have. Challenge each piece of information: "Does AI really need this explanation?" and "Does this paragraph justify its token cost?"

Prefer concise examples over verbose explanations.

### Progressive Disclosure (3-level loading)
1. **Metadata (name + description)** - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<500 lines ideal)
3. **Bundled resources** - As needed (unlimited - scripts execute without loading)

**Key pattern:** Keep SKILL.md lean. Move detailed info to `references/` and link to it with clear guidance on WHEN to read each file.

### Anatomy of a Skill

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter (name + description required)
│   └── Markdown instructions
└── Bundled Resources (optional)
    ├── scripts/       - Executable code for deterministic/repetitive tasks
    ├── references/    - Documentation loaded as needed
    └── assets/        - Files used in output (templates, icons, etc.)
```

**Important:** Do NOT create README.md, CHANGELOG.md, or other auxiliary files. The skill should only contain information needed for the AI agent to do the job.

## Skill Creation Workflow

### Phase 1: Understand & Plan (ALWAYS START HERE)

**DO NOT start coding before answering these questions:**

1. **What should this skill enable AI to do?** (concrete examples)
2. **When should it trigger?** (what user phrases/contexts)
3. **Expected output format?**
4. **Do we need test cases?** (yes for objective outputs, optional for subjective)
5. **What reusable resources?** (scripts, references, assets)

**Ask the user clarifying questions** if any of these are unclear. Better to ask now than rework later.

### Phase 2: Initialize the Skill

Use the init script to create a proper structure:

```bash
scripts/init_skill.py <skill-name> --path <output-directory> [--resources scripts,references,assets]
```

**Our conventions:**
- Output path: `./基础设定/SKILL/`
- Skill name: lowercase, hyphens only, < 64 chars
- Verb-led names preferred (e.g., `create-ppt`, `fetch-web`)

### Phase 3: Implement the Skill

#### Start with Reusable Resources
1. Create scripts first (test them!)
2. Add reference docs for domain knowledge
3. Add assets/templates for outputs

#### Write SKILL.md

**Frontmatter (CRITICAL - this controls triggering):**
- `name`: Skill identifier (lowercase, hyphens)
- `description`: PUSHY style! Include BOTH what it does AND ALL trigger contexts. Example:
  ```yaml
  description: "Create and modify PowerPoint presentations with templates, formatting, and export options. USE THIS whenever the user says: 'make a PPT', 'create a presentation', 'modify slides', 'update this deck', 'build slides', 'export PPT', or ANY mention of presentations, slides, decks, PowerPoint, etc."
  ```

**Body structure:**
1. Overview/purpose
2. Core principles (if any)
3. Step-by-step workflow
4. Script usage guide (when/how to use each script)
5. Reference file links (when to read each)
6. Output format standards

### Phase 4: Validate & Package

**Validate the skill structure:**
```bash
scripts/quick_validate.py <path/to/skill>
```

**Package for distribution:**
```bash
scripts/package_skill.py <path/to/skill-folder> [output-dir]
```

Creates a `.skill` file (zip format) that can be shared and installed.

### Phase 5: Eval-Driven Iteration (for high-quality skills)

For skills that need to be reliable and high-performance:

1. **Create test prompts** (5-10 realistic user queries)
2. **Run evaluation loop:**
   ```bash
   scripts/run_loop.py --skill <skill-path> --prompts <prompts-file>
   ```
3. **Review results** with eval-viewer
4. **Iterate** on SKILL.md based on failures
5. **Optimize trigger description:**
   ```bash
   scripts/improve_description.py --skill <skill-path> --triggers <trigger-phrases>
   ```

## Script Reference

| Script | Purpose |
|--------|---------|
| `init_skill.py` | Create new skill with proper structure |
| `quick_validate.py` | Validate skill against AgentSkills spec |
| `package_skill.py` | Package skill into .skill file |
| `improve_description.py` | Optimize trigger description for better triggering |
| `run_eval.py` | Run single evaluation pass |
| `run_loop.py` | Run full eval iteration loop |
| `aggregate_benchmark.py` | Aggregate benchmark results across iterations |
| `generate_report.py` | Generate evaluation report HTML |

## When to Use What

| Task | Tool/Approach |
|------|--------------|
| Simple skill creation | init_skill + write SKILL.md + validate + package |
| Editing existing skill | Review first → make changes → validate → test |
| Skill audit/review | Validate structure → review quality → suggest improvements |
| High-stakes skill | Full eval workflow: create test prompts → run_loop → iterate |
| Poor triggering | Use improve_description.py to optimize the description |

## Key Output Files

All skills go to: `./基础设定/SKILL/<skill-name>/`

After completion, show the user:
1. Skill name and location
2. What it does (1 sentence)
3. Trigger phrases
4. Key scripts/references included
5. Optional: Suggest how to test it

## References

For detailed patterns, see:
- `references/schemas.md` - Skill structure schemas and validation rules
