# Contributing to Learn Linux

Thank you for your interest in contributing to **Learn Linux**! This repository is a community-driven documentation resource for learning Linux fundamentals, and we welcome contributions from everyone — whether you're a seasoned Linux user or just getting started.

## Ways to Contribute

There are many ways you can help improve this project:

- **Improve explanations** — Make complex topics clearer and more beginner-friendly
- **Add new Linux topics** — Expand the curriculum with new documentation
- **Fix typos and errors** — Correct mistakes, broken links, or outdated information
- **Improve documentation structure** — Reorganize content for better learning flow
- **Add practical examples** — Include real-world commands and use cases
- **Enhance formatting** — Improve readability with better Markdown structure
- **Add diagrams** — Create visual aids to complement written content

## Contribution Workflow

Follow these steps to contribute to the project:

### 1. Fork the Repository

Create your own copy of the repository on GitHub by clicking the **Fork** button.

### 2. Clone Your Fork

```bash
git clone https://github.com/YOUR-USERNAME/Learn-Linux.git
cd Learn-Linux
```

### 3. Create a Branch

Create a descriptive branch name for your changes:

```bash
git checkout -b add-shell-scripting-basics
```

### 4. Make Your Changes

Edit or add documentation files in the `docs/` directory. Keep the following in mind:

- Write in clear, beginner-friendly language
- Use clean Markdown formatting
- Ensure technical accuracy with tested commands and examples

### 5. Commit Your Changes

Write a descriptive commit message:

```bash
git add .
git commit -m "Add shell scripting basics documentation"
```

### 6. Push to Your Fork

```bash
git push origin add-shell-scripting-basics
```

### 7. Open a Pull Request

Go to the original repository and click **New Pull Request**. Describe your changes clearly so maintainers can review them effectively.

## Guidelines

To maintain consistency and quality across the project, please follow these guidelines:

### Writing Style

- **Keep explanations beginner-friendly** — Assume the reader is new to Linux
- **Use clean Markdown formatting** — Proper headings, lists, code blocks, and links
- **Ensure technical accuracy** — Test all commands and examples before submitting
- **Be concise but thorough** — Cover topics fully without unnecessary complexity

### Content Structure

- Place new documentation in the `docs/` directory
- Use descriptive file names (kebab-case, e.g., `shell-scripting-basics.md`)
- Include a table of contents for longer documents
- Link related documents together for easy navigation

### Code Examples

- Use bash code blocks with proper syntax highlighting
- Include comments explaining what commands do
- Show both the command and expected output when helpful
- Avoid destructive commands (e.g., `rm -rf`) without clear warnings

### Diagrams and Assets

- Place diagrams and images in the `assets/` directory
- Use descriptive names and include alt text in documentation
- Prefer SVG or vector formats when possible

## Questions?

If you're unsure about anything, feel free to:

- Open an issue to discuss your idea before starting work
- Ask questions in an existing issue or pull request
- Reach out to the maintainers directly

Every contribution, no matter how small, helps make Linux more accessible to everyone. Thank you for being part of this project!
