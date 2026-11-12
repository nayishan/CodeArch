# CodeArch — Gemma 4 Developer Agent 复现工程

复现 Kaggle **"Google - The Gemma 4 Developer Agent Competition"** 的 SWE-bench 风格编码 agent 环境：
1:1 还原官方 9 个工具 + 代码图/embedding 接口，并搭建本地 mini-benchmark 用于量化评测。

## 快速开始

```bash
bash scripts/bootstrap.sh          # 恢复 venv、依赖、外部仓库
source .venv/bin/activate

python examples/test_tools.py     # 验证 9 个工具
python bench/run_eval.py --oracle # 验证 benchmark 判分器（应 15/15）
```

## 目录结构

```
src/codetools/
├── graph_builder.py   # AST 调用图 → NetworkX MultiDiGraph（与官方 graphs/*.json schema 1:1）
├── tools.py           # AgentTools：官方 9 工具 + localize_code
└── localize.py        # BM25 代码定位（数据证明强于官方 search_similar_code）

bench/
├── build_tasks.py     # 从 git 历史挖 SWE-bench 任务 + Fail-to-Pass/Pass-to-Pass 校验
├── run_eval.py        # 评测 agent，复刻官方 Phase 2 判分
└── tasks/tasks.jsonl  # 15 个 rich 任务

submission_skills/     # 官方 ADK skill 格式（可直接进 submission.zip）
notes/                 # 官方文档抓取 + 情报汇总
examples/              # mini_repo 测试仓库 + 工具验证脚本
```

## 官方 9 个工具

| # | 工具 | 签名 |
|---|---|---|
| 1 | `run_command` | `(command: str) -> str` |
| 2 | `submit_patch` | `() -> str` |
| 3 | `get_status` | `() -> str` |
| 4 | `read_file` | `(filepath, start_line=None, end_line=None) -> str` |
| 5 | `edit_file` | `(filepath, old_string, new_string, allow_multiple=False) -> str` |
| 6 | `write_file` | `(filepath, content) -> str` |
| 7 | `get_code_neighbors` | `(node, edge_type=None, max_neighbors=50) -> str` |
| 8 | `search_similar_code` | `(query, k=10) -> str` |
| 9 | `get_code_subgraph` | `(nodes: list[str]) -> str` |

## 关键情报（来自公开审计仓库）

- 官方 `search_similar_code` 接受整段 issue 时，命中 gold symbol 的比例 **0%**（必须先提取标识符）
- 官方边类型**只有 `calls`**，文档里的 `DEFINED_IN`/`IMPORTS` 过滤返回 0（静默失败）
- 129 公开任务：fastapi 67 / rich 48 / requests 13 / httpx 1；gold patch 中位 **1 文件 +8/-2 行**
- 上下文溢出是头号杀手：默认 `max_output_tokens=16384` 导致 26/120 崩溃；减半后 +6.2pp

## 环境

macOS / Python 3.14 / Ollama `gemma4:e4b`。官方 checkpoint `gemma-4-e4b-it-qat-w4a16-ct` 不可获取，
本地开发用 Ollama MLX 版近似；benchmark 结论不能直接外推到官方隐藏集。
