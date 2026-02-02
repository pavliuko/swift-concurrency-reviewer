# Swift Concurrency Reviewer

A Claude Code plugin for reviewing Swift PRs for concurrency issues. Supports two output modes: **tutor** (educational with explanations) and **reviewer** (concise issues + fixes).

## Prerequisites

This plugin requires the [Swift Concurrency Agent Skill](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill) by Antoine van der Lee.

### Install the Swift Concurrency Skill

**Step 1:** Add the marketplace

```
/plugin marketplace add AvdLee/Swift-Concurrency-Agent-Skill
```

**Step 2:** Install the skill

```
/plugin install swift-concurrency@swift-concurrency-agent-skill
```

The plugin uses this skill as its source of truth for all concurrency analysis, decision trees, and recommended patterns.

## Installation

### Add the Marketplace

```
/plugin marketplace add pavliuko/swift-concurrency-reviewer
```

### Install the Plugin

```
/plugin install swift-concurrency-reviewer@swift-concurrency-reviewer
```

### Alternative: Test Locally

```bash
claude --plugin-dir /path/to/swift-concurrency-reviewer
```

## Usage

### Basic Usage

```
/swift-concurrency-reviewer:pr-review
```

This runs in **tutor mode** with learning resources enabled (default).

### Reviewer Mode

Concise, professional output with issues and fixes only:

```
/swift-concurrency-reviewer:pr-review --style=reviewer
```

### Tutor Mode

Educational explanations with learning resources:

```
/swift-concurrency-reviewer:pr-review --style=tutor
```

### Control Learning Resources

```
# Tutor mode without learning links
/swift-concurrency-reviewer:pr-review --style=tutor --no-learning

# Reviewer mode with learning links
/swift-concurrency-reviewer:pr-review --style=reviewer --include-learning
```

## Output Modes

### Reviewer Mode

````markdown
## Swift Concurrency Issues Found: 3

### Issue 1: Sendable Violation
**File:** [`Client.swift:42-45`](https://github.com/owner/repo/blob/branch/Sources/API/Client.swift#L42-L45)
**Severity:** High
**Problem:** Non-Sendable type crosses actor boundary
**Fix:**
```swift
extension Client: @unchecked Sendable {}
```
````

### Tutor Mode

````markdown
## Swift Concurrency Review

Found 3 issues. Let's understand each one:

---

### Issue 1: Sendable Violation

**File:** [`Client.swift:42-45`](https://github.com/owner/repo/blob/branch/Sources/API/Client.swift#L42-L45)
**Severity:** High

**Current code:**
```swift
class Client {
    func fetch() async { ... }
}
```

**Why this is a problem:**
The type is passed across an actor isolation boundary but doesn't
conform to Sendable, which could lead to data races.

**Suggested fix:**
```swift
extension Client: @unchecked Sendable {}
```

**Learn more:** https://fuckingapproachableswiftconcurrency.com/en/#sendable
````

## How It Works

1. **Command** (`/pr-review`) checks prerequisites, identifies Swift files changed in the PR, and formats the final output
2. **Agent** (`review-agent`) coordinates the review and returns structured JSON findings
3. **Skill** (`swift-concurrency`) performs the actual analysis using its decision trees and patterns

The agent never analyzes concurrency on its own — it delegates everything to the skill to ensure consistent, accurate recommendations.

## Tips

- **Run before merging**: Catch concurrency issues before they land in main
- **Use tutor mode**: When learning Swift concurrency or onboarding team members
- **Use reviewer mode**: For quick PR checks during active development
- **Check prerequisites**: Ensure the `swift-concurrency` skill is installed

## Credits

- Swift Concurrency Agent Skill by [Antoine van der Lee](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill)
- Learning resources from [Fucking Approachable Swift Concurrency](https://fuckingapproachableswiftconcurrency.com/en/)

## License

MIT
