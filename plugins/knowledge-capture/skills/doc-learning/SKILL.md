---
name: doc-learning
description: Capture something you just learned (a debugging insight, a new pattern, a gotcha) and publish it as a Confluence page via the bundled Atlassian remote MCP server, so future-you can find it again.
disable-model-invocation: true
---

# Document a learning to Confluence

## Recent context (for reference, may or may not be relevant)
!`git log --oneline -10 2>/dev/null`
!`git diff --stat 2>/dev/null`

## Instructions

0. **Confirm Atlassian access.** This plugin declares the official Atlassian
   remote MCP server (`atlassian`, `https://mcp.atlassian.com/v1/mcp/authv2`)
   in its `.mcp.json` — no personal claude.ai connector setup required, and
   nothing tied to any GitHub secret. If Atlassian tools aren't available or
   calls fail with an auth error, tell the user to run `/mcp` and connect the
   `atlassian` server (one-time OAuth in the browser, per user per machine),
   then retry.

1. **Gather the learning.** If the user already described what they learned in
   their invocation, use that. Otherwise ask: what did you just learn or
   figure out, and why does it matter? Pull in supporting detail from the
   current conversation and the recent git context above if relevant (code
   snippets, commands, error messages that got resolved).

2. **Draft the page** before touching Confluence. Keep it short and
   skimmable — the bar is "future-you understands this in 30 seconds":
   - **Title**: `YYYY-MM-DD — <short topic>`
   - **Context**: the problem or task that led to this
   - **What I learned**: the core insight, in plain language
   - **Example**: the relevant code/command/config snippet, if any
   - **Gotchas**: pitfalls worth remembering
   - **Related**: repo/PR/doc links, if any

   Show the drafted title + body to the user and get a go-ahead before
   publishing anything.

3. **Resolve the Confluence target.** Don't assume — ask each time unless the
   user already specified it in this conversation:
   - Call `getAccessibleAtlassianResources` to get the `cloudId`. If more than
     one site is accessible, ask which one.
   - Call `getConfluenceSpaces` for that `cloudId` and ask which space to file
     into.
   - Ask whether this should go under an existing parent page (e.g. a running
     "Learnings" page — search with `searchConfluenceUsingCql` or
     `getPagesInConfluenceSpace` if the user isn't sure of the exact page) or
     as a new top-level page in the space.

4. **Publish** with `createConfluencePage` (or `createConfluenceFooterComment`
   if the user wants to append to an existing page instead of creating a new
   one).

5. **Report back** the published page's title and URL.

## Guidelines
- Never publish without showing the draft and getting explicit confirmation.
- If a call fails with "not authenticated"/similar, that's step 0 (run
  `/mcp`, connect `atlassian`) — don't retry blindly.
- If a call fails with a 403 "app not installed" style error even after
  connecting, that means Confluence isn't provisioned on that Atlassian
  site at all (separate from auth) — tell the user plainly and stop,
  don't fall back to creating something else.
- Prefer reusing one parent "Learnings" page per space across calls over
  scattering many unrelated top-level pages, when the user doesn't have a
  strong preference.
