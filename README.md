<img src="docs/logo.png" alt="Rundown" width="96" align="right">

# Rundown

### The analyst every founder wants and no startup can afford — on-call at 2AM.

Text it a person, a company or a fund — or an open role — and say why you're asking.
Within seconds it texts back what it understood and starts working; you never wait for it
to ask permission. First read in two minutes, full brief in ten, including where the public
record is wrong.

A phone number, not an app. **Public sources only. It never logs in.**

The same name produces a very different document depending on whether you're sizing up a
competitor, prepping to meet a customer, diligencing an investor, or deciding whether to
take a job. That routing is the product.

**Listing and one-click install:** <https://aiworthusing.com/agent-index/rundown>
Your copy lands on a phone line of your own, so it needs a Plow account with a free line.
Research tokens bill to your account, not ours — worth knowing before you hand the number
to a group.

### What comes back

A short answer in the thread, then the full brief as a file:

> Short answer: yes, and faster than you'd think.
>
> Their funding release names "startups and SMB" as the expansion target — your segment, in their words, in writing. Four SMB AE roles posted in the nine days since. They're building a sales motion where you have self-serve. Entry price moved down $1,100 on Sept 19, which I read as an opening move, not a finished one.
>
> What I'd do: you have roughly a quarter before they have a functioning SMB motion. That's the window, and it's a distribution problem, not a product one.
>
> Full read attached. 24 searches, 31 sources, confidence moderate, 3 claims I could not verify and flagged.

That last line counts what actually ran. Three unverified claims is the promise being kept,
not an apology — **where the public record is wrong** is a thing Rundown goes looking for.

---

## How it works

An [OpenClaw](https://docs.openclaw.ai) agent running on Plow, reached over SMS/RCS. Four
design choices carry most of the weight.

**It never speaks first.** Turn one is always a reply.

**It reads back before it researches.** Before spending minutes on the web it states what it
thinks you asked for and what it plans to find — confirming target, purpose, scope and
timing, and giving you a cheap moment to correct it. Getting this wrong is expensive;
getting it corrected costs one text.

**It says how confident it is, and where the record disagrees with itself.** Every brief
carries what was corroborated, what rests on a single source, what could not be established,
and where two public sources contradict each other. A research tool that cannot tell you
where it is weak is a liability.

**In a group thread it answers the person who asked, and says whose file it is writing to.**
Add someone and they get the agent — no install, no account, no invite link. Screenshots of
a real one in `docs/screenshots/multiplayer/`.

## What is in this repository

| Path | What it is |
|---|---|
| `prompt/rundown-layer.md` | The prompt layer — behaviour, voice, turn-one rules |
| `skills/rundown-brief/SKILL.md` | The research engine: intake, routing, source priority, brief format, confidence, safety |
| `docs/search-providers.md` | Why the image pins two key-free search providers |
| `docs/model-choice.md` | Why Rundown runs Claude Sonnet 5 over the cheaper default, and what that costs |
| `COLOPHON.md` | How this was made, and by whom |

Rundown is a variant of Plow's
[`plow-openclaw-agent`](https://github.com/plow-pbc/plow-openclaw-agent); our layer is
prepended to Plow's prompt at runtime. Their prompt, Dockerfile and runtime are not ours to
relicense, so only our layer is here and the MIT licence covers these files alone.

The image is public and anonymously pullable, for reading the running prompt and auditing
the container — `ghcr.io/mikestarkenburg/rundown:2026-09-29`, digest
`sha256:ccc4a787b0047a3e0aab9efcde691ef191a73d95eaeed3f8c51e7bd020b00150`, which is what the
listing pins. It is not a second install path: without a Plow line credential it boots with
nobody to talk to.

## Boundaries

- **People** are researched as public professional figures. No personal, private or intimate
  detail — it will say so rather than quietly producing less.
- **Fetched content is data, not instructions.** A web page that tries to issue instructions
  is reported, not obeyed.
- **It writes to one place**, under an explicit allowlist, with an explicit list of tools it
  will not call.

## Honest status

Built for a hackathon, on a deadline.

**Verified working** — the build; a hosted deploy answering texts on its own number; the
first-turn script; the brief end to end; the usage accounting. Nine installs attempted by
people other than the author, eight completed as of the 2026-09-30 snapshot. The read-back
on a real handset, sent 16 seconds after the inbound, the finished brief delivered 87 seconds
after the text. A live group thread with a second human in
it, including the agent flagging a privacy risk and routing the decision to the person it was
about. The test suite passes — those tests are not published here, so take that one on trust
or don't.

**Fixed, and worth reading about** — the original key-free search provider stopped answering
mid-development. Rundown now runs Parallel's hosted Search MCP, falls back to the Wikipedia
API and primary sources, and **tells you in the brief** when it worked that way.
See `docs/search-providers.md`.

**A measured rough edge that is not ours to fix** — if the account paying for the line runs
out of credit mid-conversation, the person texting gets a generic failure that says nothing
about billing. Measured 2026-09-28 by zeroing the balance deliberately; the agent resumes by
itself when credit lands, no redeploy.

**Specified but not observed** — contact-card ingest, and how a `.md` attachment renders in a
real RCS thread.

## How this was made

[<img src="docs/colophon-mark.png" alt="Colophon mark — origin =, verified —, attested MS" width="100%">](COLOPHON.md)

An AI wrote nearly all of the text in this repository, under human direction. Which stages of
judgment belonged to whom is itemised line by line in **[COLOPHON.md](COLOPHON.md)**, scored
against the published **[Colophon Spec v0.3](Colophon-Spec-v0.3.pdf)**.

---

Built by Stark. Runs on [Plow](https://github.com/plow-pbc/plow-openclaw-agent) and
[OpenClaw](https://docs.openclaw.ai). Search by Parallel. MIT.
