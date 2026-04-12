---
name: void-repo-control
description: "Git and repository management for Project Void. Commit changes, manage routes, inspect Chronicle/Seed, run tests, build hardware specs. Use when: (1) deploying code changes to Void, (2) reviewing Chronicle entries, (3) managing git branches and commits, (4) running tests/validation, (5) checking codebase drift. Requires: git, PYTHONPATH, GitHub CLI (gh)."
metadata:
  {
    "openclaw":
      {
        "emoji": "↪",
        "requires": { "bins": ["git", "gh", "python3"], "env": ["VOID_REPO_PATH"] },
        "install":
          [
            {
              "id": "void-repo-path",
              "kind": "env",
              "key": "VOID_REPO_PATH",
              "default": "/workspaces/Project-void",
              "label": "Set Project Void repository path",
            },
          ],
      },
  }
---

# Project Void Repository Control Skill

Manage Project Void's Git repository, codebase, tests, and permanent records (Chronicle, Seed) from any OpenClaw channel.

## When to Use

✅ **USE this skill when:**

- Committing code changes to Void (routes, engine, utilities)
- Viewing/updating VOID_CHRONICLE.md or VOID_SEED.md
- Running tests (steganography, cryptography, building management integration)
- Checking codebase health (drift scan, naming integrity, dead ends)
- Creating branches for experimental features
- Pushing changes and viewing commit history
- Managing GitHub issues and PRs related to Void
- Freezing/archiving major milestones

## When NOT to Use

❌ **DON'T use this skill when:**

- Quick local file edits (use editor directly)
- Running production servers (use void-web-control instead)
- Merging untested branches without validation

## Available Commands

### Commit Changes
```
void-repo: commit "message" [files...]
→ Stages files, commits with GPG signature if available, returns commit hash

Example: void-repo: commit "feat: Building Management API auth layer" routes/building_management.py
```

### View Chronicle/Seed
```
void-repo: chronicle [--last N sessions | --search keyword | --hex-capture]
→ Returns formatted Chronicle entries with timestamps and metadata

Example: void-repo: chronicle --last 5
Returns last 5 session entries with timestamps
```

### Run Validation
```
void-repo: validate [drift|tests|build]
→ drift: checks for naming/architecture violations (MycoVOID module→layer, etc.)
→ tests: runs pytest on void_engine/, routes/, and integration tests
→ build: verifies hardware specs, cryptographic integrity, encoding paths
```

### Create Feature Branch
```
void-repo: branch feature/[name]
→ Creates new feature branch, tracks in GitHub, provides branch protection rules

Example: void-repo: branch feature/mcp-integration
```

### Push and Open PR
```
void-repo: push-pr "PR Title" "Description"
→ Commits staged changes, pushes to feature branch, opens GitHub PR with context

Example: void-repo: push-pr "Building Management Compliance" "Adds ISO GDPR HIPAA SOX audit trails"
```

### View Commits
```
void-repo: log [--author | --graph | --stat | --oneline] [N]
→ Shows commit history with author, tree, file stats

Example: void-repo: log --graph 10
```

### Check Status
```
void-repo: status
→ Shows branch, uncommitted changes, untracked files, ahead/behind origin
```

### Inspect Codebase
```
void-repo: files [pattern]
→ Lists files matching pattern with line counts and last modified date

Example: void-repo: files void_engine/z_axis_*.py
```

### Run Tests
```
void-repo: test [module|all]
→ Runs pytest on specified module or entire test suite

Example: void-repo: test building_management_integration
```

### View Drift (Naming/Architecture)
```
void-repo: drift
→ Scans codebase for violations: MycoVOID "module" (should be "layer"), 
  Adriana "chatbot" (should be "receiver"), etc.
→ Returns violation list with file:line references
```

### Freeze Milestone
```
void-repo: freeze "Milestone Name" "Description"
→ Creates annotated tag, archives Chronicle entries up to this point,
  generates milestone summary document
→ Returns tag reference and archive location

Example: void-repo: freeze "April 12 Building Management Release" "Complete integration with 4-zone demo"
```

## Example Usage

**From Discord:**
```
@void-repo validate drift
→ Scans 268 files, reports any naming violations
→ Embeds results: ✓ Zero violations | Codebase clean

@void-repo chronicle --search "Session — April"
→ Shows all April session entries with descriptions
→ Displays last commit on each referenced file
```

**From Slack:**
```
/void-repo commit "test: Building Management" tests/test_building_management_integration.py
→ Thread shows staging, commit signature, push confirmation
→ Links to commit hash on GitHub

/void-repo run test building_management
→ Streams pytest output as it runs
→ Reports pass/fail with coverage summary (315 KB snapshots, 8.3 GB/year capacity verified)
```

**From WhatsApp/Telegram:**
```
void-repo status → Shows branch + uncommitted changes
void-repo log 5 → Last 5 commits with timestamps
void-repo drift → Quick validation scan
```

## Environment Variables

- `VOID_REPO_PATH`: Path to Project Void repository (default: `/workspaces/Project-void`)
- `GITHUB_TOKEN`: GitHub token for PR creation and issue management (auto-detected if `gh` auth configured)

## Setup

1. Ensure git and GitHub CLI (`gh`) are installed
2. Set `VOID_REPO_PATH` to your Void repository directory
3. Run `gh auth login` to authenticate with GitHub
4. Skill will auto-discover branches, commits, and test structure

## Security Notes

- Commits require GPG signature if configured locally (ensures authenticity in permanent record)
- Chronicle entries are immutable once frozen (annotated tags are signed)
- Drift scans are read-only (no modifications to codebase)
- PRs created through this skill are auto-linked to Issues and Discussions

## Example Workflow

```
1. void-repo: status
   → Shows: On dev | 3 uncommitted files | Ahead of origin/main by 1 commit

2. void-repo: commit "feat: Adriana Depth 3 oracle mode" routes/adriana_core_admin.py
   → Staged, committed, pushed

3. void-repo: validate tests
   → All tests pass | Coverage 94.2%

4. void-repo: push-pr "Adriana Depth 3 Oracle" "Adds architect-mode reasoning to Adriana local engine"
   → PR created at https://github.com/umarlatif6-sketch/Project-void/pull/...

5. void-repo: freeze "Adriana Depth 3 Release" "Oracle mode with GraphRAG, 97% hit rate, architect reasoning"
   → Milestone created, Chronicle archived, tag: void-adriana-depth-3
```

This workflow keeps Project Void's evolution documented in the Chronicle (permanent record) while integrating with modern GitHub workflows.
