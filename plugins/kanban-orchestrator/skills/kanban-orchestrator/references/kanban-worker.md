# Kanban Worker — Deep Reference

Pitfalls, examples, and edge cases for Hermes Kanban workers.

## Workspace Handling

| Kind | What it is | How to work |
|------|------------|-------------|
| `scratch` | Fresh tmp dir, yours alone | Read/write freely; gets GC'd when task is archived |
| `dir:<path>` | Shared persistent directory | Other runs read what you write; treat as long-lived state |
| `worktree` | Git worktree | Run `git worktree add <path> ${HERMES_KANBAN_BRANCH:-wt/$HERMES_KANBAN_TASK}` from main repo first |

## Tenant Isolation

Prefix memory entries with tenant so context doesn't leak across tenants:
- Good: `business-a: Acme is our biggest customer`
- Bad: `Acme is our biggest customer`

## Good Summary + Metadata Shapes

```python
# Coding task
kanban_complete(
    summary="shipped rate limiter — token bucket, keys on user_id with IP fallback, 14 tests pass",
    metadata={
        "changed_files": ["rate_limiter.py", "tests/test_rate_limiter.py"],
        "tests_run": 14,
        "tests_passed": 14,
    }
)

# Research task
kanban_complete(
    summary="3 competing libraries reviewed; vLLM wins on throughput",
    metadata={
        "sources_read": 12,
        "recommendation": "vLLM",
    }
)

# Review task
kanban_complete(
    summary="reviewed PR #123; 2 blocking issues found",
    metadata={
        "pr_number": 123,
        "findings": [...],
        "approved": False,
    }
)
```

## Review-Required Pattern

For code that needs human review before merging:

```python
import json

kanban_comment(
    body="review-required handoff:\n" + json.dumps({
        "changed_files": [...],
        "tests_run": 14,
        "tests_passed": 14,
    }, indent=2),
)
kanban_block(
    reason="review-required: rate limiter shipped, 14/14 tests pass — needs eyes before merging",
)
```

## Do NOT

- Call `delegate_task` as a substitute for `kanban_create`
- Call `clarify` to ask the human a question — use `kanban_comment` + `kanban_block`
- Modify files outside `$HERMES_KANBAN_WORKSPACE` unless the task body says to
- Create follow-up tasks assigned to yourself — assign to the right specialist
- Complete a task you didn't actually finish — block it instead