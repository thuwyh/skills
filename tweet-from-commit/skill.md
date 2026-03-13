---
description: Pick an interesting recent commit and generate a tweet + LinkedIn post + image prompt. Anti-AI tone built in — no buzzwords, no cringe.
argument-hint: "[--count N] [--hash COMMIT_HASH]"
---

# Tweet from Commit

Generate builder-style social content from recent git commits: **$ARGUMENTS**

## Step 1: Parse Arguments

- `--count N`: How many recent commits to scan (default: 20)
- `--hash COMMIT_HASH`: Skip selection, write for this specific commit

## Step 2: Fetch Recent Commits

```bash
git log --oneline --no-merges -<count>
```

Then for each commit, get the full diff stats and message:

```bash
git show --stat --format="%H%n%s%n%b" <hash>
```

## Step 3: Pick the Most Tweet-Worthy Commit

Rate each commit on these criteria (skip if `--hash` is provided):

| Signal | Good for tweet | Bad for tweet |
|--------|---------------|---------------|
| **User-visible impact** | Bug fix users would notice, new feature | Internal refactor nobody sees |
| **Narrative potential** | Has a story (discovered bug, clever fix, surprising insight) | Mechanical change (bump version, rename var) |
| **Relatable to other builders** | Common problem, universal pattern | Hyper-specific to this codebase |
| **Concreteness** | Specific numbers, specific bug, specific outcome | Vague "improvements" |

Pick the ONE commit with the best story. If nothing is interesting, say so honestly instead of forcing bad content.

## Step 4: Read the Actual Diff

```bash
git show <hash>
```

Understand what actually changed. You need the real details to write something authentic.

## Step 5: Write the Content

### STRICT Anti-AI Rules (apply to ALL outputs)

These are HARD rules. Violating any one of them means rewriting from scratch.

**Banned words/phrases** (NEVER use these):
- "excited to share", "thrilled", "proud to announce"
- "just shipped", "just dropped" (unless it's genuinely just now)
- "game-changer", "powerful", "robust", "seamless", "cutting-edge", "innovative"
- "deep dive", "at the end of the day", "it's not about X, it's about Y"
- "here's the thing", "let me explain", "thread 🧵"
- "leverage", "utilize", "streamline", "optimize" (as buzzwords)
- "journey", "passion", "mission"
- Any sentence starting with "I" three times in a row
- ALL dashes used as punctuation — em dash (—), en dash (–), and hyphens (-) used as separators. Rewrite using periods, commas, or restructure the sentence. Compound words with hyphens (e.g., "one-line", "real-time") are fine.
- "This is why..." as a conclusion
- "Let that sink in"
- "Spoiler alert:", "Plot twist:"

**Banned patterns**:
- No rhetorical questions followed by self-answers
- No hashtags unless genuinely part of a conversation (#buildinpublic is ok, #AI #ML #Tech is not)
- Never end with a call-to-action ("What do you think?", "Follow for more", "RT if you agree", "Agree?")
- Don't start with "So" or "So I was..."
- No "Hot take:" or "Unpopular opinion:" prefixes

### What Makes Good Builder Content

The best builder posts share a **specific moment**: a bug you found, a number that surprised you, a shortcut you discovered, a mistake you made. They feel like overhearing someone talk at a coffee shop, not reading a blog post.

### 5A: Tweet (X/Twitter)

**Structure rules**:
- Max 240 characters preferred. Under 200 is ideal. Absolute max 280.
- One idea per tweet. Don't cram.
- If you can say it in fewer words, do.
- Lowercase is fine. Sentence fragments are fine. Casual tone.
- The tweet should sound like a text message to a friend who codes, not a press release.
- No numbered lists (that's LinkedIn, not Twitter)
- No more than 1 emoji, and only if it fits naturally. Zero is better.

**Good tweet examples** (for tone reference, not templates):
- "found a bug where scheduled emails checked a sliding 24h window instead of calendar day. 200+ users getting empty emails. one-line fix."
- "TIL our search endpoint has been silently returning empty results for 2 weeks because of a validation error. nobody noticed because the fallback worked."
- "turned off a single API call that was adding ~3s latency per request. output quality unchanged. sometimes less is more."
- "our background job success rate is 28%. sounds bad until you realize 1300 of the failures are from one test account."

### 5B: LinkedIn Post

LinkedIn allows more room to tell the story, but do NOT fall into LinkedIn cringe.

**Structure rules**:
- 3-8 short paragraphs. Each paragraph 1-2 sentences max.
- First line is the hook. It must work as a standalone line that makes people click "see more". No clickbait, just the interesting part up front.
- Tell the story: what happened → what you found → what you did → what you learned.
- OK to include specific numbers, code snippets (short), or before/after comparisons.
- End with the takeaway or lesson, stated plainly. Not a motivational quote.
- Casual professional tone. You're a builder sharing a war story, not a thought leader.
- No emoji bullets (🔥 💡 🚀). Zero or one emoji total.
- No "I'm humbled", no "grateful for the journey", no "if you're a founder..."
- Max ~800 characters. Keep it tight.

**Good LinkedIn examples** (for tone):
- Hook: "Deleted one line of code yesterday. Error rate went from 12% to zero."
  Body tells the story of why it was there, what it did, how you found it.
  End: plain lesson.

- Hook: "Our API was returning empty results for 2 weeks and nobody noticed."
  Body: how the fallback masked it, how you found the validation bug.
  End: what you changed.

**LinkedIn-specific banned patterns**:
- No "I'm [name] and I [verb]..." intros
- No "Here are 5 things I learned" numbered lists
- No line breaks after every single sentence for fake drama
- No tagging people or companies for engagement
- No "Repost if you agree ♻️"

### 5C: Image Prompt

Generate a prompt for an AI image generator to create a hand-drawn style illustration that supports the post.

**Image prompt rules**:
- Style: hand-drawn / sketch / whiteboard style illustration
- The image should visually explain or diagram the core concept from the post
- Include short text labels or annotations on the image (like a whiteboard sketch)
- Keep text on the image to a few key words/numbers, not full sentences
- Think: "if someone drew this on a whiteboard to explain the concept to a coworker"
- Prefer: diagrams, before/after comparisons, simple flowcharts, annotated code snippets
- Color: limited palette, 2-3 colors max on white/light background
- Do NOT describe generic stock-photo scenes. The image should be informational, not decorative.

**Good image prompt examples**:
- "hand-drawn whiteboard sketch showing a sliding 24h window vs calendar day window, with timestamps and a confused face next to 'empty emails'. whiteboard marker style, black and orange ink"
- "hand-drawn before/after sketch: left side shows a retry loop with '10 min' arrows cycling, right side shows a single 'SKIP' record breaking the loop. simple line art, blue ink"
- "hand-drawn diagram showing an API call crossed out with a red X, arrow from '3.2s' to '0.1s' response time, small checkmark next to 'same output quality'. sketch style, blue and red on white"

## Step 6: Output

Present all three pieces like this:

```
Commit: <short hash> <commit message>

━━━ Tweet ━━━

<the tweet>

(Characters: <count>/280)

━━━ LinkedIn ━━━

<the linkedin post>

(Characters: <count>)

━━━ Image Prompt ━━━

<the image generation prompt>
```

If you wrote multiple options for the tweet (max 2), show them both and let the user pick. LinkedIn and image prompt only need one version each.

If no commit was tweet-worthy, say:

```
Looked at the last <N> commits — nothing that makes for a good post right now.
Best candidate was <hash> (<message>) but <reason it's not great>.
```

Don't force it.
