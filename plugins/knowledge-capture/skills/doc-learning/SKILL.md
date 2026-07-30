---
name: doc-learning
description: Capture something you just learned (a debugging insight, a new pattern, a gotcha) and publish it as a Confluence page via the Atlassian Rovo connector, so future-you can find it again.
disable-model-invocation: true
---

# Document a learning to Confluence

## Recent context (for reference, may or may not be relevant)
!`git log --oneline -10 2>/dev/null`
!`git diff --stat 2>/dev/null`

## Instructions

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
- If any Rovo/Confluence call fails with a 403 or an "app not installed"
  style error, stop and tell the user plainly that the Atlassian connector
  needs Confluence access authorized (via claude.ai connector settings) —
  don't retry blindly or fall back to creating something else.
- Prefer reusing one parent "Learnings" page per space across calls over
  scattering many unrelated top-level pages, when the user doesn't have a
  strong preference.
