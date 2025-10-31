# MATLAB Plain Text Live Script Generation

Generate MATLAB Live Scripts in plain text `.m` format suitable for version control and AI-assisted development.

## Required MATLAB Release

R2025a or newer

## Format Rules

### Non-Code Text
- Use `%[text]` syntax for all documentation text
- Follow Markdown conventions within `%[text]` blocks
- Avoid empty `%[text]` lines to prevent unwanted spacing
- Keep each paragraph as a single line to prevent formatting issues

### Section Structure
- Start each section with `%%`
- Follow with `%[text] ## Section Title` for the section header
- Add MATLAB code and `%[text]` documentation as needed
- Use sequential sections separated by `%%` dividers

### Mathematical Notation
- Use single dollar signs for inline LaTeX equations: `$equation$`
- Use double backslashes for LaTeX commands: `$\\alpha$`, `$\\sin(x)$`
- Keep equations within `%[text]` blocks

### Figures and Visualizations
- Create figures implicitly (no explicit `figure()` commands needed)
- Use descriptive titles and labels
- Include axis labels and legends when appropriate

### Lists
- Use Markdown list syntax within `%[text]` blocks
- End bulleted lists with a trailing backslash
- Example:
  ```
  %[text] - First item\
  %[text] - Second item\
  %[text] - Third item\
  ```

## Script Template Structure

Every Live Script should follow this structure:

1. **Title and Description**
   - Main title using `%[text] # Title`
   - Brief description of the script's purpose

2. **Main Content Sections**
   - Multiple `%%` sections with clear headers
   - Mix of MATLAB code and `%[text]` documentation
   - Demonstrate key concepts with examples

3. **Closing Appendix**
   - Metadata section with copyright or author info
   - Version information
   - Related documentation links

## Usage Guidelines

When generating Live Scripts:

- **Be specific**: Request particular topics or demonstrations
- **Request visualizations**: Ask for plots, charts, or figures when needed
- **Specify complexity**: Indicate if the script should be basic, intermediate, or advanced
- **Include examples**: Request concrete code examples for each concept

## Example Request Format

"Generate a plain text MATLAB Live Script about [topic] that:
- Demonstrates [specific concept]
- Includes [visualization type]
- Shows [number] examples
- Uses [complexity level] explanations"

## Benefits of Plain Text Format

- **Version Control Compatible**: Easy to track changes in Git
- **AI Development Ready**: Works seamlessly with AI coding assistants
- **Collaboration Friendly**: Simple to review and merge in pull requests
- **Portable**: Can be edited in any text editor

## Example Structure

```matlab
%[text] # Example Live Script Title
%[text] Brief description of what this script demonstrates.

%%
%[text] ## Introduction
%[text] This section introduces the main concepts.

% Sample MATLAB code
x = linspace(0, 2*pi, 100);
y = sin(x);

%%
%[text] ## Visualization
%[text] Plotting the sine function.

plot(x, y)
title("Sine Function")
xlabel("x")
ylabel("sin(x)")

%%
%[text] ## Conclusion
%[text] Summary of key takeaways.

%%
%[text] ---
%[text] *Copyright notice or metadata*
```
