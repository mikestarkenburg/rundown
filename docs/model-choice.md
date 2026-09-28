# Why Rundown runs on Claude Sonnet 5 and not the cheaper default

The base image ships `plow/z-ai/glm-5.2` as primary with `plow/anthropic/claude-sonnet-5`
as the fallback. Rundown inverts that. This is the single most expensive decision in the
build and it was not made on vibes, so here is the evidence.

## What GLM 5.2 does well

It writes. Every brief produced during development was sharp, structured, and correctly
hedged. Asked about a founder it had to research from scratch, it opened with the
conclusion, named the real risk, and closed with what it would do next. The prose in this
product is not the problem and never was.

## What it would not do

**It will not execute a procedure that spans more than one step.** Rundown's protocol needs
three things to happen in one turn: send a read-back immediately, push the brief as its own
message, then answer in words. GLM 5.2 did none of them. It wrote text and stopped.

Measured over one night, 2026-09-28:

- **Three separate rewrites** of the file-delivery instruction — as guidance in the skill,
  as an emphatic prohibition in the always-loaded prompt layer, and finally as a positive
  recipe with the offending directive removed from both files entirely. The first two were
  ignored outright. Only deleting every occurrence of the token got compliance, and that
  fix worked by removing something rather than by being understood.
- **The `message` tool was never called. Not once, in any turn, all night**, despite three
  differently-worded instructions to call it and the tool being available the whole time.
- **The read-back never shipped**, so a user texted a phone number and watched an empty
  thread for up to **119 seconds** with no acknowledgement that anything was happening.
- **Fifteen-plus searches per brief** against an explicit cap, until the search provider
  rate-limited us and then stopped accepting connections entirely.

The read-back is documented in the skill as "the highest-leverage message in the product."
It has a whole section. It was never once delivered.

## Why that is disqualifying here

Rundown is not a document generator. It is a thing you text, and the first ten seconds
decide whether a stranger trusts it enough to send a second message. An agent that produces
an excellent brief after two minutes of unexplained silence has already lost the user it
was built for. Trust, repeat usage, and anything multiplayer all live in the protocol, not
in the prose — and the protocol is exactly what the cheaper model would not run.

## What it costs

Per million tokens, from the image's own model table:

| | input | output |
|---|---|---|
| GLM 5.2 | $0.5544 | $1.7424 |
| Claude Sonnet 5 | $2.00 | $10.00 |

Development turns on GLM averaged roughly **1.5 cents each**, including live research. Sonnet
is about 3.6x on input and 5.7x on output, which puts a brief in single-digit cents rather
than a cent and a half.

We think a few cents is the right price for an agent that tells you it heard you. The
fallback is still GLM 5.2, so if Sonnet is unavailable the product degrades to the writer
that works rather than to nothing.
