---
name: codex-cli
description: Use Codex CLI for coding tasks with session persistence. Use when: (1) running codex exec tasks in background, (2) continuing previous Codex sessions, (3) multi-turn conversations with the same context. NOT for: simple one-liner fixes (just edit), reading code (use read tool).
metadata:
  openclaw:
    emoji: 🔥
    requires:
      anyBins: ["codex"]
---

# Codex CLI Skill

Use Codex CLI for coding tasks with session persistence.

## Quick Start

### Run New Task (Background)

```bash
# Start Codex in background with PTY, returns sessionId
bash pty:true background:true workdir:/path/to/project command:"codex exec 'Your task here'"

# Example: returns session ID like "abc-123"
```

### Continue Previous Session

```bash
# Continue the most recent session
codex resume --last

# Continue specific session
codex resume <SESSION_ID>

# Fork a session to continue
codex fork <SESSION_ID>
```

## Pattern: Session-Based Workflow

### 1. Start New Task

```bash
# Start task in background, capture session ID from output
bash pty:true background:true workdir:/tmp/project command:"codex exec 'Build a REST API'"
```

### 2. Monitor Progress

```bash
# Check if done
process action:poll sessionId:XXX

# Get output
process action:log sessionId:XXX
```

### 3. Continue Same Session

```bash
# Continue with new prompt
process action:write sessionId:XXX data:"Now add authentication"

# Or use codex resume directly
codex resume <SESSION_ID> "Add authentication"
```

## Key Commands

| Command | Description |
|---------|-------------|
| `codex exec 'prompt'` | One-shot execution |
| `codex resume --last` | Continue most recent session |
| `codex resume <ID>` | Continue specific session |
| `codex fork <ID>` | Fork session for parallel work |

## Session Management

- Sessions persist in Codex's history
- Use `--last` to quickly continue the most recent
- Fork creates independent copy for parallel tasks
