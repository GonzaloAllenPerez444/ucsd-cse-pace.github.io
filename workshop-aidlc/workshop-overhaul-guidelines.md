# Workshop Overhaul Guidelines

General rules and takeaways distilled from the Workshop 1 (Task Manager) overhaul, meant to guide the same treatment of the remaining workshops:

- `job-application-cdk.html` (Workshop 2)
- `s3-file-sharing.html` (Workshop 3)
- `ecommerce-catalog.html` (Workshop 4)

All four workshops share the same structure (a hidden "Using AI-DLC Workflow" tab and a visible "Copy-Paste Prompts" tab) and the same CSS/JS helpers, so every rule below transfers directly. Unless noted otherwise, **edit the visible Copy-Paste Prompts tab (`#tab-prompts`) only** and leave the hidden workflow tab alone.

---

## Guiding philosophy

The audience is learners who are new to building software with AI agents, and not necessarily strong programmers. The workshop should teach them to **drive an AI agent in plain language and critically review what it produces**, not to memorize syntax. Two themes run through every rule:

1. **Lower the technical bar to participate.** Learners describe intent in plain English; the AI handles the technical translation.
2. **Raise the bar on critical review.** Learners are pushed to open artifacts, search them, and judge quality, rather than rubber-stamping output.

---

## The rules

### 1. Make the fill-in-the-blank mechanic obvious and visible

- **Keep the yellow highlight on the blank, even after the learner types.** The highlight is how a learner sees exactly what got inserted into the prompt. Do not strip it on input. Use the `todo-placeholder` class (dashed, italic) for an empty blank and a `todo-filled` class (solid border, normal style) once filled.
- **Tie the input box to the blank in one short, inline note.** Put a light parenthetical on the input's label, for example: `What should the app store? (this fills the highlighted blank above)`. Do not use a separate verbose helper paragraph.
- **Why:** learners otherwise do not realize their typing is being merged into the prompt, or they lose track of it once the dashed box disappears.
- **How to apply:** confirm `updateTodoPlaceholder()` keeps the span highlighted (adds `todo-filled`, not just removes `todo-placeholder`). Add the `.todo-filled` CSS rule if the workshop file does not have it yet (none of Workshops 2-4 do). Inline every "what you type here..." helper into its label.

### 2. Explain each concept in plain language, right where it is first used

- Add a short `note-box` that defines the key concept of a step before the learner has to act on it (Workshop 1 added definitions for "user story" and "API route").
- Keep definitions to one or two sentences, jargon-free, with a concrete example tied to that workshop's project.
- **Why:** non-expert learners hit terms like "API route," "IAM policy," "CDK stack," or "catalog schema" cold; a one-line plain definition keeps them moving.
- **How to apply:** for each workshop, list the terms a beginner would not know (e.g. Workshop 2: CDK, stack, deploy; Workshop 3: S3 bucket, presigned URL, IAM; Workshop 4: schema, pagination, seed data) and add a `note-box` at the step that introduces each.

### 3. Write prompts and questions for non-experts; let the AI handle the technical translation

- Ask learners to **describe what they want in plain English**, not to write technical syntax. Workshop 1 replaced "write `DELETE /tasks/<id>`" with "describe, in plain words, what you want to be able to do," and told the AI to choose and implement the route.
- Update example text and placeholders to match (a plain-English example, not a code snippet).
- Inside the copied prompt, instruct the AI to make the technical decisions ("decide the appropriate route/resource/command yourself and implement it").
- **Why:** writing correct technical syntax is exactly the skill these learners lack and the agent already has.
- **How to apply:** scan each workshop's fill-in questions for ones that ask for syntax (HTTP verbs, ARNs, SQL, CLI flags, CDK constructs) and rephrase them as plain-language intent, pushing the implementation detail into the prompt for the AI.

### 4. Make review questions actionable, and place them at the right step

- **Actionable, not yes/no.** Replace passive "Ask yourself: is X correct?" prompts with tasks that force the learner into the generated artifact: "Open `<file>`, find the `<thing>`, and check what it does when `<edge case>`." Give two or three concrete "Try this" bullets.
- **Aligned to the producing step.** A question about an artifact belongs at the step that **creates** it, or just before the step that **consumes** it, never floating a step away. In Workshop 1 the user-story review was moved out of the units step and back to the user-story step; the units step got its own review about its own output.
- **Why:** yes/no questions get rubber-stamped; misplaced questions ask learners to evaluate something that does not exist yet (or that they already moved past).
- **How to apply:** for each workshop, walk the activities in order and confirm every `think-box` review references only artifacts that exist at that point and belong to that step's output. Convert any "does it look right?" phrasing into a "go find X and check Y" task.

### 5. Accommodate different environments instead of assuming one

- Where a step depends on a tool the learner may not have (Docker was the Workshop 1 case), provide **two paths via a toggle**, defaulting to the common one. Keep the same deliverable artifacts in both paths; only drop the tool-specific pieces.
- Soften the related prerequisite from "required" to "optional, an alternative path is provided."
- **Why:** a hard dependency blocks any learner who cannot install it, and a workshop should not stall on setup.
- **How to apply:** identify each "you must have X installed" assumption per workshop (Docker, an AWS account, a specific runtime). For each, decide whether a no-tool path is feasible and add a toggle (reuse the `btn-check` button-group + a small `toggle...()` JS helper, as in Workshop 1's Activity 7). For AWS-dependent workshops, be explicit about what genuinely requires an account versus what can run locally or be simulated.

### 6. Have the AI generate tests; do not hardcode them

- Replace fixed, hardcoded test command blocks (curl strings, exact paths, ports) with a **copy-paste prompt that asks the AI to generate a manual test plan for the app it actually built**, covering both UI walkthrough steps and command-line checks, based on the real routes/resources it created.
- Tell the learner what to do if a test fails (paste the error back, ask the AI to fix and re-test).
- **Why:** hardcoded commands drift from what the AI generated (different names, ports, routes, and any feature the learner added), and they teach copying over understanding.
- **How to apply:** find every "Test Your Application" / hardcoded `curl` or CLI block in each workshop and convert it to a generate-the-tests prompt.

### 7. Frame honestly, and keep terminology generic

- State plainly that **this prompt-by-prompt sequence is one way to use AI agents for programming, not the only way.** Mention alternatives generically (free-form conversation, spec-driven tools, more autonomous workflows).
- **Avoid product names where a general concept will do.** Workshop 1 dropped a specific product reference ("AI-DLC Workflow") from a closing note in favor of "more autonomous workflows." Keep generic terminology in learner-facing notes unless the named product is genuinely the subject of that tab.
- Use the phrasing "In this workflow, this is the last step" for the final step, signaling that other workflows exist.
- **Why:** learners should leave understanding the landscape, not believing one tool or sequence is the only correct method.
- **How to apply:** add an equivalent closing `note-box` to each workshop's final activity, and grep each file for product names in learner-facing prose, generalizing where the name is not essential.

---

## Reusable components (already present in every workshop file)

Reuse these rather than inventing new markup, so the four workshops stay consistent:

| Purpose | Class / helper |
| --- | --- |
| Plain-language concept definition / info callout | `.note-box` (blue, left border) |
| Tip or review task ("Try this") | `.think-box` (yellow, left border) |
| Approval-gate reminder | `.gate-box` (green, left border) |
| Fill-in blank inside a prompt, empty | `.todo-placeholder` (dashed yellow) |
| Fill-in blank inside a prompt, filled | `.todo-filled` (solid yellow) **- add this rule; not yet in Workshops 2-4** |
| Copyable prompt with copy button | `.prompt-box` + `copyPrompt(id, event)` |
| Live mirror of input into a blank | `updateTodoPlaceholder(spanId, value, defaultText)` **- ensure it keeps the highlight** |
| Inline label hint | `<span class="text-muted fw-normal">(...)</span>` |
| Environment toggle | Bootstrap `btn-check` button group + a small `toggle...()` JS helper |

When copying the `.todo-filled` behavior into another workshop, update both the CSS (`<style>` block) and the `updateTodoPlaceholder()` function so it toggles `todo-placeholder` off and `todo-filled` on (instead of just removing the highlight).

---

## Per-workshop overhaul checklist

For each of `job-application-cdk.html`, `s3-file-sharing.html`, and `ecommerce-catalog.html`:

- [ ] Fix `updateTodoPlaceholder()` + add `.todo-filled` so filled blanks stay highlighted (Rule 1).
- [ ] Inline every "what you type here..." helper into its label (Rule 1).
- [ ] Add plain-language `note-box` definitions for each beginner-unfamiliar term, at first use (Rule 2).
- [ ] Rephrase any syntax-writing question into plain-English intent; push implementation into the prompt (Rule 3).
- [ ] Convert yes/no review prompts into actionable "open the file and find X" tasks (Rule 4).
- [ ] Verify each review question sits at the step that produces or directly consumes its artifact (Rule 4).
- [ ] Add a toggle for any hard tool/environment dependency, keeping artifacts in both paths; soften the prerequisite (Rule 5).
- [ ] Replace hardcoded test command blocks with a generate-the-tests prompt (Rule 6).
- [ ] Add a closing "this is one way, not the only way" note and generalize product names (Rule 7).
- [ ] Open the page in a browser: tabs render, copy buttons work, toggles switch, blanks mirror and stay highlighted, no console errors.

> Note: each workshop also has its own domain-specific quirks (AWS accounts and credentials for the CDK and S3 workshops, data seeding for the catalog workshop). Apply the rules above with judgment, and capture any workshop-specific decisions in that workshop's own issues file, mirroring `workshop1_issues.md`.
