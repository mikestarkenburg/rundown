# Why the image pins two key-free search plugins

Rundown is a research agent. A research agent that cannot search is a chatbot with
opinions. Getting search to actually work in a *public* image turned out to be the
single largest piece of engineering in this build, so it is worth writing down.

## The problem, in two parts

**1. The tool was not there.** The base image runs OpenClaw with `tools.profile: "messaging"`,
which grants `group:messaging`, three `sessions_*` tools and `session_status`. It does not
include `web_search` or `web_fetch` at all. The upstream `alsoAllow` list adds filesystem
and exec, not web.

**2. Allowing the tool is not enough — it needs a provider.** The search providers bundled
with the runtime in this image are `codex`, `google`, `minimax`, `ollama` and `xai`.
Every one of them needs an account or an API key. A public image that anyone can pull
cannot carry a key: the key would be in the image, and the first person to pull it would
own it.

Until both were fixed, `openclaw config validate` refused the config outright:

```
× tools.web.search.provider: web_search provider is not available: duckduckgo
```

## The fix

OpenClaw has two key-free search providers, and both ship outside the runtime package, so
the Dockerfile fetches both at build time. `parallel-free` is the one selected; DuckDuckGo
stays installed and enabled as the alternative to switch to.

### `parallel-free` — the provider actually in use

Parallel's hosted [Search MCP](https://docs.parallel.ai/integrations/mcp/search-mcp) needs
no account and no API key, and the search runs on their infrastructure rather than out of
the container. That last part matters more than it sounds: a research agent issuing a dozen
searches per brief from one address earns an IP reputation, and the failure mode is not an
error but a captcha page returned with HTTP 200. Moving the search off the container's own
egress removes the whole class of problem.

```dockerfile
ARG PARALLEL_PLUGIN_VERSION=2026.9.6
ARG PARALLEL_PLUGIN_SHA256=f7e4981fea7838b662548ab7c9bdb39157f41f8ee688b10ffa0702355f008a4c
```

Measured on the same host, same minute: a DuckDuckGo HTML request accepted the connection
and then hung until the timeout, while `parallel-free` answered a real query — a named CTO
at a named company — in about one second with roughly 25,000 characters of ranked excerpts,
including his full dated work history. It also exposes a `web_fetch` alongside the search,
which reaches pages the container cannot fetch directly.

Parallel publishes the free tier for exactly this use and says so plainly: *"The Search MCP
is free to use — no API key required, great for exploration and light use. For production
use cases or higher rate limits, create a Parallel account and pass your API key."* If you
run Rundown hard enough to matter, get a key and switch the provider to `parallel`. The
free tier is rate-limited, keyed on a session id the agent holds steady across a
conversation.

### DuckDuckGo — the fallback

Kept for the same reason you keep a spare: it costs nothing in the image and it is a
one-string config change away.

```dockerfile
ARG DUCKDUCKGO_PLUGIN_VERSION=2026.9.6
ARG DUCKDUCKGO_PLUGIN_SHA256=ca28e014a6618e7259b99c27a24194c32f9273e3d8f7c6cf5f0a5c54f1342b3e
RUN cd /tmp \
 && npm pack "@openclaw/duckduckgo-plugin@${DUCKDUCKGO_PLUGIN_VERSION}" >/dev/null \
 && echo "${DUCKDUCKGO_PLUGIN_SHA256}  openclaw-duckduckgo-plugin-${DUCKDUCKGO_PLUGIN_VERSION}.tgz" | sha256sum -c - \
 && mkdir -p /opt/plow/extensions/duckduckgo \
 && tar -xzf "openclaw-duckduckgo-plugin-${DUCKDUCKGO_PLUGIN_VERSION}.tgz" -C /opt/plow/extensions/duckduckgo --strip-components=1 \
 && rm "openclaw-duckduckgo-plugin-${DUCKDUCKGO_PLUGIN_VERSION}.tgz" \
 && chmod -R a+rX /opt/plow/extensions
```

and the boot config allows the tools, selects the provider, and loads the plugin.

## Why it is version-pinned and checksummed

`npm pack` resolves a version at build time from a mutable registry. This container holds
a live credential, so a build-time download is a supply-chain surface: whoever controls
that tarball controls code running next to the token. The version is pinned and the
tarball is verified by sha256 before it is extracted, which means a silently republished
package fails the build instead of shipping.

This is the same discipline the upstream Dockerfile already applies to its two other
build-time downloads. It is not novel; it is just applied consistently.

## The honest caveat

Both plugins are re-asserted on every boot (it is listed in `ownedPaths`) rather than relying
on it surviving from first boot. Bumping either the version or the hash is an edit somebody
has to review, which is the point.

Key-free also means rate-limited. That is a real tradeoff, accepted deliberately: a public
image that works for everyone beats a better image that only works for whoever holds the
key. The escalation path is open and documented rather than hidden — set `PARALLEL_API_KEY`
and change one string.
