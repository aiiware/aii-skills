# Aii Skills

Official skill repository for [Aii CLI](https://www.npmjs.com/package/@aiiware/aii) — reusable agent prompts that extend your AI assistant with specialized capabilities.

## Available Skills

| Skill | Description |
|-------|-------------|
| [code-review](./code-review/) | Review code for bugs, security, and best practices |
| [refactor](./refactor/) | Refactor code with clear goals and safe transformations |
| [explain](./explain/) | Explain code, architecture, or concepts in depth |
| [test-gen](./test-gen/) | Generate unit tests for functions and modules |
| [doc-gen](./doc-gen/) | Generate documentation for code and APIs |

## Installation

Install skills using the Aii CLI (`v0.6.0+`):

```bash
# Shorthand format
/skills install aiiware/skills/code-review
/skills install aiiware/skills/refactor --user

# Absolute GitHub URL
/skills install https://github.com/aiiware/skills/tree/main/code-review
/skills install https://github.com/aiiware/skills/tree/main/refactor --user

# List installed skills
/skills

# Uninstall
/skills uninstall code-review
```

**Project-level** (default): Installed to `.aii/skills/` in your project directory.

**User-level** (`--user`): Installed to `~/.aii/skills/` for global access across all projects.

## Skill Structure

Each skill is a directory containing a `SKILL.md` file:

```
skill-name/
└── SKILL.md
```

The `SKILL.md` file uses YAML frontmatter for metadata and Markdown for instructions:

```markdown
---
name: skill-name
description: Short description of what the skill does
allowed-tools: Read Grep Glob Bash
---

# Skill Name

Instructions for the agent...
Use $ARGUMENTS for user-provided input.
```

## Creating Your Own Skills

1. Create a directory: `.aii/skills/my-skill/`
2. Add a `SKILL.md` with frontmatter and instructions
3. Use `$ARGUMENTS` to accept user input
4. Specify `allowed-tools` to control which tools the skill can use

See the [Aii CLI documentation](https://www.npmjs.com/package/@aiiware/aii#creating-custom-skills) for details.

## License

MIT License - Copyright 2025-present AiiWare.com
