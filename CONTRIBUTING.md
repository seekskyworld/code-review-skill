# Contributing to AI Code Review Guide

Thank you for your interest in contributing! This document provides guidelines for contributing to this project.

## How to Contribute

### Reporting Issues

- Use GitHub Issues to report bugs or suggest enhancements
- Provide clear descriptions and examples
- Include relevant code snippets when applicable

### Submitting Changes

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Commit with clear messages (`git commit -m "Add: description"`)
5. Push to your fork (`git push origin feature/your-feature`)
6. Open a Pull Request

## Contribution Types

### Adding New Language Support

When adding a new programming language:

1. Add a new section in `SKILL.md` with:
   - Language-specific review patterns
   - Common anti-patterns with ❌/✅ examples
   - A review checklist

2. Add corresponding entries in `references/common-bugs-checklist.md`

3. Format:
   ```markdown
   ### [Language] Code Review

   ```[language]
   // ❌ Bad pattern
   code example

   // ✅ Good pattern
   code example
   ```

   #### [Language] Review Checklist
   - [ ] Check item 1
   - [ ] Check item 2
   ```

### Adding Framework Patterns

When adding framework-specific patterns:

1. Ensure you reference official documentation
2. Include version numbers (e.g., "React 19", "Vue 3.4+")
3. Provide working code examples
4. Add corresponding checklist items

### Improving Existing Content

- Fix typos or grammatical errors
- Update outdated patterns
- Add more edge cases
- Improve code examples

## Code Example Guidelines

### Format

- Use ❌ for anti-patterns
- Use ✅ for recommended patterns
- Include comments explaining why

### Quality

- Examples should be realistic, not contrived
- Show both the problem and the solution
- Keep examples focused and minimal

## Commit Message Format

```
Type: Short description

Longer description if needed

- Detail 1
- Detail 2
```

Types:
- `Add`: New feature or content
- `Fix`: Bug fix or correction
- `Update`: Update existing content
- `Refactor`: Restructure without changing behavior
- `Docs`: Documentation only changes

## Questions?

Feel free to open an issue for any questions about contributing.
