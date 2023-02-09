# Limit Lab Practice Standard - Limit实验室实践标准

## 审核标准

### 编码约定

#### 格式化

在执行审计时，需确保代码使用约束工具执行格式化或确保目前认定的编码约定。

| 编程语言 | 约束工具/编码约定 |
| ------- | --- |
| Rust | `rustfmt —check` |
| JS/TS | ESLint |
| Elixir | Elixir Phoenix 最佳实践 | <!-- TODO: specific name? -->

#### 通用规范

##### LPS-1001: Large Function

除此之外还需要将超越长度超越特定行数的函数或方法执行拆分，如无法拆分需要在函数或方法前使用注释标注 `large function`。


## 发版标准

### 测试

测试覆盖率须**超过 90%**，并完成测试
