# LawLink plugin marketplace

A Claude plugin marketplace for LawLink. Add this repository in Claude
(**Customize -> Plugins -> Add marketplace**) and the plugin below becomes
installable.

## What is in here

```
.claude-plugin/marketplace.json     the catalogue Claude reads
plugins/lawlink/
|-- .claude-plugin/plugin.json      manifest - name, description, version
|-- .mcp.json                       the LawLink MCP connector
`-- skills/
    `-- lawlink-canary/SKILL.md     connectivity test (see below)
```

## Round one is a test, not a product

`lawlink-canary` is deliberately the only skill. It exists to answer four
questions the documentation does not, before any real content is written:

| # | Test | Question it answers |
|---|------|---------------------|
| A | Add the marketplace, install the plugin | Does the pipe work at all? |
| B | Type `/` in the chat | Do plugin skills appear in the slash menu? |
| C | Read the canary's answer to Q3 | Did `.mcp.json` register the connector on web? |
| D | Bump to V2, push, do not re-Sync | Do updates arrive on their own? |

The canary prints a version token and reports how it was invoked and whether it
can see LawLink tools, so one run answers B, C and D at once.

### Running test D

1. In `plugins/lawlink/skills/lawlink-canary/SKILL.md`, change both `V1`
   occurrences to `V2`.
2. Bump `version` in `plugins/lawlink/.claude-plugin/plugin.json` to `1.0.1`.
3. Commit and push.
4. In Claude, WITHOUT reopening the Add-marketplace dialog, invoke the canary
   again.

`V2` back means updates propagate on their own. `V1` means a manual sync is
required. Worth repeating once without bumping `version`, since the docs say
updates are gated on that field - that separates "the version gates it" from
"the sync gates it".

## Connector URL

`plugins/lawlink/.mcp.json` currently points at the ngrok development tunnel:

```
https://zebra-tasting-outscore.ngrok-free.dev/mcp
```

THIS URL CHANGES every time the tunnel restarts. Change it to the production
URL before sharing this marketplace outside the team.

## After the tests pass

Add real skills as sibling directories under `plugins/lawlink/skills/`. Each is
a folder with a `SKILL.md` carrying `name` and `description` frontmatter. Keep
the set small and stable - content that changes often is better served from the
LawLink server at call time than shipped in a plugin release.
