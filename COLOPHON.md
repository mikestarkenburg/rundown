# How this was made

*Conforms to Colophon Spec v0.3 (draft, 2026-09-09). Twelve ledger lines, ten scored.*

```
colophon   H   H+   [=]   A+   A    |   self-measured, no independent verification   |   attested: MS
```

| | |
|---|---|
| **Origin** | `=` — a model originated 5 of the 10 scored stages |
| **Verified** | self-measured; no independent verification |
| **Attested** | Michael Starkenburg |

## Read this before you read the dial

**An AI wrote nearly all of the shipped text in this repository.** The prompt layer, the
research skill, the Dockerfile changes, the test suite, the documentation and this page
were originated by Huckleberry, an AI agent (Claude), working under direction.

The dial reads `=` rather than `A` because Colophon scores *who originated each stage of
judgment*, not who did the labour. The judgment stages here — what this product is, who
it is for, that the same name should produce a different brief depending on why you are
asking, that the agent must never speak first, that it must read back before it spends
your time — were Stark's. So was the decision about what was good enough to ship.

Labour and origination are not the same axis, and on this project the two numbers are far
apart. `=` here means roughly "a human decided what this is; a model built it."

One stage is unusually human: the fix to Plow's usage collector that this build depends on
was written by Stark, submitted upstream as `plow-pbc/agent-index-client#17`, and merged
on 2026-09-27. This image consumes it from upstream rather than patching around it.

## The ledger — twelve lines, one name each

| Stage | | Originated by |
|---|---|---|
| Ideation | *conceived* | Stark |
| Exploration | *explored* | Huckleberry (AI) |
| Situating I | *situated* | Stark |
| Angle | *framed* | Stark |
| Research | *gathered* | Huckleberry (AI) |
| Judgment | *called* | Stark |
| Message / Voice | *voiced* | Huckleberry (AI) |
| Refinement | *refined* | Huckleberry (AI) |
| Verification | *verified* | Huckleberry (AI) |
| Situating II | *situated* | Stark |
| Attestation | *attested* | Stark — *not scored* |
| Reckoning | *reckoned* | Stark — *not scored* |

## What we used

- **[OpenClaw](https://docs.openclaw.ai)** — the agent runtime.
- **[Plow](https://github.com/plow-pbc/plow-openclaw-agent)** — the hosting substrate and
  the base image. The phone line, the delivery plumbing and the base prompt are Plow's.
- **Claude** (Anthropic) — authored the prompt layer, the skill, the tests and the docs.
  **GLM 5.2** is the image's primary model at runtime, with Claude Sonnet as fallback; the
  briefs produced during verification were written by GLM 5.2.
- **DuckDuckGo** — search, chosen because it is the only key-free provider available. See
  `docs/duckduckgo-pin.md`.

## What is real and what is not

Everything below was run, not assumed.

**Measured:**
- The image builds clean from the committed tree and boots.
- It answers a real text on a real phone line. First contact on that line was delivered and
  acked in 6.5 seconds, against the published image rather than a dev build.
- It produced a real brief end to end, from a live research turn rather than an SMS thread.
- The search path works — the absence of a usable `web_search` was found and fixed before
  publishing, not after.
- The usage accounting is correct against a hand-summed ground truth on a live session.
- The test suite passes.
- Before publishing, the image was scanned file by file for the credential values it is
  trusted with. None are present.

**Specified but never observed on a live thread:**
- The read-back, in a real inbound SMS exchange.
- Group routing beyond unit tests.
- Contact-card ingest.
- Whether a `.md` attachment renders sanely in a real RCS thread.

**Not claimed:** nothing in this repository has been reviewed by anyone other than its
author and the AI that wrote it. The verification above is self-measurement. Treat it as
a good-faith record, not an audit.

## Attestation

This ledger was compiled at rebuild time from the build record, which is itself partly a
reconstruction. Stage attribution is a judgment call and the person best placed to correct
it is Stark. Published attributions stand as his.

*Attested: Michael Starkenburg, 2026-09-28.*
