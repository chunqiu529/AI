# Python Copilot 指导（编码规范）

目的：为 Copilot 提示和自动补全提供一致的 Python 编码规范指导，提升代码可读性、可维护性与一致性。

适用范围：所有新编写或重构的 Python 代码（项目层面可按需覆盖）。

主要规范要点：

- 风格与格式化：
  - 遵循 PEP 8；项目中启用 `black`（建议行宽 88）和 `isort` 统一格式与导入排序。
  - 使用 4 空格缩进（由 `black` 管理时无需手动对齐）。

- 命名规范：
  - 模块、函数、变量：使用小写字母与下划线（snake_case）。
  - 类名：使用首字母大写的驼峰（CamelCase）。
  - 常量：使用全大写带下划线（UPPER_SNAKE_CASE）。
  - 私有成员以单前导下划线 `_name` 表示。

- 注释与文档：
  - 所有公共函数/类要有 docstring，使用 Google 或 NumPy 风格（团队内统一）。
  - 简短函数内注释使用英文或项目首选语言，必要时说明“为什么”而非“如何”。

- 类型与静态检查：
  - 优先在公共 API 上添加类型注解（函数参数与返回值）。
  - 在 CI 中运行 `mypy` 做静态类型检查（逐步覆盖已有代码）。

- 导入与依赖：
  - 遵循分组原则：标准库、第三方、项目本地；各组之间空一行。
  - 禁止使用通配符导入（from module import *）。

- 错误处理与异常：
  - 避免空的 `except:`，捕获具体异常类型。
  - 使用自定义异常类来表达特定错误语义。

- 可读性与函数粒度：
  - 函数应短小、职责单一；复杂逻辑拆分成小函数并加单元测试。
  - 单行不超过 88 个字符（与 `black` 保持一致）。

- 常见陷阱：
  - 避免可变默认参数（使用 None 并在函数内初始化）。
  - 优先使用 `with` 上下文管理资源（文件、锁、网络连接）。

- 字符串与格式化：
  - 使用 f-string（Python 3.6+）替代 `%` 或 `str.format()`。

- 日志：
  - 使用 `logging` 模块记录信息，禁止使用 print 做生产日志。
  - 日志级别明确（DEBUG/INFO/WARNING/ERROR/CRITICAL），不要在库代码中配置根日志器。

- 测试与 CI：
  - 编写单元测试（推荐 pytest），测试要覆盖关键逻辑与边界条件。
  - 在 CI 中运行格式化、静态检查、单元测试（pre-commit + CI pipeline）。

- 代码审查与提交：
  - 小步提交、清晰的提交信息；提交前确保本地通过格式化和 lint 检查。

示例（命名与类型）：

```python
class DataProcessor:
    def __init__(self, config: dict) -> None:
        self.config = config

    def process(self, items: list[str]) -> dict:
        """处理条目并返回统计结果。

        Args:
            items: 字符串列表

        Returns:
            包含统计信息的字典
        """
        result: dict = {}
        # 处理逻辑...
        return result
```

工具链建议：

- `black`、`isort`、`flake8`、`mypy`、`pytest`、`pre-commit`

备注：本文件为团队级模板，项目可基于此进一步细化约定（例如 docstring 风格、行宽、是否强制类型覆盖比例等）。
