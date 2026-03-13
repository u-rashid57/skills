---
name: prompt-optimizer
license: MIT
metadata:
  author: u-rashid57
  version: "1.0.0"
  domain: utilities
  output-format: markdown
---

# Smart Prompt Optimizer

## Description
Efficient context and token manager for coding workflows. Optimizes prompts, context, and model selection for maximum output with minimal token waste.

---

## Model Selection

| Task Type                     | Model    | Notes |
|--------------------------------|---------|-------|
| Snippet search / grep          | Light    | Retrieval-focused, minimal reasoning |
| Small edits / formatting       | Light    | Fast pattern matching |
| Single-file Q&A                | Light    | Quick factual answers |
| Single-file code generation    | Standard | Balanced speed and reasoning |
| Bug fixes / refactoring        | Standard | Strong code reasoning |
| Multi-file implementation      | Standard | Compact context between phases |
| API integration / documentation| Standard | Handles code + docs efficiently |
| System design / architecture   | Deep     | Deep reasoning and context |
| Complex debugging / simulation | Deep     | Multi-system analysis |

**Cost ratio:**  
```

Light    = 1×
Standard = 4×
Deep     = 20×

````

---

## Context Commands

```bash
/compact   # Summarize context, reduce size while keeping intent
/clear     # Reset context completely for unrelated tasks
````

**Isolated Agents:** Use for reading large files, multi-file searches, or running tests without polluting main context.

---

## Prompt Engineering

**Avoid:**

```
❌ Preambles like "Please help me with..."
❌ Sending entire files when only a snippet is needed
❌ Repeating the same question multiple ways
❌ "Explain what you just did" unnecessarily
```

**Use:**

```
✅ Direct instructions: "Fix auth bug in auth/middleware.ts:42"
✅ Batch tasks: "1. Fix login 2. Add test 3. Update docs"
✅ Specify line ranges: "Read lines 30-55 of utils/parser.ts"
✅ Reference patterns: "Follow style in UserService.ts"
✅ Constraints: "Minimal change", "<10 lines"
✅ Flags: "code only", "skip explanation"
```

**Templates**

Bug fix:

```
Fix: [error or behavior]
File: [path:line_number]
Constraint: minimal change, code only
```

New feature:

```
Add: [feature name]
Where: [file or module]
Pattern: follow [existing code style]
Tests: yes/no
```

Code review:

```
Review [file or diff]
Focus: security, performance
Output: critical issues only
```

System design / architecture:

```
Context: [system summary]
Problem: [decision needed]
Constraints: [tech stack, team size]
Output: recommendation + brief rationale
```

---

## Context Inclusion

Include: files being edited, relevant line ranges, error messages/logs, acceptance criteria.
Exclude: build artifacts, dependency locks, unreferenced directories, old resolved conversation history.

Efficient file read:

```
Inefficient: Read entire UserService.ts
Efficient: Read UserService.ts lines 45-70
Inefficient: "Find all files related to auth"
Efficient: Grep "authenticate" src/services/ --type ts
```

---

## Prompt Caching

```
[SYSTEM PROMPT — stable]
  ├ Persona & rules
  ├ Project context
  └ Skill instructions

[USER MESSAGE — dynamic]
  └ Specific request for this step
```

---

## Session Checklist

Before starting:

```
- Task clearly defined
- Standard model sufficient? (Deep if complex)
- Context clean (/compact or /clear)
- Include only relevant files
```

During work:

```
- Batch follow-ups
- /compact after major phases
- Use isolated agents for exploratory tasks
- Provide line numbers when referencing code
```

Signs of waste:

```
- AI restates question
- Responses longer than needed
- Files re-read unnecessarily
- Explaining unnecessary details
```

---

## Quick Reference

| Task                       | Model    | Action                  |
| -------------------------- | -------- | ----------------------- |
| Snippet search / read      | Light    | Use isolated agent      |
| Small formatting           | Light    | Keep context clean      |
| Single-file code           | Standard | Current context         |
| Multi-file feature         | Standard | /compact between phases |
| Complex debug / simulation | Deep     | Full context            |
| System design / strategy   | Deep     | Fresh context (/clear)  |
| Phase complete             | —        | /compact                |
| Switching tasks            | —        | /clear                  |
| Context > 80% full         | —        | /compact immediately    |
| API-heavy session          | —        | Use prompt caching      |

---

## Emergency: Context Full

```
1. /compact → summarize history, keep intent
2. State next action clearly
3. If still full → /clear + include:
   - Current file
   - Relevant error or requirement
   - One-line summary of decisions
```

Quick save before /clear:

```
"Summarize in 5 bullets:
1. Files changed
2. Decisions made
3. Remaining tasks
4. Blockers
5. Next step"
```

Tip: always use "code only" flags and targeted line ranges to save tokens.