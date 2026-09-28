# Rundown

**A research analyst you text.** A phone number, not an app. Nothing to install on the
phone, no login, no form, no account anywhere.

Text Rundown a name — a person, a company, a fund, an open role — and say why you are
asking. Within seconds it texts back what it understood and starts working. You never wait
for it to ask permission. Then it reads the public web and sends a brief written for the
decision you are about to make.

The same name produces a very different document depending on whether you are sizing up
a competitor, prepping to meet a customer, diligencing an investor, or deciding whether
to take a job. That routing is the product.

**Install:** <https://aiworthusing.com/agent-index/rundown>

### What comes back

A short answer in the thread, then the full brief as a file. This is the shape, from the
spec in `skills/rundown-brief/SKILL.md`:

> Short answer: yes, and faster than you'd think.
>
> Their funding release names "startups and SMB" as the expansion target — your segment, in their words, in writing. Four SMB AE roles posted in the nine days since. They're building a sales motion where you have self-serve. Entry price moved down $1,100 on Sept 19, which I read as an opening move, not a finished one.
>
> What I'd do: you have roughly a quarter before they have a functioning SMB motion. That's the window, and it's a distribution problem, not a product one.
>
> Full read attached. 24 searches, 31 sources, confidence moderate, 3 claims I could not verify and flagged.

That last line is a report, not a flourish — it counts what actually ran. When search is
degraded Rundown says so in the brief rather than quietly returning less.

---

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

## What is in this repository

This repo contains the parts of Rundown that are **ours to publish**:

| Path | What it is |
|---|---|
| `prompt/rundown-layer.md` | The Rundown prompt layer — the agent's behaviour, voice, and turn-one rules |
| `skills/rundown-brief/SKILL.md` | The research engine: intake, routing, source priority, brief format, confidence, and safety rules |
| `docs/search-providers.md` | Why the image pins two key-free search providers, and how |
| `docs/model-choice.md` | Why Rundown runs Claude Sonnet 5 instead of the cheaper default, and what that costs |
| `COLOPHON.md` | How this was made, and by whom |
| `LICENSE` | MIT |

**What is deliberately not here.** Rundown runs as a variant of Plow's
[`plow-openclaw-agent`](https://github.com/plow-pbc/plow-openclaw-agent). In the running
image, `prompt/AGENTS.md` is our `# Rundown` layer *prepended* to Plow's own
`# Plow assistant` prompt, which wins nothing on conflict but is present. That upstream
prompt, the Dockerfile, and the runtime are Plow's work and are not ours to relicense, so
only our layer is published here. The MIT license below covers the files in this repo and
nothing else.

## Running it yourself

The supported way in is the install link above — Rundown runs hosted, on a phone line, and
that path needs nothing from you but a text message.

If you would rather see the image, it is public:

```
ghcr.io/mikestarkenburg/rundown:2026-09-28.1
```

Digest `sha256:8f31f253aaadb0540841cfb28f95d12bd50496e1ceb4c14b0decd092e1190ed7`. It is
anonymously pullable — no GitHub account, no credential. That digest is the exact build
that answered the live text recorded under **Honest status**, not a later rebuild that
ought to be equivalent.

Be aware of what self-hosting does **not** get you: the image expects a Plow line
credential and a phone number attached to it, and without one it will boot and have nobody
to talk to. Pulling it is useful for reading the running prompt and auditing what the
container actually contains. It is not a second install path, and this repo does not
pretend otherwise.

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

- **Verified working:** the build, boot on a real phone line and the first inbound text
  answered on it, the first-turn script, the brief end to end, and the usage accounting.
  The read-back too — on a real handset, on a follow-up text rather than a first contact,
  sent 17 seconds after the inbound, with the finished brief arriving 98 seconds after the
  text. The test suite passes.
- **Fixed after a bad night, and worth reading about:** search. The original key-free
  provider stopped answering during a long development session — first a browser challenge,
  then refused connections — and by the end several unrelated search engines were challenging
  the same address. That read as IP reputation earned by our own testing volume rather than
  an outage, but it could not be proven from one network, so it is not claimed here. The fix
  was not a workaround: OpenClaw has a second key-free provider, Parallel's hosted Search
  MCP, which runs the search on their infrastructure instead of out of the container. Same
  host, same minute, the old path hung until timeout while the new one answered a real query
  in about a second. Details and the honest tradeoffs are in `docs/search-providers.md`.
  Rundown still falls back to the Wikipedia API and primary sources, and it **tells you in
  the brief** when it worked that way.
- **Specified but not observed on a live thread:** group routing beyond unit tests,
  contact-card ingest, and how a `.md` attachment renders in a real RCS thread.

`COLOPHON.md` is more specific about which is which.

## Credits

Built by Stark. Runs on [Plow](https://github.com/plow-pbc/plow-openclaw-agent)
and [OpenClaw](https://docs.openclaw.ai). Search by Parallel.
