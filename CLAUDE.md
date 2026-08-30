# homeschool-board

Single-file learning board for the user's young kids (roughly 6–9), used on
phones. Live at https://homeschool-board.vercel.app/

## Working agreement

**Make the change and push it. Do not hand back a checklist of manual steps.**
The user is non-technical about git and tooling. Their only manual step should
be looking at the live site. Keep explanations short and outcome-focused.

## Architecture

Everything is in **`index.html`** — markup, CSS, and JS inline. No build step,
no dependencies, no other source files.

`git push origin main` auto-deploys to Vercel in ~30s. Verify with:

```bash
curl -s https://homeschool-board.vercel.app/ | grep -q "<some new symbol>"
```

## Three footguns that have already caused live bugs

1. **The whole script is wrapped in an IIFE** (`(function(){ "use strict"; … })()`).
   Inline `onclick="foo()"` handlers resolve against **global** scope, so any
   function called from markup must be exported at the bottom of the IIFE:
   ```js
   window.playVideo=playVideo; window.closeVideo=closeVideo; window.sayDevotionText=sayDevotionText;
   ```
   Forgetting this makes buttons silently do nothing. Prefer `addEventListener`
   in JS over new inline handlers.

2. **`$` vs `$$`.** `$` is `querySelector` (one node); `$$` is `querySelectorAll`
   (array). Calling `.forEach` on `$(...)` throws and kills the rest of the
   handler — this silenced every reading tile's `say()` call for a while.

3. **HTML attributes don't take backslash escapes.** `onclick="f('A Child\'s …')"`
   truncates the string. Use `&#39;`.

Cheap guard after editing — extract and syntax-check the script:

```bash
node -e "const fs=require('fs'),h=fs.readFileSync('index.html','utf8');fs.writeFileSync('app.js',h.match(/<script>([\s\S]*?)<\/script>/)[1])"
node --check app.js
```

## Adding a subject module

Follow the existing pattern (States, Healthy Me and Screen Smart are the newest
and cleanest examples):

1. `--<name>` + `--<name>-ink` CSS vars, a `body[data-subject="<name>"]` rule,
   and a `.tile.<name>` gradient.
2. A `.tile` button on the home screen with `data-open="<name>"`.
3. A `<div class="screen" id="screen-<name>">` with `.topnav`, a
   `[data-tabs]` group, and `.panel[data-panel]` blocks.
4. A `build<Name>()` called at the bottom of the IIFE.
5. For a quiz: a `<div class="card" id="quiz-<name>">`, a `<name>Makers()`
   returning `[{q, a, pool}]` factories, an entry in the `makerFns` map in
   `nextQuestion()`, and a `startQuiz("<name>")` line in the tab handler.

**Tab `data-tab` names must be globally unique** — the tab handler's
`startQuiz` dispatch matches on them across all tab groups.

Quiz `pool` must contain the answer. A pool of exactly 2 is fine and renders a
clean Yes/No question.

## Content voice

Read-aloud text is written to be spoken to a 6–9 year old: short sentences,
concrete, and it explains *why* rather than just instructing. The kids already
know the rules — this is reinforcement, so keep it warm, never scolding.

## Testing on this machine

Chrome's `--window-size` and the `resize_window` tool are **ignored** here
(display is at 125% scaling; viewport stays ~1521px). Don't trust a resize.

What works instead:
- Drive the live site with the Chrome tools and assert on DOM side effects.
  Readout elements update only if a handler ran to completion, which makes
  them a reliable proxy for "did `say()` get reached".
- Reason about the responsive CSS math directly (`.screen` padding 14/12px
  mobile, `.card` padding clamps to 18px at phone widths). `auto-fill`
  `minmax()` grids can only overflow if the min column exceeds the content
  box; `body` also has `overflow-x:hidden` as a backstop.

**Clean up test state.** `localStorage` keys `hb_travel` and `hb_day` are real
family data on the user's own browser — remove anything you write while testing.

## Facts the content depends on

The family lives in **Texas, just south of Austin**. Grandma lives with them.
Hot summers matter for the hydration content.
