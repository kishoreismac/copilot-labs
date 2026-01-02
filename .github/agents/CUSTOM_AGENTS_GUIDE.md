# GitHub Copilot Custom Agents Guide

## Overview
Custom agents are specialized AI assistants that you can create to handle specific tasks within your repository. They extend GitHub Copilot's capabilities with focused expertise and predefined workflows.

## 📁 File Structure

Custom agents must be placed in:
```
.github/agents/your-agent-name.agent.md
```

Each agent file uses a special markdown format with front matter and instructions.

## 🏗️ Basic Structure

```markdown
```chatagent
---
description: 'Brief description of what the agent does'
tools:
  - tool_name_1
  - tool_name_2
---

# Agent Instructions

Detailed instructions for the agent...
```\`\`\`
```

## 🔧 Available Tools

You can grant agents access to these tools in the `tools:` section:

### File Operations
- **read_file** - Read contents of files
- **create_file** - Create new files
- **replace_string_in_file** - Edit existing files
- **multi_replace_string_in_file** - Make multiple edits efficiently
- **list_dir** - List directory contents
- **file_search** - Search for files by pattern

### Code Intelligence
- **semantic_search** - Search code by meaning/intent
- **grep_search** - Text/regex search across files
- **list_code_usages** - Find all references to a symbol
- **get_errors** - Get compilation/lint errors

### Execution
- **run_in_terminal** - Execute shell commands
- **get_terminal_output** - Get output from background processes
- **run_notebook_cell** - Execute Jupyter notebook cells

### Git Operations
- **get_changed_files** - View git diffs
- **github-pull-request_*** - Work with pull requests

### Project Setup
- **create_new_workspace** - Initialize new projects
- **create_and_run_task** - Create VS Code tasks

### Advanced
- **runSubagent** - Launch sub-agents for complex tasks
- **manage_todo_list** - Track multi-step work

## 📝 Writing Agent Instructions

### 1. Define Clear Purpose
```markdown
## Purpose
This agent specializes in [specific task]. It helps with:
- Specific capability 1
- Specific capability 2
- Specific capability 3
```

### 2. Specify When to Use
```markdown
## When to Use This Agent
Invoke this agent when you need to:
- Scenario 1
- Scenario 2
- Use case 3
```

### 3. Document Capabilities
```markdown
## Capabilities

### Feature 1: Name
Description of what this does and how it works.

### Feature 2: Name
Description of another capability.
```

### 4. Define Workflow
```markdown
## Workflow

1. **Step 1**: What happens first
2. **Step 2**: Next action
3. **Step 3**: Final steps
```

### 5. Set Clear Boundaries
```markdown
## Limitations

This agent will NOT:
- Thing it won't do 1
- Thing it won't do 2
- Unsafe operation 3
```

### 6. Provide Examples
```markdown
## Example Invocations

**Basic usage:**
`@agent-name do something with file.cs`

**Advanced usage:**
`@agent-name perform complex task with options`
```

## 🎯 Best Practices

### 1. **Be Specific**
- Clearly define the agent's scope
- Don't try to make one agent do everything
- Focus on a specific domain or task type

### 2. **Specify Required Tools**
- Only include tools the agent actually needs
- More tools = more complexity
- Start minimal, add tools as needed

### 3. **Provide Context**
- Explain domain-specific terminology
- Include relevant best practices
- Reference coding standards or patterns

### 4. **Define Output Format**
- Specify how results should be presented
- Include example outputs
- Define success criteria

### 5. **Safety First**
- Always define what the agent WON'T do
- Require confirmation for destructive operations
- Specify when to ask for human input

### 6. **Progress Reporting**
- Tell the agent how to report progress
- Define milestones for long-running tasks
- Specify error handling approach

## 💡 Example Agent Templates

### Simple Code Review Agent
```markdown
```chatagent
---
description: 'Reviews code for common issues and suggests improvements'
tools:
  - read_file
  - grep_search
  - get_errors
---

# Code Review Agent

Review code files for:
- Syntax and compilation errors
- Common anti-patterns
- Style guide violations
- Security concerns

Always provide specific line numbers and fix suggestions.
```\`\`\`
```

### Test Generator Agent
```markdown
```chatagent
---
description: 'Generates unit tests for C# classes'
tools:
  - read_file
  - create_file
  - semantic_search
  - run_in_terminal
---

# Test Generator

Analyze C# classes and generate comprehensive unit tests:
1. Read the source file
2. Identify public methods and properties
3. Generate xUnit test cases
4. Include edge cases and null checks
5. Run tests to verify they compile

Output test files following the naming convention: {ClassName}Tests.cs
```\`\`\`
```

### Documentation Agent
```markdown
```chatagent
---
description: 'Generates and updates project documentation'
tools:
  - read_file
  - create_file
  - file_search
  - semantic_search
---

# Documentation Agent

Create and maintain documentation:
- README files
- API documentation
- Code comments
- Architecture diagrams (in markdown)

Use clear, concise language appropriate for the target audience.
```\`\`\`
```

## 🚀 Invoking Custom Agents

Once created, invoke your agent in chat using:

```
@agent-name [your request]
```

For example:
```
@code-quality-demo analyze the Calculator project
@test-generator create tests for CalculatorOperations.cs
@docs-agent update the README with recent changes
```

## 🔍 Testing Your Agent

1. **Start Simple**: Test with a basic request first
2. **Verify Tool Access**: Ensure the agent can use its tools
3. **Check Boundaries**: Verify it respects its limitations
4. **Test Edge Cases**: Try unusual or complex requests
5. **Iterate**: Refine instructions based on results

## 📊 Advanced Features

### Context Awareness
Agents automatically have access to:
- Current file being edited
- Selected code
- Git repository information
- Workspace structure
- Recent errors

### Chaining Operations
Agents can perform multiple steps:
```markdown
Workflow:
1. Search for relevant files using semantic_search
2. Read each file with read_file
3. Analyze content
4. Generate summary
5. Create output file with create_file
```

### Conditional Logic
Guide the agent with conditional instructions:
```markdown
If errors are found:
- Use get_errors to see compilation issues
- Prioritize fixing critical errors first
- Report each fix as it's applied

If no errors:
- Focus on code quality improvements
- Suggest optimizations
- Update documentation
```

### Sub-Agent Delegation
For complex tasks, delegate to sub-agents:
```markdown
For large codebases:
1. Use runSubagent to analyze each module separately
2. Compile results from each sub-agent
3. Generate unified report
```

## 🎨 Styling Your Agent

### Use Clear Headings
```markdown
# Main Agent Title
## Major Section
### Subsection
```

### Use Lists for Steps
```markdown
1. First step
2. Second step
3. Third step
```

### Use Emphasis
```markdown
**Important**: Always verify before deleting files
*Note*: This may take several minutes
```

### Use Code Blocks
```markdown
Example usage:
`@agent-name command`

Expected output:
```
Result details here
```\`\`\`
```

## 🔐 Security Considerations

### Safe Tool Usage
- **read_file**: Safe for any file
- **create_file**: Safe, but check for overwrites
- **replace_string_in_file**: Requires careful validation
- **run_in_terminal**: CAUTION - can execute any command

### Recommended Safeguards
```markdown
Security rules:
- Never run commands with sudo without explicit approval
- Validate file paths before deletion
- Confirm before modifying production code
- Don't commit secrets or credentials
- Ask before making destructive changes
```

## 📚 Real-World Examples

### CI/CD Helper Agent
```markdown
```chatagent
---
description: 'Helps set up and troubleshoot CI/CD pipelines'
tools:
  - read_file
  - create_file
  - file_search
  - run_in_terminal
---

# CI/CD Agent

Assist with continuous integration and deployment:
- Create GitHub Actions workflows
- Configure build scripts
- Set up test automation
- Troubleshoot pipeline failures
- Review deployment configs
```\`\`\`
```

### Performance Analyzer Agent
```markdown
```chatagent
---
description: 'Analyzes code for performance bottlenecks'
tools:
  - semantic_search
  - read_file
  - grep_search
  - run_in_terminal
---

# Performance Agent

Identify and fix performance issues:
- Profile code execution
- Find N+1 queries
- Detect memory leaks
- Suggest caching strategies
- Recommend algorithm improvements
```\`\`\`
```

## 🎓 Learning Resources

### Start Here
1. Copy the demo agent template
2. Modify the description and purpose
3. Add 2-3 relevant tools
4. Test with a simple request
5. Iterate and improve

### Experiment With
- Different tool combinations
- Various instruction styles
- Multi-step workflows
- Error handling strategies

### Best Learned By Doing
Create agents for tasks you do frequently:
- Code formatting
- Test generation
- Documentation updates
- Refactoring assistance
- Dependency management

## 🤝 Contributing

When creating agents for team use:
1. **Document clearly** - Others need to understand usage
2. **Version control** - Commit agents to repository
3. **Share examples** - Show successful invocations
4. **Gather feedback** - Improve based on team input
5. **Maintain actively** - Update as tools and needs change

## 📞 Getting Help

If your agent isn't working as expected:
1. Check tool names are correct
2. Verify the agent file is in `.github/agents/`
3. Review the agent instructions for clarity
4. Test with simpler requests first
5. Check if tools have access permissions

---

**Ready to create your first agent?** Start with the demo template and customize it for your needs!
