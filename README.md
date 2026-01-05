# Opencode Agentic toolkit

Inspired by https://github.com/rstacruz/agentic-toolkit/tree/main

Collection of what agentic toolkit that I find useful on my everyday workflow. I recommend browsing `command/` and `skill/` and taking what you need.

## Pre-requisites

1. Install [OpenCode](https://opencode.ai).
2. Set up artefacts directories:

- Add to global gitignore: `echo artefacts >> ~/.config/git/global_ignore` (or wherever your global ignore is)
- I instruct AI agents to make a log of approved plans and changes inside `artefacts/` for easier session/model switching

3. Pick-and-choose what you want to copy:

- Copy `command/` files into `~/.config/opencode/command/`
- Copy `skill/` files into `~/.config/opencode/skill/` (ensure you use OpenCode 1.0.190+)

## Core tools

### Continue from here

> [`/continue-from-here` command](command/atk/continue-from-here.md)

One of my favourite ways to work! Forget prompting, just add comments. Inspired by [Aider's watch files mode](https://aider.chat/docs/usage/watch.html).

- works best when you have clean git state
- add `// AI:` comments around your code.
- run `/continue-from-here`.

```ts
// schema.prisma
model Word {
  id   String @id @default(uuid())
  word String
  // AI: add `language` column

  @@unique([lang, word])
  @@map("words")
}

// word.ts
function getWord(word: string /* AI: also query by `language` */ ) {
  // AI: use validateWord here
    return db.word.where({ word })
}

function validateWord(wordObject) {
  // AI: implement this. add typescript types to the params
}
```

### Plan mode

> [`/plan` command](command/atk/plan.md)

Ask the agent to confirm actions before doing them. Unlike OpenCode's built in plan mode:

- No need to switch to plan/build agents
- More eager to get your input with questions
- stores approved plans and changes inside the current project's `artefacts/`
- answering Agent questions is easy as multiple choice questionnaire

I use this:

- As a replacement for the built-in `plan` mode
- For almost every change (_/plan change test names to be descriptive_ → _(answer q's)_ → _go_)

```markdown
## User

/plan add dark mode toggle to settings

## Assistant

**Plan mode engaged.**

[Researches codebase, creates todo list]

Before I draft the plan, I have a clarifying question:

1. **Theme detection:** Should the app detect system theme preference automatically?
   - a. Yes, respect system preference (recommended)
   - b. No, manual toggle only

## User

1a

## Assistant

Here's the implementation plan:

1. Add theme context provider with system detection
2. Create toggle component in settings page
3. Update CSS with theme variables
4. Persist preference to localStorage

Ready when you say **go**.

## User

go

## Assistant

**Plan mode disengaged.** [Begins implementation]
```
