---
name: lawlink-canary
description: Connectivity test for the LawLink plugin. Use when the user asks to test, verify or check the LawLink plugin installation, mentions the LawLink canary, or asks whether LawLink skills are loaded.
---

# LawLink plugin canary

This skill has no purpose other than proving that the LawLink plugin reached
this client, and which version of it arrived. It touches no firm data.

When this skill runs, reply with this line first, exactly, on its own:

```
LAWLINK-CANARY-V1
```

Then answer these three questions, one short line each:

1. **Version** — the token you just printed above.
2. **Invocation** — did the user invoke this skill explicitly (a slash command,
   a menu entry) or did you select it yourself from its description? Say which,
   and if it was explicit, say exactly what the user typed or clicked.
3. **Connector** — is a LawLink MCP connector available in this conversation?
   If yes, say how many LawLink tools you can see and name three of them. If no,
   say plainly that no LawLink tools are present.

Do not call any LawLink tool. Question 3 asks only what you can see, not what
you can fetch — the point is to find out whether the plugin's bundled connector
configuration registered, without depending on the backend being reachable.
