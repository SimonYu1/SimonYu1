# Git 学习大纲

## 一、Git 基础概念

### 1. 版本控制简介
版本控制是一种记录文件变化的系统，能够让用户在需要时回溯到某个特定版本。它广泛应用于软件开发、文档管理等领域，帮助团队协作和个人项目管理。

### 2. Git 与其他版本控制工具的区别
- **集中式版本控制（如 SVN）**：所有版本记录存储在中央服务器，用户需联网才能操作。
- **分布式版本控制（如 Git）**：每个用户都有完整的版本库副本，支持离线操作。
- Git 的分布式特性使其更适合现代开发场景，尤其是分布式团队协作。

### 3. Git 的工作原理
Git 使用快照机制记录文件状态，而非传统的差异比较。每次提交时，Git 会保存文件的完整快照，只有文件发生变化时才存储新的版本。这种设计使 Git 更高效且易于恢复。

### 4. Git 的核心概念
- **工作区（Working Directory）**：用户当前操作的文件目录。
- **暂存区（Staging Area）**：用于保存即将提交的文件快照。
- **版本库（Repository）**：包含所有提交记录的数据库，分为本地版本库和远程版本库。

### 5. Git 的优势
- 高效的分支管理。
- 支持离线操作。
- 强大的社区支持和工具生态。

## 二、Git 配置
- 基本配置（用户名、邮箱、编辑器等）
- SSH 密钥配置

## 三、Git 仓库操作
- 初始化仓库（git init）
- 克隆仓库（git clone）
- 仓库结构与文件状态

## 四、基本命令与操作
- 文件的添加与提交（git add, git commit）
- 查看状态与历史（git status, git log）
- 文件修改与撤销（git checkout, git reset, git revert）

## 五、分支管理
- 分支的创建与切换（git branch, git checkout）
- 分支合并（git merge, git rebase）
- 分支冲突解决
- 分支删除与管理

## 六、远程仓库操作
- 添加远程仓库（git remote）
- 推送与拉取（git push, git pull, git fetch）
- 远程分支管理

## 七、标签管理
- 创建与删除标签（git tag）
- 标签的推送与拉取

## 八、Git 工作流
- 常见工作流（集中式、功能分支、Git Flow、Forking Flow）
- 团队协作流程

## 九、进阶操作
- Stash 临时保存（git stash）
- Cherry-pick 选择性提交（git cherry-pick）
- Submodule 子模块管理

## 十、Git 常见问题与解决方案
- 冲突处理
- 回滚与恢复
- 清理无用数据

## 十一、Git 与 GitHub/GitLab 集成
- 账号注册与仓库托管
- Pull Request/Merge Request 流程
- Issue 与项目管理
