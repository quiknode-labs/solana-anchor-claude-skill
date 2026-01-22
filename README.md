# Solana Anchor Claude Skill

A Claude Code skill for creating and editing Solana Anchor projects with best practices and conventions.

> [!NOTE]
> Looking for a Claude RULES file (`CLAUDE.md`) for Solana? You're in the right place. We've updated this repo to use the newer Claude Code skills format, which provides better integration and automatic application of guidelines.

## What are Claude Skills?

Claude Code supports "skills" - reusable instruction sets that Claude automatically applies when working on your code. Skills are stored in `.claude/skills/` directories and can be invoked manually or triggered automatically based on context.

This repo includes a Claude skill that teaches Claude the specific patterns and best practices for Solana/Anchor development, including proper variable naming, avoiding magic numbers, using modern Anchor 0.32.1 syntax, and following Solana-specific conventions.

## How do I use this skill?

### Claude Code

**Option 1: Per-user skill** (if you work on multiple Solana projects with the same standards)

Copy the skill to your home directory:

```bash
cp -r solana-anchor-claude-skill ~/.claude/skills/
```

This installs the skill globally for all your projects. The skill won't be committed to git and is personal to your machine.

**Option 2: Per-project skill** (recommended for sharing with your team)

Copy the skill into your project directory:

```bash
cp -r solana-anchor-claude-skill /path/to/your/project/.claude/skills/
```

This installs the skill only for that specific project. The skill can be committed to git and shared with your team.

**Using the skill:**

Once installed, the skill will automatically apply the Solana/Anchor guidelines when Claude Code is working on your code. You can also manually invoke it with `/solana-anchor-claude-skill`.
