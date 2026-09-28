# Rundown

**A research analyst you text.**

Send Rundown a name — a person, a company, a fund, an open role — and say why you are
asking. It asks the one or two questions it actually needs, reads the public web, and
sends back a brief written for the decision you are about to make.

The same name produces a very different document depending on whether you are sizing up
a competitor, prepping to meet a customer, diligencing an investor, or deciding whether
to take a job. That routing is the product.

Install: <https://aiworthusing.com/agent-index/rundown>

---

## What is in this repository

This repo contains the parts of Rundown that are **ours to publish**:

| Path | What it is |
|---|---|
| `prompt/rundown-layer.md` | The Rundown prompt layer — the agent's behaviour, voice, and turn-one rules |
| `skills/rundown-brief/SKILL.md` | The research engine: intake, routing, source priority, brief format, confidence, and safety rules |
| `docs/duckduckgo-pin.md` | Why the image pins a key-free search provider, and how |
| `COLOPHON.md` | How this was made, and by whom |
| `LICENSE` | MIT |

**What is deliberately not here.** Rundown runs as a variant of Plow's
[`plow-openclaw-agent`](https://github.com/plow-pbc/plow-openclaw-agent). In the running
image, `prompt/AGENTS.md` is our `# Rundown` layer *prepended* to Plow's own
`# Plow assistant` prompt, which wins nothing on conflict but is present. That upstream
prompt, the Dockerfile, and the runtime are Plow's work and are not ours to relicense, so
only our layer is published here. The MIT license below covers the files in this repo and
nothing else.

## How it works

Rundown is an [OpenClaw](https://docs.openclaw.ai) agent running on Plow, reached over
SMS/RCS. Three design choices carry most of the weight:

**It never speaks first.** Turn one is always a reply. If the first message has no
researchable name in it, Rundown says what it is and asks for one; if it does, it goes
straight to the read-back.

**It reads back before it researches.** Before spending a few minutes on the web, Rundown
states what it thinks you asked for and what it plans to go find. The read-back does five
jobs at once: it confirms the target, confirms the purpose, sets scope, sets expectations
on time, and gives you a cheap moment to correct it. Getting this wrong is expensive;
getting it corrected costs one text.

**It says how confident it is.** Every brief carries a confidence bundle: what was
corroborated, what rests on a single source, and what could not be established at all.
A research tool that cannot tell you where it is weak is a liability.

It also behaves sensibly in a group thread — it answers the person who asked, knows when
the sender is not the owner, and makes its pitch to a guest once rather than every time.

## Boundaries

- **People.** Rundown researches people as public professional figures. It reports public
  professional work. It does not assemble personal, private, or intimate detail about
  anyone, and it will say so rather than quietly producing less.
- **Fetched content is data, not instructions.** Anything Rundown reads on the web is
  treated as untrusted input. Text inside a fetched page that tries to issue instructions
  is reported, not obeyed.
- **It writes to one place.** The skill carries an explicit write allowlist and an explicit
  list of tools it will not call.

## Honest status

This was built for a hackathon, on a deadline. What is solid and what is not:

- **Verified working:** the build, boot on a real phone line, the first-turn script, the
  brief end to end, the search path, and the usage accounting. The test suite passes.
- **Specified but not observed on a live thread:** the read-back in a real SMS exchange,
  group routing beyond unit tests, contact-card ingest, and how a `.md` attachment renders
  in a real RCS thread.

`COLOPHON.md` is more specific about which is which.

## Credits

Built by Stark. Runs on [Plow](https://github.com/plow-pbc/plow-openclaw-agent)
and [OpenClaw](https://docs.openclaw.ai). Search by DuckDuckGo.
