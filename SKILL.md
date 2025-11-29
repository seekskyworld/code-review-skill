---
name: code-review-excellence
description: Master effective code review practices to provide constructive feedback, catch bugs early, and foster knowledge sharing while maintaining team morale. Use when reviewing pull requests, establishing review standards, or mentoring developers.
---

# Code Review Excellence

Transform code reviews from gatekeeping to knowledge sharing through constructive feedback, systematic analysis, and collaborative improvement.

## When to Use This Skill

- Reviewing pull requests and code changes
- Establishing code review standards for teams
- Mentoring junior developers through reviews
- Conducting architecture reviews
- Creating review checklists and guidelines
- Improving team collaboration
- Reducing code review cycle time
- Maintaining code quality standards

## Core Principles

### 1. The Review Mindset

**Goals of Code Review:**
- Catch bugs and edge cases
- Ensure code maintainability
- Share knowledge across team
- Enforce coding standards
- Improve design and architecture
- Build team culture

**Not the Goals:**
- Show off knowledge
- Nitpick formatting (use linters)
- Block progress unnecessarily
- Rewrite to your preference

### 2. Effective Feedback

**Good Feedback is:**
- Specific and actionable
- Educational, not judgmental
- Focused on the code, not the person
- Balanced (praise good work too)
- Prioritized (critical vs nice-to-have)

```markdown
❌ Bad: "This is wrong."
✅ Good: "This could cause a race condition when multiple users
         access simultaneously. Consider using a mutex here."

❌ Bad: "Why didn't you use X pattern?"
✅ Good: "Have you considered the Repository pattern? It would
         make this easier to test. Here's an example: [link]"

❌ Bad: "Rename this variable."
✅ Good: "[nit] Consider `userCount` instead of `uc` for
         clarity. Not blocking if you prefer to keep it."
```

### 3. Review Scope

**What to Review:**
- Logic correctness and edge cases
- Security vulnerabilities
- Performance implications
- Test coverage and quality
- Error handling
- Documentation and comments
- API design and naming
- Architectural fit

**What Not to Review Manually:**
- Code formatting (use Prettier, Black, etc.)
- Import organization
- Linting violations
- Simple typos

## Review Process

### Phase 1: Context Gathering (2-3 minutes)

```markdown
Before diving into code, understand:

1. Read PR description and linked issue
2. Check PR size (>400 lines? Ask to split)
3. Review CI/CD status (tests passing?)
4. Understand the business requirement
5. Note any relevant architectural decisions
```

### Phase 2: High-Level Review (5-10 minutes)

```markdown
1. **Architecture & Design**
   - Does the solution fit the problem?
   - Are there simpler approaches?
   - Is it consistent with existing patterns?
   - Will it scale?

2. **File Organization**
   - Are new files in the right places?
   - Is code grouped logically?
   - Are there duplicate files?

3. **Testing Strategy**
   - Are there tests?
   - Do tests cover edge cases?
   - Are tests readable?
```

### Phase 3: Line-by-Line Review (10-20 minutes)

```markdown
For each file:

1. **Logic & Correctness**
   - Edge cases handled?
   - Off-by-one errors?
   - Null/undefined checks?
   - Race conditions?

2. **Security**
   - Input validation?
   - SQL injection risks?
   - XSS vulnerabilities?
   - Sensitive data exposure?

3. **Performance**
   - N+1 queries?
   - Unnecessary loops?
   - Memory leaks?
   - Blocking operations?

4. **Maintainability**
   - Clear variable names?
   - Functions doing one thing?
   - Complex code commented?
   - Magic numbers extracted?
```

### Phase 4: Summary & Decision (2-3 minutes)

```markdown
1. Summarize key concerns
2. Highlight what you liked
3. Make clear decision:
   - ✅ Approve
   - 💬 Comment (minor suggestions)
   - 🔄 Request Changes (must address)
4. Offer to pair if complex
```

## Review Techniques

### Technique 1: The Checklist Method

```markdown
## Security Checklist
- [ ] User input validated and sanitized
- [ ] SQL queries use parameterization
- [ ] Authentication/authorization checked
- [ ] Secrets not hardcoded
- [ ] Error messages don't leak info

## Performance Checklist
- [ ] No N+1 queries
- [ ] Database queries indexed
- [ ] Large lists paginated
- [ ] Expensive operations cached
- [ ] No blocking I/O in hot paths

## Testing Checklist
- [ ] Happy path tested
- [ ] Edge cases covered
- [ ] Error cases tested
- [ ] Test names are descriptive
- [ ] Tests are deterministic
```

### Technique 2: The Question Approach

Instead of stating problems, ask questions to encourage thinking:

```markdown
❌ "This will fail if the list is empty."
✅ "What happens if `items` is an empty array?"

❌ "You need error handling here."
✅ "How should this behave if the API call fails?"

❌ "This is inefficient."
✅ "I see this loops through all users. Have we considered
    the performance impact with 100k users?"
```

### Technique 3: Suggest, Don't Command

```markdown
## Use Collaborative Language

❌ "You must change this to use async/await"
✅ "Suggestion: async/await might make this more readable:
    ```typescript
    async function fetchUser(id: string) {
        const user = await db.query('SELECT * FROM users WHERE id = ?', id);
        return user;
    }
    ```
    What do you think?"

❌ "Extract this into a function"
✅ "This logic appears in 3 places. Would it make sense to
    extract it into a shared utility function?"
```

### Technique 4: Differentiate Severity

```markdown
Use labels to indicate priority:

🔴 [blocking] - Must fix before merge
🟡 [important] - Should fix, discuss if disagree
🟢 [nit] - Nice to have, not blocking
💡 [suggestion] - Alternative approach to consider
📚 [learning] - Educational comment, no action needed
🎉 [praise] - Good work, keep it up!

Example:
"🔴 [blocking] This SQL query is vulnerable to injection.
 Please use parameterized queries."

"🟢 [nit] Consider renaming `data` to `userData` for clarity."

"🎉 [praise] Excellent test coverage! This will catch edge cases."
```

## Language-Specific Patterns

### Python Code Review

```python
# Check for Python-specific issues

# ❌ Mutable default arguments
def add_item(item, items=[]):  # Bug! Shared across calls
    items.append(item)
    return items

# ✅ Use None as default
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

# ❌ Catching too broad
try:
    result = risky_operation()
except:  # Catches everything, even KeyboardInterrupt!
    pass

# ✅ Catch specific exceptions
try:
    result = risky_operation()
except ValueError as e:
    logger.error(f"Invalid value: {e}")
    raise

# ❌ Using mutable class attributes
class User:
    permissions = []  # Shared across all instances!

# ✅ Initialize in __init__
class User:
    def __init__(self):
        self.permissions = []
```

### TypeScript/JavaScript Code Review

```typescript
// Check for TypeScript-specific issues

// ❌ Using any defeats type safety
function processData(data: any) {  // Avoid any
    return data.value;
}

// ✅ Use proper types
interface DataPayload {
    value: string;
}
function processData(data: DataPayload) {
    return data.value;
}

// ❌ Not handling async errors
async function fetchUser(id: string) {
    const response = await fetch(`/api/users/${id}`);
    return response.json();  // What if network fails?
}

// ✅ Handle errors properly
async function fetchUser(id: string): Promise<User> {
    try {
        const response = await fetch(`/api/users/${id}`);
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}`);
        }
        return await response.json();
    } catch (error) {
        console.error('Failed to fetch user:', error);
        throw error;
    }
}

// ❌ Mutation of props
function UserProfile({ user }: Props) {
    user.lastViewed = new Date();  // Mutating prop!
    return <div>{user.name}</div>;
}

// ✅ Don't mutate props
function UserProfile({ user, onView }: Props) {
    useEffect(() => {
        onView(user.id);  // Notify parent to update
    }, [user.id]);
    return <div>{user.name}</div>;
}
```

### React Code Review

React 审查重点：Hooks 规则、性能优化的适度性、组件设计、以及现代 React 19/RSC 模式。

```tsx
// === Hooks 规则 ===

// ❌ 条件调用 Hooks — 违反 Hooks 规则
function BadComponent({ isLoggedIn }) {
  if (isLoggedIn) {
    const [user, setUser] = useState(null);  // Error!
  }
  return <div>...</div>;
}

// ✅ Hooks 必须在组件顶层调用
function GoodComponent({ isLoggedIn }) {
  const [user, setUser] = useState(null);
  if (!isLoggedIn) return <LoginPrompt />;
  return <div>{user?.name}</div>;
}

// === useEffect 常见错误 ===

// ❌ 依赖数组缺失或不完整
function BadEffect({ userId }) {
  const [user, setUser] = useState(null);
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, []);  // 缺少 userId 依赖！
}

// ✅ 完整的依赖数组
function GoodEffect({ userId }) {
  const [user, setUser] = useState(null);
  useEffect(() => {
    let cancelled = false;
    fetchUser(userId).then(data => {
      if (!cancelled) setUser(data);
    });
    return () => { cancelled = true; };  // 清理函数
  }, [userId]);
}

// ❌ useEffect 用于派生状态（反模式）
function BadDerived({ items }) {
  const [filteredItems, setFilteredItems] = useState([]);
  useEffect(() => {
    setFilteredItems(items.filter(i => i.active));
  }, [items]);  // 不必要的 effect + 额外渲染
  return <List items={filteredItems} />;
}

// ✅ 直接在渲染时计算，或用 useMemo
function GoodDerived({ items }) {
  const filteredItems = useMemo(
    () => items.filter(i => i.active),
    [items]
  );
  return <List items={filteredItems} />;
}

// ❌ useEffect 用于事件响应
function BadEventEffect() {
  const [query, setQuery] = useState('');
  useEffect(() => {
    if (query) {
      analytics.track('search', { query });  // 应该在事件处理器中
    }
  }, [query]);
}

// ✅ 在事件处理器中执行副作用
function GoodEvent() {
  const [query, setQuery] = useState('');
  const handleSearch = (q: string) => {
    setQuery(q);
    analytics.track('search', { query: q });
  };
}

// === useMemo / useCallback 过度使用 ===

// ❌ 过度优化 — 常量不需要 useMemo
function OverOptimized() {
  const config = useMemo(() => ({ timeout: 5000 }), []);  // 无意义
  const handleClick = useCallback(() => {
    console.log('clicked');
  }, []);  // 如果不传给 memo 组件，无意义
}

// ✅ 只在需要时优化
function ProperlyOptimized() {
  const config = { timeout: 5000 };  // 简单对象直接定义
  const handleClick = () => console.log('clicked');
}

// ❌ useCallback 依赖总是变化
function BadCallback({ data }) {
  // data 每次渲染都是新对象，useCallback 无效
  const process = useCallback(() => {
    return data.map(transform);
  }, [data]);
}

// ✅ useMemo + useCallback 配合 React.memo 使用
const MemoizedChild = React.memo(function Child({ onClick, items }) {
  return <div onClick={onClick}>{items.length}</div>;
});

function Parent({ rawItems }) {
  const items = useMemo(() => processItems(rawItems), [rawItems]);
  const handleClick = useCallback(() => {
    console.log(items.length);
  }, [items]);
  return <MemoizedChild onClick={handleClick} items={items} />;
}

// === 组件设计 ===

// ❌ 在组件内定义组件 — 每次渲染都创建新组件
function BadParent() {
  function ChildComponent() {  // 每次渲染都是新函数！
    return <div>child</div>;
  }
  return <ChildComponent />;
}

// ✅ 组件定义在外部
function ChildComponent() {
  return <div>child</div>;
}
function GoodParent() {
  return <ChildComponent />;
}

// ❌ Props 总是新对象引用
function BadProps() {
  return (
    <MemoizedComponent
      style={{ color: 'red' }}  // 每次渲染新对象
      onClick={() => {}}         // 每次渲染新函数
    />
  );
}

// ✅ 稳定的引用
const style = { color: 'red' };
function GoodProps() {
  const handleClick = useCallback(() => {}, []);
  return <MemoizedComponent style={style} onClick={handleClick} />;
}

// === Error Boundaries & Suspense ===

// ❌ 没有错误边界
function BadApp() {
  return (
    <Suspense fallback={<Loading />}>
      <DataComponent />  {/* 错误会导致整个应用崩溃 */}
    </Suspense>
  );
}

// ✅ Error Boundary 包裹 Suspense
function GoodApp() {
  return (
    <ErrorBoundary fallback={<ErrorUI />}>
      <Suspense fallback={<Loading />}>
        <DataComponent />
      </Suspense>
    </ErrorBoundary>
  );
}

// === React 19 / Server Components ===

// ❌ 在 Server Component 中使用客户端特性
// app/page.tsx (Server Component by default)
function BadServerComponent() {
  const [count, setCount] = useState(0);  // Error! No hooks in RSC
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}

// ✅ 交互逻辑提取到 Client Component
// app/counter.tsx
'use client';
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}

// app/page.tsx (Server Component)
function GoodServerComponent() {
  const data = await fetchData();  // 可以直接 await
  return (
    <div>
      <h1>{data.title}</h1>
      <Counter />  {/* 客户端组件 */}
    </div>
  );
}

// ❌ 'use client' 放置不当 — 整个树都变成客户端
// layout.tsx
'use client';  // 这会让所有子组件都成为客户端组件
export default function Layout({ children }) { ... }

// ✅ 只在需要交互的组件使用 'use client'
// 将客户端逻辑隔离到叶子组件
```

#### React Review Checklist

```markdown
## Hooks 规则

- [ ] Hooks 在组件/自定义 Hook 顶层调用
- [ ] 没有条件/循环中调用 Hooks
- [ ] useEffect 依赖数组完整
- [ ] useEffect 有清理函数（订阅/定时器/请求）
- [ ] 没有用 useEffect 计算派生状态

## 性能优化（适度原则）

- [ ] useMemo/useCallback 只用于真正需要的场景
- [ ] React.memo 配合稳定的 props 引用
- [ ] 没有在组件内定义子组件
- [ ] 没有在 JSX 中创建新对象/函数（除非传给非 memo 组件）
- [ ] 长列表使用虚拟化（react-window/react-virtual）

## 组件设计

- [ ] 组件职责单一，不超过 200 行
- [ ] 逻辑与展示分离（Custom Hooks）
- [ ] Props 接口清晰，使用 TypeScript
- [ ] 避免 Props Drilling（考虑 Context 或组合）

## 状态管理

- [ ] 状态就近原则（最小必要范围）
- [ ] 复杂状态用 useReducer
- [ ] 全局状态用 Context 或状态库
- [ ] 避免不必要的状态（派生 > 存储）

## 错误处理

- [ ] 关键区域有 Error Boundary
- [ ] Suspense 配合 Error Boundary 使用
- [ ] 异步操作有错误处理

## Server Components (RSC)

- [ ] 'use client' 只用于需要交互的组件
- [ ] Server Component 不使用 Hooks/事件处理
- [ ] 客户端组件尽量放在叶子节点
- [ ] 数据获取在 Server Component 中进行

## 测试

- [ ] 使用 @testing-library/react
- [ ] 用 screen 查询元素
- [ ] 用 userEvent 代替 fireEvent
- [ ] 优先使用 *ByRole 查询
- [ ] 测试行为而非实现细节
```

### React 19 Actions & Forms

React 19 引入了 Actions 系统和新的表单处理 Hooks，简化异步操作和乐观更新。

```tsx
// === useActionState (替代 useFormState) ===

// ❌ 传统方式：多个状态变量
function OldForm() {
  const [isPending, setIsPending] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [data, setData] = useState(null);

  const handleSubmit = async (formData: FormData) => {
    setIsPending(true);
    setError(null);
    try {
      const result = await submitForm(formData);
      setData(result);
    } catch (e) {
      setError(e.message);
    } finally {
      setIsPending(false);
    }
  };
}

// ✅ React 19: useActionState 统一管理
import { useActionState } from 'react';

function NewForm() {
  const [state, formAction, isPending] = useActionState(
    async (prevState, formData: FormData) => {
      try {
        const result = await submitForm(formData);
        return { success: true, data: result };
      } catch (e) {
        return { success: false, error: e.message };
      }
    },
    { success: false, data: null, error: null }
  );

  return (
    <form action={formAction}>
      <input name="email" />
      <button disabled={isPending}>
        {isPending ? 'Submitting...' : 'Submit'}
      </button>
      {state.error && <p className="error">{state.error}</p>}
    </form>
  );
}

// === useFormStatus ===

// ❌ Props 透传表单状态
function BadSubmitButton({ isSubmitting }) {
  return <button disabled={isSubmitting}>Submit</button>;
}

// ✅ useFormStatus 访问父 <form> 状态（无需 props）
import { useFormStatus } from 'react-dom';

function SubmitButton() {
  const { pending, data, method, action } = useFormStatus();
  // 注意：必须在 <form> 内部的子组件中使用
  return (
    <button disabled={pending}>
      {pending ? 'Submitting...' : 'Submit'}
    </button>
  );
}

// ❌ useFormStatus 在 form 同级组件中调用——不工作
function BadForm() {
  const { pending } = useFormStatus();  // 这里无法获取状态！
  return (
    <form action={action}>
      <button disabled={pending}>Submit</button>
    </form>
  );
}

// ✅ useFormStatus 必须在 form 的子组件中
function GoodForm() {
  return (
    <form action={action}>
      <SubmitButton />  {/* useFormStatus 在这里面调用 */}
    </form>
  );
}

// === useOptimistic ===

// ❌ 等待服务器响应再更新 UI
function SlowLike({ postId, likes }) {
  const [likeCount, setLikeCount] = useState(likes);
  const [isPending, setIsPending] = useState(false);

  const handleLike = async () => {
    setIsPending(true);
    const newCount = await likePost(postId);  // 等待...
    setLikeCount(newCount);
    setIsPending(false);
  };
}

// ✅ useOptimistic 即时反馈，失败自动回滚
import { useOptimistic } from 'react';

function FastLike({ postId, likes }) {
  const [optimisticLikes, addOptimisticLike] = useOptimistic(
    likes,
    (currentLikes, increment: number) => currentLikes + increment
  );

  const handleLike = async () => {
    addOptimisticLike(1);  // 立即更新 UI
    try {
      await likePost(postId);  // 后台同步
    } catch {
      // React 自动回滚到 likes 原值
    }
  };

  return <button onClick={handleLike}>{optimisticLikes} likes</button>;
}

// === 配合 Server Actions (Next.js 15+) ===

// ❌ 客户端调用 API
'use client';
function ClientForm() {
  const handleSubmit = async (formData: FormData) => {
    const res = await fetch('/api/submit', {
      method: 'POST',
      body: formData,
    });
    // ...
  };
}

// ✅ Server Action + useActionState
// actions.ts
'use server';
export async function createPost(prevState: any, formData: FormData) {
  const title = formData.get('title');
  await db.posts.create({ title });
  revalidatePath('/posts');
  return { success: true };
}

// form.tsx
'use client';
import { createPost } from './actions';

function PostForm() {
  const [state, formAction, isPending] = useActionState(createPost, null);
  return (
    <form action={formAction}>
      <input name="title" />
      <SubmitButton />
    </form>
  );
}
```

#### React 19 Forms Checklist

```markdown
## useActionState
- [ ] 使用 useActionState 替代多个 useState（isPending/error/data）
- [ ] Action 函数返回新状态，不是直接 setState
- [ ] 正确处理错误状态和成功状态
- [ ] 配合 Server Actions 时标记 'use server'

## useFormStatus
- [ ] 在 <form> 的子组件中调用，不是同级
- [ ] 不依赖 props 传递表单状态
- [ ] 只用于读取状态，不用于触发提交

## useOptimistic
- [ ] 用于需要即时反馈的操作（点赞、收藏等）
- [ ] 提供合理的乐观更新逻辑
- [ ] 服务端失败时 UI 会自动回滚
- [ ] 不用于关键数据（如支付）
```

### Suspense & Streaming SSR

Suspense 和 Streaming 是 React 18+ 的核心特性，在 2025 年的 Next.js 15 等框架中广泛使用。

```tsx
// === 基础 Suspense 用法 ===

// ❌ 传统加载状态管理
function OldComponent() {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    fetchData().then(setData).finally(() => setIsLoading(false));
  }, []);

  if (isLoading) return <Spinner />;
  return <DataView data={data} />;
}

// ✅ Suspense 声明式加载状态
function NewComponent() {
  return (
    <Suspense fallback={<Spinner />}>
      <DataView />  {/* 内部使用 use() 或支持 Suspense 的数据获取 */}
    </Suspense>
  );
}

// === 多个独立 Suspense 边界 ===

// ❌ 单一边界——所有内容一起加载
function BadLayout() {
  return (
    <Suspense fallback={<FullPageSpinner />}>
      <Header />
      <MainContent />  {/* 慢 */}
      <Sidebar />      {/* 快 */}
    </Suspense>
  );
}

// ✅ 独立边界——各部分独立流式传输
function GoodLayout() {
  return (
    <>
      <Header />  {/* 立即显示 */}
      <div className="flex">
        <Suspense fallback={<ContentSkeleton />}>
          <MainContent />  {/* 独立加载 */}
        </Suspense>
        <Suspense fallback={<SidebarSkeleton />}>
          <Sidebar />      {/* 独立加载 */}
        </Suspense>
      </div>
    </>
  );
}

// === Error Boundary 配合 Suspense ===

// ❌ Suspense 没有错误处理
function NoErrorHandling() {
  return (
    <Suspense fallback={<Loading />}>
      <DataComponent />  {/* 抛错会导致整个应用崩溃 */}
    </Suspense>
  );
}

// ✅ Error Boundary 包裹 Suspense
import { ErrorBoundary } from 'react-error-boundary';

function WithErrorHandling() {
  return (
    <ErrorBoundary
      fallback={<ErrorUI />}
      onError={(error) => logError(error)}
    >
      <Suspense fallback={<Loading />}>
        <DataComponent />
      </Suspense>
    </ErrorBoundary>
  );
}

// === Next.js 15 Streaming ===

// app/page.tsx - 自动 Streaming
export default async function Page() {
  // 这个 await 不会阻塞整个页面
  const data = await fetchSlowData();
  return <div>{data}</div>;
}

// app/loading.tsx - 自动 Suspense 边界
export default function Loading() {
  return <Skeleton />;
}

// === 嵌套 Suspense 策略 ===

// ❌ 过深嵌套——难以调试
function OverNested() {
  return (
    <Suspense>
      <Suspense>
        <Suspense>
          <Suspense>
            <Component />
          </Suspense>
        </Suspense>
      </Suspense>
    </Suspense>
  );
}

// ✅ 按用户体验需求分层
function WellStructured() {
  return (
    <Suspense fallback={<AppShell />}>  {/* 应用骨架 */}
      <Layout>
        <Suspense fallback={<NavSkeleton />}>
          <Navigation />  {/* 导航优先 */}
        </Suspense>
        <Suspense fallback={<ContentSkeleton />}>
          <MainContent />  {/* 主内容 */}
        </Suspense>
      </Layout>
    </Suspense>
  );
}

// === use() Hook (React 19) ===

// ✅ 在组件中读取 Promise
import { use } from 'react';

function Comments({ commentsPromise }) {
  const comments = use(commentsPromise);  // 自动触发 Suspense
  return (
    <ul>
      {comments.map(c => <li key={c.id}>{c.text}</li>)}
    </ul>
  );
}

// 父组件创建 Promise，子组件消费
function Post({ postId }) {
  const commentsPromise = fetchComments(postId);  // 不 await
  return (
    <article>
      <PostContent id={postId} />
      <Suspense fallback={<CommentsSkeleton />}>
        <Comments commentsPromise={commentsPromise} />
      </Suspense>
    </article>
  );
}
```

#### Suspense & Streaming Checklist

```markdown
## Suspense 边界设计
- [ ] 按用户体验需求划分 Suspense 边界
- [ ] 独立内容使用独立的 Suspense 边界
- [ ] 避免过深嵌套（2-3 层为宜）
- [ ] 提供有意义的 fallback（骨架屏 > 简单 Spinner）

## 错误处理
- [ ] 每个 Suspense 有对应的 Error Boundary
- [ ] Error Boundary 提供恢复机制（retry 按钮）
- [ ] 错误被正确记录和上报

## Streaming SSR (Next.js)
- [ ] 慢数据使用 Suspense 包裹，不阻塞整个页面
- [ ] loading.tsx 用于路由级别加载状态
- [ ] 理解 Server Component vs Client Component 边界
- [ ] 避免在 layout 层级使用会触发全页 Suspense 的数据获取

## 性能考虑
- [ ] 优先显示重要内容（Navigation, Header）
- [ ] 非关键内容延迟加载
- [ ] 使用 startTransition 处理非紧急更新
```

### TanStack Query (React Query) v5

TanStack Query 是 React 生态中最流行的数据获取库，v5 是当前稳定版本。

```tsx
// === 基础配置 ===

// ❌ 不正确的默认配置
const queryClient = new QueryClient();  // 默认配置可能不适合

// ✅ 生产环境推荐配置
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60 * 5,  // 5 分钟内数据视为新鲜
      gcTime: 1000 * 60 * 30,    // 30 分钟后垃圾回收（v5 重命名）
      retry: 3,
      refetchOnWindowFocus: false,  // 根据需求决定
    },
  },
});

// === 使用 queryOptions (v5 新增) ===

// ❌ 重复定义 queryKey 和 queryFn
function Component1() {
  const { data } = useQuery({
    queryKey: ['users', userId],
    queryFn: () => fetchUser(userId),
  });
}

function prefetchUser(queryClient, userId) {
  queryClient.prefetchQuery({
    queryKey: ['users', userId],  // 重复！
    queryFn: () => fetchUser(userId),  // 重复！
  });
}

// ✅ queryOptions 统一定义，类型安全
import { queryOptions } from '@tanstack/react-query';

const userQueryOptions = (userId: string) =>
  queryOptions({
    queryKey: ['users', userId],
    queryFn: () => fetchUser(userId),
  });

function Component1({ userId }) {
  const { data } = useQuery(userQueryOptions(userId));
}

function prefetchUser(queryClient, userId) {
  queryClient.prefetchQuery(userQueryOptions(userId));
}

// getQueryData 也是类型安全的
const user = queryClient.getQueryData(userQueryOptions(userId).queryKey);

// === 避免常见陷阱 ===

// ❌ staleTime 为 0 导致过度请求
useQuery({
  queryKey: ['data'],
  queryFn: fetchData,
  // staleTime 默认为 0，每次组件挂载都会 refetch
});

// ✅ 设置合理的 staleTime
useQuery({
  queryKey: ['data'],
  queryFn: fetchData,
  staleTime: 1000 * 60,  // 1 分钟内不会重新请求
});

// ❌ 在 queryFn 中使用不稳定的引用
function BadQuery({ filters }) {
  useQuery({
    queryKey: ['items'],  // queryKey 没有包含 filters！
    queryFn: () => fetchItems(filters),  // filters 变化不会触发重新请求
  });
}

// ✅ queryKey 包含所有影响数据的参数
function GoodQuery({ filters }) {
  useQuery({
    queryKey: ['items', filters],  // filters 是 queryKey 的一部分
    queryFn: () => fetchItems(filters),
  });
}

// === Suspense 模式 (v5 稳定) ===

// ❌ 使用 useQuery + enabled 实现条件查询
function BadSuspenseQuery({ userId }) {
  const { data } = useSuspenseQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
    enabled: !!userId,  // useSuspenseQuery 不支持 enabled！
  });
}

// ✅ 组件组合实现条件渲染
function GoodSuspenseQuery({ userId }) {
  // useSuspenseQuery 保证 data 是 T 不是 T | undefined
  const { data } = useSuspenseQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
  });
  return <UserProfile user={data} />;
}

function Parent({ userId }) {
  if (!userId) return <NoUserSelected />;
  return (
    <Suspense fallback={<UserSkeleton />}>
      <GoodSuspenseQuery userId={userId} />
    </Suspense>
  );
}

// === 乐观更新 (v5 简化) ===

// ❌ 手动管理缓存的乐观更新（复杂）
const mutation = useMutation({
  mutationFn: updateTodo,
  onMutate: async (newTodo) => {
    await queryClient.cancelQueries({ queryKey: ['todos'] });
    const previousTodos = queryClient.getQueryData(['todos']);
    queryClient.setQueryData(['todos'], (old) => [...old, newTodo]);
    return { previousTodos };
  },
  onError: (err, newTodo, context) => {
    queryClient.setQueryData(['todos'], context.previousTodos);
  },
  onSettled: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] });
  },
});

// ✅ v5 简化：使用 variables 进行乐观 UI
function TodoList() {
  const { data: todos } = useQuery(todosQueryOptions);
  const { mutate, variables, isPending } = useMutation({
    mutationFn: addTodo,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['todos'] });
    },
  });

  return (
    <ul>
      {todos?.map(todo => <TodoItem key={todo.id} todo={todo} />)}
      {/* 乐观显示正在添加的 todo */}
      {isPending && <TodoItem todo={variables} isOptimistic />}
    </ul>
  );
}

// === 状态字段变化 (v5) ===

// v4: isLoading 表示首次加载或后续获取
// v5: isPending 表示没有数据，isLoading = isPending && isFetching

const { data, isPending, isFetching, isLoading } = useQuery({...});

// isPending: 缓存中没有数据（首次加载）
// isFetching: 正在请求中（包括后台刷新）
// isLoading: isPending && isFetching（首次加载中）

// ❌ v4 代码直接迁移
if (isLoading) return <Spinner />;  // v5 中行为可能不同

// ✅ 明确意图
if (isPending) return <Spinner />;  // 没有数据时显示加载
// 或
if (isLoading) return <Spinner />;  // 首次加载中
```

#### TanStack Query Checklist

```markdown
## 查询配置
- [ ] 使用 queryOptions 统一定义查询
- [ ] queryKey 包含所有影响数据的参数
- [ ] 设置合理的 staleTime（不是默认的 0）
- [ ] gcTime（原 cacheTime）根据需求配置

## Suspense 模式
- [ ] 使用 useSuspenseQuery（不是 useQuery + suspense: true）
- [ ] 不在 useSuspenseQuery 中使用 enabled
- [ ] 条件查询通过组件组合实现

## Mutations
- [ ] 成功后 invalidateQueries 相关查询
- [ ] 使用 variables 实现简单乐观更新
- [ ] 复杂场景使用 onMutate/onError/onSettled

## 性能
- [ ] 避免过多 useQuery 调用（>100 个查询可能影响性能）
- [ ] 大列表考虑分页或虚拟化
- [ ] 使用 select 只订阅需要的数据

## v5 迁移
- [ ] cacheTime → gcTime
- [ ] 理解 isPending vs isLoading 区别
- [ ] useQuery 单一对象参数（移除重载）
```

### Vue 3 Code Review

```vue
<!-- Check for Vue 3 Composition API issues -->

<!-- ❌ 解构 reactive 会丢失响应性 -->
<script setup lang="ts">
const state = reactive({ count: 0, name: 'Vue' })
const { count, name } = state  // 丢失响应性！
</script>

<!-- ✅ 使用 toRefs 保持响应性 -->
<script setup lang="ts">
const state = reactive({ count: 0, name: 'Vue' })
const { count, name } = toRefs(state)  // 保持响应性
// 或者直接使用 ref
const count = ref(0)
const name = ref('Vue')
</script>

<!-- ❌ 直接修改 props -->
<script setup lang="ts">
const props = defineProps<{ user: User }>()
props.user.name = 'New Name'  // 永远不要直接修改 props！
</script>

<!-- ✅ 使用 emit 通知父组件更新 -->
<script setup lang="ts">
const props = defineProps<{ user: User }>()
const emit = defineEmits<{
  update: [name: string]
}>()
const updateName = (name: string) => emit('update', name)
</script>

<!-- ❌ defineProps 缺少类型声明 -->
<script setup lang="ts">
const props = defineProps(['title', 'count'])  // 无类型检查
</script>

<!-- ✅ 使用类型声明 + withDefaults -->
<script setup lang="ts">
interface Props {
  title: string
  count?: number
  items?: string[]
}
const props = withDefaults(defineProps<Props>(), {
  count: 0,
  items: () => []  // 对象/数组默认值需要工厂函数
})
</script>

<!-- ❌ watch 缺少清理函数，可能内存泄漏 -->
<script setup lang="ts">
watch(searchQuery, async (query) => {
  const controller = new AbortController()
  const data = await fetch(`/api/search?q=${query}`, {
    signal: controller.signal
  })
  results.value = await data.json()
  // 如果 query 快速变化，旧请求不会被取消！
})
</script>

<!-- ✅ 使用 onCleanup 清理副作用 -->
<script setup lang="ts">
watch(searchQuery, async (query, _, onCleanup) => {
  const controller = new AbortController()
  onCleanup(() => controller.abort())  // 取消旧请求

  try {
    const data = await fetch(`/api/search?q=${query}`, {
      signal: controller.signal
    })
    results.value = await data.json()
  } catch (e) {
    if (e.name !== 'AbortError') throw e
  }
})
</script>

<!-- ❌ computed 中产生副作用 -->
<script setup lang="ts">
const fullName = computed(() => {
  console.log('Computing...')  // 副作用！
  otherRef.value = 'changed'   // 修改其他状态！
  return `${firstName.value} ${lastName.value}`
})
</script>

<!-- ✅ computed 只用于派生状态 -->
<script setup lang="ts">
const fullName = computed(() => {
  return `${firstName.value} ${lastName.value}`
})
// 副作用放在 watch 或事件处理中
watch(fullName, (name) => {
  console.log('Name changed:', name)
})
</script>

<!-- ❌ v-for 中使用 index 作为 key -->
<template>
  <li v-for="(item, index) in items" :key="index">
    {{ item.name }}
  </li>
</template>

<!-- ✅ 使用唯一标识作为 key -->
<template>
  <li v-for="item in items" :key="item.id">
    {{ item.name }}
  </li>
</template>

<!-- ❌ v-if 和 v-for 同时使用 -->
<template>
  <li v-for="user in users" v-if="user.active" :key="user.id">
    {{ user.name }}
  </li>
</template>

<!-- ✅ 使用 computed 过滤 -->
<script setup lang="ts">
const activeUsers = computed(() =>
  users.value.filter(user => user.active)
)
</script>
<template>
  <li v-for="user in activeUsers" :key="user.id">
    {{ user.name }}
  </li>
</template>

<!-- ❌ 传递 props 到 composable 丢失响应性 -->
<script setup lang="ts">
const props = defineProps<{ userId: string }>()
const { user } = useUser(props.userId)  // 丢失响应性！
</script>

<!-- ✅ 使用 toRef 或 computed 保持响应性 -->
<script setup lang="ts">
const props = defineProps<{ userId: string }>()
const userIdRef = toRef(props, 'userId')
const { user } = useUser(userIdRef)  // 保持响应性
// 或使用 computed
const { user } = useUser(computed(() => props.userId))
</script>
```

#### Vue 3 Review Checklist

```markdown
## 响应性系统
- [ ] ref 用于基本类型，reactive 用于对象
- [ ] 没有解构 reactive 对象（或使用了 toRefs）
- [ ] props 传递给 composable 时保持了响应性
- [ ] shallowRef/shallowReactive 用于大型对象优化

## Props & Emits
- [ ] defineProps 使用 TypeScript 类型声明
- [ ] 复杂默认值使用 withDefaults + 工厂函数
- [ ] defineEmits 有完整的类型定义
- [ ] 没有直接修改 props

## Watchers
- [ ] watch/watchEffect 有适当的清理函数
- [ ] 异步 watch 处理了竞态条件
- [ ] flush: 'post' 用于 DOM 操作的 watcher
- [ ] 避免过度使用 watcher（优先用 computed）

## 模板
- [ ] v-for 使用唯一且稳定的 key
- [ ] v-if 和 v-for 没有在同一元素上
- [ ] 事件处理使用 kebab-case
- [ ] 大型列表使用虚拟滚动

## Composables
- [ ] 相关逻辑提取到 composables
- [ ] composables 返回响应式引用（不是 .value）
- [ ] 纯函数不要包装成 composable
- [ ] 副作用在组件卸载时清理

## 性能
- [ ] 大型组件拆分为小组件
- [ ] 使用 defineAsyncComponent 懒加载
- [ ] 避免不必要的响应式转换
- [ ] v-memo 用于昂贵的列表渲染
```

### Rust Code Review

Rust 审查的重点：编译器能捕获内存安全问题，但审查者需要关注编译器无法检测的问题——业务逻辑、API 设计、性能和可维护性。

```rust
// === 所有权与借用 ===

// ❌ clone() 是"Rust 的胶带"——用于绕过借用检查器
fn bad_process(data: &Data) -> Result<()> {
    let owned = data.clone();  // 为什么需要 clone？
    expensive_operation(owned)
}

// ✅ 审查时问：clone 是否必要？能否用借用？
fn good_process(data: &Data) -> Result<()> {
    expensive_operation(data)  // 传递引用
}

// ❌ Arc<Mutex<T>> 可能隐藏不必要的共享状态
struct BadService {
    cache: Arc<Mutex<HashMap<String, Data>>>,  // 真的需要共享？
}

// ✅ 考虑是否需要共享，或者设计可以避免
struct GoodService {
    cache: HashMap<String, Data>,  // 单一所有者
}

// === Unsafe 代码审查（最关键！）===

// ❌ unsafe 没有安全文档——这是红旗
unsafe fn bad_transmute<T, U>(t: T) -> U {
    std::mem::transmute(t)
}

// ✅ 每个 unsafe 必须解释：为什么安全？什么不变量？
/// Transmutes `T` to `U`.
///
/// # Safety
///
/// - `T` and `U` must have the same size and alignment
/// - `T` must be a valid bit pattern for `U`
/// - The caller ensures no references to `t` exist after this call
unsafe fn documented_transmute<T, U>(t: T) -> U {
    // SAFETY: Caller guarantees size/alignment match and bit validity
    std::mem::transmute(t)
}

// === 异步代码 ===

// ❌ 在 async 上下文中阻塞——会饿死其他任务
async fn bad_async() {
    let data = std::fs::read_to_string("file.txt").unwrap();  // 阻塞！
    std::thread::sleep(Duration::from_secs(1));  // 阻塞！
}

// ✅ 使用异步 API
async fn good_async() {
    let data = tokio::fs::read_to_string("file.txt").await?;
    tokio::time::sleep(Duration::from_secs(1)).await;
}

// ❌ 跨 .await 持有 std::sync::Mutex——可能死锁
async fn bad_lock(mutex: &std::sync::Mutex<Data>) {
    let guard = mutex.lock().unwrap();
    async_operation().await;  // 持锁等待！
    process(&guard);
}

// ✅ 最小化锁范围，或使用 tokio::sync::Mutex
async fn good_lock(mutex: &std::sync::Mutex<Data>) {
    let data = mutex.lock().unwrap().clone();  // 立即释放
    async_operation().await;
    process(&data);
}

// === 错误处理 ===

// ❌ 库代码用 anyhow——调用者无法 match 错误
pub fn parse_config(s: &str) -> anyhow::Result<Config> { ... }

// ✅ 库用 thiserror，应用用 anyhow
#[derive(Debug, thiserror::Error)]
pub enum ConfigError {
    #[error("invalid syntax at line {line}: {message}")]
    Syntax { line: usize, message: String },
    #[error("missing required field: {0}")]
    MissingField(String),
    #[error(transparent)]
    Io(#[from] std::io::Error),
}

pub fn parse_config(s: &str) -> Result<Config, ConfigError> { ... }

// ❌ 吞掉错误上下文
fn bad_error() -> Result<()> {
    operation().map_err(|_| anyhow!("failed"))?;  // 原始错误丢失
    Ok(())
}

// ✅ 使用 context 保留错误链
fn good_error() -> Result<()> {
    operation().context("failed to perform operation")?;
    Ok(())
}

// === 性能 ===

// ❌ 不必要的 collect——中间分配
fn bad_sum(items: &[i32]) -> i32 {
    items.iter()
        .filter(|x| **x > 0)
        .collect::<Vec<_>>()  // 不必要！
        .iter()
        .sum()
}

// ✅ 惰性迭代
fn good_sum(items: &[i32]) -> i32 {
    items.iter().filter(|x| **x > 0).sum()
}

// ❌ 字符串拼接在循环中重复分配
fn bad_concat(items: &[&str]) -> String {
    let mut s = String::new();
    for item in items {
        s = s + item;  // 每次都重新分配！
    }
    s
}

// ✅ 预分配或用 join
fn good_concat(items: &[&str]) -> String {
    items.join("")  // 或用 with_capacity
}

// === Trait 设计 ===

// ❌ 过度抽象——不是 Java，不需要 Interface 一切
trait Processor { fn process(&self); }
trait Handler { fn handle(&self); }
trait Manager { fn manage(&self); }  // Trait 过多

// ✅ 只在需要多态时创建 trait
// 具体类型通常更简单、更快
```

#### Rust Review Checklist

```markdown
## 编译器不能捕获的问题

**业务逻辑正确性**
- [ ] 边界条件处理正确
- [ ] 状态机转换完整
- [ ] 并发场景下的竞态条件

**API 设计**
- [ ] 公共 API 难以误用
- [ ] 类型签名清晰表达意图
- [ ] 错误类型粒度合适

## 所有权与借用

- [ ] clone() 是有意为之，文档说明了原因
- [ ] Arc<Mutex<T>> 真的需要共享状态吗？
- [ ] RefCell 的使用有正当理由
- [ ] 生命周期不过度复杂

## Unsafe 代码（最重要）

- [ ] 每个 unsafe 块有 SAFETY 注释
- [ ] unsafe fn 有 # Safety 文档节
- [ ] 解释了为什么是安全的，不只是做什么
- [ ] 列出了必须维护的不变量
- [ ] unsafe 边界尽可能小
- [ ] 考虑过是否有 safe 替代方案

## 异步/并发

- [ ] 没有在 async 中阻塞（std::fs、thread::sleep）
- [ ] 没有跨 .await 持有 std::sync 锁
- [ ] spawn 的任务满足 'static
- [ ] 锁的获取顺序一致
- [ ] Channel 缓冲区大小合理

## 错误处理

- [ ] 库：thiserror 定义结构化错误
- [ ] 应用：anyhow + context
- [ ] 没有生产代码 unwrap/expect
- [ ] 错误消息对调试有帮助
- [ ] must_use 返回值被处理

## 性能

- [ ] 避免不必要的 collect()
- [ ] 大数据传引用
- [ ] 字符串用 with_capacity 或 write!
- [ ] impl Trait vs Box<dyn Trait> 选择合理
- [ ] 热路径避免分配

## 代码质量

- [ ] cargo clippy 零警告
- [ ] cargo fmt 格式化
- [ ] 文档注释完整
- [ ] 测试覆盖边界条件
- [ ] 公共 API 有文档示例
```

## Advanced Review Patterns

### Pattern 1: Architectural Review

```markdown
When reviewing significant changes:

1. **Design Document First**
   - For large features, request design doc before code
   - Review design with team before implementation
   - Agree on approach to avoid rework

2. **Review in Stages**
   - First PR: Core abstractions and interfaces
   - Second PR: Implementation
   - Third PR: Integration and tests
   - Easier to review, faster to iterate

3. **Consider Alternatives**
   - "Have we considered using [pattern/library]?"
   - "What's the tradeoff vs. the simpler approach?"
   - "How will this evolve as requirements change?"
```

### Pattern 2: Test Quality Review

```typescript
// ❌ Poor test: Implementation detail testing
test('increments counter variable', () => {
    const component = render(<Counter />);
    const button = component.getByRole('button');
    fireEvent.click(button);
    expect(component.state.counter).toBe(1);  // Testing internal state
});

// ✅ Good test: Behavior testing
test('displays incremented count when clicked', () => {
    render(<Counter />);
    const button = screen.getByRole('button', { name: /increment/i });
    fireEvent.click(button);
    expect(screen.getByText('Count: 1')).toBeInTheDocument();
});

// Review questions for tests:
// - Do tests describe behavior, not implementation?
// - Are test names clear and descriptive?
// - Do tests cover edge cases?
// - Are tests independent (no shared state)?
// - Can tests run in any order?
```

### Pattern 3: Security Review

```markdown
## Security Review Checklist

### Authentication & Authorization
- [ ] Is authentication required where needed?
- [ ] Are authorization checks before every action?
- [ ] Is JWT validation proper (signature, expiry)?
- [ ] Are API keys/secrets properly secured?

### Input Validation
- [ ] All user inputs validated?
- [ ] File uploads restricted (size, type)?
- [ ] SQL queries parameterized?
- [ ] XSS protection (escape output)?

### Data Protection
- [ ] Passwords hashed (bcrypt/argon2)?
- [ ] Sensitive data encrypted at rest?
- [ ] HTTPS enforced for sensitive data?
- [ ] PII handled according to regulations?

### Common Vulnerabilities
- [ ] No eval() or similar dynamic execution?
- [ ] No hardcoded secrets?
- [ ] CSRF protection for state-changing operations?
- [ ] Rate limiting on public endpoints?
```

## Giving Difficult Feedback

### Pattern: The Sandwich Method (Modified)

```markdown
Traditional: Praise + Criticism + Praise (feels fake)

Better: Context + Specific Issue + Helpful Solution

Example:
"I noticed the payment processing logic is inline in the
controller. This makes it harder to test and reuse.

[Specific Issue]
The calculateTotal() function mixes tax calculation,
discount logic, and database queries, making it difficult
to unit test and reason about.

[Helpful Solution]
Could we extract this into a PaymentService class? That
would make it testable and reusable. I can pair with you
on this if helpful."
```

### Handling Disagreements

```markdown
When author disagrees with your feedback:

1. **Seek to Understand**
   "Help me understand your approach. What led you to
    choose this pattern?"

2. **Acknowledge Valid Points**
   "That's a good point about X. I hadn't considered that."

3. **Provide Data**
   "I'm concerned about performance. Can we add a benchmark
    to validate the approach?"

4. **Escalate if Needed**
   "Let's get [architect/senior dev] to weigh in on this."

5. **Know When to Let Go**
   If it's working and not a critical issue, approve it.
   Perfection is the enemy of progress.
```

## Best Practices

1. **Review Promptly**: Within 24 hours, ideally same day
2. **Limit PR Size**: 200-400 lines max for effective review
3. **Review in Time Blocks**: 60 minutes max, take breaks
4. **Use Review Tools**: GitHub, GitLab, or dedicated tools
5. **Automate What You Can**: Linters, formatters, security scans
6. **Build Rapport**: Emoji, praise, and empathy matter
7. **Be Available**: Offer to pair on complex issues
8. **Learn from Others**: Review others' review comments

## Common Pitfalls

- **Perfectionism**: Blocking PRs for minor style preferences
- **Scope Creep**: "While you're at it, can you also..."
- **Inconsistency**: Different standards for different people
- **Delayed Reviews**: Letting PRs sit for days
- **Ghosting**: Requesting changes then disappearing
- **Rubber Stamping**: Approving without actually reviewing
- **Bike Shedding**: Debating trivial details extensively

## Templates

### PR Review Comment Template

```markdown
## Summary
[Brief overview of what was reviewed]

## Strengths
- [What was done well]
- [Good patterns or approaches]

## Required Changes
🔴 [Blocking issue 1]
🔴 [Blocking issue 2]

## Suggestions
💡 [Improvement 1]
💡 [Improvement 2]

## Questions
❓ [Clarification needed on X]
❓ [Alternative approach consideration]

## Verdict
✅ Approve after addressing required changes
```

## Resources

- **references/code-review-best-practices.md**: Comprehensive review guidelines
- **references/common-bugs-checklist.md**: Language-specific bugs to watch for
- **references/security-review-guide.md**: Security-focused review checklist
- **assets/pr-review-template.md**: Standard review comment template
- **assets/review-checklist.md**: Quick reference checklist
- **scripts/pr-analyzer.py**: Analyze PR complexity and suggest reviewers
