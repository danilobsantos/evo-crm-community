---
name: evo-code-review
description: Perform adversarial code review finding specific issues. Use when the user says "run code review" or "review this code"
---

IT IS CRITICAL THAT YOU FOLLOW THIS COMMAND: LOAD the FULL {project-root}/_evo/bmm/workflows/4-implementation/code-review/workflow.md, READ its entire contents and follow its directions exactly!

## Linear input handling

If the argument is a Linear issue URL or identifier (e.g. `https://linear.app/<org>/issue/EVO-123/...` or `EVO-123`):

1. Fetch the issue with `mcp__claude_ai_Linear__get_issue` to capture title, description, acceptance criteria, status, and assignee.
2. **Always** fetch comments with `mcp__claude_ai_Linear__list_comments` and scan them for a PR link (typically a `github.com/.../pull/<number>` URL pasted by the dev when work is ready). Inspect every comment — devs often paste the PR after reassignments, so the link may not be in the first or latest comment.
3. If a PR link is found, treat that PR as the review target: load it with `gh pr view <number> --repo <owner>/<repo>`, get the diff with `gh pr diff`, and use the PR metadata (`headRefName`, `baseRefName`, files, commits) to drive the review. Cross-check the PR changes against the Linear acceptance criteria.
4. If no PR link is present in the comments, ask the user for the PR URL or branch before continuing — do not attempt to guess the branch from the Linear `gitBranchName` field, since devs frequently use a different branch name.
5. Treat the Linear acceptance criteria as the canonical spec for the AC validation step in the workflow, in addition to (or in place of) the story file.
