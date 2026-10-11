# Pathly component notes (v0.2)

**v0.2 changes:** product renamed **Pathly**; **light theme only** (dark mode removed); visual refresh inspired by a soft dashboard reference: cool blue-grey canvas, borderless white cards with soft shadows (radius 20px), pill buttons, icon chips, KPI card, curve chart, table card, side navigation. Violet stays the brand color; a dark "ink" pill button (`.btn--dark`) is used for the single hero action in a card. The Figma file still shows v0.1 (see DESIGN-SYSTEM-BRIEF.md).

Figma (v0.1, behind the code): private working file. Tokens: [tokens.css](tokens.css).
Rules for all components: use semantic tokens only (`--color-*`, `--space-*`, `--radius-*`, `--density-*`), logical CSS properties for RTL-readiness, WCAG 2.2 AA, visible `:focus-visible` using `--shadow-focus-ring`.

Verification status: Status Pill, Deadline Marker and Form Field verified visually in Figma. **Button: fixes applied but not yet visually re-verified** (Secondary fill binding, focus ring rendering).

---

## 1. Status Pill
**Purpose.** Shows the state of a document, request, application or payment.
**Use when** an item has one of the nine states. **Don't use** for deadlines (use Deadline Marker) or for decorative tags.
**Anatomy.** Icon (16px, 14px compact) + label + tinted background + 1px border. Fully rounded.
**Variants.** State (9): Needed, Received, In review, Needs fixing, Complete, Expired, Waiting on third party, Locked, Not applicable. Density (2): Comfortable, Compact. Not applicable uses a dashed border.
**Tokens.** `--color-status-{state}-bg|text|border`; padding `--space-3` (comfortable) / `--space-2` (compact) horizontal, `--space-1` vertical; `--radius-full`.
**Accessibility.**
- Meaning is carried by icon + text label + color. Never color alone.
- Text contrast on tint is 5.3:1 to 7.5:1.
- Icons are decorative (`aria-hidden="true"`); the label is the accessible name.
**Content.** Use the state names exactly as written. Do not abbreviate.
**CSS sketch.**
```css
.status-pill {
  display: inline-flex; align-items: center; gap: var(--space-1);
  padding-block: var(--space-1); padding-inline: var(--space-3);
  border-radius: var(--radius-full);
  border: var(--border-width-default) solid var(--pill-border);
  background: var(--pill-bg); color: var(--pill-text);
}
.status-pill[data-state="needed"] {
  --pill-bg: var(--color-status-needed-bg);
  --pill-text: var(--color-status-needed-text);
  --pill-border: var(--color-status-needed-border);
}
```

## 2. Deadline Marker
**Purpose.** Shows when something is due, separately from its state.
**Use when** an item has a due date. **Don't use** on Complete, Locked, Not applicable or Waiting on third party.
**Variants.** Timing (4): Upcoming (calendar icon, secondary text), Due soon, Due today (clock icon, amber text), Overdue (solid red pill, white text). Density (2). Label is a text property.
**Rules.**
- Overdue is the **only** solid red element in the portal.
- Overdue sits next to the state pill; it never replaces the state.
- Show absolute date + weekday + relative time: "Fri 12 Oct, in 3 days". Clients may think in the Jalali calendar, so dates must be unambiguous.
**Accessibility.** White on `--color-status-overdue-bg` is 5.6:1 (light). Text label always present.

## 3. Button
**Purpose.** Triggers an action.
**Variants.** Style (4): Primary, Secondary, Ghost, Danger. State (4): Default, Hover, Focus, Disabled. Density (2): Comfortable (48px high, client), Compact (36px, staff).
**Use.**
- **Primary**: one per screen: the main next step ("Upload document").
- **Secondary**: supporting actions ("Choose file"). Tinted fill + brand border so it is visible against the page.
- **Ghost**: low emphasis ("Cancel", "See sample").
- **Danger**: staff console only: reject, delete. Never as a client-facing primary action.
**Tokens.** Height `--density-control-height`; horizontal padding `--density-control-padding-x`; `--radius-md`; text styles `text-button-lg` / `text-button-md`. Hover: `--color-bg-brand-hover`, `--color-bg-brand-subtle-hover`, `--color-status-overdue-border`. Disabled: `--color-bg-disabled` + `--color-text-disabled`.
**Accessibility.**
- Touch target at least 44px on the client portal.
- Focus: `box-shadow: var(--shadow-focus-ring)` on `:focus-visible`.
- Disabled buttons are exempt from contrast rules but should be avoided: prefer enabled buttons with a clear error message.
- Use `<button>` for actions and `<a>` for navigation.
**Content.** Say exactly what happens: "Upload document", not "Submit". Sentence case. No more than four words.

## 4. Form Field
**Purpose.** Collects a single value with a visible label, optional help text and an error message.
**Variants.** State (5): Default, Focus, Filled, Error, Disabled. Density (2). Label is a text property; value and message texts are edited per instance.
**Rules.**
- The label is always visible. Never use a placeholder as the label.
- Identity fields use "Full name (as on passport)". Spelling must match the passport exactly. Show the passport spelling as the canonical name elsewhere in the portal.
- Errors say what is wrong **and** how to fix it: "Enter your name exactly as it appears on your passport."
**Tokens.** Border `--color-border-input` (1px, 5.5:1 against white); Focus `--color-border-focus` 2px; Error `--color-status-needs-fixing-text` 2px with a warning icon; Disabled `--color-bg-disabled`.
**Accessibility.**
- Link the label with `for`/`id`; link help/error text with `aria-describedby`; set `aria-invalid="true"` on error.
- An error summary at the top of the form should link to each failed field (GOV.UK pattern).
- Never rely on red alone: the error state also shows the icon, the message and a thicker border.

---

## 5. Built in code (see index.html and screens/)
Request card, document upload slot (camera-first, resumable, file spec visible), stage tracker, banner, timeline, message thread, empty state, staff review panel with rejection reasons.

## 6. New in v0.2

**Page shell.** Top bar card (logo, notifications, account) + side navigation card + content. On phones the side navigation becomes a bottom bar. Active item: grey pill, bold label. Staff console uses the Compact density.

**KPI card + icon chip.** One white card with up to four figures. Each has a 44px tinted chip (mint, amber, blue, rose, teal). The tint never carries meaning alone: the label always does. On phones it becomes a single column.

**Side card, meters, help card.** One headline figure, an illustration, two thin meters and one dark action. Avoid currency amounts until the payment channels are confirmed: use percentages.

**Path chart.** Smooth curve with one highlighted point and a dark tooltip. Client: case journey (done = brand violet, next = dotted). Staff: weekly volume. Always provide `<title>` and `<desc>`; the chart scrolls sideways on narrow screens instead of shrinking its text; narrow client screens show the progress summary instead.

**Table card, tabs, switch.** Title, tabs (ARIA tablist with arrow keys), search, switch (`role="switch"`), then a data table inside a horizontal scroll container. Row actions carry a hidden name ("Review Bank statement for Sara A."). Selecting a row opens the detail below and moves focus to its heading.

**Illustrations.** Four line illustrations in `art.svg` (trip, help, review, empty). Outlines use the text color, fills use brand tints. Decorative: `aria-hidden="true"`.

**Dark button.** `.btn--dark` (ink pill with arrow). One per card, for the main forward action ("See payment details", "Open call list", "Send for review").
