# Copilot instructions

## Build, test, and run

There is no build, package manager, linter, or test runner. The whole app is the single self-contained `index.html` (inline CSS and JS, no external dependencies or network requests). Keep it that way — do not add a server, bundler, CDN links, or extra files.

- **Run:** open `file:///<repo>/index.html` directly in a browser (e.g. the integrated browser canvas).
- **Smoke test:** there is no test suite. Verify changes in the browser by evaluating JavaScript against the page: click `.option-btn` elements, dispatch `keydown` events on `document.activeElement`, then assert on `#score-badge`, `#feedback-banner`, `#progress-bar[aria-valuenow]`, `#progress-fill` width, and `#results-view`/`#results-score`/`#results-title`. To check a single behavior, run only that assertion after reloading the page.
- **Reduced motion:** to verify, inject the rules from the `@media (prefers-reduced-motion: reduce)` block into a temporary `<style>` and check that `getComputedStyle(el).animationName === 'none'`.

## Architecture

`index.html` has three parts that must stay in sync:

1. **CSS (`<style>`)** — all colors are CSS custom properties defined in `:root` and overridden in `@media (prefers-color-scheme: dark)`. Components only reference variables, never raw theme colors.
2. **Markup** — a single `main.quiz-container` with a header (mission-control title, score badge, progress bar), `#question-view`, and `#results-view`. The two views are toggled by JS (`style.display` on the question view, `.active` class on the results view).
3. **Script** — a `QUESTIONS` array plus module-level state (`currentIndex`, `score`, `answered`). Flow: `renderQuestion()` → `selectOption(i)` → `handleNext()` → `showResults()` → `restartQuiz()`. A single document-level `keydown` listener handles keys `1`–`4`, `ArrowUp`/`ArrowDown` (wrapping focus between options), and `Enter`/`Space` to advance.

## Conventions

- **Question shape:** `{ question, options: [4 strings], answer: <0-based index>, fact }`. Keyboard shortcuts assume exactly four options. `fact` is shown in the feedback banner for both correct and wrong answers.
- **Question count:** JS uses `QUESTIONS.length`, but the initial HTML hard-codes `10` (`Score: 0 / 10`, `aria-valuemax="10"`, `Question 1 of 10`). Update those if the count changes.
- **Result tiers** live in `getReaction(score)` (10, ≥8, ≥5, else); each tier needs `emoji`, an accessible `label` (applied as `aria-label` on the `role="img"` emoji), `title`, and `message`.
- **Themes:** any new color must be added as a variable in both `:root` and the dark-mode `:root` override.
- **Motion:** every new animation or transition must be disabled for reduced-motion users — either add its selector to the `prefers-reduced-motion: reduce` block or gate it behind `prefers-reduced-motion: no-preference` (as the star background `body::before` does).
- **Accessibility:** answers are native `<button>` elements with `aria-label="Option N: …"`; decorative glyphs (key badges, ✓/✕, rocket, "Mission control" label) are `aria-hidden`. Focus is moved deliberately: first option on render, Next button after answering, Play Again on results. Score, feedback, and views use `aria-live="polite"`. Preserve these when changing UI.
- **Feedback classes:** `.correct` (green `pulseGreen`) and `.wrong` (red `shakeRed`) on option buttons and `#feedback-banner`; the correct option is always revealed after any answer and all options are disabled.
- **Styling:** sans-serif system stack, 15px body, narrow centered column (`max-width: 500px`).
- UI copy in the page is English.
