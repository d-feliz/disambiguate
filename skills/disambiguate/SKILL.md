---
name: disambiguate
description: Interactively disambiguate an idea, request, or spec through one-at-a-time clarifying questions with options, until every ambiguous term and interpretation is pinned down, then restate the clarified idea. Use ONLY when the user explicitly invokes it — e.g. "/disambiguate", "disambiguate this", "let's disambiguate", or "run the disambiguation skill". Do NOT auto-trigger on requests that merely seem ambiguous.
---

# Disambiguate

Turn a vague or ambiguous idea into a precise, unambiguous statement by interviewing the user — one question at a time — and then restating the idea in its clarified form.

This skill exists because silently picking one interpretation of an ambiguous request is how work gets done wrong. When invoked, the standard for proceeding is **100% certainty**: if a term, scope, referent, or intent could plausibly mean more than one thing, that is a question to ask — never an assumption to make.

## The core loop

1. **Read the idea** the user wants disambiguated (it may be in the invoking message, earlier in the conversation, or in an attached file).
2. **Scan for ambiguity.** Build a private list of everything that isn't 100% certain. Sources of ambiguity include:
   - **Terms with multiple meanings** — domain jargon, overloaded words ("model", "client", "release"), acronyms with several expansions.
   - **Unclear referents** — "it", "the app", "that feature": which one?
   - **Unstated scope** — does "users" mean all users or a segment? Does "the search feature" include filters?
   - **Ambiguous intent** — is this a plan, a question, a request to build, a thought experiment?
   - **Implicit constraints** — platform, audience, timeframe, format, success criteria that could go multiple ways.
   - **Quantifiers and comparatives** — "fast", "most", "better": relative to what?
3. **Ask ONE question** — the highest-leverage ambiguity first (usually the one whose answer changes the meaning of everything else, or invalidates other questions). Each question must:
   - Name the ambiguous term or phrase explicitly, quoting the user's own words.
   - Offer the plausible interpretations as **options** (2–4), each a concrete, mutually exclusive reading.
   - Include an escape hatch when the option set might not be exhaustive (e.g. "something else").
4. **Wait for the answer.** Never ask the next question in the same turn. Never proceed on a guess.
5. **Re-scan.** The answer may resolve other questions on the list, spawn new ones, or reveal that an earlier answer needs refinement. Update the list and return to step 3.
6. **Repeat until the list is empty.** There is no question budget — ask as many as it takes. Ten questions that produce certainty beat three that leave gaps. But don't pad: only ask questions whose answer actually changes the meaning. If something is genuinely certain from context, don't ask about it.
7. **Restate and stop.** When nothing ambiguous remains, deliver the clarified idea (format below) and end. Do NOT proceed to implement, plan, or otherwise act on the idea unless the user then asks.

## Asking questions

If an interactive option-selection tool is available (e.g. `ask_user_input_v0`), use it — one question per call, 2–4 tappable options. Otherwise, ask in prose with numbered options.

Good question shape:

> When you say **"release"**, do you mean:
> 1. The app-store release process (EAS build → TestFlight → review)
> 2. A GitHub release/tag on the repo
> 3. Merging to the release branch
> 4. Something else

Rules:
- **One question per turn.** Even if two ambiguities feel related, resolve them sequentially — the first answer often reshapes the second question.
- **Options are interpretations, not preferences.** The goal is "which of these did you *mean*", not "which would you *like*". (If the user's idea genuinely contains an undecided choice, it's fine to surface it — but frame it as "your idea leaves X open; is that intentional, or do you mean one of these?")
- **Quote the user's words** so they can see exactly which part of their idea triggered the question.
- **Options must be genuinely distinct.** If you can't articulate how two readings differ in consequence, they aren't separate options.
- **No leading.** Order options neutrally; don't mark one as recommended.
- If an answer is itself ambiguous, ask a follow-up on it before moving on.

## The final restatement

When the ambiguity list is empty, output:

```
## Clarified idea

[The idea restated precisely, in 1–3 paragraphs or a short structured spec —
every previously ambiguous term now pinned to its confirmed meaning.]

## Resolved ambiguities

- "[original phrase]" → [confirmed meaning]
- "[original phrase]" → [confirmed meaning]
...
```

Then stop. One closing line offering next steps is fine ("Want me to act on this?"), but do not start acting.

## What this skill is NOT

- Not a brainstorming or refinement tool — it pins down what the user *already means*, it doesn't improve the idea.
- Not a requirements interview — don't ask about things the user never mentioned unless their absence makes the stated idea ambiguous.
- Not automatic — outside explicit invocation, normal judgment about when to ask clarifying questions applies.
