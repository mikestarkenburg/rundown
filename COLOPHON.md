# How this was made

<img src="docs/colophon-mark.png" alt="Colophon mark: origin =, verified —, attested MS" width="100%">

*Conforms to **[Colophon Spec v0.3](Colophon-Spec-v0.3.pdf)** (draft, 2026-09-09) — the full
standard is published in this repository. Twelve ledger lines, ten scored.*

```
colophon   H   H+   [=]   A+   A    |   self-measured, no independent verification   |   attested: MS
```

| | |
|---|---|
| **Origin** | `=` — a model originated 4 of the 10 scored stages |
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
| Refinement | *refined* | **Stark** |
| Verification | *verified* | Huckleberry (AI) |
| Situating II | *situated* | Stark |
| Attestation | *attested* | Stark — *not scored* |
| Reckoning | *reckoned* | *pending — has not fired* |

**Refinement changed hands on 2026-09-29.** The line was AI's at first attestation. It is
Stark's now, and the spec's wording for the stage is the reason: *"cut, reordered, reversed
a claim. Stark's edit wins."* The demo video went through four cuts and three of them were
his — caption rewrites, a music swap for energy, both calls-to-action on the end card. In
the same window he reversed two claims this project had already published to itself: a
leaderboard standing computed against the wrong cohort, and a readiness rule this author had
described from recollection instead of reading. Typo fixes and accepted suggestions would not
have moved it. Reversed claims do.

**Reckoning has not fired and is not being claimed.** It is a public scorecard of this
project's own prior calls, graded after publication, and the spec is explicit that it is what
licenses high-AI work in the first place. There is now material for one. There is not yet one.

## What we used

- **[OpenClaw](https://docs.openclaw.ai)** — the agent runtime.
- **[Plow](https://github.com/plow-pbc/plow-openclaw-agent)** — the hosting substrate and
  the base image. The phone line, the delivery plumbing and the base prompt are Plow's.
- **Claude** (Anthropic) — authored the prompt layer, the skill, the tests and the docs,
  and runs the agent. The Plow base image ships **GLM 5.2** as primary with Claude Sonnet
  as fallback; Rundown inverts that and runs **Claude Sonnet 5** as primary. The reasoning,
  and what it costs, are in `docs/model-choice.md`. Briefs written during early development
  were GLM's, before the inversion.
- **[Parallel](https://parallel.ai)** — search, via their hosted Search MCP, which is
  key-free by design rather than by loophole. **DuckDuckGo** stays installed as a one-string
  fallback. Both, and why we switched, are in `docs/search-providers.md`.
- **Whisper** (OpenAI, local) — transcribed the recorded takes behind the demo video, and
  then re-transcribed every extracted quote to prove none of them carried a take marker.
- **"District Four"**, Kevin MacLeod (incompetech.com), **CC BY 4.0** — the music bed on the
  demo video. Attribution is a licence condition; the credit block travels with the video.

## What is real and what is not

Everything below was run, not assumed.

**Measured:**
- The image builds clean from the committed tree and boots.
- It answers a real text on a real phone line. First contact on that line was delivered and
  acked in 6.5 seconds, against the published image rather than a dev build.
- It produced a real brief end to end, from a live research turn rather than an SMS thread.
- The read-back, on a real inbound text on a real handset, on a follow-up message rather than
  a first contact. Sent 16 seconds after the inbound; seven live searches; the finished brief
  delivered 87 seconds after the text, session closed at 98. Confirmed received by the
  recipient, not inferred from a log. Basis, 2026-09-28 container log: inbound 10:43:35Z,
  read-back tool call 10:43:51, brief delivered 10:45:02, session end 10:45:13.
- **Group routing, on a live RCS thread with a second human in it** — observed 2026-09-29, and
  the reason this line moved out of the "never observed" list below. Unprompted, the agent told
  the guest which file it was writing to and that his additions would go to his own rather than
  the owner's. It then asked him a question the owner had not asked. When the owner proposed
  publishing the thread, the agent flagged the personal-information risk *before* the guest
  answered and routed the decision to the person it was about; consent is on the recording.
  Three screenshots in `docs/screenshots/multiplayer/`.
- The search path was wired and working when built — the absence of a usable `web_search` was
  found and fixed before publishing, not after. **It then stopped answering us**, along with
  several unrelated engines, after a night of heavy automated querying from one address. The
  fallback to the Wikipedia API and primary sources carried the briefs written in that window,
  and the agent reported the degradation itself rather than hiding it. Search was then moved to
  a second key-free provider and measured working again: seven live searches inside one real
  inbound turn, every one returning.
- The usage accounting is correct against a hand-summed ground truth on a live session.
- The test suite passes. Those tests live in the build tree, not in this repository.
- Before publishing, the image was scanned file by file for the credential values it is
  trusted with. None are present.
- Installs by people other than the author, on their own accounts and their own phone lines:
  nine attempted, seven completed.

**Specified but never observed on a live thread:**
- Contact-card ingest.
- Whether a `.md` attachment renders sanely in a real RCS thread.

**Not claimed:** nothing in this repository has been reviewed by anyone other than its
author and the AI that wrote it. The verification above is self-measurement, and under the
spec a self-check is refinement, not verification — which is why the middle token is a dash
and not a gold check. Treat it as a good-faith record, not an audit.

## Attestation

This ledger was compiled at rebuild time from the build record, which is itself partly a
reconstruction. Stage attribution is a judgment call and the person best placed to correct
it is Stark. Published attributions stand as his.

*Attested: Michael Starkenburg, 2026-09-28. Ledger amended 2026-09-30 (refinement,
reckoning, and the group-routing line); re-attestation pending.*
