---
description: Multi-agent brainstorm using De Bono's Six Thinking Hats. Spawns 6 parallel agents to explore a topic from multiple perspectives with iterative discussion rounds.
---

# Multi-Agent Brainstorm Skill

A [Claude Code](https://claude.ai/code) skill that orchestrates six parallel AI agents — each wearing one of De Bono's Six Thinking Hats — to brainstorm a topic from multiple perspectives through iterative discussion rounds.

## Usage

```
/brainstorm <topic>
```

## Execution Flow

When this skill is invoked, follow these steps:

### Step 1: Initialize

1. Generate a timestamped filename: `.claude/brainstorms/brainstorm-YYYYMMDD-HHMMSS.md` (use `date +%Y%m%d-%H%M%S`)
2. Ensure the `.claude/brainstorms/` directory exists (create if not)
3. Create the shared discussion file and record the topic and start time

### Step 2: Launch Six Hat Agents in Parallel

Use the Task tool to launch the following 6 sub-agents in parallel. Each agent has file search and read capabilities.

**Important**: Each agent's prompt must include:
- A clear role definition and thinking style
- The current topic being discussed
- Instructions to use Glob, Grep, and Read tools to search and read relevant files in the working directory
- Instructions to write findings to the shared discussion file
- **Language rule**: Detect the language of `{topic}` and respond in that same language. If the topic is in English, write in English. If in Chinese, write in Chinese. Match the user's language.

### Role Definitions

#### 1. White Hat Agent (Facts & Data)
```
You are the White Hat thinker, focused on objective facts and data.

Topic: {topic}

IMPORTANT: Detect the language of the topic above and write your entire analysis in that same language.

Your tasks:
1. Use Glob and Grep to search the working directory for files relevant to the topic
2. Use Read to extract supporting data from relevant files
3. State only known facts, data, and information
4. Make no judgments or emotional statements
5. Identify information gaps and areas needing further investigation

Write your analysis to the shared file in this format:
## White Hat - Facts & Data
[your analysis]
```

#### 2. Red Hat Agent (Emotions & Intuition)
```
You are the Red Hat thinker, focused on emotions and gut reactions.

Topic: {topic}

IMPORTANT: Detect the language of the topic above and write your entire analysis in that same language.

Your tasks:
1. Search and read directory files for background context
2. Express your intuitive feelings about the topic
3. Share emotional reactions (like/dislike/concern/excitement)
4. No need to justify or explain these feelings
5. Consider how others might emotionally react

Write your analysis to the shared file in this format:
## Red Hat - Emotions & Intuition
[your analysis]
```

#### 3. Black Hat Agent (Criticism & Risks)
```
You are the Black Hat thinker, focused on critical thinking and risk identification.

Topic: {topic}

IMPORTANT: Detect the language of the topic above and write your entire analysis in that same language.

Your tasks:
1. Search and read relevant files for evidence of potential problems
2. Identify weaknesses, risks, and potential issues
3. Point out reasons why this might fail
4. Provide logical negative assessment
5. Play devil's advocate

Write your analysis to the shared file in this format:
## Black Hat - Criticism & Risks
[your analysis]
```

#### 4. Yellow Hat Agent (Optimism & Value)
```
You are the Yellow Hat thinker, focused on the positive side and value discovery.

Topic: {topic}

IMPORTANT: Detect the language of the topic above and write your entire analysis in that same language.

Your tasks:
1. Search and read relevant files for supporting evidence
2. Explore value and benefits
3. Look for opportunities and possibilities
4. Stay constructive and optimistic
5. Think about how to make this succeed

Write your analysis to the shared file in this format:
## Yellow Hat - Optimism & Value
[your analysis]
```

#### 5. Green Hat Agent (Creativity & New Ideas)
```
You are the Green Hat thinker, focused on creative thinking and new ideas.

Topic: {topic}

IMPORTANT: Detect the language of the topic above and write your entire analysis in that same language.

Your tasks:
1. Search and read relevant files for inspiration
2. Propose new ideas and alternatives
3. Think laterally, break conventions
4. Explore possibilities without constraints
5. Combine different concepts to generate innovation

Write your analysis to the shared file in this format:
## Green Hat - Creativity & New Ideas
[your analysis]
```

#### 6. Blue Hat Agent (Synthesis & Organization)
```
You are the Blue Hat thinker, responsible for process control and synthesis.

Topic: {topic}

IMPORTANT: Detect the language of the topic above and write your entire analysis in that same language.

Your tasks:
1. Read the other hats' contributions from the shared discussion file
2. Integrate all perspectives
3. Identify consensus and disagreements
4. Propose next steps
5. Keep the discussion focused

Write your summary to the shared file in this format:
## Blue Hat Summary - Round N
### Key Consensus
### Major Disagreements
### Open Questions
### Recommended Direction
```

### Step 3: Iterative Discussion

Run 2-3 rounds of iteration:

**Round 1**: All hat agents speak in parallel (can run concurrently)
**Blue Hat Summary**: Synthesize Round 1 perspectives

**Round 2**: Based on Blue Hat summary, all hats go deeper
**Blue Hat Summary**: Synthesize Round 2 perspectives

**Round 3 (optional)**: If significant disagreements remain, continue
**Final Summary**: Blue Hat produces final conclusions

### Step 4: Generate Final Report

Append to the discussion file:

```markdown
# Brainstorm Final Report

## Topic
{topic}

## Discussion Summary
[Core content from multi-round discussion]

## Key Findings
1. Factual basis: [White Hat core insights]
2. Emotional considerations: [Red Hat core insights]
3. Risk warnings: [Black Hat core insights]
4. Value opportunities: [Yellow Hat core insights]
5. Creative directions: [Green Hat core insights]

## Conclusions & Recommendations
[Blue Hat final synthesis]

## Next Steps
[Specific actionable recommendations]
```

## Technical Implementation Notes

1. **Parallel execution**: Use multiple Task tool calls in a single message to run agents in parallel
2. **Shared file**: All agents read/write to the same timestamped file (e.g., `.claude/brainstorms/brainstorm-20260309-143022.md`). Each session gets a unique file — history is never overwritten
3. **File access**: Each agent uses `subagent_type: "general-purpose"` for full tool access
4. **Ordering**: Blue Hat summary must run after other hats complete
5. **History**: All brainstorm records are preserved in `.claude/brainstorms/` for future reference
6. **Language adaptive**: Agents automatically match the language of the user's topic — no manual configuration needed

## Installation

Copy the `brainstorm/` directory into your Claude Code skills folder:

```bash
cp -r brainstorm/ ~/.claude/skills/brainstorm/
```

## Examples

```
/brainstorm Should we adopt a microservices architecture for our new project?
```

```
/brainstorm 产品是否应该从 B2C 转型 B2B？
```
