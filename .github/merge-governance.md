# Repository merge governance

This policy applies to this repository. Changes use an issue-linked pull request
and squash merge. The owner operates the official Codex review gate.

## Merge checklist

1. Update a behind branch using GitHub's Update branch button or `gh pr update-branch`.
2. Wait for the required GitHub Actions checks on the updated branch, where configured.
3. Request official Codex review with `@codex review` for the current PR head.
4. Require a clean verdict from `chatgpt-codex-connector[bot]` explicitly covering
   that head. A Completed summary alone is insufficient: inspect the final review
   and findings. A new commit, including a branch update, needs a new review.
5. Fix actionable findings, obtain a new clean review, and resolve review conversations.
6. Read the head SHA again and merge with
   `gh pr merge <PR> --squash --match-head-commit <full-head-sha>`.
7. Verify the merged commit and applicable post-merge workflows or runtime convergence
   before closing the issue and cleaning task branches and worktrees.

Every PR, including Dependabot, follows this checklist. If official Codex cannot
review the current head, keep the PR open and record the blocker in its issue.
Do not use an administrator bypass to substitute for these checks.

## Native protection and its limits

Auto-Merge is disabled because GitHub does not wait for a Codex comment or reaction.
Automatically allowing branch updates only exposes the update operation; neither
this setting nor Auto-Merge automatically refreshes all behind PR branches.
Do not add a comment parser, fabricated Codex status, or automatic merge controller.

The native protection baseline requires pull requests and resolved conversations,
blocks deletion and force pushes of the default branch, and uses linear history.
Approving review count remains zero for a single-owner repository: self-approval
cannot provide an independent approval. Required CI, where present, is bound to
GitHub Actions and requires up-to-date branches. Documentation/evidence repositories
without CI keep human and Codex review rather than adding an empty green check.
Codex's review verdict is an owner-operated requirement, not an enforced native
required check. Settings acceptance and actual effective rules are recorded in
the linked implementation issue; this document alone does not prove enforcement.

## Actions and dependencies

Pin Actions to full commit SHAs and retain a version comment. Use GitHub's native
Actions permissions policy and read-only default workflow tokens; grant job-level
write access only where an existing publishing or credential flow needs it.
Dependabot Actions updates retain the same review requirements. Existing paused
custom-provider review paths remain paused and do not substitute for official Codex.

References: [GitHub rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets),
[GitHub Auto-Merge](https://docs.github.com/en/pull-requests/how-tos/merge-and-close-pull-requests/automatically-merging-a-pull-request),
[official Codex review](https://developers.openai.com/codex/integrations/github), and
[GitHub CLI merge](https://cli.github.com/manual/gh_pr_merge).
