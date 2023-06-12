# Limit Lab Frontend Code Style Guideline - Limit 实验室前端代码风格指南

除 [Readme](../README.md) 中提到的规范外，还有以下规范：

## JavaScript

1. 如非必要，不要使用默认导出，使用命名导出。

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

默认导出不利于 IDE 和 Linter 的检查，会在代码重构时造成不必要的麻烦。

## TypeScript

1. 如非必要，不要使用 `any` 类型。

2. 原始数据类型尽量让 TypeScript 自己推断，对于对象和函数，需显式声明类型。

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

3. 提交代码前需要确保代码通过（Pass） `tsc` 检查。

## React

1. 用于 React 的大部分类型声明都可以从 `@types/react` 取到，如无特殊情况，不要自己声明类型。如果不清楚相关类型，可以翻阅 [React TypeScript Cheatsheets](https://react-typescript-cheatsheet.netlify.app/)

2. 对于函数组件，使用箭头函数和 `React.FC` 声明类型。

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

3. 对于 Props 有多个属性的组件，需将 Props 用 `interface` 声明。

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

## CSS

1. 一个项目中最好只使用一种样式方案，如 `tailwind` 或 `CSS Modules` 不要混用。

2. 不要使用任何方式拼接 CSS 类名，对于 `tailwind` 项目，可以使用 `tailwind-merge`，对于其他样式方案，可以使用 `clsx`，如果能拿到 DOM 节点，请使用 `classList`。

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
