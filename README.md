# review-rules

Code-review rules of Agent Orchestrator by Danylo Bashynskyi (`BashkaCoder/agent-orchestrator`).
A Unity repo uses them with `.agent-orchestrator.json`:

```json
"review": { "rulesRepo": "https://github.com/BashkaCoder/review-rules.git", "overrides": "<RepoFolderName>" }
```

`scripts/Sync-ReviewRules.ps1` turns them into glob-scoped rules for the writing agent (only the rules whose
`Applies to` matches the files it edits), and the read-only reviewer checks the change against them.

## Format
- `rules/<section>.md` — one block per rule:
  ```
  ### CS-001: Short title
  - **Priority:** Critical | Architecture | Performance | Best Practices | Style
  - **Applies to:** `Assets/**/*.cs`, `Packages/**/*.cs`      (globs relative to the Unity project; no glob = a process rule for reviewers)
  - **Rule:** what must hold.
  - **Why:** the reason.
  - **Example:** optional.
  ```
- `overrides/<RepoFolderName>.md` — per project: `- **Overrides:** CS-001` replaces that rule (or `- **Rule:** disabled`
  turns it off); a block without `Overrides` adds a project-only rule.
- IDs: `<LETTERS>-<digits>`, never reused.
