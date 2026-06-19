---
name: proto-to-prod
description: >
  Use when taking a project's user interface from a rough prototype to a
  production-ready state, especially when the person directing you is
  non-technical. Covers auditing the prototype, prioritizing by user impact,
  adding the missing states (loading / error / empty), accessibility,
  responsiveness, visual polish, performance, security hardening, and shipping
  safely — while communicating in plain language and never breaking the working
  app.
---

# Taking a UI from Prototype to Production

## What this skill is for

A prototype proves an idea works. Production means real people can use it every
day without hitting walls. The gap between the two is mostly invisible work:
what happens while data loads, what happens when something fails, what happens
on a phone, what happens when a person can't use a mouse, what happens when a
hacker pokes at it.

Your job is to close that gap **autonomously**. The person directing you cannot
read code, cannot run commands, and cannot tell you which technical option to
pick. Treat that as the core constraint, not a footnote.

## The two rules that override everything else

1. **Never leave the app broken.** At every step the prototype must still run.
   Work in small increments, verify after each one, and if a change breaks
   something, fix it or revert it before moving on. The operator is trusting you
   with something that currently works.

2. **The operator never touches code or a terminal.** You run every command,
   read every error, and make every technical decision yourself. When you truly
   need them, you ask a *product* question in plain words, never a technical
   one.

If you ever feel the urge to write "run this command" or "open this file and
change line 12" to the operator — stop. Do it yourself instead.

---

## Working with a non-technical operator

This shapes how you communicate at every step.

**Translate everything into outcomes they care about.** Report what changed for
the *user of the app*, not what you did to the code.

- Don't say: "Added responsive breakpoints at 640/768/1024 and refactored the
  flex container."
- Do say: "The app now works properly on phones — nothing gets cut off or
  overlaps anymore."

**Make the decisions they can't make.** If a choice is purely technical (which
library, how to structure a component, which animation approach), just pick a
sensible, widely-used default and move on. Don't surface it. They hired you to
have that judgment.

**Only ask when it's genuinely a product or experience decision** — something
about what the app *should do* that you can't infer. When you do ask:

- Frame it from the user's side of the screen.
  Bad: "Should I gate the route behind auth middleware?"
  Good: "When someone isn't signed in and tries to open their dashboard,
  should we send them to a sign-in page, or show a limited preview? I'd suggest
  the sign-in page — it's the standard and least confusing. Want me to go with
  that?"
- Always include a recommended default so they can just say "yes."
- Batch questions together instead of interrupting repeatedly.

**Flag anything involving money, security, or privacy — every time.** The
operator can't catch these. Call out clearly, in plain language:

- Anything that will cost money (paid services, API usage, hosting tiers).
- Anything that collects or stores personal data.
- Anything that needs an account, an API key, or a credit card.

**When they must do a manual step, give exact click-by-click instructions.**
Some things you can't do for them — creating an account on a hosting service,
generating an API key, approving a payment. For these, write numbered steps a
total beginner could follow, including what the screen will look like and what
to copy back to you. Never assume prior knowledge.

---

## The workflow

Move through these phases in order. Don't skip the audit to start fixing things
— you'll fix the loud problems and miss the dangerous quiet ones.

### Phase 0 — Get a baseline and define "done"

1. Get the prototype running yourself and confirm it works. Take note of the
   current state so you can always return to a working version.
2. Make sure the project is under version control (e.g. git). If it isn't, set
   it up and make an initial commit — this is your safety net. Commit after
   every meaningful, verified change from here on.
3. In one short message, confirm with the operator what the app is *for* and who
   uses it, and roughly where they want it to end up ("just shareable with a few
   people" vs "real public launch"). This sets the bar. Don't over-interview —
   one or two plain questions.

### Phase 1 — Audit (look, don't change)

Walk the whole app and catalog the gaps using the checklist in the next
section. Don't fix anything yet. Produce a short plain-language summary for the
operator: what's solid, what's missing, and what's risky. Group it by how much
it affects the user, not by technical category.

### Phase 2 — Prioritize and get a go-ahead

Order the work by **user-visible impact and risk first**:

1. Things that are broken or unsafe (security holes, crashes, lost data).
2. Things every user hits (missing loading/error states, broken on mobile,
   confusing flows).
3. Polish (visual consistency, animations, refinement).
4. Nice-to-haves.

Present the plan in plain words with a rough sense of order. Get a quick
go-ahead, then execute. You don't need permission for each small step — just for
the overall direction and for anything that costs money or affects privacy.

### Phase 3 — Execute in small, safe increments

One concern at a time. After each change: verify it works, verify you didn't
break anything else, commit. Keep the app runnable throughout. Give the operator
brief progress updates framed as outcomes, not a play-by-play of code.

### Phase 4 — Harden for production

Security, performance, error resilience, and build configuration. These are the
things that don't show up in a quick click-through but matter the moment real
people and real load arrive.

### Phase 5 — Verify and ship

Do a full quality pass (see "Verifying your own work"), then handle deployment.
Finish with a short plain-language handoff: what's live, how to reach it, what
they should know, and anything that will need attention later.

---

## The production audit checklist

This is the substance of the work. Prototypes almost always pass the "happy
path" and fail everything else. Go category by category.

### Completeness — no dead ends or leftovers

- Every button and link does something real (no buttons that do nothing).
- No placeholder text left visible ("Lorem ipsum", "TODO", "Coming soon" that
  was never meant to ship).
- Navigation works in every direction, including the browser back button.
- Nothing references a feature that doesn't exist yet without explaining it.

### Real data instead of fake data

- Hardcoded or mock data is replaced with the real source.
- Long lists handle large amounts of data (pagination or lazy loading), not just
  the three sample items.
- The app handles data that doesn't exist yet (a brand-new user with nothing).

### The three states prototypes always forget

This is the single most common prototype-to-production gap. The prototype shows
data that's already loaded. Real apps spend a lot of time *not* in that state.

- **Loading:** show a spinner or skeleton placeholder while data is being
  fetched, so the screen is never blank or frozen-looking.
- **Error:** when something fails, show a clear message in plain language plus a
  way to recover (a "try again" button). Never a blank screen or a raw error
  dump.
- **Empty:** when there's genuinely nothing to show, show a helpful empty state
  that tells the person what to do next, not just emptiness.

Test each one deliberately — don't assume they work because the happy path does.

### Forms and input

- Every input validates, with a clear message right next to it explaining what's
  wrong and how to fix it.
- The submit button disables while submitting, so people can't double-submit.
- If a submission fails, the person's typed input is preserved, not wiped.
- Sensible defaults are pre-filled where possible.
- The form can be submitted with the keyboard (Enter), not just by clicking.

### Works on every screen size

- Test phone, tablet, and desktop widths.
- No horizontal scrolling, no cut-off content, no overlapping elements.
- Tap targets (buttons, links) are big enough for a thumb.
- Text stays readable; images scale instead of breaking the layout.

### Accessibility — usable by everyone

This is a real production requirement, not optional polish. In many places it's
also a legal one.

- The whole app is usable with only a keyboard (Tab to move, Enter/Space to
  activate), and the focused element is always clearly visible.
- Images have descriptive alt text; decorative ones are marked as decorative.
- Every form input has a visible, associated label.
- Color contrast is strong enough to read (don't put light gray on white).
- No information is conveyed by color alone (e.g. don't mark errors only by
  turning something red — add an icon or text too).
- Animations respect the "reduce motion" setting for people who get motion sick.

### Visual consistency and polish

- Spacing, type sizes, and colors are consistent and intentional, not slightly
  different on every screen. Use a small shared set of values (tokens) rather
  than one-off numbers everywhere.
- Interactive elements have clear hover, active, and focus states.
- Things line up. Misalignment is what makes a prototype feel unfinished.
- Any animation is purposeful and quick. Too much animation reads as cheap;
  restraint reads as polished.
- **Copy counts.** Buttons say exactly what they do ("Save changes", not
  "Submit"). An action keeps the same name through the whole flow ("Publish"
  leads to "Published"). Error and empty screens give direction in the app's
  voice, not vague apologies.

### Performance

- The app loads fast on a normal connection — the first screen shouldn't make
  people wait.
- Images are appropriately sized and compressed, and load lazily when off-screen.
- Expensive actions (search-as-you-type, etc.) are debounced so they don't fire
  on every keystroke.
- Large bundles are split so the first load isn't dragging in everything at once.

### Security and privacy

These are the ones the operator absolutely cannot catch, so you must.

- **No secrets in the code that ships to the browser.** API keys, passwords, and
  tokens go in environment variables / server-side config, never hardcoded in
  front-end code where anyone can read them.
- Protected pages actually check that the person is allowed in — don't rely on
  just hiding a link.
- User input is treated as untrusted (sanitized) so it can't be used to inject
  malicious content.
- No sensitive personal data sits in places it shouldn't (browser local storage,
  logs, URLs).
- The app is served over HTTPS in production.
- Dependencies are reasonably up to date and free of known critical
  vulnerabilities.

### Robustness — it doesn't fall over

- One broken component shouldn't take down the whole app. Add error boundaries so
  a failure in one area shows a localized message instead of a white screen.
- Slow or failed network requests are handled gracefully (timeouts, retries
  where sensible).
- The app survives unexpected or malformed data without crashing.
- There's a real "page not found" (404) page for bad URLs.

### Build and deploy readiness

- The production build completes successfully with no errors.
- No console errors or warnings in normal use.
- Production configuration is separate from development (correct URLs, keys,
  settings for the live environment).
- The basics are set: page title, favicon, and social/meta tags so links look
  right when shared.
- Consider lightweight error monitoring so you find out when real users hit
  problems.

### Code health (for whoever maintains it next)

The operator won't read the code, but a future agent or developer will.

- Remove dead code, commented-out blocks, and leftover debug logging.
- Keep a consistent structure and naming so the project stays navigable.
- Leave brief comments only where the *why* isn't obvious from the code.

---

## Verifying your own work

You can't ask the operator to test for you — they wouldn't know what to look
for. Verify everything yourself before calling anything done:

- The production build runs clean, with no errors or warnings.
- Click through every main flow start to finish, as a real user would.
- **Deliberately trigger the unhappy paths:** force a loading state, simulate a
  failed request, view a screen with no data, submit a form with bad input.
- Resize down to a narrow phone width and check every screen.
- Navigate the whole app using only the keyboard.
- Open the browser console and confirm it's clean.

If you can take screenshots in your environment, do — looking at the rendered
result catches problems that reading code never will.

---

## Guardrails — things you never do

- Never delete or overwrite the operator's data or content without explicit,
  specific confirmation.
- Never commit secrets (API keys, passwords, tokens) to version control.
- Never deploy to production without confirming with the operator first.
- Never make a change that's hard to undo without explaining it in plain terms
  and getting a clear yes.
- Never report something as "done" that you haven't actually verified yourself.
- Never hand the operator a technical task you could do yourself.
- Never leave the app in a broken state at the end of a working session.

---

## Definition of production-ready

You're done when all of these are true:

- Every user-facing flow works, including loading, error, and empty states.
- The app works and looks right on phone, tablet, and desktop.
- It's keyboard-usable, readable, and meets basic accessibility expectations.
- No secrets are exposed; protected areas are actually protected.
- It loads quickly and survives bad data and failed requests without crashing.
- The production build is clean and deployed (or ready to deploy) with proper
  configuration.
- You've handed the operator a short, plain-language summary of what's live and
  what to watch.
