# Skills

A collection of reusable [Claude Code](https://claude.ai/code) skills.

## Available Skills

| Skill | Description | Usage |
|-------|-------------|-------|
| [brainstorm](./brainstorm/) | Multi-agent brainstorm using De Bono's Six Thinking Hats | `/brainstorm <topic>` |

## What Are Skills?

Skills are reusable prompt templates for Claude Code that define specialized workflows. When you type `/brainstorm <topic>`, Claude Code loads the skill definition and follows the defined process — in this case, spawning six parallel agents for structured brainstorming.

## Installation

Copy a skill directory into your Claude Code skills folder:

```bash
# Clone this repo
git clone https://github.com/thuwyh/skills.git

# Copy the skill(s) you want
cp -r skills/brainstorm/ ~/.claude/skills/brainstorm/
```

Then invoke it in Claude Code with `/brainstorm <topic>`.

## Language Support

All skills are language-adaptive — they automatically detect the language of your input and respond accordingly. Works with English, Chinese, and other languages.

## Contributing

To add a new skill, create a directory with a `skill.md` file following the [Claude Code skill format](https://docs.anthropic.com/en/docs/claude-code). PRs welcome!
