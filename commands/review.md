---
description: "Review Swift PR for concurrency issues using the Swift Concurrency skill"
argument-hint: "[--style=reviewer|tutor] [--include-learning|--no-learning]"
allowed-tools: ["Bash", "Glob", "Grep", "Read", "Task"]
---

# Swift Concurrency PR Review

Run a Swift concurrency review on a pull request using the specialized `review-agent` powered by the `swift-concurrency` skill.

**Arguments:** "$ARGUMENTS"

## Review Workflow:

1. **Dependency Check**

   Verify all required plugins are available:

   - Check if the `swift-concurrency` skill/plugin is installed and accessible
   - Record the result (plugin name, version if available, status)
   - If the plugin is NOT available:
     - Display an error message explaining the missing dependency
     - Exit immediately

2. **Parse Arguments**

   Parse the provided arguments to determine review mode:

   - `--style=reviewer` - Concise, professional code review style (issues + fixes only)
   - `--style=tutor` - Educational style with explanations and learning resources (DEFAULT)
   - `--include-learning` - Include learning URLs
   - `--no-learning` - Exclude learning URLs

   **Defaults:**
   - Style: `tutor`
   - Learning: `--include-learning` for tutor mode, `--no-learning` for reviewer mode

3. **Determine Review Scope**

   - Check if PR exists: `gh pr view --json number,headRefName,baseRefName`
   - If no PR exists, report that and exit
   - Get the PR's changed files: `gh pr diff --name-only`
   - Filter to only `.swift` files
   - If no Swift files changed, report that and exit

4. **Detect GitHub Base URL**

   Construct the base URL for clickable file links:
   - Run `git remote get-url origin` to get the remote URL
   - Use the PR's head branch from step 3
   - Parse owner/repo from the remote URL
   - Construct: `https://github.com/{owner}/{repo}/blob/{head_branch}/`
   - Store as `github_base` for passing to the agent

5. **Launch Review Agent**

   Launch the `swift-concurrency-reviewer:review-agent` agent with the parsed configuration:

   ```
   Task tool with subagent_type: swift-concurrency-reviewer:review-agent
   Prompt: Review the following Swift files for concurrency issues.
           Style: [reviewer|tutor]
           Include learning resources: [true|false]
           GitHub base URL: [github_base]
           Files: [list of changed Swift files]
   ```

6. **Return Results**

   Format the output in the following order:

   **a) Version**

   ```
   swift-concurrency-reviewer: v[VERSION]
   ```

   To get `[VERSION]`: Read `.claude-plugin/plugin.json` from this plugin's directory and extract the `version` field.

   **b) Dependencies**

   ```
   swift-concurrency plugin: [Available|Not Available]
   ```

   **c) Review Report**

   Display the formatted report from the review agent.

## Usage Examples:

**Default (tutor mode with learning resources):**
```
/swift-concurrency-reviewer:review
```

**Reviewer mode (concise, no learning links):**
```
/swift-concurrency-reviewer:review --style=reviewer
```

**Tutor mode without learning links:**
```
/swift-concurrency-reviewer:review --style=tutor --no-learning
```

**Reviewer mode with learning links:**
```
/swift-concurrency-reviewer:review --style=reviewer --include-learning
```

## Prerequisites:

- The `swift-concurrency` plugin must be installed
- A pull request must exist for the current branch

## Notes:

- The agent uses the Swift Concurrency skill as its source of truth
- All recommendations follow the skill's decision tree and patterns
- Learning URLs are fetched directly from https://fuckingapproachableswiftconcurrency.com/en/
- Reviews only files changed in the PR, not the entire codebase
