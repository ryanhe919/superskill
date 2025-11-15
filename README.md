# 开发技能库

为 Claude Code 提供的综合性软件开发技能库。该库提供专家级别的代码审查、测试、重构、文档、部署和调试技能。

## 特性

该技能库包含以下专业技能：

- **代码审查**：安全审查、性能分析和代码质量评估
- **测试**：单元测试生成、集成测试和覆盖率分析
- **重构**：代码重构、设计模式和依赖管理
- **文档**：API 文档和代码文档生成
- **部署**：CI/CD 流水线设置和 Docker 容器化
- **调试**：错误分析和错误处理实现

## 技能概览

### 代码审查

#### 安全审查
执行全面的安全代码审查，重点关注：
- 输入验证（SQL 注入、XSS、命令注入）
- 身份验证和授权
- 数据保护和加密
- OWASP Top 10 漏洞

在 Claude Code 中使用：
```
使用 security-review 技能分析此代码库的安全漏洞
```

#### 性能审查
分析代码的性能问题：
- 算法效率（时间/空间复杂度）
- 数据库查询优化
- 内存管理
- 前端性能
- 缓存策略

使用方法：
```
使用 performance-review 技能识别性能瓶颈
```

#### 代码质量审查
确保代码的可维护性和最佳实践：
- 代码结构和组织
- 命名规范
- 代码重复（DRY 原则）
- 错误处理
- 文档质量

使用方法：
```
使用 code-quality 技能审查此代码的可维护性
```

### 测试

#### 单元测试生成器
生成全面的单元测试，包括：
- 完整的测试覆盖率（正常路径、边界情况、错误）
- AAA 模式（准备、执行、断言）
- 模拟外部依赖
- 参数化测试

使用方法：
```
使用 unit-test-generator 技能为此模块创建测试
```

#### 集成测试生成器
创建集成测试，涵盖：
- 组件交互
- API 端点测试
- 数据库操作
- 端到端工作流

使用方法：
```
使用 integration-test-generator 技能测试这些 API 端点
```

#### 测试覆盖率分析器
分析测试覆盖率并识别缺口：
- 覆盖率指标（行、分支、函数）
- 缺失的测试场景
- 风险评估
- 优先级建议

使用方法：
```
使用 test-coverage-analyzer 技能评估我们的测试套件
```

### 重构

#### 代码重构
通过以下方式提高代码质量：
- 提取方法/函数
- 简化条件语句
- 消除重复
- 优化性能
- 处理代码异味

使用方法：
```
使用 code-refactor 技能重构此模块
```

#### 设计模式
识别并实现设计模式：
- 创建型模式（单例、工厂、建造者）
- 结构型模式（适配器、装饰器、外观）
- 行为型模式（策略、观察者、命令）

使用方法：
```
使用 design-patterns 技能改进架构
```

#### 依赖清理
分析和优化依赖项：
- 识别未使用的依赖
- 查找过时的包
- 检测安全漏洞
- 建议更轻量的替代方案

使用方法：
```
使用 dependency-cleanup 技能优化我们的依赖项
```

### 文档

#### API 文档生成器
创建全面的 API 文档：
- 端点文档
- 请求/响应模式
- 身份验证详情
- 多语言代码示例
- 错误参考

使用方法：
```
使用 api-docs-generator 技能为我们的 REST API 编写文档
```

#### 代码文档生成器
生成代码文档：
- 函数/方法文档（JSDoc、docstrings 等）
- 类文档
- 模块文档
- 类型定义
- 使用示例

使用方法：
```
使用 code-docs-generator 技能为此代码库编写文档
```

### 部署

#### CI/CD 流水线设置
为以下平台创建 CI/CD 流水线：
- GitHub Actions
- GitLab CI
- Jenkins
- 代码质量检查、测试、构建和部署

使用方法：
```
使用 ci-cd-setup 技能创建 GitHub Actions 流水线
```

#### Docker 设置
容器化应用程序，包括：
- 多阶段 Dockerfile
- Docker Compose 配置
- 安全最佳实践
- 镜像优化
- Kubernetes 部署清单

使用方法：
```
使用 docker-setup 技能容器化此应用程序
```

### 调试

#### 错误分析器
系统性分析和调试问题：
- 根因分析
- 逐步调试方法
- 常见错误模式
- 修复建议
- 预防策略

使用方法：
```
使用 bug-analyzer 技能调试此错误
```

#### 错误处理器
实现健壮的错误处理：
- 自定义错误类
- Try-catch 模式
- 全局错误处理器
- 输入验证
- 带退避的重试逻辑
- 结构化日志记录

使用方法：
```
使用 error-handler 技能实现错误处理
```

## 安装

### 方式 1：克隆并链接（推荐用于开发）

```bash
# 克隆仓库
git clone https://github.com/ryanhe919/superskill.git

# 进入 Claude Code 技能目录
cd ~/.config/claude-code/skills  # Linux/Mac
# 或
cd %APPDATA%/claude-code/skills  # Windows

# 创建符号链接
ln -s /path/to/superskill/skills/* .
```

### 方式 2：直接复制技能

```bash
# 将技能复制到 Claude Code 目录
cp -r superskill/skills/* ~/.config/claude-code/skills/
```

### 方式 3：使用 Git 子模块

```bash
cd ~/.config/claude-code/skills
git submodule add https://github.com/ryanhe919/superskill.git
```

## 在 Claude Code 中使用

安装后，可以在 Claude Code 中使用自然语言调用技能：

```
使用 security-review 技能分析此身份验证模块
```

```
使用 unit-test-generator 技能为 UserService 类创建测试
```

```
使用 docker-setup 技能容器化此 Node.js 应用程序
```

## 技能分类

```
skills/
├── code-review/
├── testing/
├── refactoring/
├── documentation/
├── deployment/
└── debugging/
```

## 贡献

欢迎贡献！要添加新技能：

1. 在适当的类别目录中创建新的 markdown 文件
2. 遵循现有技能格式：
   - 清晰描述技能的功能
   - 逐步方法论
   - 代码示例
   - 最佳实践
   - 输出格式指南
3. 在 Claude Code 中测试技能
4. 提交拉取请求

## 技能开发指南

每个技能应该：
- 有明确、专注的目的
- 提供系统性的方法论
- 包含实用示例
- 遵循最佳实践
- 尽可能与语言/框架无关
- 提供可操作的输出

## 许可证

MIT License

## 作者

为 Claude Code 创建

## 支持

如有问题或疑问：
- 在 GitHub 上提交 issue
- 查看 Claude Code 文档
- 查阅现有技能示例

## 路线图

计划添加：
- 数据库优化技能
- 架构审查技能
- 迁移辅助技能
- 代码生成模板
- 性能分析技能
- 基础设施即代码技能
