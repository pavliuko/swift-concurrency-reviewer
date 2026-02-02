---
description: "Review Swift code for concurrency issues using the Swift Concurrency skill"
argument-hint: "[--style=reviewer|tutor] [--include-learning|--no-learning]"
allowed-tools: ["Bash", "Read", "Task"]
---

# Swift Concurrency Code Review

Run a comprehensive Swift concurrency review using specialized agent powered by the https://github.com/AvdLee/Swift-Concurrency-Agent-Skill by Antoine van der Lee.

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
   - `--include-learning` - Include learning URLs from mapping file
   - `--no-learning` - Exclude learning URLs

   **Defaults:**
   - Style: `tutor`
   - Learning: `--include-learning` for tutor mode, `--no-learning` for reviewer mode

3. **Launch Review Agent**

   Launch the `swift-concurrency-reviewer` agent with the parsed configuration:

   ```
   Task tool with subagent_type: swift-concurrency-reviewer
   Prompt: Review the following Swift files for concurrency issues.
           Style: [reviewer|tutor]
           Include learning resources: [true|false]
   ```

4. **Return Results**

   Format the output in the following order:

   **a) Version**

   ```
   - swift-concurrency-reviewer: v[VERSION]
   ```

   **b) Dependencies**

   ```
   - swift-concurrency plugin: [Available|Not Available]
   ```

   To get `[VERSION]`: Read `.claude-plugin/plugin.json` from this plugin's directory and extract the `version` field.

   **c) Review Report**

   Display the formatted report from the review agent based on the selected style.

## Usage Examples:

**Default (tutor mode with learning resources):**
```
/swift-concurrency-reviewer:review-concurrency
```

**Reviewer mode (concise, no learning links):**
```
/swift-concurrency-reviewer:review-concurrency --style=reviewer
```

**Tutor mode without learning links:**
```
/swift-concurrency-reviewer:review-concurrency --style=tutor --no-learning
```

**Reviewer mode with learning links:**
```
/swift-concurrency-reviewer:review-concurrency --style=reviewer --include-learning
```

## Prerequisites:

- The `swift-concurrency` plugin must be installed

## Tips:

- **Run before commits**: Catch concurrency issues early
- **Use tutor mode**: When learning or onboarding team members
- **Use reviewer mode**: For quick checks during active development

## Notes:

- The agent uses the Swift Concurrency skill as its source of truth
- All recommendations follow the skill's decision tree and patterns
- Learning URLs are fetched directly from https://fuckingapproachableswiftconcurrency.com/en/
