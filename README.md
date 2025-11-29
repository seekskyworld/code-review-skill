# AI Code Review Guide

> A comprehensive code review skill for Claude Code, covering React 19, Vue 3, Rust, TypeScript, and more.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

This is a Claude Code skill designed to help developers conduct effective code reviews. It provides:

- **Language-specific patterns** for React 19, Vue 3, Rust, TypeScript/JavaScript, Python, SQL
- **Modern framework support** including React Server Components, TanStack Query v5, Suspense & Streaming
- **Comprehensive checklists** for security, performance, and code quality
- **Best practices** for giving constructive feedback

## Features

### Supported Languages & Frameworks

| Category | Coverage |
|----------|----------|
| **React** | Hooks rules, useEffect patterns, useMemo/useCallback, React 19 Actions (useActionState, useFormStatus, useOptimistic), Server Components, Suspense & Streaming |
| **Vue 3** | Composition API, reactivity system, defineProps/defineEmits, watch cleanup |
| **Rust** | Ownership & borrowing, unsafe code review, async/await, error handling (thiserror vs anyhow) |
| **TypeScript** | Type safety, async/await patterns, common pitfalls |
| **TanStack Query** | v5 best practices, queryOptions, useSuspenseQuery, optimistic updates |

### Content Statistics

- **SKILL.md**: ~1,800 lines of review guidelines and code examples
- **common-bugs-checklist.md**: ~1,200 lines of bug patterns and checklists
- Additional reference files for security review, PR templates, and more

## Installation

### For Claude Code Users

Copy the skill to your Claude Code plugins directory:

```bash
# Clone the repository
git clone https://github.com/tt-a1i/ai-code-review-guide.git

# Copy to Claude Code skills directory
cp -r ai-code-review-guide ~/.claude/commands/code-review-excellence
```

Or add to your existing Claude Code plugin:

```bash
cp -r ai-code-review-guide/SKILL.md ~/.claude/plugins/your-plugin/skills/code-review/
cp -r ai-code-review-guide/references ~/.claude/plugins/your-plugin/skills/code-review/
```

## Usage

Once installed, you can invoke the skill in Claude Code:

```
Use code-review-excellence skill to review this PR
```

Or reference it in your custom commands.

## File Structure

```
ai-code-review-guide/
├── SKILL.md                    # Main skill definition
├── README.md                   # This file
├── LICENSE                     # MIT License
├── CONTRIBUTING.md             # Contribution guidelines
├── references/
│   ├── common-bugs-checklist.md    # Language-specific bug patterns
│   ├── security-review-guide.md    # Security review checklist
│   └── code-review-best-practices.md
├── assets/
│   ├── review-checklist.md         # Quick reference checklist
│   └── pr-review-template.md       # PR review comment template
└── scripts/
    └── pr-analyzer.py              # PR complexity analyzer
```

## Key Topics Covered

### React 19

- `useActionState` - Unified form state management
- `useFormStatus` - Access parent form status without prop drilling
- `useOptimistic` - Optimistic UI updates with automatic rollback
- Server Actions integration with Next.js 15+

### Suspense & Streaming SSR

- Suspense boundary design patterns
- Error Boundary integration
- Next.js 15 streaming with `loading.tsx`
- `use()` Hook for Promise consumption

### TanStack Query v5

- `queryOptions` for type-safe query definitions
- `useSuspenseQuery` best practices
- Optimistic updates (simplified v5 approach)
- `isPending` vs `isLoading` vs `isFetching`

### Rust

- Ownership patterns and common pitfalls
- `unsafe` code review requirements (SAFETY comments)
- Async/await patterns (avoiding blocking in async context)
- Error handling (thiserror for libraries, anyhow for applications)

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Areas for Contribution

- Additional language support (Go, Java, C++, etc.)
- More framework-specific patterns
- Translations to other languages
- Bug pattern submissions

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Inspired by effective code review practices from the software engineering community
- React documentation for React 19 features
- TanStack Query documentation
- TkDodo's blog for React Query best practices

## References

- [React v19 Official Documentation](https://react.dev/blog/2024/12/05/react-19)
- [TanStack Query v5 Documentation](https://tanstack.com/query/latest)
- [Vue 3 Composition API](https://vuejs.org/guide/extras/composition-api-faq.html)
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
