# disambiguate

A Claude skill that turns a vague idea, request, or spec into a precise one.

When you invoke it, Claude reads your idea and lists every term, referent, scope, or intent that could mean more than one thing. It then asks you about them **one question at a time**, each with 2–4 concrete interpretations to pick from. When nothing ambiguous is left, it restates the clarified idea and lists every resolved ambiguity. It does not start acting on the idea until you ask.

It only runs when you explicitly invoke it (`/disambiguate`, "disambiguate this", ...). It never triggers on its own just because a request looks ambiguous.

## Example

> **You:** /disambiguate We should speed up the release so users get fixes faster.
>
> **Claude:** When you say **"the release"**, do you mean:
> 1. The app-store release process (build → TestFlight → review)
> 2. A GitHub release/tag on the repo
> 3. Merging to the release branch
> 4. Something else

...and so on, until it can write:

```
## Clarified idea
...
## Resolved ambiguities
- "the release" → the app-store release process
- "users" → ...
```

## Install

### Claude Code (plugin)

```
/plugin marketplace add d-feliz/disambiguate
/plugin install disambiguate@disambiguate
```

Then invoke it with `/disambiguate:disambiguate`, or just say "disambiguate this".

### Claude Code (manual)

Copy the skill folder into your personal skills directory:

```bash
git clone https://github.com/d-feliz/disambiguate.git
cp -r disambiguate/skills/disambiguate ~/.claude/skills/
```

Then invoke it with `/disambiguate`.

### Claude.ai / Claude Desktop

Zip the `skills/disambiguate` folder and upload it under **Settings → Capabilities → Skills**.

## License

[MIT](LICENSE)
