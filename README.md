# UAV-Detect-Lite

A first research project focused on **RGB UAV (drone) detection** with an emphasis on:
- **lightweight models** (runs on consumer GPUs)
- **small-object performance** (far-away tiny drones)

This repo is structured so you can move from “I can train a baseline” to “I can write a paper” without getting lost.

## What this project is (first paper, safest route)
- Task: UAV detection in RGB images/videos (bounding boxes)
- Main line: lightweight + small-object improvement
- Deliverables: reproducible runs, clean ablations, speed/complexity, size-bucket metrics, paper draft

## What this project is NOT (for now)
- RGB-IR fusion
- heavy multi-object tracking
- multi-UAV collaboration

You can add those later, after you finish the first paper.

## Hardware target
RTX 4060 Laptop (8GB VRAM, Windows WDDM). Use AMP and conservative batch sizes.

## Repo layout
- `docs/` project plan + experiment rules
- `tools/` dataset checks + evaluation scripts
- `models/` model modifications
- `reports/` tables/plots/qualitative figures
- `runs/` raw training runs (usually not committed)
- `paper/` paper draft and figures

Start with: `docs/PROJECT_PLAN.md`

## License
UESTC

# UAV-Detect-Lite（无人机轻量检测）

这是一个面向“第一次科研/第一次发文”的项目：在可见光（RGB）图片/视频中检测无人机（输出目标框）。主线聚焦：
- 轻量化（消费级 GPU 可训练可推理）
- 小目标性能（远距离小无人机更容易漏检）

仓库结构按“从能跑 baseline → 能做消融 → 能写论文”的顺序设计，避免走弯路。

## 项目定位（首篇最稳路线）
- 任务：RGB 无人机检测（bounding box）
- 主线：轻量化 + 小目标提升
- 交付物：可复现实验、严格消融、速度/复杂度、小目标分桶评估、可视化对比、论文草稿

## 暂时不做（后续再扩展）
- RGB-IR 融合
- 重型多目标跟踪
- 多机协同

先把第一篇做完整，再扩展。

## 硬件目标
RTX 4060 Laptop（8GB 显存，Windows WDDM）。训练开启 AMP，batch 保守一点。

## 仓库结构
- `docs/`：计划与实验规范
- `tools/`：数据检查、评估脚本
- `models/`：模型改动
- `reports/`：表格/图/对比结果
- `runs/`：训练输出（一般不提交）
- `paper/`：论文草稿与图表

从 `docs/PROJECT_PLAN.md` 开始。

## License
UESTC

