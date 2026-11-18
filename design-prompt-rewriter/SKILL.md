---
name: design-prompt-rewriter
description: Rewrites a non-designer's casual UI/UX/animation feedback ("it blinks", "feels stiff", "stops too suddenly") into a precise prompt using design principles and vocabulary (easing, momentum, spring, velocity, stagger, gestures). Use when the user pastes vague visual or interaction complaints, says "rewrite this like a designer", "make this prompt better for UI", or invokes /design-prompt-rewriter. Output is the rewritten prompt only, not code.
---

# Design Prompt Rewriter

Input: vague, plain-language feedback about how a screen looks, moves, or feels. The writer does not know design terms.
Output: one rewritten prompt a developer or AI can act on. It keeps the writer's intent and swaps feelings for design principles.

Vocabulary source: `references/keywords.md` (read it when a symptom is not covered by the table below).

## Process

1. Read all the input. It may be several messages in a row (complaint, then "better but...", then a new wish). Treat them as one conversation and merge them into one final prompt that reflects the **latest** state of what the writer wants.
2. For each complaint, find the cause behind the feeling (table below), then name the fix with the right keyword.
3. Write the prompt. Keep the writer's own subject words (clouds, sun, card, sheet). Do not rename their things.
4. Output only the rewritten prompt. No preamble, no explanation of the terms, no code.

## Feeling to principle

| What the writer says | Likely cause | Say this instead |
|---|---|---|
| "blinks", "flickers", "flashes" | State is re-rendered or scrubbed per frame, no continuity, layout shift, or remount | Stop recomputing under the finger; keep one continuous animated value; crossfade instead of swap; reserve space (no layout shift) |
| "stiff", "robotic", "mechanical" | `linear` or fixed-duration timing, no velocity | Use a spring (stiffness, damping) or `ease-out`; start from gesture velocity |
| "too sudden", "stops abruptly", "snaps" | No deceleration | Carry momentum on release, `ease-out` / decay to a stop, no snap back |
| "laggy", "choppy", "janky", "slow" | Animating layout props, JS thread, too many re-renders | Run on the UI thread, animate only `transform` and `opacity`, 60fps |
| "too fast", "can't see it" | Duration under ~150ms or no stagger | 200 to 300ms, stagger items 30 to 80ms |
| "too slow", "drags", "waits" | Duration over ~500ms | Shorten to 200 to 300ms, faster exit than entrance |
| "bouncy", "too much" | Underdamped spring or overshoot | Raise damping, critically damped, no overshoot |
| "dead", "flat", "boring" | No feedback or secondary motion | Press feedback (scale ~0.96), subtle ambient motion, haptic, follow-through |
| "jumps", "pops in" | Instant mount | Fade plus slide 8 to 16px in, `ease-out` |
| "disappears weirdly" | Instant unmount | Exit animation, `ease-in`, faster than enter |
| "follows my finger" / "should follow" | Gesture-driven | 1:1 tracking while dragging, hand over to spring on release |
| "keep moving after I let go" | Momentum | Use release velocity, decay / `withDecay`, ease out to stop |
| "do it after I let go" | Release-triggered | One-shot animation on gesture end, independent of the finger |
| "feels cheap", "not premium" | Default easing, no polish | Sharp `ease-out` (quart / expo), spring, consistent durations, subtle shadow / blur |
| "can't tell it's tappable" | No affordance or feedback | Affordance, pressed state, 44pt touch target |
| "I don't know if it worked" | No feedback | Success / error state, toast, haptic, optimistic update |
| "loads weird", "empty then pops" | No loading state | Skeleton shimmer, crossfade to content, reserve space |
| "hard to read", "crowded" | Hierarchy, spacing | Visual hierarchy, whitespace, 4/8pt spacing scale, contrast 4.5:1 |
| "wrong direction" (Arabic, RTL) | Not mirrored | Mirror slide direction and swipe sides for RTL |
| "makes me dizzy" | Large motion, parallax | Reduce motion fallback: swap slide / zoom for a short fade |

If a complaint is not in the table, look in `references/keywords.md`, pick the closest principle, and use its exact term.

## Writing rules

- **Specific over adjective.** Replace "smoother" with a mechanism: spring, ease-out, velocity, momentum. Add numbers only when they are standard (200 to 300ms, 50ms stagger, scale 0.96). Never invent exact numbers that depend on the writer's design.
- **Separate while-touching from after-release.** Gesture work almost always has three phases: while dragging, on release, at rest. Name each phase that the writer implies.
- **Say what must not happen** when the complaint implies it ("no snap back", "no layout shift", "no blink").
- **Keep scope.** Fix only what the writer asked. Do not add new features, new screens, or restyle things they did not mention.
- **Platform-aware, not platform-guessing.** If the writer or the project makes the stack clear (React Native, web, SwiftUI), use its terms (Reanimated `withDecay`, `withSpring`, CSS `transition`). If unclear, stay platform-neutral.
- **Reduced motion and RTL:** mention them only when the change adds visible motion or direction, and keep it to one short clause.
- **Length:** 1 to 4 sentences for 1 to 4 complaints. One short paragraph max. No bullet lists unless the input had 5+ distinct requests.
- **Plain voice.** Imperative, direct, no filler, no "please", no explanation of why the principle works.
- **One question max.** If the input is truly ambiguous (which element?), ask one short question instead of guessing. Otherwise do not ask.

## Output format

Only the rewritten prompt, as plain text. If the input had several separate asks, merge them into one flowing paragraph in the order the user would experience them: touch, release, rest.

## Example

Input:
```
When I swipe fast, the screen blinks. Fix it.
Better, but now it feels stiff. Make it smoother.
The clouds stop too suddenly. Can they keep moving a bit after I let go?
Can the sun move too? But only after I let go.
```

Output:
```
Stop scrubbing the time of day under the finger. While dragging, the sky follows the finger 1:1. On release, use the swipe's velocity: the clouds carry that momentum and ease out to a stop, with no snap back. The sun plays one full arc as a one-shot animation after release, separate from the finger.
```

Why it works: "blinks" became "stop scrubbing under the finger" (continuity), "stiff" became velocity-driven motion, "stop too suddenly" became momentum plus ease-out, "only after I let go" became a release-triggered one-shot. The three phases (drag, release, rest) are explicit.

More examples:

Input: `the popup just appears, looks cheap`
Output: `Animate the popup in: fade and scale from 0.95 to 1 with a slight 8px slide up, 200ms, ease-out. Exit with a faster ease-in fade. Dim the background with a scrim that fades in at the same time.`

Input: `list loads and then everything jumps around`
Output: `Show skeleton rows with a shimmer while loading, sized to match the final rows so nothing shifts. When data arrives, crossfade each skeleton into its row with a short stagger (about 50ms between rows). Reserve space for images to avoid layout shift.`

Input: `button feels dead when I tap`
Output: `Add press feedback: scale to about 0.96 on press-in with a quick ease-out (100ms), spring back on release, and a light haptic tap. Show a clear pressed state and disable the button while the request runs.`
