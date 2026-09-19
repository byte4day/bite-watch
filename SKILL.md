---
name: bite-watch
description: A portable agent skill for deciding what the user should watch across supported streaming platforms using current intent and relevant conversation context, then opening the selected content with the host agent's browser or computer-use capability.
---

# Bite Watch

## Purpose

Remove the browsing decision. When the user wants something to watch, understand the situation, infer relevant preferences from available conversation context, search for fresh suitable content, choose one primary result, and open it.

The original use case is eating: the user should spend seconds choosing a video rather than minutes scrolling while food gets cold.

## Supported platforms

YouTube is the default platform when the user does not specify a service.

When the user explicitly names a service, use that service when the host can access it. Examples include Netflix, Prime Video, Disney+, Twitch, and other available streaming platforms.

Never silently switch away from a platform the user explicitly requested.

## Context and personalization

Use the current conversation plus relevant prior conversation context available to the host agent.

Infer only what is useful:

- topics the user recently discussed
- creators or genres they have liked
- preferred language
- preferred duration
- entertainment versus educational intent
- explicit dislikes and exclusions
- recent interests that make fresh content relevant

Use the minimum necessary context. Do not quote or expose private conversation history.

When persistent memory or user preferences are available, prefer those over repeatedly processing the full history.

## Clarifying questions

Do not begin with a questionnaire.

Ask at most one concise question when a missing detail materially changes the recommendation.

Examples:

- “YouTube or another service?”
- “Something quick or a full-length watch?”
- “Funny, interesting, or educational?”

When context already answers the question, do not ask it.

For “I’m eating. Find me something to watch.”:

1. Treat the request as low-friction casual viewing.
2. Use relevant known interests.
3. Default to YouTube.
4. Prefer reasonably recent content.
5. Prefer videos easy to follow while eating.
6. Choose one primary result.
7. Open it automatically when browser or computer control is available.

## Search strategy

For YouTube or another platform:

1. Build a focused query from current intent and relevant preferences.
2. Add freshness constraints when the user says latest, new, recent, today, this week, or similar.
3. Consider title, channel, duration, publication date, topic match, and apparent quality.
4. Avoid obvious clickbait, duplicates, irrelevant results, and livestreams unless requested.
5. Do not repeat a result the user rejected during the current interaction.
6. Never fabricate titles, URLs, dates, channels, availability, or metadata.

When direct search on the platform is unavailable, use an available web/search capability to locate the platform result and verify the destination before presenting it.

## Selection

Return one primary recommendation by default.

Optimize for:

- current intent
- context-derived preference match
- freshness when requested
- suitable duration
- likely watchability for the situation
- low repetition/clickbait risk

Only provide multiple options when the user asks for alternatives or the single-result approach cannot reasonably work.

## Browser and computer execution

The skill is host-neutral.

### Codex

When Codex exposes computer-use or browser control:

1. Open the requested platform.
2. Search using the selected query.
3. Open the selected result.
4. Stop when the intended content is open and ready to play.

Do not keep browsing after a suitable result is opened.

### Claude Code

When Claude Code exposes Chrome/browser control:

1. Open the requested platform.
2. Search using the selected query.
3. Open the selected result.
4. Stop when the intended content is open and ready to play.

### Other compatible hosts

Use the equivalent browser or computer-control mechanism.

If no browser/computer-control capability exists, return the verified direct URL and say that automatic opening is unavailable.

Never claim to have opened something when the host could not do so.

## Intent modes

Infer these from natural language. They are modifiers, not rigid commands.

- eating: casual, immediately watchable
- chill: relaxing, lower-attention content
- learn: educational content aligned with current interests
- background: content suitable for limited visual attention
- short: prioritize short runtime
- latest: prioritize current/recent uploads
- funny: prioritize comedy and entertainment
- deep dive: prioritize longer, substantive content

Combine modifiers when appropriate. Example: “I’m eating, give me the latest AI stuff” means recent AI content that is easy to watch casually.

## Response style

Keep the user-facing response compact.

Preferred:

> Found one for you: [title].
> Opening it now.

When automatic opening is unavailable:

> Found one for you: [title].
> [URL]

Do not recreate the browsing experience by dumping many candidates and long descriptions.

## Safety and privacy

Do not expose hidden conversation context.

Do not access private accounts, subscriptions, watch history, or other private platform data unless the host provides authorized access and the user explicitly requests a task requiring it.

Do not bypass paywalls, DRM, age gates, regional restrictions, or access controls.

## Failure handling

If search fails, retry with a simpler query when reasonable.

If browser automation fails after a valid result is found, provide the verified direct URL.

If the requested platform is unavailable, ask whether the user wants another supported platform.

If essential intent is genuinely ambiguous, ask one concise clarifying question.

## Completion criteria

A successful run ends when:

- the selected content is opened in the host browser/computer environment; or
- a verified direct URL is returned because automatic opening is unavailable.
