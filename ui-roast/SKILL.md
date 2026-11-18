---
name: ui-roast
description: Roasts a UI/UX (screenshot, screen recording description, component code, or running app) across four lenses (UI, UX flow and states, Feel/motion, Style) with funny but specific, honest critique, then turns every roast into a ready-to-use fix prompt written with design principles. Use when the user says "roast this UI", "roast my screen", "what's wrong with this design", "critique this UX", "review this screen like a designer", or invokes /ui-roast. Output is the roast plus fix prompts, not code changes.
---

# UI Roast

Input: something visual or interactive. A screenshot path, a pasted image, a component or screen file, a description of how it moves, or a running app the user points at.
Output: a short roast, ranked, where every joke is attached to a real finding and every finding ends with a fix prompt the user can paste into a coding session.

Fix prompts follow the `design-prompt-rewriter` style: plain, specific, design vocabulary, one short paragraph each. Vocabulary source: `references/keywords.md`.

## Process

1. **Look first.** Image: read it. Code: read the screen file and its styles/animation code. Running app: use the run skill or the user's screenshots. Do not roast from the description alone if the real thing is available.
2. **Scan through the checklist below.** Only report what you actually see or read. Never invent an issue to pad the list.
3. **Rank by user impact**, not by how funny the joke is. Cap at 5 roasts. If more exist, keep them in a one-line "also spotted" list at the end.
4. **Write each roast** in the format below.
5. **Close with one combined prompt** that fixes the top issues together, so the user can paste a single block.

Do not edit files or write code unless the user asks after the roast.

## Checklist (cover every lens: UI, UX, Feel, Style)

Lens map, used to tag each roast: **UI** = 1 to 4, 9 · **UX** = 5, 6, 10, 11, 12, 13, 15 · **Feel** = 7, 8 · **Style** = 14.

1. **Hierarchy:** can you tell in 2 seconds what the screen is for and what to tap first? One clear primary action?
2. **Spacing and alignment:** consistent 4/8pt scale, aligned edges, enough whitespace, nothing cramped or floating.
3. **Typography:** too many sizes or weights, tiny text, bad line length, truncation, no scale for dynamic type.
4. **Color and contrast:** text contrast 4.5:1, color-only meaning, raw one-off colors vs the design tokens, dark mode.
5. **Touch targets and affordance:** 44pt / 48dp minimum, does a button look like a button, pressed states.
6. **Feedback and states:** loading (skeleton vs blank), empty, error with retry, success, disabled, offline.
7. **Motion (if visible or in code):** linear easing, durations under 150ms or over 500ms, no stagger, no interruptibility, momentum missing after gestures, layout shift, animating width/height instead of transform/opacity, blinking on state change.
8. **Gestures and scroll:** conflicting gestures, no momentum, no rubber-banding, missing pull-to-refresh or swipe-back, scroll-jacking, keyboard covering inputs.
9. **Consistency:** same thing styled two ways, icon styles mixed, corner radii or shadows that don't match.
10. **Copy and microcopy:** vague buttons ("Submit"), jargon, error messages that blame the user.
11. **Accessibility:** labels, focus order, reduced-motion fallback, screen reader roles.
12. **Localization / RTL:** mirrored layout, direction of slides and swipes, icons that should flip, mixed Arabic and English alignment.
13. **Flow and information architecture:** how many steps to finish the task, is the next step obvious, are labels and menu groups what a user would expect, dead ends, back path, too many choices on one screen (Hick's law), onboarding that blocks instead of guides. Needs 2 or more screens or a described journey. With one screen, judge only what it implies (entry points, exits, progress cues).
14. **Style and visual language:** does the look hold together as one system (radius, shadow, blur, gradient, illustration and icon style, imagery quality), does it fit the product and its trust level (a health or payments app should feel calm and credible, not noisy), is the effect doing a job or just decorating (glass, glow, gradients), does it look dated or like a default template.
15. **Platform conventions and responsiveness:** back gesture and navigation match the platform (iOS HIG, Material), sheets, tab bars and dialogs behave as users expect, safe areas and notch respected, dark mode works, layout survives small phones, large text, tablets, and landscape or web breakpoints if relevant.

Project context: if the working directory has a `CLAUDE.md` or `AGENTS.md` with UI rules (colors from a shared theme, `useRFLayout` for RTL, bottom sheet components, 5 states for infinite scroll), check against those rules too and cite them.

## Motion check (for the Feel lens)

A still image has no time in it. To judge hover, transitions, and easing, get one of these, best first:

1. **Code.** Read the transition or animation code: CSS `transition` / `@keyframes`, Framer Motion, Reanimated (`withTiming`, `withSpring`, `withDecay`), `Animated`. Check duration, easing, what property is animated (`transform` and `opacity` vs width, height, top), stagger, interruptibility, reduced-motion fallback.
2. **Running web app.** Use whichever browser tool is available (built-in browser or Claude in Chrome). Hover or click the element and take a screenshot before, during (right after the trigger), and after. Read the real values with JS: `getComputedStyle(el).transition` and `el.getAnimations()`. Check hover, focus, pressed, and disabled states, not only the default one.
3. **Screen recording to frames.** Read the video as images; the file reader cannot play video.
   - Check `which ffmpeg`. Trim to the moment that matters, then sample frames into a new empty folder: `ffmpeg -ss <start> -t <seconds> -i <video> -vf "fps=20,scale=480:-1" <outdir>/%03d.png`. Use fps 20 to 30 for 200 to 300ms transitions, 10 for slow scenes.
   - For an overview, make a contact sheet: `ffmpeg -i <video> -vf "fps=10,scale=240:-1,tile=6x4" <outdir>/sheet.png`, then read the frames around the change.
   - Record Android with `adb shell screenrecord --time-limit 8 /sdcard/m.mp4` then `adb pull`. Record the iOS simulator with `xcrun simctl io booted recordVideo m.mov`. Check `which adb` first; it may live under the Android SDK `platform-tools`.
4. **The user's description.** Last resort. Ask what they see at the start, middle, and end of the motion.

What frames can tell: a state or feedback exists or is missing, order of events, rough duration (frame count times frame interval), layout shift, a frame where content disappears (the blink), overshoot, whether the highlight moves or just jumps.
What frames cannot tell: the exact easing curve, jank, spring feel. Say so, and point to the code for those.

Evidence labels: end every Feel roast's "Why" with where it came from: `(from code)`, `(from frames, about 50ms resolution)`, or `(from computed styles)`. Say "about" for any duration read from frames. Never present a guessed number as measured.

If none of 1 to 4 is available, mark `Feel –` in the Lenses line and ask for one of them.

## Roast rules

- **Roast the design, never the person.** No jokes about the author, the team, or the user's taste. The screen is the target.
- **Every joke carries a finding.** If you cannot say what is wrong and why, cut the joke.
- **Funny in one line, serious in the next.** Roast line first (max ~20 words), then the plain reason with the principle name.
- **Be specific.** Point to the element ("the 11px gray caption under the price"), not "the typography".
- **Honest, not cruel.** If something is good, say it once in a single line at the top. Do not invent praise.
- **No fake severity.** Label each: `Blocker` (users fail or get hurt), `Annoying` (users struggle), `Polish` (users feel it).
- **Balance the lenses.** Do not fill all 5 slots with UI nits. If a Flow or Style problem exists, it competes on impact with the rest. Say which lenses you could not judge instead of silently skipping them.
- **Style is opinion, so argue from the product.** Tie each Style roast to the product's audience, trust level, or an inconsistency inside the app. Never "I don't like gradients".
- **Keep the user's stack in mind** when writing fix prompts (React Native, web, SwiftUI). Use its terms if known, neutral otherwise.
- **Tone:** dry, quick, not edgy. No profanity, no memes that need explaining.

## Output format

```
Verdict: <one line, funny, honest>
Good: <one line, real strength>
Lenses: UI ✓ · UX ✓ · Feel ✓ · Style ✓   (mark – with a short reason for any lens you could not judge, e.g. "Feel – still image")

1. [Blocker|Annoying|Polish] [UI|UX|Feel|Style] <Roast line>
   Why: <what is wrong, principle name, one sentence>
   Fix prompt: "<1 to 3 sentences, design vocabulary, paste-ready>"

2. ...

Also spotted: <comma list, only if more than 5>

Combined fix prompt:
"<one paragraph merging the top fixes in the order the user experiences them: first look, touch, release, rest>"

Next: <one thing to do in under 2 minutes, e.g. "paste the combined prompt into a new session">
```

## Example

Input: screenshot of a booking screen. Gray "Confirm" button, 12px gray text under a big stock-photo banner, a spinner on blank white while loading, bottom sheet pops in instantly.

Output:

```
Verdict: A booking screen that hides the one thing you came to do.
Good: Spacing between the session cards is consistent and breathes.
Lenses: UI ✓ · UX ✓ · Feel ✓ (from the code) · Style ✓ — Flow not judged, one screen only

1. [Blocker] [UI] The Confirm button is dressed as a disabled button and is trying very hard to go unnoticed.
   Why: Weak visual hierarchy and no affordance. Gray on gray fails contrast and the primary action does not look tappable.
   Fix prompt: "Make Confirm the single primary action: filled brand color from the shared theme, white label at 4.5:1 contrast or better, sticky at the bottom with a 48dp target. Add a pressed state (scale to about 0.96, 100ms ease-out) and a light haptic. Disable it with a clear style only while the request runs."

2. [Annoying] [UX] A lonely spinner on a blank page, like waiting at an empty airport.
   Why: No skeleton, so the layout jumps when data arrives (layout shift) and perceived performance suffers.
   Fix prompt: "Replace the spinner with skeleton rows with a shimmer, sized to match the final cards so nothing shifts. Crossfade each skeleton into its card with about 50ms stagger."

3. [Annoying] [Feel] The bottom sheet teleports in like it has somewhere better to be.
   Why: Instant mount with no transition, and no drag-to-dismiss, so it feels cheap and disconnected from touch.
   Fix prompt: "Present the sheet with a spring (no overshoot) sliding up from the bottom over about 300ms, with a scrim fading in at the same time. Let the user drag it down to dismiss, with momentum from the release velocity and rubber-banding at the top. Exit with a faster ease-in."

4. [Polish] [Style] The banner is a stock photo of a smiling stranger on a screen about booking a doctor.
   Why: Generic imagery weakens trust for a health product and clashes with the calm flat icons used elsewhere on the screen (visual language inconsistency).
   Fix prompt: "Replace the stock photo with the app's own illustration style or a calm solid tint with the same corner radius as the cards, so the header matches the icon set and reads as credible rather than generic."

5. [Polish] [UI] The 12px gray caption under the banner is whispering the price.
   Why: Tiny low-contrast text, no scale for dynamic type, price is the key info but has the lowest hierarchy.
   Fix prompt: "Promote the price to a bold body-large style next to the title, keep the caption at 14px minimum with 4.5:1 contrast, and let both scale with the system font size."

Combined fix prompt:
"On first load show skeleton cards with a shimmer sized to the final layout, then crossfade to content with a 50ms stagger and no layout shift. Replace the stock photo with the app's illustration style or a calm tint using the card corner radius. Make the price bold and prominent, the caption at least 14px with 4.5:1 contrast. Make Confirm the filled primary action, sticky at the bottom with a 48dp target, a 0.96 press scale and a light haptic. The bottom sheet springs up over about 300ms with a fading scrim, can be dragged down to dismiss with release momentum and rubber-banding, and exits faster with ease-in."

Next: paste the combined prompt into a new session, or run `/design-prompt-rewriter` on any single fix to tune it.
```

## When the input is thin

- Only a vague description ("my app feels bad")? Ask for one thing: a screenshot or the screen's file path. Do not roast blind.
- Motion cannot be judged from a still image. Say so in one line, ask for the animation code, a screen recording, or a running app (see Motion check), then roast what you can see.
- One screen only: judge UI, UX states, Feel (if code or recording exists), and Style. Mark Flow as not judged and offer to review the journey if they share the next screens or describe the steps.
- Multiple screens: do a flow pass first (steps, labels, dead ends, back path), then roast the worst screen, and say which you picked.
