# MATLAB AI Coding Rules

MATLAB coding rules and guidelines for AI coding assistants like Cursor, Windsurf, Claude Code, and GitHub Copilot.

## Overview

Rule files for AI coding assistants to generate MATLAB code that follows coding standards.

## Available Rules

### [matlab-coding-standards.md](matlab-coding-standards.md)
Comprehensive MATLAB coding standards covering:
- Naming conventions for variables, functions, classes, and properties
- Statement and expression guidelines
- Formatting rules and whitespace conventions
- Function and class authoring best practices
- Error handling patterns

**Use this rule when**: Writing or reviewing any MATLAB code for standards compliance

### [live-script-generation.md](live-script-generation.md)
Guidelines for generating MATLAB Live Scripts in plain text `.m` format:
- Plain text Live Script formatting rules
- Section structure and documentation
- Mathematical notation and LaTeX support
- Visualization best practices
- Version control compatibility

**Use this rule when**: Creating MATLAB Live Scripts that need to be version-controlled or AI-generated

### [matlab-performance-optimization.md](matlab-performance-optimization.md)
Performance optimization guidelines for MATLAB development:
- Profiling and benchmarking techniques
- Vectorization and implicit expansion
- Memory management and pre-allocation
- Parallel computing with parfor and parfeval
- App Designer performance optimization
- JIT compiler optimization patterns
- File I/O performance strategies

**Use this rule when**: Writing performance-critical MATLAB code, optimizing existing code, or building responsive applications

## Quick Start

Choose your AI coding tool and follow the appropriate setup instructions:

### Cursor

**Option 1: Project Rules (Recommended)**

1. Create a `.cursor/rules` directory in your project
2. Copy the rule files you want to use:
   ```bash
   mkdir .cursor/rules
   cp matlab-coding-standards.md .cursor/rules/
   cp live-script-generation.md .cursor/rules/
   cp matlab-performance-optimization.md .cursor/rules/
   ```
3. Cursor will automatically load these rules for your project

**Option 2: Reference with @**

In your Cursor chat, reference rules directly:
```
@matlab-coding-standards.md Write a function to calculate eigenvalues
```

**Option 3: Global Rules**

Add to Cursor Settings > General > Rules for AI:
```
When writing MATLAB code, follow the standards at @matlab-coding-standards.md
```

### Windsurf

**Option 1: Workspace Rules**

1. Create a `.windsurf/rules` directory in your workspace
2. Copy the rule files:
   ```bash
   mkdir .windsurf/rules
   cp matlab-coding-standards.md .windsurf/rules/
   cp live-script-generation.md .windsurf/rules/
   cp matlab-performance-optimization.md .windsurf/rules/
   ```
3. Windsurf will automatically apply these rules to your workspace

**Option 2: Global Rules**

Add to your `global_rules.md`:
```markdown
# MATLAB Development

When working with MATLAB code:
1. Follow naming conventions: lowerCamelCase for functions, UpperCamelCase for classes
2. Use arguments blocks for input validation
3. Limit function inputs to 6 or fewer
4. Use spaces (4 per indent), not tabs
5. Keep lines <= 120 characters

For complete standards, reference: [path/to/matlab-coding-standards.md]
```

### Claude Code

**Option 1: Project CLAUDE.md (Recommended)**

Create a `CLAUDE.md` file in your project root:
```markdown
# MATLAB Project Rules

When working with MATLAB code in this project, import and follow these rules:

@matlab-coding-standards.md
@live-script-generation.md
@matlab-performance-optimization.md
```

**Option 2: Direct Reference**

In your conversation with Claude Code, use the @ symbol:
```
Please review this code for standards compliance @matlab-coding-standards.md

[paste your code]
```

**Option 3: User-Level Rules**

Add to `~/.claude/CLAUDE.md`:
```markdown
# MATLAB Development Rules

When I'm working with MATLAB code, always:
- Follow the standards in @matlab-coding-standards.md
- Use lowerCamelCase for functions and UpperCamelCase for classes
- Validate inputs using arguments blocks
- Keep code readable with proper spacing and comments
```

### GitHub Copilot

**Option 1: Repository Instructions**

Create `.github/copilot-instructions.md`:
```markdown
# MATLAB Coding Standards

This repository uses MATLAB. Follow these standards:

## Naming Conventions
- Functions: lowerCamelCase
- Classes: UpperCamelCase
- Variables: lowerCamelCase (descriptive names)
- Name-Value arguments: UpperCamelCase

## Code Style
- Indentation: 4 spaces (no tabs)
- Line length: <= 120 characters
- Use arguments blocks for input validation
- Limit functions to <= 6 input arguments
- Use " for strings (not ')

## Documentation
- H1 line immediately after function declaration
- Include help text with syntax, inputs, outputs
- Use %% for section dividers

For complete standards, see: matlab-coding-standards.md
```

**Option 2: Path-Scoped Instructions**

Create `.github/instructions/matlab.instructions.md`:
```markdown
---
applyTo:
  - "**/*.m"
  - "**/*.mlx"
---

# MATLAB Code Standards

When generating or modifying MATLAB code:
- Follow naming: lowerCamelCase for functions, UpperCamelCase for classes
- Use arguments blocks for validation
- 4-space indentation, <= 120 char lines
- Include clear function documentation
- Avoid global variables, eval, command syntax in functions

See matlab-coding-standards.md for complete guidelines.
```

## Usage Examples

### Example 1: Writing a New Function

**Cursor / Windsurf / Claude Code:**
```
@matlab-coding-standards.md Write a function that validates email addresses using regular expressions. Include input validation and proper documentation.
```

**GitHub Copilot:**
```
// Write a MATLAB function that validates email addresses
// Follow the project's coding standards
// Include arguments block for validation
```

### Example 2: Creating a Live Script

**Any Tool:**
```
@live-script-generation.md Generate a plain text MATLAB Live Script that demonstrates linear regression with:
- Introduction to the concept
- Sample data generation
- Fitting a linear model
- Visualization with plot
- Explanation of results
```

### Example 3: Reviewing Code

**Claude Code:**
```
Review this MATLAB code for standards compliance @matlab-coding-standards.md

function result = calculateAverage(data)
x=mean(data);
result=x;
end
```

**Cursor:**
```
@matlab-coding-standards.md Check if this code follows our standards and suggest improvements:

[paste code]
```

### Example 4: Refactoring

**Any Tool with access to rules:**
```
Refactor this MATLAB function to follow @matlab-coding-standards.md:
- Improve variable naming
- Add input validation with arguments block
- Fix formatting and spacing
- Add proper documentation

[paste code]
```

### Example 5: Class Design

**With Cursor/Windsurf/Claude Code:**
```
@matlab-coding-standards.md Design a MATLAB value class for representing a 2D point with:
- Properties: X and Y coordinates
- Methods: distance, midpoint, translate
- Input validation
- Proper documentation
```

### Example 6: Performance Optimization

**Any Tool:**
```
@matlab-performance-optimization.md Optimize this MATLAB function that processes large datasets:

[paste code]

Focus on vectorization, memory pre-allocation, and avoiding common anti-patterns.
```

## Combining Multiple Rules

You can reference multiple rule files in a single prompt:

**Cursor / Claude Code:**
```
@matlab-coding-standards.md @live-script-generation.md

Create a plain text Live Script that demonstrates object-oriented programming in MATLAB. Include a class definition and usage examples. Follow all coding standards.
```

**GitHub Copilot (.github/copilot-instructions.md):**
```markdown
# Combined MATLAB Rules

Apply all:
1. MATLAB Coding Standards (matlab-coding-standards.md)
2. Live Script Generation rules (live-script-generation.md)
3. Performance Optimization (matlab-performance-optimization.md)

When creating Live Scripts, use plain text format AND follow naming/formatting standards.
When writing computational code, apply performance optimization best practices.
```

## Best Practices

### Rule File Organization
- **Keep rules modular**: Each file focuses on one aspect of MATLAB development
- **Reference, don't copy**: Use @ references to avoid duplication
- **Update centrally**: Maintain rules in this repo, reference them in projects

### Using Rules Effectively
- **Be specific**: Reference the exact rule file you need
- **Combine rules**: Use multiple @ references when needed
- **Provide context**: Include relevant code or requirements with rule references
- **Request validation**: Ask AI to verify compliance with rules

### Tool-Specific Tips

**Cursor:**
- Project rules in `.cursor/rules` are always active
- Use @ for selective rule application
- Global rules apply to all projects

**Windsurf:**
- Workspace rules are project-specific
- Global rules apply everywhere
- Use file patterns to scope rules

**Claude Code:**
- CLAUDE.md is read automatically on startup
- @ syntax allows dynamic rule inclusion
- Can chain multiple @ references

**GitHub Copilot:**
- Instructions are always active in the repo
- Path-scoped rules target specific file types
- Keep instructions concise (it gets included in every request)

## Contributing

To add new rules or improve existing ones:

1. Create a new `.md` file following the existing format
2. Include a clear header describing the rule's purpose
3. Organize rules into logical sections
4. Add examples where helpful
5. Update this README with usage examples

## License

Based on:
- [MATLAB Coding Guidelines](https://github.com/mathworks/MATLAB-Coding-Guidelines)
- [MATLAB Prompts](https://github.com/matlab/prompts)

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

## Additional Resources

- [MATLAB Coding Guidelines (Full)](https://github.com/mathworks/MATLAB-Coding-Guidelines)
- [MATLAB Documentation](https://www.mathworks.com/help/matlab/)

---

**Note**: Always review AI-generated code and adapt rules to your project's needs.
