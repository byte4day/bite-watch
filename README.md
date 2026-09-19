# Bite Watch

Bite Watch is a portable agent skill for deciding what to watch without wasting time scrolling through platforms.

It uses relevant conversation context to infer the user's current intent and preferences, searches for suitable content, selects one primary result, and opens it through the host agent's browser or computer-use capability.

## Built for

- Codex
- Claude Code
- Cursor and other compatible agent runtimes

## Core flow

```
User intent
   ↓
Relevant conversation context
   ↓
Platform selection
   ↓
Search
   ↓
Ranking
   ↓
One recommendation
   ↓
Browser / computer action
```

Example:

> I'm eating. Find me something good to watch.

Bite Watch should avoid a long questionnaire, use known interests when available, prefer fresh content when appropriate, select one result, and open it.

## Platform behavior

YouTube is the default when no service is specified.

If the user explicitly asks for Netflix, Prime Video, Disney+, Twitch, or another supported service, preserve that choice.

## Installation

Copy `SKILL.md` into the skill location supported by your agent runtime.

The skill itself does not require a dedicated backend. Search and browser/computer actions are delegated to capabilities available in the host agent.

## Design principles

- Minimize decision time.
- Ask only necessary questions.
- Use context for personalization.
- Prefer one strong recommendation.
- Never pretend an action succeeded.
- Do not expose private conversation history.

## License

MIT
