# RAGDB

[![Python 3.11](https://img.shields.io/badge/python-3.11-3776AB?logo=python&logoColor=white)](https://www.python.org/downloads/release/python-3110/) [![PySide6](https://img.shields.io/badge/UI-PySide6-41CD52?logo=qt&logoColor=white)](https://doc.qt.io/qtforpython/) [![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE) [![Latest release](https://img.shields.io/github/v/release/Qorq-byte/RAG_db_test?display_name=tag)](https://github.com/Qorq-byte/RAG_db_test/releases) [![Windows](https://img.shields.io/badge/platform-Windows%20x64-0078D6?logo=windows&logoColor=white)](https://github.com/Qorq-byte/RAG_db_test/releases)

**面向个人资料的本地优先桌面 RAG 知识库，支持导入、检索、引用和学习。**

[English](README.md) · [使用指南](docs/user-guide.md) · [发布版本](https://github.com/Qorq-byte/RAG_db_test/releases) · [项目状态](task_plan.md)

RAGDB 使用 Python 和 PySide6 构建，桌面端和 CLI 共用同一套核心服务。它可以导入本地文件、目录、网页和公开 GitHub 仓库，建立向量与关键词混合索引。你可以查看原文证据、提出带依据的问题，并生成学习内容，同时将知识库保存在本机。

## 主要能力

- 按课程、项目和研究主题管理知识集合
- 支持 TXT、Markdown、PDF、DOCX、PPTX 和常见代码文件
- 支持网页、公开 GitHub 仓库、目录监听和元数据过滤
- 语义向量与 SQLite FTS 混合检索，可选重排模型
- 带来源快照的问答和受保护的证据选择
- 持久化会话、摘要、提纲、笔记、练习题和知识卡片
- 嵌入模型与问答模型独立配置，支持本地 Ollama、Sentence Transformers 和 OpenAI 兼容服务
- 原子化嵌入重建、旧索引维护、备份、诊断和操作日志
- 原生 Windows 桌面端与命令行工具

## 工作流

```text
导入 → 解析 → 切片 → 向量化与索引 → 检索 → 带引用回答
```

活动嵌入配置会和知识库一起保存。更换嵌入模型时，程序会在新的命名空间重建并验证，成功后才切换。检索没有足够证据时，程序会明确返回无法回答的结果，不继续调用问答模型。

## 快速开始

### Windows 安装版

从 [Releases 页面](https://github.com/Qorq-byte/RAG_db_test/releases)下载最新版安装包。安装包包含程序运行环境，不包含模型权重、Ollama、Tesseract 或 API Key。

### 从源码运行

需要 Python 3.11、[uv](https://docs.astral.sh/uv/)、Git 和可用的模型服务。

```powershell
git clone https://github.com/Qorq-byte/RAG_db_test.git
cd RAG_db_test
uv sync --frozen
Copy-Item config.example.toml config.toml
uv run ragdb doctor
uv run ragdb-gui
```

在桌面端“模型”页面或本地 `config.toml` 中配置嵌入和问答模型。API Key 应保存在系统凭据库、环境变量或本机 `.env` 中，不要提交到仓库。

## CLI 示例

```powershell
uv run ragdb collection create ai-notes
uv run ragdb ingest text "RAG 会先检索证据再生成回答。" --collection ai-notes --title "RAG 笔记"
uv run ragdb search "证据" --collection ai-notes
uv run ragdb chat ask "RAG 如何使用检索到的证据？" --collection ai-notes
uv run ragdb task list --collection ai-notes
```

执行 `uv run ragdb --help` 查看完整命令。网页、仓库、备份、索引维护、元数据过滤和评估流程见[使用指南](docs/user-guide.md)。

## 检索基线

固定 `ragdb-learning-v1` 数据集包含 16 条标注查询。当前离线基线为 `Recall@3 = 0.968750`，平均查询耗时 `80.850ms`，P95 为 `113.201ms`。完整报告和复现命令见[基线验收记录](docs/acceptance/2026-11-25-recall-and-scale.md)。

## 隐私与安全

SQLite 数据库、Chroma 索引、导入缓存、会话和日志保存在配置的数据目录中。选择云端嵌入或问答服务时，相关文本和上下文会发送给该服务。启用外部服务前请阅读[使用指南](docs/user-guide.md)。

安全问题请按照 [SECURITY.md](SECURITY.md) 私下报告。公开 Issue 不要附带凭据、签名 URL、私人文档或未脱敏日志。

## 开发

```powershell
uv sync --frozen
uv run pytest -q
uv run python -m compileall -q src tests
```

应用服务位于 `src/ragdb/application`，领域接口位于 `src/ragdb/domain`，基础设施适配器位于 `src/ragdb/infrastructure`，PySide6 页面位于 `src/ragdb/desktop`，桌面端和 CLI 的服务组装位于 `src/ragdb/runtime.py`。

提交 Pull Request 前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。社区参与遵循 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。

## 许可证

RAGDB 使用 [MIT License](LICENSE) 发布。
