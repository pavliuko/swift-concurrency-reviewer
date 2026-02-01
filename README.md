# Swift Concurrency Reviewer

A Claude Code plugin for reviewing Swift code for concurrency issues. Supports two output modes: **tutor** (educational with explanations) and **reviewer** (concise issues + fixes).

## Prerequisites

This plugin requires the **Swift Concurrency Agent Skill** to be installed:

```
/plugin install swift-concurrency@swift-concurrency-agent-skill
```

The plugin uses this skill as its source of truth for all concurrency analysis, decision trees, and recommended patterns.

## Installation

### Option 1: Test Locally

```bash
claude --plugin-dir /path/to/swift-concurrency-reviewer
```

### Option 2: Install from Marketplace

Once published to a marketplace:

```
/plugin install swift-concurrency-reviewer@your-marketplace
```

## Usage

### Basic Usage

```
/swift-concurrency-reviewer:review-concurrency
```

This runs in **tutor mode** with learning resources enabled (default).

### Reviewer Mode

Concise, professional output with issues and fixes only:

```
/swift-concurrency-reviewer:review-concurrency --style=reviewer
```

### Tutor Mode

Educational explanations with learning resources:

```
/swift-concurrency-reviewer:review-concurrency --style=tutor
```

### Control Learning Resources

```
# Tutor mode without learning links
/swift-concurrency-reviewer:review-concurrency --style=tutor --no-learning

# Reviewer mode with learning links
/swift-concurrency-reviewer:review-concurrency --style=reviewer --include-learning
```

## Output Modes

### Reviewer Mode

```markdown
## Swift Concurrency Issues Found: 3

### Issue 1: Sendable Violation
**File:** Sources/API/Client.swift:L42-L45
**Confidence:** 85
**Problem:** Non-Sendable type crosses actor boundary
**Fix:** Add `@unchecked Sendable` with documented safety invariant
**Skill Reference:** sendable.md
```

### Tutor Mode

```markdown
## Swift Concurrency Review

Found 3 issues. Let's understand each one:

---

### Issue 1: Sendable Violation

**Location:** Sources/API/Client.swift (lines 42-45)
**Confidence:** 85

**Current code:**
[code block]

**Why this is a problem:**
The type is passed across an actor isolation boundary but doesn't
conform to Sendable, which could lead to data races...

**Suggested fix:**
[corrected code]

**Skill guidance:** See sendable.md for conformance patterns

**Learn more:** https://fuckingapproachableswiftconcurrency.com/en/#sendable
```

## How It Works

1. **Command** (`/review-concurrency`) checks prerequisites and identifies changed Swift files
2. **Agent** (`swift-concurrency-reviewer`) coordinates the review process
3. **Skill** (`swift-concurrency`) performs the actual analysis using its decision trees and patterns
4. Results are formatted according to the selected output mode

The agent never analyzes concurrency on its own — it delegates everything to the skill to ensure consistent, accurate recommendations based on the skill's comprehensive knowledge base.

## Tips

- **Run before commits**: Catch concurrency issues early
- **Use tutor mode**: When learning Swift concurrency or onboarding team members
- **Use reviewer mode**: For quick checks during active development
- **Check prerequisites**: Ensure the `swift-concurrency` skill is installed

## Credits

- Swift Concurrency Agent Skill by [Antoine van der Lee](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill)
- Learning resources from [Fucking Approachable Swift Concurrency](https://fuckingapproachableswiftconcurrency.com/en/)

## License

MIT
