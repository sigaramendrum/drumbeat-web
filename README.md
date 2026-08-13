# drumbeat-web

Public landing site for Drumbeat.

**PUBLIC repo.** No strategy, IP, pilot, or roadmap content — marketing/landing only.
Anything sensitive belongs in the private `drumbeat` repo, never here.

Deploy (when ready): GitHub -> Settings -> Pages -> Deploy from branch (main / root).

## Working in this repo

Run this once per clone, before your first commit:

```
git config core.hooksPath .githooks
```

It arms two hooks: `.githooks/pre-commit`, which blocks a commit containing a secret, and
`.githooks/commit-msg`, which blocks an internal reference in the commit message. `core.hooksPath`
is local git config and cannot be committed, so a fresh clone has neither until you set it.

The message hook matters more than it looks. gitleaks scans commit content and never sees a
message, so CI cannot cover that at all — and a message cannot be edited once pushed.

Requires `gitleaks` 8.19.0 or newer (`brew install gitleaks`). The hook fails closed: no
scanner, no commit.

CI runs a **broader** scan on every push and pull request — the hook checks the staged diff,
CI checks all history on all refs. But CI only runs after the push, and on a public repo a
pushed secret is readable by anyone from that moment, so deleting it is not a fix; rotating
the credential is. The hook is what runs before that point.

It is a speed bump, not a control: `git commit --no-verify` skips it, a clean merge fires
`pre-merge-commit` rather than `pre-commit`, and a clone where nobody ran the command above
has no hook at all. Treat CI and GitHub's push protection as the controls that hold.

One more gap worth knowing, because it is invisible: gitleaks' default configuration exempts
whole file classes from every rule — `.svg`, images, fonts, PDFs, Office documents, and nine
lockfile names such as `package-lock.json` and `pnpm-lock.yaml`. A key inside any of those
passes the hook. CI runs a second pass that copies the textual ones to non-exempt names and
scans the copies, so those are caught, but only after the push.
