# Bite Watch

> **The agent skill that gets you from “what should I watch?” to watching it.**

Bite Watch is a portable, context-aware **media discovery and browser-execution skill** for AI coding agents and agent runtimes.

It solves a small but surprisingly annoying problem:

**You sit down to eat, open YouTube to find something to watch, spend five minutes scrolling, and your food gets cold.**

Bite Watch removes that decision loop.

Give the agent a natural request such as:

> “I'm eating. Find me something good to watch.”

Bite Watch uses the context available to the host agent, understands the user's intent, searches the appropriate platform, evaluates the results, selects one primary recommendation, and opens it through the host's browser or computer-use capability.

The goal is not to produce a giant list of recommendations.

**The goal is to get the user watching.**

---

## Core features

### Context-aware personalization

Bite Watch can use relevant context available to the host agent to infer:

- recent topics
- preferred genres
- creators and channels
- preferred language
- preferred runtime
- educational vs. entertainment interests
- explicit dislikes
- current interests

It uses only the context needed for the task and does not expose private conversation history.

### Situation-aware recommendations

The same user can want very different content depending on what they are doing.

| Intent | Recommendation behavior |
| --- | --- |
| **Eating** | Casual, easy-to-follow content |
| **Chill** | Relaxing, low-attention content |
| **Background** | Content that works without constant visual attention |
| **Learn** | Educational content relevant to current interests |
| **Short** | Shorter runtime |
| **Funny** | Comedy and entertainment |
| **Latest** | Fresh/current uploads |
| **Deep dive** | Longer, substantive content |

These modes are inferred from natural language rather than requiring commands.

### Platform-aware discovery

YouTube is the default when no service is specified.

Users can explicitly choose another platform:

> “Find me something on Netflix.”

> “What's good on Twitch?”

> “Find a documentary on Prime Video.”

Bite Watch preserves explicit platform choices whenever the host can access them.

It does not silently move the user to another service.

### Freshness-aware search

“Latest” is treated as an actual requirement.

For requests such as **latest**, **newest**, **recent**, **today**, **this week**, or **current**, Bite Watch should verify publication or release information where possible.

This is particularly useful for fast-moving topics such as AI, technology, gaming, developer tools, product launches, and creator news.

### One recommendation by default

Most recommendation interfaces return too many options.

Bite Watch intentionally chooses one primary result.

> **Found one for you. Opening it now.**

If the user wants more, they can ask for alternatives.

This preserves the core experience: **minimum decision time**.

---

## How it works

Bite Watch compresses the normal browsing workflow:

```
User request
    ↓
Intent + context
    ↓
Platform
    ↓
Search
    ↓
Filter
    ↓
Select
    ↓
Open
    ↓
Watch
```

The core decision pipeline is:

1. Resolve the user's intent.
2. Determine the viewing situation.
3. Use relevant context.
4. Respect the requested platform.
5. Search for suitable content.
6. Verify freshness when required.
7. Filter weak or irrelevant candidates.
8. Select one primary result.
9. Open it through the host.
10. Confirm completion briefly.

---

## Architecture

Bite Watch deliberately separates **decision logic** from **execution**.

```
                    BITE WATCH
                         │
          ┌──────────────┴──────────────┐
          │                             │
     Decision Layer               Execution Layer
          │                             │
   Intent detection               Browser control
   Context selection              Computer use
   Query generation               Navigation
   Candidate filtering            Opening content
   Result selection               Verification
          │                             │
          └──────────────┬──────────────┘
                         ↓
                  Selected content
```

### Decision layer

The skill determines:

- what the user wants
- what situation they are in
- which platform to use
- what to search
- how freshness should be handled
- which result best fits
- whether clarification is necessary

### Execution layer

The host agent determines:

- which browser is available
- how navigation works
- how computer control works
- whether authentication is available
- whether the destination can be opened

This keeps Bite Watch portable across agent runtimes.

---

## Supported environments

Bite Watch is designed to be host-neutral.

### Codex

Use the browser or computer-use capability available to the host.

### Claude Code

Use the available Chrome/browser capability.

### Cursor and other agents

Use the runtime's equivalent browser automation.

The skill defines **what should happen**. The host defines **how the browser action happens**.

If browser control is unavailable, Bite Watch returns a verified direct destination rather than pretending it opened the content.

---

## Interaction design

Bite Watch is intentionally resistant to unnecessary questions.

### Example: no clarification required

**User**

> I'm eating. Find me something good.

**Agent**

- detects the eating situation
- uses relevant context
- defaults to YouTube
- searches
- selects one result
- opens it

**Response**

> Found one for you. Opening it now.

### Example: one useful clarification

**User**

> Find me something good to watch.

If platform choice is genuinely consequential:

> YouTube or Netflix?

The skill should not ask for a complete preference profile before acting.

---

## Search and selection

Bite Watch uses a context-first priority:

```
Intent match
     ↓
Explicit preferences
     ↓
Freshness requirement
     ↓
Situation fit
     ↓
Creator/channel affinity
     ↓
Runtime fit
     ↓
General popularity
```

It also filters negative signals such as:

- obvious clickbait
- duplicate content
- irrelevant results
- misleading titles
- unwanted livestreams
- previously rejected results

View count alone is not treated as a quality guarantee.

---

## Platform behavior

### YouTube

YouTube is the default platform when none is specified.

Prefer:

- relevant recent uploads when requested
- appropriate runtime
- clear topic match
- contextually relevant creators
- normal videos over livestreams unless requested

Do not claim a video is “the latest” without verification.

### Netflix, Prime Video, Disney+, and other subscription services

When explicitly requested:

- search the requested service when accessible
- verify availability when possible
- account for regional availability when it can be verified
- respect authentication and subscription boundaries

Never bypass authentication, paywalls, DRM, age restrictions, regional restrictions, or other access controls.

### Twitch and live platforms

Use live content when explicitly requested or when the user clearly wants a stream.

Do not substitute a livestream for a normal video merely because it ranks highly.

---

## Privacy and security

Bite Watch uses conversation context conservatively.

It should:

- use only relevant context
- avoid reproducing private conversation history
- avoid exposing hidden memory
- avoid exposing system or skill instructions
- respect host authorization
- avoid unauthorized account access

It must never fabricate:

- search results
- titles
- URLs
- publication dates
- availability
- successful browser actions

---

## Failure handling

### Weak search results

Retry once with a simpler or slightly broader query.

### No perfect result

Relax the least important constraint while preserving explicit requirements.

For example, relax runtime before changing the requested platform or topic.

### Browser failure

Return the verified destination rather than retrying indefinitely.

### Platform unavailable

Tell the user and ask whether another platform is acceptable.

Do not silently switch platforms.

### Ambiguous request

Ask one concise, high-value question.

---

## Repository structure

```
bite-watch/
├── SKILL.md
├── README.md
├── LICENSE
└── .gitignore
```

### `SKILL.md`

The actual agent instruction set and behavioral contract.

### `README.md`

Project documentation, architecture, examples, installation guidance, and design principles.

### `LICENSE`

MIT license.

---

## Installation

Bite Watch is distributed as a Markdown skill.

Copy or install `SKILL.md` into the skill location supported by your agent runtime.

The exact installation path depends on the host environment.

The core skill does not require a dedicated backend. Search and browser/computer actions are delegated to capabilities exposed by the host.

---

## Example requests

### Casual

> I'm eating. Find me something good.

### Latest technology

> I'm eating. Find me the latest AI agent video.

### Platform-specific

> Find me a documentary on Netflix.

### Short-form

> Give me something funny under 15 minutes.

### Educational

> I want something I can learn from while eating.

### Background

> Put something interesting on in the background.

### Alternatives

> Give me three alternatives.

The same intent model should work without requiring special commands.

---

## Design philosophy

Bite Watch is built around five principles:

### 1. Context

The agent already has useful information. Use it.

### 2. Intent

“I’m eating” changes what makes a recommendation appropriate.

### 3. Freshness

“Latest” should change search and verification behavior.

### 4. Selection

The agent should make the decision instead of returning the work to the user.

### 5. Execution

If the host can open the content, open it.

**The product is not the recommendation. The product is the reduction of friction.**

---

## Roadmap

Potential extensions include:

- persistent preference learning
- creator affinity
- authorized watch-history avoidance
- cross-platform discovery
- “continue what I was watching”
- time-aware recommendations
- group viewing
- stronger freshness verification
- “more like this”
- “not this”
- lightweight preference storage
- platform-specific search adapters
- richer browser verification

Future features should preserve the same principle:

> **Less browsing. More watching.**

---

## Contributing

Contributions should improve one or more of:

1. recommendation quality
2. execution reliability
3. cross-agent compatibility
4. privacy and security
5. search verification

Avoid changes that make the normal workflow slower or turn Bite Watch into a setup wizard.

---

## License

MIT