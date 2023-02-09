# Limit Lab Practice Standard - Limit实验室实践标准

## 审核标准 LPS-1

### 编码约定 LPS-1000

#### 格式化 LPS-1001: Format

在执行审计时，需确保代码使用约束工具执行格式化或确保目前认定的编码约定。

| 编程语言 | 约束工具/编码约定 |
| ------- | --- |
| Rust | `rustfmt —check` |
| JS/TS | ESLint |
| Elixir | Elixir Phoenix 最佳实践 | <!-- TODO: specific name? -->

#### 大函数拆分 LPS-1002: Large Function

除此之外还需要将超越长度超越特定行数的函数或方法执行拆分，如无法拆分需要在函数或方法前使用注释标注 `FIXME: large function`。

| LANG   | LINES |
| ------ | ----- |
| Elixir | 20    |
| Rust   | 30    |

E.g.

```rust
// FIXME: large function
fn foo() {
    // 35 lines of code
}
```


## 发版标准 LPS-2

### 合并 LPS-2000

#### 合并小提交 LPS-2001: Merge Small Commit

在发布对应版本前，需使用 `git rebase -i` 合并针对于同功能修改的小提交

### 测试 LPS-2100

#### 测试覆盖率 LPS-2101: Test Coverage

在发布稳定版本（release）时，需保证测试覆盖率须保证测试覆盖率不能降低超过  5%  
在发布预览版本（preview）时，需保证测试覆盖率须保证测试覆盖率不能降低超过 10%

| Publish | Test Converage |
| ------- | -------------- |
| Release | > -5%          |
| Preview | > -10%         |
