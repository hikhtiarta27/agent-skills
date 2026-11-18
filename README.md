# design-skills

Two Claude Code skills for people who aren't designers.

| Skill | What it does |
|---|---|
| `design-prompt-rewriter` | Turns vague UI feedback ("it blinks", "feels stiff") into a precise prompt using design principles (easing, momentum, spring, velocity). |
| `ui-roast` | Roasts a screen (screenshot, code, or description) with specific, honest critique, then gives a paste-ready fix prompt for each issue. |

Each skill ships with `references/keywords.md`, a UI/UX/animation vocabulary (easing, physics, gestures, scroll, accessibility, performance).

## Install

```bash
npx skills add https://github.com/hikhtiarta27/design-skills
```

Or copy the two folders into `~/.claude/skills/` and start a new session.

## Use

```
/design-prompt-rewriter the popup just appears, looks cheap
/ui-roast path/to/screenshot.png
```
