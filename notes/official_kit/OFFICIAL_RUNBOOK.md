# Official Kaggle Runbook

目标：只使用 Kaggle 官方 Linux、官方 harness、官方模型运行；本地只负责准备提交包和保存版本记录。

## 运行前

```bash
cd /Users/jiamingwei/gitCode/kaggle/notes/official_kit
bash scripts/package.sh
shasum -a 256 submission.zip > submission.zip.sha256
```

记录以下内容：

- Git commit：`git -C /Users/jiamingwei/gitCode/kaggle rev-parse HEAD`
- submission 包 SHA-256：`submission.zip.sha256`
- 运行时间和 Kaggle Notebook 版本
- 使用的任务数据集版本
- 使用的官方 wheelhouse 数据集版本

`submission.zip` 和校验文件只作为本次运行的输入，不直接修改。

## Kaggle Notebook

在 Kaggle Notebook 中添加：

1. `gemma-4-developer-agent` 官方比赛数据；
2. `gemma-4-developer-agent-wheelhouse` 官方依赖数据；
3. 本次生成的 `submission.zip`，或将 submission 目录上传为 Notebook 输入。

Notebook 内完成：

```bash
unzip -q /kaggle/input/<submission-input>/submission.zip -d /kaggle/working/submission
mkdir -p /kaggle/working/wheelhouse
cp /kaggle/input/<wheelhouse-input>/*.whl /kaggle/working/wheelhouse/

pip install --no-index --find-links /kaggle/working/wheelhouse \
  swegemma adk-submission adk-eval-core google-adk
```

先只运行一题：

```bash
swegemma eval \
  --tasks /kaggle/input/<official-data>/tasks.jsonl \
  --snapshots-dir /kaggle/input/<official-data>/snapshots \
  --submission-dir /kaggle/working/submission \
  --results-dir /kaggle/working/results/<run-id> \
  --sandbox docker \
  --task-id rich_3063
```

## 必须保存的回溯材料

将以下内容保存到 Kaggle Notebook 输出或独立 Kaggle Dataset 版本：

```text
run_metadata.json
submission.zip.sha256
任务 ID 和数据集版本
完整命令
安装包列表：pip freeze
results/<run-id>/
  task_results.jsonl
  summary.json
  traces/
  patches/
```

`run_metadata.json` 至少包含：

```json
{
  "run_id": "2025-...",
  "git_commit": "...",
  "submission_sha256": "...",
  "task_ids": ["rich_3063"],
  "harness": "official Kaggle environment",
  "data_version": "...",
  "wheelhouse_version": "..."
}
```

## 运行顺序

```text
本地提交包
→ SHA-256 和 Git 记录
→ Kaggle 官方 Notebook
→ 1 题验证加载和运行
→ 保存完整结果和 trace
→ 5 题回归
→ 再决定是否扩大到完整任务集
```

本机下载的 wheelhouse 不作为 macOS 本地运行依赖；保留作下载缓存，但正式安装和运行都在 Kaggle 官方 Linux 环境完成。
