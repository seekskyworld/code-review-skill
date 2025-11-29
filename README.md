# AI Code Review Guide

[English](#english) | [中文](#中文)

---

## English

> A comprehensive code review skill for Claude Code, covering React 19, Vue 3, Rust, TypeScript, and more.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

### Overview

This is a Claude Code skill designed to help developers conduct effective code reviews. It provides:

- **Language-specific patterns** for React 19, Vue 3, Rust, TypeScript/JavaScript, Python, SQL
- **Modern framework support** including React Server Components, TanStack Query v5, Suspense & Streaming
- **Comprehensive checklists** for security, performance, and code quality
- **Best practices** for giving constructive feedback

### Features

#### Supported Languages & Frameworks

| Category | Coverage |
|----------|----------|
| **React** | Hooks rules, useEffect patterns, useMemo/useCallback, React 19 Actions (useActionState, useFormStatus, useOptimistic), Server Components, Suspense & Streaming |
| **Vue 3** | Composition API, reactivity system, defineProps/defineEmits, watch cleanup |
| **Rust** | Ownership & borrowing, unsafe code review, async/await, error handling (thiserror vs anyhow) |
| **TypeScript** | Type safety, async/await patterns, common pitfalls |
| **TanStack Query** | v5 best practices, queryOptions, useSuspenseQuery, optimistic updates |

#### Content Statistics

- **SKILL.md**: ~1,800 lines of review guidelines and code examples
- **common-bugs-checklist.md**: ~1,200 lines of bug patterns and checklists
- Additional reference files for security review, PR templates, and more

### Installation

#### For Claude Code Users

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

### Usage

Once installed, you can invoke the skill in Claude Code:

```
Use code-review-excellence skill to review this PR
```

Or reference it in your custom commands.

### File Structure

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

### Key Topics Covered

#### React 19

- `useActionState` - Unified form state management
- `useFormStatus` - Access parent form status without prop drilling
- `useOptimistic` - Optimistic UI updates with automatic rollback
- Server Actions integration with Next.js 15+

#### Suspense & Streaming SSR

- Suspense boundary design patterns
- Error Boundary integration
- Next.js 15 streaming with `loading.tsx`
- `use()` Hook for Promise consumption

#### TanStack Query v5

- `queryOptions` for type-safe query definitions
- `useSuspenseQuery` best practices
- Optimistic updates (simplified v5 approach)
- `isPending` vs `isLoading` vs `isFetching`

#### Rust

- Ownership patterns and common pitfalls
- `unsafe` code review requirements (SAFETY comments)
- Async/await patterns (avoiding blocking in async context)
- Error handling (thiserror for libraries, anyhow for applications)

### Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

#### Areas for Contribution

- Additional language support (Go, Java, C++, etc.)
- More framework-specific patterns
- Translations to other languages
- Bug pattern submissions

### License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### References

- [React v19 Official Documentation](https://react.dev/blog/2024/12/05/react-19)
- [TanStack Query v5 Documentation](https://tanstack.com/query/latest)
- [Vue 3 Composition API](https://vuejs.org/guide/extras/composition-api-faq.html)
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)

---

## 中文

> 一个全面的 Claude Code 代码审查技能，覆盖 React 19、Vue 3、Rust、TypeScript 等。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

### 概述

这是一个为 Claude Code 设计的代码审查技能，旨在帮助开发者进行高效的代码审查。它提供：

- **语言特定模式**：覆盖 React 19、Vue 3、Rust、TypeScript/JavaScript、Python、SQL
- **现代框架支持**：包括 React Server Components、TanStack Query v5、Suspense & Streaming
- **全面的检查清单**：安全、性能和代码质量检查
- **最佳实践**：如何提供建设性的反馈

### 特性

#### 支持的语言和框架

| 分类 | 覆盖内容 |
|------|----------|
| **React** | Hooks 规则、useEffect 模式、useMemo/useCallback、React 19 Actions（useActionState、useFormStatus、useOptimistic）、Server Components、Suspense & Streaming |
| **Vue 3** | Composition API、响应性系统、defineProps/defineEmits、watch 清理 |
| **Rust** | 所有权与借用、unsafe 代码审查、async/await、错误处理（thiserror vs anyhow） |
| **TypeScript** | 类型安全、async/await 模式、常见陷阱 |
| **TanStack Query** | v5 最佳实践、queryOptions、useSuspenseQuery、乐观更新 |

#### 内容统计

- **SKILL.md**：约 1,800 行审查指南和代码示例
- **common-bugs-checklist.md**：约 1,200 行错误模式和检查清单
- 额外的安全审查、PR 模板等参考文件

### 安装

#### Claude Code 用户

将技能复制到 Claude Code 插件目录：

```bash
# 克隆仓库
git clone https://github.com/tt-a1i/ai-code-review-guide.git

# 复制到 Claude Code skills 目录
cp -r ai-code-review-guide ~/.claude/commands/code-review-excellence
```

或添加到现有的 Claude Code 插件：

```bash
cp -r ai-code-review-guide/SKILL.md ~/.claude/plugins/your-plugin/skills/code-review/
cp -r ai-code-review-guide/references ~/.claude/plugins/your-plugin/skills/code-review/
```

### 使用方法

安装后，可以在 Claude Code 中调用该技能：

```
使用 code-review-excellence skill 来审查这个 PR
```

或在自定义命令中引用。

### 文件结构

```
ai-code-review-guide/
├── SKILL.md                    # 主技能定义
├── README.md                   # 本文件
├── LICENSE                     # MIT 许可证
├── CONTRIBUTING.md             # 贡献指南
├── references/
│   ├── common-bugs-checklist.md    # 语言特定的错误模式
│   ├── security-review-guide.md    # 安全审查清单
│   └── code-review-best-practices.md
├── assets/
│   ├── review-checklist.md         # 快速参考清单
│   └── pr-review-template.md       # PR 审查评论模板
└── scripts/
    └── pr-analyzer.py              # PR 复杂度分析器
```

### 核心内容

#### React 19

- `useActionState` - 统一的表单状态管理
- `useFormStatus` - 无需 props 透传即可访问父表单状态
- `useOptimistic` - 带自动回滚的乐观 UI 更新
- 与 Next.js 15+ Server Actions 集成

#### Suspense & Streaming SSR

- Suspense 边界设计模式
- Error Boundary 集成
- Next.js 15 streaming 与 `loading.tsx`
- `use()` Hook 消费 Promise

#### TanStack Query v5

- `queryOptions` 类型安全的查询定义
- `useSuspenseQuery` 最佳实践
- 乐观更新（v5 简化方案）
- `isPending` vs `isLoading` vs `isFetching` 区别

#### Rust

- 所有权模式和常见陷阱
- `unsafe` 代码审查要求（SAFETY 注释）
- Async/await 模式（避免在异步上下文中阻塞）
- 错误处理（库用 thiserror，应用用 anyhow）

### 贡献

欢迎贡献！请阅读 [CONTRIBUTING.md](CONTRIBUTING.md) 了解贡献指南。

#### 可贡献的方向

- 添加更多语言支持（Go、Java、C++ 等）
- 更多框架特定模式
- 翻译成其他语言
- 提交错误模式

### 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件。

### 致谢

- 灵感来自软件工程社区的代码审查最佳实践
- React 19 特性来自 React 官方文档
- TanStack Query 文档
- TkDodo 的 React Query 最佳实践博客

### 参考资料

- [React v19 官方文档](https://react.dev/blog/2024/12/05/react-19)
- [TanStack Query v5 文档](https://tanstack.com/query/latest)
- [Vue 3 Composition API](https://vuejs.org/guide/extras/composition-api-faq.html)
- [Rust API 指南](https://rust-lang.github.io/api-guidelines/)
