# VoxSign — Mobile UI Design System

_Version 1.0 · Applies to VoxSign-IOS and VoxSign-Android. All mobile UI work must
follow this document. Copy on voxsign.ai derives from BRAND.md; every visual and
interaction decision on mobile derives from this document._

---

## 1. Design Principles

1. **Simple beats clever.** One primary action per screen. No decorative
   controls, no non-essential affordances (no "cancel/undo/done" chips that add
   no value). If a control is not needed for the core loop, it is hidden in
   Settings.
2. **Voice is the input.** Hold-to-talk is the largest, most prominent control
   on the conversation screen. Typing is secondary: it appears only when the
   user taps the text field.
3. **State is visible.** A green/red status dot always shows where voice is
   processed (on-device = green, cloud = red). The machine name always matches
   the actually connected host — never show a red dot while a green connection
   is live.
4. **Instant feedback.** Pressing hold-to-talk must react in ≤ 100 ms (color +
   waveform appear immediately; recognition may follow). The overlay is
   frosted-glass translucent, never a solid opaque cover.
5. **Honesty about data.** Auto-named sessions, explicit local↔cloud switch,
   no hidden cloud calls. What the UI says is what the app does.

## 2. Color Tokens

Dark theme is the primary experience. Light mode is an adaptation.

| Token | Value | Use |
|---|---|---|
| `bg/base` | `#0b0f14` | App background (dark default) |
| `bg/surface` | `#131a22` | Cards, drawers, sheets |
| `bg/surface-2` | `#1b2530` | Raised elements, input bar |
| `bg/overlay` | `rgba(11,15,20,0.55)` | Frosted overlay behind hold-to-talk |
| `accent/cyan` | `#22d3ee` | Primary brand accent, active states, waveform idle |
| `accent/cyan-deep` | `#0891b2` | Gradient end, pressed states |
| `state/green` | `#10b981` | On-device / connected (machine dot, send-ok) |
| `state/red` | `#ef4444` | Cloud / routed-out / disconnected (machine dot, recording wave) |
| `text/primary` | `#f1f5f9` | Headings, primary labels |
| `text/secondary` | `#94a3b8` | Body, timestamps, hints |
| `text/disabled` | `#475569` | Disabled input, unavailable machine |
| `line/border` | `rgba(148,163,184,0.18)` | Hairlines, separators |
| `danger` | `#ef4444` | Errors, destructive actions |

Contrast: all interactive text ≥ WCAG AA (4.5:1) on its background; large text
≥ 3:1. Green/red states are never the only signal — always pair with a label
("On device" / "Cloud").

## 3. Typography

System font stack (SF Pro on iOS, Roboto on Android). No custom fonts.

| Role | Size | Weight | Line height | Use |
|---|---|---|---|---|
| Display | 28 | 700 | 34 | Empty-state headline, onboarding |
| Title | 22 | 600 | 28 | Screen titles, session name |
| Subtitle | 17 | 600 | 22 | Section headers, machine name |
| Body | 16 | 400 | 22 | Messages, descriptions |
| Caption | 13 | 400 | 18 | Timestamps, hints, status labels |
| Button/Label | 16 | 600 | 20 | Hold-to-talk, primary actions |

Numbers, durations and waveform labels use tabular figures.

## 4. Spacing & Layout

- **Grid:** 4 pt base. Use 4 / 8 / 12 / 16 / 20 / 24 / 32 / 40.
- **Screen margin:** 16 pt horizontal; 8 pt vertical rhythm.
- **Touch targets:** ≥ 44×44 pt (48 dp Android) for every interactive control.
- **Safe areas:** respect notch / home-indicator insets on both platforms.

Layout zones (portrait phone):

```
┌──────────────────────────────┐
│ Top bar     (48 pt)         │  machine/cloud name + status dot · drawer · new
├──────────────────────────────┤
│ Conversation (flex)         │  message bubbles, date/time dividers
├──────────────────────────────┤
│ Input zone  (≥ 96 pt)       │  [ Hold to talk ]  ·  keyboard toggle
└──────────────────────────────┘
```

## 5. Corner Radii

| Surface | Radius |
|---|---|
| Cards / drawer items | 12 |
| Message bubbles | 14 (16 on the sender side) |
| Machine picker sheet | 16 |
| Primary button (hold-to-talk) | Full (pill) |
| Input bar | 16 |

## 6. Components

### 6.1 Top bar
- Left: drawer (history) entry — opens the session drawer (left-side panel,
  split view, not a modal popup).
- Center: **machine / cloud name** with status dot (green=on-device, red=cloud
  or disconnected). Tapping the name opens the machine picker.
- Right: **New session** only. Settings live in the drawer's bottom-left.
- No connection indicator text beyond the dot + machine name.

### 6.2 Session drawer
- Left-side panel, full-height, overlaid as a split view (content visible
  beside it when open).
- Sections: **Roles** and **Domains** (fold/unfold by single tap; folder-like
  hierarchy). Unclassified/new sessions appear at top until assigned.
- Long-press (double-tap) on a Role/Domain → **Internalize** (archive: hides
  from the drawer; accessible later under Settings → History).
- Bottom-left: Settings entry. Bottom-right area: machine picker shortcut.
- Auto-named sessions: the system derives a short title from the first turns
  (no "New chat 47").

### 6.3 Conversation
- User voice message: bubble with waveform thumbnail + duration + time.
- User text message: plain bubble.
- Assistant reply: bubble with text; citations/sources when applicable.
- Empty state: centered hint ("Start a conversation") + big hold-to-talk.
- No "recording/calibrating" progress text; the waveform IS the indicator.

### 6.4 Input zone (hold-to-talk)
- **Primary control:** a large pill button, centered, full-width (min 240×64),
  icon = waveform, label "Hold to talk". The largest element on screen.
- **Pressed state:** an immediate (< 100 ms) frosted translucent overlay rises
  from the bottom (bg/overlay), a **red waveform animation** plays, and the
  hint "Release to send · Slide up to cancel" appears. The overlay is
  translucent frosted glass — never solid white or solid black; about half the
  previous height; the waveform fills the space without an arc.
- **Release:** sends. **Slide up:** cancels. No intermediate "recording…"
  labels.
- **Text toggle:** keyboard icon next to the hold button; tapping it swaps to
  the text input with the hold button hidden (typing mode is secondary).

### 6.5 Machine / cloud switch
- Picker lists: **VoxSign Cloud** (default, green when connected) + self-hosted
  machines (with status dot per machine).
- Selecting a machine is the ONLY thing that changes the name + dot shown in
  the top bar. A green default (Cloud) must never silently display as red, and
  vice-versa.
- **Red machine (unavailable/disconnected): input is blocked.** Tapping the
  input area shows a prompt: "Machine offline — pick another machine", then
  opens the picker. No input is accepted on a red host.
- Self-hosted machines are user-configured in Settings; Cloud is always the
  default host.

### 6.6 Dialogs & sheets
- Machine picker, role/domain create, internalize confirm: bottom sheet
  (radius 16), one clear primary action.
- Confirmations only when irreversible or external (archive, delete).

### 6.7 States
| State | Visual |
|---|---|
| Empty | hint + primary hold button |
| Loading | subtle progress on the waveform / top bar only |
| Error | inline message + retry where meaningful |
| Machine red | blocked input + prompt to switch machine |
| Recording | red waveform overlay, translucent |

## 7. Motion

- Press feedback: 60–100 ms, instant color/scale (waveform appears without
  waiting for ASR).
- Waveform animation: continuous gentle oscillation; smooth, not jittery.
- Page/sheet transitions: 200–250 ms ease-out; drawer: 220 ms.
- No decorative loops, no infinite spinners except genuine loading.

## 8. Accessibility

- All targets ≥ 44 pt; VoiceOver/TalkBack labels on every control (hold-to-talk
  announces "Hold to record").
- Color is never the only signal (dot + label).
- RTL support (Arabic): layout mirrors; waveform and press hints flip.
- Reduced motion: respect system setting — static waveform fallback.

## 9. Localization & RTL (en / zh-Hans / ar)

VoxSign ships **three languages**: English (default), Simplified Chinese, and
Arabic. **Arabic is a strategic language** — the GCC region is a primary market
and Arabic must feel native, not translated.

### 9.1 Language policy
- Languages: `en` (default) · `zh-Hans` · `ar`. App language follows the system
  setting by default, with an in-app override in Settings.
- The brand name **VoxSign is never translated**; product terms follow a frozen
  glossary (see 9.6).
- Harness templates, iOS UI strings, Android strings, and the ASR language
  layer must all resolve the same three languages.

### 9.2 Arabic (ar) — first-class support
- **Full RTL:** every screen mirrors — top bar order, drawer side, message
  alignment, hold-to-talk slide-up-cancel direction, waveform animation
  direction, progress direction, chevrons, and time/date layout.
- **Type:** use the platform Arabic face (SF Pro Arabic / Noto Naskh Arabic /
  Roboto Arabic). Line height +20% for Arabic script; text must be elastic —
  Arabic strings commonly run ~30% longer than English.
- **Numerals:** Western digits by default (common in GCC products); an
  Arabic-Indic digit option may be enabled per user locale. Never mix both in
  one screen.
- **Dates & times:** localize format per locale; support Hijri calendar display
  as an opt-in.
- **Voice:** Arabic ASR is a first-class target — MSA plus Gulf-dialect
  recognition, with the same hold-to-talk flow. ASR language follows the
  session language, not the device locale.
- **Touch & accessibility:** all RTL affordances stay ≥ 44 pt and announce in
  Arabic via TalkBack/VoiceOver.

### 9.3 RTL mirroring rules
When locale is `ar`, mirror the following (no exceptions):
- Top bar: drawer entry on the right, new-session on the left.
- Drawer slides from the right; folder chevrons point left.
- Message bubbles: user messages right-aligned, assistant left-aligned.
- Hold-to-talk: "slide up to cancel" mirrors to slide-down (gesture text is
  localized); waveform fills from the leading edge.
- Loading/progress bars and spinners keep physical direction but flip
  horizontal progress.

### 9.4 String & layout rules
- All strings come from a language resource layer — no hard-coded UI text.
- Test each screen at +30% text length (Arabic overflow is a layout bug).
- Numbers, durations ("1s"), and waveform labels use locale-aware figures.

### 9.5 ASR & Harness
- Harness i18n adds `ar` alongside `en`/`zh`: language detection must recognize
  Arabic script (U+0600–U+06FF) and pin `ar` when Options.Lang = "ar".
- ASR layer: Arabic models/dictionaries are a supported profile; the hold-to-talk
  protocol is language-agnostic.

### 9.6 Glossary (frozen, trilingual)
| EN | zh-Hans | ar |
|---|---|---|
| Hold to talk | 按住说话 | اضغط وتحدث |
| On device / Cloud | 本机 / 云端 | على الجهاز / السحابة |
| New session | 新会话 | جلسة جديدة |
| Roles / Domains | 角色 / 域 | الأدوار / النطاقات |
| Internalize | 内化 | أرشفة |
| Machine offline | 机器离线 | الجهاز غير متصل |

## 10. Platform Notes

- **iOS:** follow HIG; SwiftUI; haptic on press (medium), on send (light);
  ship English + Simplified Chinese + Arabic with full RTL.
- **Android:** follow Material guidance where this spec is silent; Compose;
  TalkBack; ship the same three languages and RTL mirroring; keep feature
  parity with iOS.
- Both platforms must stay in sync with this document; any deviation requires
  a spec update first.

## 11. Design Tokens (JSON)

Tokens below are the single source of truth for colors, spacing, type and
radii. Apps should consume these values directly.

```json
{
  "color": {
    "bgBase": "#0b0f14", "bgSurface": "#131a22", "bgSurface2": "#1b2530",
    "overlay": "rgba(11,15,20,0.55)",
    "accentCyan": "#22d3ee", "accentCyanDeep": "#0891b2",
    "stateGreen": "#10b981", "stateRed": "#ef4444",
    "textPrimary": "#f1f5f9", "textSecondary": "#94a3b8", "textDisabled": "#475569",
    "border": "rgba(148,163,184,0.18)", "danger": "#ef4444"
  },
  "spacing": [4, 8, 12, 16, 20, 24, 32, 40],
  "radius": { "card": 12, "bubble": 14, "bubbleSender": 16, "sheet": 16, "inputBar": 16, "button": 999 },
  "type": {
    "display": [28, 700, 34], "title": [22, 600, 28], "subtitle": [17, 600, 22],
    "body": [16, 400, 22], "caption": [13, 400, 18], "button": [16, 600, 20]
  },
  "touchTarget": 44,
  "motion": { "pressMs": 80, "sheetMs": 250, "drawerMs": 220 }
}
```

---

## Appendix: Deviations Are Bugs

Any mobile screen that contradicts this document (wrong color token, tiny
target, opaque overlay, missing red-dot blocking, name/dot mismatch) is a bug
and must be fixed before release. Update this document first when a deliberate
change is needed.
