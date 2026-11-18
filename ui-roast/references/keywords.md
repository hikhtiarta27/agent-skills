# UI / UX / Animation Keywords

Vocabulary for describing motion and interaction on web and mobile apps. Use these words in prompts, tickets, and design reviews.

Sections:
1. [Easing](#1-easing)
2. [Timing and duration](#2-timing-and-duration)
3. [Physics and momentum](#3-physics-and-momentum)
4. [Animation types](#4-animation-types)
5. [Transitions and navigation](#5-transitions-and-navigation)
6. [Gestures](#6-gestures)
7. [Scroll behavior](#7-scroll-behavior)
8. [Micro-interactions and feedback](#8-micro-interactions-and-feedback)
9. [Loading and perceived performance](#9-loading-and-perceived-performance)
10. [Layout and component patterns](#10-layout-and-component-patterns)
11. [Visual effects](#11-visual-effects)
12. [Web: CSS / JS keywords](#12-web-css--js-keywords)
13. [Mobile: React Native / iOS / Android keywords](#13-mobile-react-native--ios--android-keywords)
14. [UX principles and laws](#14-ux-principles-and-laws)
15. [Accessibility and motion safety](#15-accessibility-and-motion-safety)
16. [Performance](#16-performance)
17. [Prompt cheat sheet](#17-prompt-cheat-sheet)

---

## 1. Easing

Easing = how speed changes over time. Never use `linear` for UI movement (only for spinners and progress).

| Keyword | Feel | Use for |
|---|---|---|
| `linear` | Constant speed, robotic | Spinners, progress bars, marquee |
| `ease` | Slow start, fast middle, slow end (CSS default) | Generic fallback |
| `ease-in` | Starts slow, ends fast | Elements **leaving** the screen |
| `ease-out` | Starts fast, ends slow (decelerate) | Elements **entering** the screen, most UI |
| `ease-in-out` | Slow, fast, slow | Elements moving between two on-screen points |
| `ease-out-cubic` / `-quart` / `-quint` / `-expo` | Increasingly sharp deceleration | Snappy, premium feel |
| `ease-in-out-cubic` | Smooth, balanced | Modals, reordering |
| `ease-out-back` / overshoot | Goes past target, settles back | Playful pop-ins, buttons, badges |
| `ease-in-back` / anticipation | Pulls back before moving | Playful exits, wind-up |
| `ease-out-elastic` | Wobbles around target | Rare, playful only |
| `ease-out-bounce` | Bounces at the end | Rare, playful only |
| `cubic-bezier(x1, y1, x2, y2)` | Custom curve | Brand-specific motion |
| `steps(n)` | Discrete jumps | Sprite sheets, typewriter |

Rules of thumb:
- **Enter = ease-out. Exit = ease-in. Move = ease-in-out.**
- Prefer sharp ease-out (`cubic-bezier(0.22, 1, 0.36, 1)` = easeOutQuint) over default `ease` for a premium feel.
- Exits should be faster than entrances.

Named design-system curves:
- Material 3: `emphasized` (`0.2, 0, 0, 1`), `emphasized-decelerate` (`0.05, 0.7, 0.1, 1`), `emphasized-accelerate` (`0.3, 0, 0.8, 0.15`), `standard`
- iOS: default spring, `easeInOut`, `curveEaseOut`
- Common presets: `easeOutQuad`, `easeOutCubic`, `easeOutQuart`, `easeOutExpo`, `easeInOutCubic`

---

## 2. Timing and duration

| Keyword | Meaning |
|---|---|
| `duration` | Total animation time |
| `delay` | Wait before start |
| `stagger` | Offset delay between sibling items (list cascading in) |
| `cascade` / `waterfall` | Items appear one after another |
| `sequence` | Animations run one after another |
| `parallel` / `concurrent` | Animations run together |
| `orchestration` | Coordinating many animations into one choreography |
| `choreography` | Designed relationship between several moving elements |
| `keyframes` | Defined states along a timeline |
| `loop` / `iteration-count` / `infinite` | Repeat |
| `alternate` / `yoyo` / `reverse` | Play forward then backward |
| `fill-mode` (`forwards`, `backwards`, `both`) | Keep start/end state outside the run |
| `timeline` | Scrubbable sequence of animations |
| `frame rate` / `60fps` / `120fps` | Smoothness target (ProMotion = 120) |
| `frame budget` | 16.6ms per frame at 60fps |
| `jank` / `dropped frames` / `stutter` | Visible lag |

Typical durations:
- **Micro-interactions** (hover, press, toggle): 100–200ms
- **Small elements** (tooltip, dropdown, chip): 150–250ms
- **Medium** (modal, bottom sheet, drawer): 250–400ms
- **Large / full-screen transitions**: 300–500ms
- **Anything over ~500ms** feels slow unless it is decorative or storytelling
- **Stagger step**: 30–80ms per item, cap total at ~400ms

---

## 3. Physics and momentum

| Keyword | Meaning |
|---|---|
| `momentum` | Movement continues after finger lifts |
| `inertia` | Same idea: velocity carries on and decays |
| `fling` | Quick swipe that triggers momentum |
| `deceleration rate` | How fast momentum decays (`normal`, `fast`) |
| `friction` | Decay force that slows motion |
| `spring` | Physics-based animation, no fixed duration |
| `stiffness` | How strongly spring pulls to target (higher = snappier) |
| `damping` | How fast oscillation dies (higher = less bounce) |
| `mass` | Weight of the object (higher = slower, heavier) |
| `velocity` | Starting speed, usually from the gesture |
| `overshoot` | Goes past the target before settling |
| `underdamped` | Bouncy spring |
| `critically damped` | Fastest settle with no bounce |
| `overdamped` | Slow, no bounce |
| `bounce` | Spring bounciness parameter (SwiftUI) |
| `rubber-banding` / `elastic overscroll` | Resistance when dragging past bounds |
| `snap` / `snap-to` / `magnetic` | Settles onto defined points |
| `settle` | Final resting phase of a spring |
| `gravity` | Falling or dropping motion |
| `follow-through` / `overlap` | Parts of an element finish at different times (Disney principle) |
| `drag with resistance` | Movement slows as it nears a limit |
| `interruptible` | User can grab or cancel mid-animation |
| `velocity-preserving` | New animation starts with the old one's speed |

Why springs: they respond to gesture velocity and are interruptible, so they feel physical. Prefer springs for anything driven by touch.

---

## 4. Animation types

| Keyword | Meaning |
|---|---|
| `fade` (in/out) | Opacity change |
| `slide` (up/down/left/right) | Translate in from an edge |
| `scale` / `zoom` / `grow` / `shrink` | Size change |
| `pop` | Scale up with overshoot |
| `pulse` | Repeating scale or opacity beat |
| `shake` / `wiggle` / `jiggle` | Horizontal oscillation (error, delete mode) |
| `bounce` | Vertical spring |
| `rotate` / `spin` / `flip` | Rotation (2D or 3D) |
| `morph` / `shape morph` | One shape or icon turns into another |
| `shared element transition` / `hero` / `magic move` | Element persists between two screens |
| `matched geometry` | SwiftUI name for shared element |
| `FLIP` (First, Last, Invert, Play) | Technique for layout animation |
| `layout animation` | Animate position and size changes automatically |
| `crossfade` / `dissolve` | One content replaces another via opacity |
| `reveal` / `wipe` / `mask` / `clip-path` | Content uncovered by a moving mask |
| `expand` / `collapse` / `accordion` | Height or size toggle |
| `parallax` | Layers move at different speeds |
| `ken burns` | Slow pan and zoom on an image |
| `typewriter` | Characters appear one by one |
| `count up` / `number ticker` / `odometer` / `rolling digits` | Animated number change |
| `skeleton shimmer` | Moving highlight on placeholder |
| `ripple` | Expanding circle from touch point (Material) |
| `confetti` / `particles` / `burst` | Celebration effect |
| `Lottie` | JSON vector animation exported from After Effects |
| `Rive` | Interactive state-machine vector animation |
| `sprite sheet` | Frame-by-frame image strip |
| `path animation` / `stroke draw` / `line drawing` | SVG stroke-dashoffset reveal |
| `scroll-driven animation` | Progress tied to scroll position |
| `view transition` | Browser-native animated DOM state change |
| `blur-in` / `blur-out` | Blur plus fade for soft entrance |
| `squash and stretch` | Deform on motion to suggest weight |
| `anticipation` | Small opposite motion before the main one |
| `staging` / `focus` | Direct attention to one element at a time |
| `ambient animation` | Subtle idle motion (float, breathe, glow) |
| `looping` | Repeating background animation |

---

## 5. Transitions and navigation

| Keyword | Meaning |
|---|---|
| `push` / `pop` | Stack navigation forward / back |
| `modal` / `present` / `dismiss` | Screen over current one |
| `bottom sheet` / `half sheet` / `detents` | Draggable panel from bottom with snap heights |
| `drawer` / `side sheet` | Panel from the side |
| `tab switch` / `swipe between tabs` | Horizontal paging |
| `fade through` | Out then in, no spatial relation (Material) |
| `fade through scale` | Same with slight scale |
| `shared axis` (X / Y / Z) | Spatial transition along an axis (Material) |
| `container transform` | Card expands into a full screen (Material) |
| `zoom transition` | Thumbnail grows to full view |
| `cross-fade` | Overlap two states |
| `slide from right` / `slide from bottom` | Directional push or modal |
| `interactive pop` / `swipe back` | Back gesture follows the finger |
| `interactive dismiss` / `pull down to close` | Drag modal to dismiss |
| `page curl` / `cube` / `card stack` / `carousel` | Decorative paging styles |
| `route transition` | Animation between pages (web SPA) |
| `skeleton to content` | Placeholder swaps for real data |
| `optimistic update` | UI changes immediately, server confirms later |
| `empty-to-filled` | Animated transition from empty state |
| `onboarding flow` / `coach mark` / `spotlight` | Guided intro highlights |
| `deep link transition` | Cold-start animation from external link |

---

## 6. Gestures

| Keyword | Meaning |
|---|---|
| `tap` / `press` | Quick touch |
| `double tap` | Two quick taps (like, zoom) |
| `long press` / `press and hold` | Hold, opens menu or drag mode |
| `pan` / `drag` | Move finger while touching |
| `swipe` | Fast directional pan |
| `fling` | Swipe with high release velocity |
| `pinch` / `pinch to zoom` | Two-finger scale |
| `rotate gesture` | Two-finger twist |
| `edge swipe` | Swipe from screen edge (back, drawer) |
| `swipe actions` / `swipe to delete` / `swipe to reveal` | Row slides to show buttons |
| `pull to refresh` | Drag down past top to reload |
| `pull to dismiss` | Drag to close sheet |
| `drag and drop` / `reorder` | Move items |
| `hover` | Pointer over (web only) |
| `focus` / `focus-visible` | Keyboard or programmatic focus |
| `active` / `pressed state` | While pointer is down |
| `hit slop` / `touch target` | Tappable area (min 44pt iOS, 48dp Android) |
| `gesture handler` / `gesture recognizer` | Native layer that detects gestures |
| `gesture conflict` / `simultaneous gesture` | Two gestures competing (scroll vs swipe) |
| `activation threshold` / `dead zone` | Min movement before gesture starts |
| `velocity threshold` | Min speed to count as a fling |
| `haptic feedback` / `taptic` | Vibration response (light, medium, heavy, selection, success, error) |

---

## 7. Scroll behavior

| Keyword | Meaning |
|---|---|
| `smooth scroll` | Animated programmatic scroll |
| `momentum scroll` / `inertial scroll` | Keeps scrolling after release |
| `overscroll` / `bounce` | Pull past edge (iOS) |
| `overscroll glow` / `stretch` | Android edge effect |
| `scroll snap` / `snap points` / `paging` | Settles on items |
| `sticky header` / `pinned` | Stays at top while scrolling |
| `collapsing header` / `large title` / `parallax header` | Header shrinks on scroll |
| `hide on scroll` / `show on scroll up` | Bars slide away |
| `scroll-linked animation` | Animation progress = scroll offset |
| `scroll-triggered animation` / `reveal on scroll` | Play when entering viewport |
| `scroll-jacking` | Hijacking native scroll (usually bad) |
| `infinite scroll` / `load more` / `pagination` | Fetch next page |
| `virtualization` / `windowing` / `recycling` | Render only visible rows |
| `scroll restoration` | Return to previous position |
| `scroll to top` / `scroll into view` | Programmatic scroll |
| `scroll indicator` / `scrollbar` | Position hint |
| `pinned section` / `scrollytelling` | Content changes while a section stays fixed |
| `intersection observer` | Detect element entering viewport |
| `keyboard avoiding` | Scroll or shift so the input stays visible |

---

## 8. Micro-interactions and feedback

| Keyword | Meaning |
|---|---|
| `micro-interaction` | Tiny animated response to a single action |
| `affordance` | Visual hint that something is interactive |
| `signifier` | Cue that shows what an action does |
| `feedback` | System acknowledging an action |
| `state change` | Default, hover, focus, pressed, disabled, loading, error, success |
| `toggle` / `switch` | On/off with thumb slide |
| `checkbox tick` | Animated check draw |
| `button press` | Scale to ~0.96 with quick ease-out |
| `ripple` / `touch feedback` | Visual press response |
| `like animation` / `heart burst` | Favorite feedback |
| `success checkmark` | Draw-on checkmark after completion |
| `error shake` | Input shakes after invalid submit |
| `inline validation` | Real-time field feedback |
| `toast` / `snackbar` | Brief non-blocking message |
| `banner` / `alert` / `dialog` | Increasing levels of interruption |
| `tooltip` / `popover` / `hint` | Contextual help |
| `badge` / `dot` / `counter` | Notification indicator |
| `progress indicator` | Determinate (known) or indeterminate (unknown) |
| `stepper` / `wizard` | Multi-step progress |
| `undo` / `snackbar with undo` | Reversible action |
| `confirmation` | Extra step before destructive action |
| `delight` | Small unexpected positive moment |
| `skeuomorphic` | Mimics real-world physical object |
| `sound feedback` | Audio cue for action |
| `cursor change` | Pointer, grab, not-allowed (web) |
| `magnetic button` | Button pulls toward cursor |
| `tilt` / `3D card hover` | Card rotates with pointer |
| `spotlight hover` / `glow follow` | Light follows cursor |

---

## 9. Loading and perceived performance

| Keyword | Meaning |
|---|---|
| `skeleton screen` / `placeholder` / `shimmer` | Gray layout shapes while loading |
| `spinner` / `activity indicator` | Indeterminate loading |
| `progress bar` / `top loader` | Determinate or fake progress |
| `optimistic UI` | Show success before server replies |
| `lazy loading` | Load when needed |
| `prefetch` / `preload` / `prerender` | Load ahead of time |
| `blur-up` / `LQIP` / `blurhash` / `thumbhash` | Tiny blurred preview before full image |
| `progressive image` | Low to high resolution |
| `stale-while-revalidate` | Show cached data, refresh in background |
| `pull to refresh` | Manual reload |
| `empty state` | No data yet (explain + CTA) |
| `error state` / `retry` | Failure with recovery |
| `offline state` | No connection UI |
| `splash screen` / `launch screen` | App start display |
| `cold start` / `warm start` | Time to first screen |
| `time to interactive (TTI)` | When the UI responds |
| `perceived performance` | How fast it feels vs. how fast it is |
| `Doherty threshold` | Response under 400ms keeps users engaged |
| `layout shift (CLS)` | Content jumping as things load; reserve space |
| `content-aware placeholder` | Skeleton matching final layout |
| `debounce` / `throttle` | Limit how often input or scroll triggers work |
| `Suspense` / `streaming` | Show parts as they become ready |

---

## 10. Layout and component patterns

| Keyword | Meaning |
|---|---|
| `responsive` / `adaptive` | Layout adjusts to size / switches per device |
| `breakpoint` | Width where layout changes |
| `mobile-first` | Design smallest screen first |
| `grid` / `flex` / `masonry` | Layout systems |
| `safe area` / `notch` / `dynamic island` / `home indicator` | Insets to respect |
| `spacing scale` (4/8pt) | Consistent gaps |
| `visual hierarchy` | What the eye reads first |
| `whitespace` / `negative space` | Breathing room |
| `card` / `tile` / `list item` / `cell` | Content containers |
| `carousel` / `slider` / `swiper` | Horizontal scrolling set |
| `segmented control` / `tabs` / `pills` / `chips` | Option switchers |
| `bottom navigation` / `tab bar` / `navigation rail` | Primary navigation |
| `app bar` / `top bar` / `nav bar` | Top header |
| `FAB` (floating action button) | Primary round action |
| `mega menu` / `dropdown` / `popover` / `context menu` | Menus |
| `command palette` | Keyboard-driven search (⌘K) |
| `search-as-you-type` / `autocomplete` / `typeahead` | Live suggestions |
| `date picker` / `time wheel` / `stepper` / `slider` | Input controls |
| `sticky CTA` / `bottom bar CTA` | Persistent primary action |
| `modal` / `lightbox` / `overlay` / `scrim` | Layer over content (scrim = dim backdrop) |
| `elevation` / `shadow` / `z-index` | Depth levels |
| `sheet detents` | Heights a bottom sheet snaps to (small, medium, large) |
| `dark mode` / `theme` / `design tokens` | Color and style systems |
| `RTL` / `bidi` / `mirroring` | Right-to-left language layouts (Arabic) |
| `truncation` / `ellipsis` / `line clamp` | Cut long text |
| `dynamic type` / `font scaling` | OS text size setting |
| `density` | Compact vs comfortable spacing |
| `pull-down menu` / `action sheet` | Choice list from an action |
| `masonry` / `bento grid` | Mixed-size tile layout |
| `sidebar` / `rail` / `split view` | Wide-screen navigation |

---

## 11. Visual effects

| Keyword | Meaning |
|---|---|
| `glassmorphism` / `frosted glass` / `backdrop blur` | Translucent blurred panels |
| `liquid glass` | iOS 26 refractive translucent material |
| `neumorphism` | Soft extruded look |
| `gradient` (linear, radial, conic, mesh, animated) | Color blends |
| `glow` / `bloom` / `halo` | Soft light around element |
| `drop shadow` / `inner shadow` / `layered shadow` | Depth |
| `border gradient` / `shine` / `sheen` / `sweep` | Moving light highlight |
| `noise` / `grain` | Texture overlay |
| `vignette` | Darkened edges |
| `blur` (gaussian, progressive, variable) | Defocus |
| `blend mode` (multiply, screen, overlay, soft-light) | Layer mixing |
| `masking` / `clip` / `cutout` | Shape visibility control |
| `shader` / `Skia` / `Metal` / `WebGL` / `canvas` | GPU-drawn effects |
| `3D transform` / `perspective` / `depth` | Z-axis effects |
| `parallax` / `tilt` / `gyro` | Motion-reactive depth |
| `duotone` / `tint` / `color overlay` | Color treatments |
| `shimmer` | Light sweep across surface |
| `aurora` / `blob` / `ambient background` | Soft moving color fields |
| `bento` / `spotlight` / `grid lines` | Trendy landing page styles |
| `skeumorph` / `flat` / `material` | Overall visual language |

---

## 12. Web: CSS / JS keywords

CSS properties:
- `transition` (`property duration timing-function delay`)
- `animation` / `@keyframes`
- `transform` (`translate`, `scale`, `rotate`, `skew`, `perspective`), `transform-origin`
- `will-change` (hint browser, use sparingly)
- `opacity`, `filter`, `backdrop-filter`
- `clip-path`, `mask-image`
- `scroll-behavior: smooth`
- `scroll-snap-type`, `scroll-snap-align`
- `overscroll-behavior`
- `position: sticky`
- `animation-timeline: scroll() | view()` (scroll-driven animations)
- `@starting-style` (animate entry from `display: none`)
- `transition-behavior: allow-discrete`
- `interpolate-size: allow-keywords` (animate to `height: auto`)
- `@property` (animate custom properties, gradients)
- `linear()` easing function (spring-like curves in CSS)
- `view-transition-name`, `::view-transition-old/new`
- `prefers-reduced-motion`, `prefers-color-scheme`
- `contain`, `content-visibility`
- `aspect-ratio`, `container queries` (`@container`)
- `touch-action`, `-webkit-overflow-scrolling: touch`
- `:hover`, `:focus-visible`, `:active`, `:has()`

JS / APIs:
- `requestAnimationFrame` (rAF)
- Web Animations API (`element.animate()`)
- View Transitions API (`document.startViewTransition`)
- `IntersectionObserver`, `ResizeObserver`
- `Pointer Events`, `touch events`
- `Popover API`, `<dialog>`

Libraries:
- **Framer Motion / Motion** (`layout`, `layoutId`, `AnimatePresence`, `whileHover`, `whileTap`, `useSpring`)
- **GSAP** (`timeline`, `ScrollTrigger`, `stagger`, `ease`)
- **React Spring**, **Lenis** (smooth scroll), **Locomotion**
- **Anime.js**, **Auto-Animate**, **Lottie-web**, **Rive**, **Three.js**, **Theatre.js**
- **Tailwind**: `transition`, `duration-200`, `ease-out`, `animate-pulse`, `animate-spin`, `group-hover`, `motion-reduce:`

Web-specific concepts:
- `hover` states (no touch equivalent)
- `cursor` styling and custom cursors
- `scroll-jacking` (avoid)
- `FLIP` technique
- `compositor-only properties` (`transform`, `opacity`)
- `reflow` / `repaint` / `layout thrashing`
- `Core Web Vitals` (LCP, INP, CLS)
- `hydration` flash / `FOUC`

---

## 13. Mobile: React Native / iOS / Android keywords

React Native:
- **Reanimated**: `useSharedValue`, `useAnimatedStyle`, `withTiming`, `withSpring`, `withDecay` (momentum), `withDelay`, `withSequence`, `withRepeat`, `interpolate`, `Extrapolation.CLAMP`, `runOnJS`, `runOnUI`, worklet, `useAnimatedScrollHandler`, `useDerivedValue`
- **Reanimated layout animations**: `entering`, `exiting`, `layout`, `FadeIn`, `FadeOut`, `SlideInRight`, `ZoomIn`, `LinearTransition`, `SharedTransition`
- **Gesture Handler**: `Gesture.Pan()`, `Gesture.Tap()`, `Gesture.Pinch()`, `Gesture.Simultaneous`, `Gesture.Exclusive`, `GestureDetector`, `activeOffsetX`, `failOffsetY`
- **Built-in `Animated`**: `Animated.timing`, `Animated.spring`, `Animated.decay`, `useNativeDriver`
- `LayoutAnimation` (simple layout transitions)
- **Skia** (`@shopify/react-native-skia`), **Lottie**, **Rive**
- **Bottom sheet**: `@gorhom/bottom-sheet` (`snapPoints`, `enablePanDownToClose`, `BottomSheetModal`)
- `FlatList` / `FlashList` (`getItemLayout`, `windowSize`, `removeClippedSubviews`)
- `KeyboardAvoidingView`, `keyboardShouldPersistTaps`
- `Pressable` (`pressed` state, `hitSlop`, `android_ripple`)
- `useNativeDriver`, UI thread vs JS thread, `InteractionManager`
- `Haptics` (`expo-haptics`, `react-native-haptic-feedback`)
- `react-native-screens`, native stack transitions (`animation: 'slide_from_right' | 'fade' | 'simple_push'`, `presentation: 'modal' | 'transparentModal' | 'formSheet'`)

iOS (UIKit / SwiftUI):
- `UIViewPropertyAnimator`, `UISpringTimingParameters`, `CASpringAnimation`
- SwiftUI: `withAnimation`, `.spring(response:dampingFraction:)`, `.snappy`, `.smooth`, `.bouncy`, `matchedGeometryEffect`, `.transition`, `PhaseAnimator`, `KeyframeAnimator`, `.contentTransition(.numericText())`, `.scrollTransition`, `.sensoryFeedback`
- `UIFeedbackGenerator`, `UIImpactFeedbackGenerator`
- `UIBlurEffect`, vibrancy, `.ultraThinMaterial`
- Sheet `detents`, `.presentationDetents`
- `rubber banding`, `scroll deceleration` (`.normal`, `.fast`)
- Human Interface Guidelines (HIG)

Android (Compose / Material):
- `animateFloatAsState`, `animateDpAsState`, `AnimatedVisibility`, `AnimatedContent`, `Crossfade`, `updateTransition`
- `spring(dampingRatio, stiffness)`, `tween`, `keyframes`, `FastOutSlowInEasing`, `LinearOutSlowInEasing`
- `Modifier.animateContentSize()`, `SharedTransitionLayout`
- `MotionLayout`, `ObjectAnimator`, `ValueAnimator`
- Material 3: `ripple`, `state layer`, `elevation overlay`, `predictive back`, `edge-to-edge`
- Material motion: `container transform`, `shared axis`, `fade through`, `fade`

---

## 14. UX principles and laws

| Name | One-liner |
|---|---|
| Fitts's Law | Bigger and closer targets are faster to hit |
| Hick's Law | More choices = slower decisions |
| Miller's Law | ~7±2 items in working memory |
| Jakob's Law | Users expect your app to work like others they use |
| Doherty Threshold | Respond within 400ms |
| Von Restorff Effect | The different item is remembered |
| Serial Position Effect | First and last items are remembered best |
| Zeigarnik Effect | Unfinished tasks stay in mind (progress bars, onboarding) |
| Peak-End Rule | Experience judged by the peak and the end |
| Aesthetic-Usability Effect | Pretty UIs feel easier to use |
| Law of Proximity / Similarity / Common Region | Grouping through placement, look, containers |
| Tesler's Law | Complexity cannot be removed, only moved |
| Postel's Law | Be liberal in what you accept from users |
| Goal-Gradient Effect | Motivation rises near the goal |
| Progressive disclosure | Show advanced options only when needed |
| Recognition over recall | Show options, don't make users remember |
| Fogg Behavior Model | Behavior = motivation + ability + prompt |
| Hooked model | Trigger, action, variable reward, investment |
| Nielsen's 10 heuristics | Visibility of status, match to real world, user control, consistency, error prevention, etc. |

UX vocabulary:
- `user flow` / `journey map` / `happy path` / `edge case`
- `friction` / `cognitive load` / `decision fatigue`
- `onboarding` / `activation` / `time-to-value` / `aha moment`
- `empty state` / `zero state` / `first-run experience`
- `call to action (CTA)` / `primary` / `secondary` / `tertiary` / `destructive`
- `affordance` / `signifier` / `mental model`
- `dark pattern` (avoid: confirm-shaming, forced continuity)
- `wireframe` / `mockup` / `prototype` / `design system` / `component library`
- `A/B test` / `usability test` / `heatmap` / `session replay`
- `skeleton` / `shimmer` / `optimistic`
- `above the fold` / `F-pattern` / `Z-pattern`
- `nudge` / `gamification` / `streak` / `reward`
- `error prevention` / `error recovery` / `forgiving UI`
- `content hierarchy` / `scannability` / `microcopy`
- `localization (i18n/l10n)` / `RTL`

---

## 15. Accessibility and motion safety

| Keyword | Meaning |
|---|---|
| `prefers-reduced-motion` / `Reduce Motion` | OS setting: tone down or remove motion |
| `reduced motion fallback` | Swap slide/zoom for a simple fade |
| `vestibular` | Motion sickness triggers (parallax, large zoom, spinning) |
| `photosensitivity` | Avoid flashing more than 3 times per second |
| `pause / stop / hide` | Users must be able to stop auto-playing motion (WCAG 2.2.2) |
| `focus ring` / `focus-visible` | Keyboard focus indicator |
| `focus trap` / `focus management` | Keep focus in modal; restore on close |
| `screen reader` (VoiceOver, TalkBack) | Assistive tech |
| `accessibilityLabel` / `aria-label` | Spoken label |
| `accessibilityRole` / `role` | Semantic role |
| `aria-live` / `announce` | Announce dynamic updates |
| `contrast ratio` | 4.5:1 text, 3:1 large text and UI (WCAG AA) |
| `touch target size` | 44pt iOS / 48dp Android |
| `dynamic type` / `font scale` | Respect user text size |
| `color-blind safe` | Don't rely on color alone |
| `keyboard navigation` / `tab order` | Operate without a mouse |
| `bold text` / `increase contrast` / `reduce transparency` | OS display settings |
| `RTL mirroring` | Flip layout and directional animations (slide-in direction, back arrow) |

Rule: every animation needs a reduced-motion version. In RTL, slide directions and swipe gestures must mirror.

---

## 16. Performance

| Keyword | Meaning |
|---|---|
| `60fps` / `120fps` | Target frame rate |
| `UI thread` vs `JS thread` | Run animations on UI thread so JS load doesn't drop frames |
| `native driver` | Animation computed natively |
| `worklet` | Function running on UI thread (Reanimated) |
| `compositor` / `GPU-accelerated` | Cheap properties: `transform`, `opacity` |
| `layout thrashing` | Reading and writing layout repeatedly |
| `reflow` / `repaint` | Expensive browser recalculation |
| `will-change` | Pre-promote a layer (use sparingly) |
| `memoization` (`memo`, `useMemo`, `useCallback`) | Avoid needless re-render |
| `virtualized list` | Render only visible rows |
| `image caching` / `resizing` / `WebP` / `AVIF` | Smaller, faster images |
| `bundle size` / `code splitting` / `lazy import` | Smaller initial load |
| `hermes` | RN JS engine |
| `profiler` / `flame chart` / `Perfetto` / `Instruments` | Measure before optimizing |
| `jank` / `hitch` / `frame drop` | Visible stutter |
| `overdraw` | Painting the same pixels repeatedly |
| `memory pressure` | Too much in RAM → crashes or freezes |

Animate only `transform` and `opacity` where possible. Avoid animating `width`, `height`, `top`, `left`, `margin` (they trigger layout).

---

## 17. Prompt cheat sheet

Copy-paste phrasing:

- "Fade in with `ease-out`, 200ms, slide up 12px."
- "Staggered list entrance, 50ms between items, `ease-out-cubic`."
- "Bottom sheet with spring physics, snap points at 40% and 90%, pan down to close, rubber-band at top."
- "Press feedback: scale to 0.96, 100ms ease-out, haptic light impact."
- "Momentum scroll with snap to each card, `decelerationRate: fast`."
- "Shared element transition from the thumbnail to the detail hero image, 350ms, `emphasized` easing."
- "Skeleton shimmer while loading, crossfade to content, reserve space to avoid layout shift."
- "Optimistic update on like, heart burst, roll back with a toast if the request fails."
- "Collapsing large header that shrinks on scroll, with parallax on the image."
- "Number count-up with rolling digits, 600ms, `ease-out-expo`."
- "Error: field shakes horizontally 3 times, 300ms, border turns red, `aria-live` announcement."
- "Respect reduced motion: replace slide and scale with a 150ms fade."
- "RTL: mirror the slide direction and swipe-to-delete side."
- "Run on the UI thread with Reanimated worklets; only animate `transform` and `opacity`."
- "Interruptible spring: if the user grabs mid-animation, continue from current velocity."
- "Use `withDecay` for fling momentum, clamp to content bounds with rubber-banding."

Default recipe if unsure:
- Duration 200–300ms
- Enter: `ease-out` (or spring, stiffness ~300, damping ~28)
- Exit: `ease-in`, ~20% faster than enter
- Animate `transform` and `opacity` only
- Provide a reduced-motion fallback
