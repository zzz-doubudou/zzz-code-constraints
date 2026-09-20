# zzz-code-constraints

[![GitHub](https://img.shields.io/badge/GitHub-zzz--doubudou%2Fzzz--code--constraints-181717?logo=github)](https://github.com/zzz-doubudou/zzz-code-constraints)

一个用于 Codex 的轻量代码约束 Skill。它补充 `karpathy-guidelines`，重点控制修改范围、方法抽取、调用链风险、注释质量和与风险相称的验证。

## 适用场景

显式使用以下方式启用：

```text
Use $zzz-code-constraints for this coding task.
```

Skill 也可以自动发现，但不会把自身作为所有代码任务的通用工作流。数据库脚本、C# 测试细则和库存服务示例只在相关任务中按需读取。

## 核心规则

### 控制范围

- 从目标类型、方法、错误或数据库对象开始定向检查。
- 只读取当前任务需要的上下文，只有依赖、契约、测试或构建错误证明必要时才扩大范围。
- 不顺手重构无关代码，不添加臆测功能。

### 修改决策

- 请求清楚时，检查相关代码后直接实施，并给出简短计划。
- 低风险且有轻微歧义时，采用合理假设并说明，不强制用户在“先计划”和“直接修改”之间二选一。
- 只有用户要求确认，或修改公共 API、数据库 Schema、架构边界、安全、认证、支付、生产数据或不可逆外部行为时，才在修改前确认。

### 方法与封装

- 不为一次性需求创建抽象、包装层或扩展点。
- 不创建只改名、只转发参数、只包装一个条件或只隐藏短表达式的方法。
- 公共方法需要真实业务复用、独立边界，以及明确的可维护性或可读性收益。
- 主业务流程涉及锁、事务、状态、失败回写、补偿、持久化或提交后消息时，应保持关键生命周期可见。

### 方法调用链

方法调用链是一个方法调用另一个方法，被调用方法继续调用其他方法，直到行为完成的连续链路。同一方法内的直接调用数量和方法调用链深度都只是风险信号，不是强制阈值。

需要结合职责边界、状态变化、异常处理、业务判断和维护成本判断是否过度封装。不能仅因为调用数量或层数达到某个数字就暂停、拆分或要求用户决定。只有风险已经影响维护性，或提取、合并、内联会改变业务结构且取舍不明确时，才反馈建议并请求决定。

### 注释与验证

- 方法、属性和常量需要有价值的注释；方法内部的非显然业务判断、状态变化、异常处理、算法和兼容性处理也需要注释。注释不重复代码名称。
- 按修改风险执行相关验证；未执行时说明原因。
- 涉及敏感操作、生产数据或不可逆行为时，保留必要的授权和确认边界。

## 按需参考

- 数据库脚本：读取 `references/database.md`。
- C# 测试和代码组织：读取 `references/testing-csharp.md`。
- 库存操作服务示例：只有目标代码确实属于该领域时，读取 `references/stock-operation-example.md`。

## 安装

将本目录复制到：

```text
<CODEX_HOME>\skills\zzz-code-constraints\
```

未设置 `CODEX_HOME` 时，通常使用：

```text
%USERPROFILE%\.codex\skills\zzz-code-constraints\
```

目录结构至少包含：

```text
zzz-code-constraints/
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
`-- references/
```

## 验证

更新 Skill 后运行：

```powershell
python scripts\quick_validate.py path\to\zzz-code-constraints
```

Skill 目录不声明具体许可证；请根据仓库实际授权方式补充 License。
