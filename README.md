# Limit Lab Practice Standard - Limit 实验室实践标准

## 开发与审核标准 LPS-1

适用于开发活动与审核。审核时确保需审核内容完全遵循 LPS-1 标准。

### 编码约定 LPS-1000

#### 格式化 LPS-1001: Format

在执行审计时，需确保代码使用约束工具执行格式化或确保目前认定的编码约定。

| 编程语言 | 约束工具/编码约定       |
| -------- | ----------------------- |
| Rust     | `rustfmt —check`        |
| JS/TS    | ESLint & Prettier       |
| Elixir   | Elixir Phoenix 最佳实践 | <!-- TODO: specific name? -->

对于 `Rust`，需确保使用本 repo 中的 `rustfmt.toml` 进行格式化；对于 `JS/TS`，需确保使用本 repo 中的 `prettierrc.config.js` 进行格式化。

#### 大函数拆分 LPS-1002: Large Function

除此之外还需要将超越长度超越特定行数的函数或方法执行拆分，如无法拆分需要在函数或方法前使用注释标注 `FIXME: large function`。

| LANG   | LINES |
| ------ | ----- |
| Elixir | 20    |
| Rust   | 40    |

E.g.

```rust
// FIXME: large function
fn foo() {
    // 45 lines of code
}
```

#### 文档 LSP-1003: Documentation

对于拥有 `pub` 或 `public` 等公开修饰符或公开的函数、方法、成员、属性等需要标注完整文档。
对于非公开函数、方法，如其长度超过 10 行则需要标注完整文档。

| Access    | Documentation          |
| --------- | ---------------------- |
| public    | Full                   |
| otherwise | if over 10 lines, full |

#### 模块化 LPS-1004: Module

对于前端项目，不要使用默认导出，使用命名导出。

```tsx
// True ✅
export const ComponentA = () => {
    return <div>Component A</div>
}

// True ✅
// vite.config.ts
export default defineConfig({
    // ...
})

// False ❌
export default () => {
    return <div>Component A</div>
}
```

#### 类型 LPS-1005: Type

对于 `TypeScript`，如非必要，不要使用 `any` 类型。

原始数据类型尽量让 TypeScript 自己推断，对于对象和函数，需显式声明类型。

```ts
// True ✅
const name = 'Limit'

// False ❌
const name: string = 'Limit'

// True ✅
const names: Array<string> = []

// False ❌
const names = [] // Oops, never[] 😨

// True ✅
const handler: ChangeEventHandler<HTMLInputElement> = (e) => {
    e.target // e.target is HTMLInputElement
}

// False ❌
const handler = (e) => {
    e.target // e is any 😨
}
```

提交代码前需要确保代码通过（Pass） `tsc` 检查。

#### React LPS-1006: React

用于 React 的大部分类型声明都可以从 `@types/react` 取到，如无特殊情况，不要自己声明类型。如果不清楚相关类型，可以翻阅 [React TypeScript Cheatsheets](https://react-typescript-cheatsheet.netlify.app/)

对于函数组件，使用箭头函数和 `React.FC` 声明类型。

```tsx
import { FC, PropsWithChildren } from 'react'

// True ✅
const ComponentA: FC<PropsWithChildren> = props => {
    return <div>Component A</div>
}

// False ❌
const ComponentA = (props: PropsWithChildren) => {
    return <div>Component A</div>
}
```

对于 Props 有多个属性的组件，需将 Props 用 `interface` 声明。

```tsx
interface ComponentAProps extends PropsWithChildren {
    name: string
    link: string
    description: string
}

// True ✅
const ComponentA: FC<ComponentAProps> = props => {
    return <div>Component A</div>
}

// False ❌
const ComponentA: FC<PropsWithChildren<{ name: string }>> = props => {
    return <div>Component A</div>
}
```

#### CSS LPS-1007: CSS

一个项目中最好只使用一种样式方案，如 `tailwind` 或 `CSS Modules` 不要混用。

不要使用任何方式拼接 CSS 类名，对于 `tailwind` 项目，可以使用 `tailwind-merge`，对于其他样式方案，可以使用 `clsx`，如果能拿到 DOM 节点，请使用 `classList`。

```tsx
// True ✅
import { twJoin } from 'tailwind-merge'

export const ComponentA = props => {
    const className = twJoin('text-color-500', props.className)
    return <div className={className}></div>
}

// True ✅
import clsx from 'clsx'

export const ComponentA = props => {
    const className = clsx('homepage__button', props.className)
    return <div className={className}></div>
}

// True ✅  
const button = document.querySelector('.homepage__button')
button.classList.add('text-color-500')

// False ❌
export const ComponentA = props => {
    const className = 'text-color-500' + ` ${props.className}`
    return <div className={className}></div>
}
```

### 版本管理约定 LPS-1100

#### 分支名 LPS-1101

分支名应当全部使用形如 `{category}/{name}` 的名字，其中 `category` 为此分支对应的类型，同 commit 类型（`feat`, `fix`, `refactor`）等，`name` 为此分支的主题，如 `card`, `pipeline` 等。

- True ✅
  - `feat/pipeline`
  - `refactor/card`
- False ❌
  - `lemon-nb`
  - `tHiSiSsOmEbRaNcH`

## 版本发布标准 LPS-2

### 合并 LPS-2000

#### 合并小提交 LPS-2001: Merge Small Commit

在发布对应版本前，需使用 `git rebase -i` 合并针对于同功能修改的小提交

### 测试 LPS-2100

#### 测试覆盖率 LPS-2101: Test Coverage

在发布稳定版本（release）时，需保证测试覆盖率须保证测试覆盖率不能降低超过 5%
在发布预览版本（preview）时，需保证测试覆盖率须保证测试覆盖率不能降低超过 10%

| Publish | Test Converage |
| ------- | -------------- |
| Release | > -5%          |
| Preview | > -10%         |
