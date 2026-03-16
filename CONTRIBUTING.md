# 贡献指南 | Contributing Guide

[中文](#中文) | [English](#english)

---

## 中文

感谢您考虑为 A-Stockit 做出贡献！我们欢迎所有形式的贡献，包括但不限于代码、文档、问题报告和功能建议。

### 🌟 贡献方式

#### 1. 报告问题

如果您发现了 bug 或有功能建议：

- 在提交新 issue 前，请先搜索现有 issues 以避免重复
- 使用清晰、描述性的标题
- 提供详细的问题描述，包括：
  - 复现步骤
  - 预期行为
  - 实际行为
  - 环境信息（操作系统、Python 版本等）
  - 相关日志或截图

#### 2. 提交代码

**开始之前：**

1. Fork 本仓库
2. 克隆您的 fork：
   ```bash
   git clone https://github.com/your-username/A-Stockit.git
   cd A-Stockit
   ```
3. 创建新分支：
   ```bash
   git checkout -b feature/your-feature-name
   # 或
   git checkout -b fix/your-bug-fix
   ```

**开发流程：**

1. 安装开发依赖：
   ```bash
   pip install -e .[a_share,feishu]
   pip install -e .[dev]  # 如果有开发依赖
   ```

2. 进行您的更改
3. 确保代码符合项目规范（见下文）
4. 提交更改：
   ```bash
   git add .
   git commit -m "feat: add new feature" # 或 "fix: resolve bug"
   ```
5. 推送到您的 fork：
   ```bash
   git push origin feature/your-feature-name
   ```
6. 创建 Pull Request

#### 3. 改进文档

文档改进同样重要！您可以：

- 修正拼写或语法错误
- 改进现有文档的清晰度
- 添加示例或使用案例
- 翻译文档

### 📋 Pull Request 规范

**PR 标题格式：**

使用语义化提交信息格式：

```
<type>(<scope>): <subject>

类型（type）：
- feat: 新功能
- fix: Bug 修复
- docs: 文档更新
- style: 代码格式（不影响功能）
- refactor: 重构（既不是新功能也不是 bug 修复）
- perf: 性能优化
- test: 测试相关
- chore: 构建过程或辅助工具的变动

范围（scope）可选：
- skill: 技能相关
- core: 核心功能
- market: 市场数据
- docs: 文档
- ci: CI/CD

示例：
- feat(skill): add technical-scan skill
- fix(market): resolve data normalization issue
- docs: update installation guide
```

**PR 描述应包含：**

1. **变更摘要**：简要说明做了什么
2. **动机**：为什么需要这个变更
3. **测试**：如何测试这些变更
4. **相关 Issue**：如果有，使用 `Closes #123` 或 `Fixes #456`
5. **截图**（如适用）：UI 变更或新功能的视觉展示

**PR 检查清单：**

- [ ] 代码遵循项目的代码风格
- [ ] 已添加必要的文档
- [ ] 已添加或更新测试（如适用）
- [ ] 所有测试通过
- [ ] 提交信息遵循规范
- [ ] 已更新 CHANGELOG.md（如适用）

### 💻 代码规范

**Python 代码风格：**

- 遵循 PEP 8 规范
- 使用 4 个空格缩进
- 最大行长度：100 字符
- 使用类型提示（Type Hints）
- 添加文档字符串（Docstrings）

**示例：**

```python
from typing import List, Optional

def analyze_stock(
    symbol: str,
    period: int = 30,
    indicators: Optional[List[str]] = None
) -> dict:
    """
    分析指定股票的技术指标。

    Args:
        symbol: 股票代码
        period: 分析周期（天数）
        indicators: 要计算的指标列表，默认为 None（使用所有指标）

    Returns:
        包含分析结果的字典

    Raises:
        ValueError: 如果股票代码无效
    """
    if not symbol:
        raise ValueError("股票代码不能为空")

    # 实现逻辑...
    return {}
```

**技能开发规范：**

每个新技能必须包含：

1. `SKILL.md` - 技能契约文档，包含：
   - Frontmatter（name, description 等）
   - 输入参数说明
   - 输出格式说明
   - 使用示例

2. `run.py` - 执行器（如果是代码支持的技能）

3. `_docs/skills/<skill-name>.md` - 详细文档

4. 在 `_registry/skills.json` 中注册

**提交信息规范：**

- 使用现在时态："add feature" 而不是 "added feature"
- 使用祈使语气："move cursor to..." 而不是 "moves cursor to..."
- 首行不超过 72 字符
- 如有必要，在空行后添加详细描述

### 🧪 测试

在提交 PR 前，请确保：

1. 运行现有测试：
   ```bash
   pytest
   ```

2. 测试您的更改：
   ```bash
   # 测试特定技能
   python3 skills/astockit/your-skill/run.py <args>
   ```

3. 如果添加新功能，请添加相应测试

### 📝 文档规范

**Markdown 文档：**

- 使用清晰的标题层级
- 代码块指定语言
- 添加目录（对于长文档）
- 使用相对链接引用项目内文件

**SKILL.md 模板：**

```markdown
---
name: skill-name
description: Brief description of what this skill does
argument-hint: [optional-args]
---

# Skill Name

## Purpose

What this skill does and when to use it.

## Usage

\`\`\`bash
/skill-name <args>
\`\`\`

## Parameters

- `param1`: Description
- `param2`: Description

## Output

Description of what the skill returns.

## Examples

\`\`\`bash
/skill-name example-input
\`\`\`
```

### 🔄 开发工作流

1. **同步上游更改**：
   ```bash
   git remote add upstream https://github.com/original-owner/A-Stockit.git
   git fetch upstream
   git merge upstream/main
   ```

2. **保持分支整洁**：
   - 一个 PR 只做一件事
   - 及时 rebase 到最新的 main 分支
   - 避免合并提交（使用 rebase）

3. **响应审查意见**：
   - 及时回复审查意见
   - 进行必要的修改
   - 标记已解决的对话

### 🎯 优先级领域

我们特别欢迎以下方面的贡献：

- 🔧 **新技能开发**：添加新的量化分析技能
- 📊 **数据源集成**：支持更多数据提供商
- 🧪 **测试覆盖**：提高测试覆盖率
- 📖 **文档改进**：完善文档和示例
- 🌐 **国际化**：改进多语言支持
- ⚡ **性能优化**：提升执行效率

### 💬 社区

- **问题讨论**：使用 GitHub Issues
- **功能建议**：使用 GitHub Discussions
- **实时交流**：[加入我们的社区]（待添加）

### 📜 许可证

通过贡献代码，您同意您的贡献将在 MIT 许可证下发布。

---

## English

Thank you for considering contributing to A-Stockit! We welcome all forms of contributions, including but not limited to code, documentation, issue reports, and feature suggestions.

### 🌟 Ways to Contribute

#### 1. Report Issues

If you find a bug or have a feature suggestion:

- Search existing issues before creating a new one to avoid duplicates
- Use a clear, descriptive title
- Provide detailed description including:
  - Steps to reproduce
  - Expected behavior
  - Actual behavior
  - Environment info (OS, Python version, etc.)
  - Relevant logs or screenshots

#### 2. Submit Code

**Before you start:**

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/your-username/A-Stockit.git
   cd A-Stockit
   ```
3. Create a new branch:
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/your-bug-fix
   ```

**Development workflow:**

1. Install development dependencies:
   ```bash
   pip install -e .[a_share,feishu]
   pip install -e .[dev]  # if dev dependencies exist
   ```

2. Make your changes
3. Ensure code follows project standards (see below)
4. Commit your changes:
   ```bash
   git add .
   git commit -m "feat: add new feature" # or "fix: resolve bug"
   ```
5. Push to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```
6. Create a Pull Request

#### 3. Improve Documentation

Documentation improvements are equally important! You can:

- Fix typos or grammar errors
- Improve clarity of existing documentation
- Add examples or use cases
- Translate documentation

### 📋 Pull Request Guidelines

**PR Title Format:**

Use semantic commit message format:

```
<type>(<scope>): <subject>

Types:
- feat: New feature
- fix: Bug fix
- docs: Documentation update
- style: Code formatting (no functional changes)
- refactor: Refactoring (neither feature nor bug fix)
- perf: Performance optimization
- test: Test-related
- chore: Build process or auxiliary tool changes

Scope (optional):
- skill: Skill-related
- core: Core functionality
- market: Market data
- docs: Documentation
- ci: CI/CD

Examples:
- feat(skill): add technical-scan skill
- fix(market): resolve data normalization issue
- docs: update installation guide
```

**PR Description should include:**

1. **Summary of changes**: Brief explanation of what was done
2. **Motivation**: Why this change is needed
3. **Testing**: How to test these changes
4. **Related Issues**: If any, use `Closes #123` or `Fixes #456`
5. **Screenshots** (if applicable): Visual demonstration of UI changes or new features

**PR Checklist:**

- [ ] Code follows project code style
- [ ] Necessary documentation has been added
- [ ] Tests have been added or updated (if applicable)
- [ ] All tests pass
- [ ] Commit messages follow conventions
- [ ] CHANGELOG.md has been updated (if applicable)

### 💻 Code Standards

**Python Code Style:**

- Follow PEP 8 guidelines
- Use 4 spaces for indentation
- Maximum line length: 100 characters
- Use type hints
- Add docstrings

**Example:**

```python
from typing import List, Optional

def analyze_stock(
    symbol: str,
    period: int = 30,
    indicators: Optional[List[str]] = None
) -> dict:
    """
    Analyze technical indicators for a specified stock.

    Args:
        symbol: Stock symbol
        period: Analysis period in days
        indicators: List of indicators to calculate, None for all

    Returns:
        Dictionary containing analysis results

    Raises:
        ValueError: If stock symbol is invalid
    """
    if not symbol:
        raise ValueError("Stock symbol cannot be empty")

    # Implementation logic...
    return {}
```

**Skill Development Standards:**

Each new skill must include:

1. `SKILL.md` - Skill contract documentation with:
   - Frontmatter (name, description, etc.)
   - Input parameter descriptions
   - Output format specifications
   - Usage examples

2. `run.py` - Executor (if code-backed skill)

3. `_docs/skills/<skill-name>.md` - Detailed documentation

4. Registration in `_registry/skills.json`

**Commit Message Guidelines:**

- Use present tense: "add feature" not "added feature"
- Use imperative mood: "move cursor to..." not "moves cursor to..."
- First line should not exceed 72 characters
- Add detailed description after blank line if necessary

### 🧪 Testing

Before submitting a PR, ensure:

1. Run existing tests:
   ```bash
   pytest
   ```

2. Test your changes:
   ```bash
   # Test specific skill
   python3 skills/astockit/your-skill/run.py <args>
   ```

3. Add corresponding tests if adding new features

### 📝 Documentation Standards

**Markdown Documentation:**

- Use clear heading hierarchy
- Specify language for code blocks
- Add table of contents (for long documents)
- Use relative links for project files

**SKILL.md Template:**

```markdown
---
name: skill-name
description: Brief description of what this skill does
argument-hint: [optional-args]
---

# Skill Name

## Purpose

What this skill does and when to use it.

## Usage

\`\`\`bash
/skill-name <args>
\`\`\`

## Parameters

- `param1`: Description
- `param2`: Description

## Output

Description of what the skill returns.

## Examples

\`\`\`bash
/skill-name example-input
\`\`\`
```

### 🔄 Development Workflow

1. **Sync upstream changes**:
   ```bash
   git remote add upstream https://github.com/original-owner/A-Stockit.git
   git fetch upstream
   git merge upstream/main
   ```

2. **Keep branches clean**:
   - One PR should do one thing
   - Rebase to latest main branch regularly
   - Avoid merge commits (use rebase)

3. **Respond to review comments**:
   - Reply to review comments promptly
   - Make necessary changes
   - Mark resolved conversations

### 🎯 Priority Areas

We especially welcome contributions in:

- 🔧 **New Skill Development**: Add new quantitative analysis skills
- 📊 **Data Source Integration**: Support more data providers
- 🧪 **Test Coverage**: Improve test coverage
- 📖 **Documentation Improvement**: Enhance docs and examples
- 🌐 **Internationalization**: Improve multi-language support
- ⚡ **Performance Optimization**: Improve execution efficiency

### 💬 Community

- **Issue Discussion**: Use GitHub Issues
- **Feature Suggestions**: Use GitHub Discussions
- **Real-time Chat**: [Join our community] (to be added)

### 📜 License

By contributing code, you agree that your contributions will be licensed under the MIT License.

---

<div align="center">

**Thank you for contributing to A-Stockit! 🎉**

**感谢您为 A-Stockit 做出贡献！🎉**

</div>
