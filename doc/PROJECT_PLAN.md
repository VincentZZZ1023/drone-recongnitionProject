# Project Plan (12–14 weeks, full-time)

This is the “safest” plan for a first paper: **RGB UAV detection**, with **one or two small, defensible model changes** and **clean evidence** (ablations + speed/complexity + small-object buckets).

---

## High-level milestones
- Week 0: lock decisions (framework, task, metrics)
- Week 1: dataset pipeline + sanity checks
- Week 2: baseline training + evaluation + failure library + size buckets
- Weeks 3–4: clean ablations (attention + lightweight conv)
- Weeks 5–6: small-object enhancement (choose ONE)
- Weeks 7–8: full comparison + final model freeze
- Weeks 8–9: generalization/robustness slices
- Weeks 9–12: paper writing (parallel with reruns)
- Weeks 12–14: buffer (3 seeds, polish, release)

---

## Week 0 (2–3 days): lock decisions
### Tasks
- Choose framework: Ultralytics YOLOv8
- Fix task definition: UAV detection (single class recommended)
- Fix evaluation protocol: mAP + PR + FPS + Params/FLOPs + size buckets
- Fix experiment logging format and naming rules

### Deliverables
- `docs/SPEC.md` (task, scope, metrics, target FPS, dataset plan)
- `docs/EXPERIMENT_PROTOCOL.md` (reproducibility rules)

### Gate (don’t move on until this passes)
- One dry-run training command runs without errors
- You have a single source of truth for configs

---

## Week 1: dataset pipeline + sanity checks
### Tasks
- Convert dataset to YOLO format:
  - `datasets/uav/images/{train,val,test}`
  - `datasets/uav/labels/{train,val,test}`
- Write dataset sanity checks:
  - broken images
  - empty labels
  - invalid boxes (out of range)
  - wrong class ids
- Visualize at least 50 labeled samples (random)

### Deliverables
- `tools/check_dataset.py`
- `docs/DATASET_CARD.md` (source, split, label definition, examples)
- `reports/dataset_preview/` (image grids)

### Gate
- 0 critical dataset errors
- Visual checks confirm boxes are aligned

---

## Week 2: baseline + evaluation + failure library + size buckets
### Tasks
- Train baseline YOLOv8n @ imgsz=640 with AMP
- Evaluate baseline:
  - mAP50, mAP50-95, Recall, PR curve
  - Params, FLOPs
  - FPS/latency at fixed imgsz
- Build a failure library (>= 30 examples), categorized:
  - small/far objects missed
  - motion blur
  - backlight
  - clutter background
  - false positives
- Implement size-bucket evaluation (Small / Medium / Large by bbox area)

### Deliverables
- `reports/baseline/metrics.json`
- `plots/PR_curve_baseline.png`
- `docs/FAILURE_CASES.md` + `reports/failure_cases/`
- `tools/eval_by_size.py` + `reports/baseline/size_buckets.csv`

### Gate
- Baseline is repeatable across two runs (same config, similar metrics)
- You can explain the top 2–3 failure modes with evidence

---

## Weeks 3–4: clean ablations (attention + lightweight conv)
Goal: make one or two changes that are easy to defend and easy to measure.

### Strict experiment matrix (control variables)
- A0: baseline (YOLOv8n)
- A1: + attention (CBAM-style) at ONE insertion point
- A2: + lightweight conv (Ghost/Depthwise) at ONE block (e.g., head)
- A3: A1 + A2 combined

### Tasks
- Keep training settings identical across A0–A3
- Report for each run:
  - overall metrics
  - size-bucket metrics
  - speed/params/flops
  - qualitative comparisons (same images/frames)

### Deliverables
- `reports/ablation_A/summary_table.csv`
- `reports/ablation_A/qualitative/`
- `models/` minimal modules (attention/conv)

### Gate
- A3 improves Small bucket metric vs A0
- Cost is documented and acceptable

---

## Weeks 5–6: small-object enhancement (choose ONE)
Pick ONE path to avoid exploding experiments.

### Option S1 (method-level, stronger): add a high-res head (P2)
- Expect better small-object results
- Watch FPS cost

### Option S2 (training-level, safer): training tweaks
- imgsz 640 → 768 (or similar)
- crop strategy / copy-paste small objects if available

### Deliverables
- `reports/small_object/size_bucket_comparison.csv`
- `docs/DESIGN_CHOICE.md` (why S1 or S2)

### Gate
- Clear small-object improvement with bounded cost

---

## Weeks 7–8: full comparisons + freeze final model
### Tasks
- Compare with 2–3 baselines under the same protocol
- Freeze final model + config snapshot (stop changing architecture)
- Standardize speed benchmark (warmup + average)

### Deliverables
- `reports/final/comparison_table.csv`
- `reports/final/speed_benchmark.md`
- `models/final_config_snapshot.*`

### Gate
- Final model is frozen
- All key tables can be reproduced

---

## Weeks 8–9: generalization + robustness slices
### Tasks
- Cross-scene test (optional: small self-collected set)
- Robustness slices:
  - backlight
  - motion blur
  - clutter background
- Produce qualitative grids (before/after)

### Deliverables
- `reports/generalization/`
- `plots/robustness_slices.png`

---

## Weeks 9–12: paper writing (parallel with reruns)
### Recommended writing order
1) Experiments
2) Method
3) Introduction
4) Related Work

### Must-have paper evidence
- contributions (2–3 bullets)
- ablation tables (A0–A3 + S*)
- size-bucket evaluation
- speed/complexity analysis
- failure cases + limitations

### Deliverables
- `paper/` (LaTeX or Markdown)
- `paper/figures/` generated from scripts
- `docs/REPRODUCIBILITY.md`

---

## Weeks 12–14: buffer (stability + polish)
### Tasks
- Final model with 3 seeds (mean/std)
- Clean plots/tables, consistent formatting
- Tag and release v1.0

### Deliverables
- GitHub Release v1.0
- `RELEASE_NOTES.md`
# 项目计划（12–14 周，全天投入）

这是“首篇最稳”的计划：只做 **RGB 无人机检测**，只做 **1–2 个小而好解释的改动**，用 **干净的证据**（消融 + 速度/复杂度 + 小目标分桶）把论文撑起来。

---

## 里程碑总览
- Week 0：固定选型（框架/任务/指标）
- Week 1：数据管线 + 数据检查
- Week 2：baseline 训练 + 评估 + 失败案例库 + 分桶评估
- Weeks 3–4：严格消融（注意力 + 轻量卷积）
- Weeks 5–6：小目标专项增强（只选一个）
- Weeks 7–8：全面对比 + 最终模型冻结
- Weeks 8–9：泛化/鲁棒性切片
- Weeks 9–12：写论文（与复跑并行）
- Weeks 12–14：缓冲（3 seeds、打磨、release）

---

## Week 0（2–3 天）：固定三件事（后面别反复改）
### 任务
- 框架：Ultralytics YOLOv8
- 任务：无人机检测（建议单类别最稳）
- 评估协议：mAP + PR + FPS + Params/FLOPs + 小目标分桶
- 实验记录格式与命名规则固定

### 交付物
- `docs/SPEC.md`（任务定义、范围、指标、目标 FPS、数据计划）
- `docs/EXPERIMENT_PROTOCOL.md`（复现实验规则）

### 验收门槛
- 能无报错跑一次 dry-run 训练
- 配置来源唯一，不混乱

---

## Week 1：数据闭环 + 数据检查
### 任务
- 数据整理成 YOLO 结构：
  - `datasets/uav/images/{train,val,test}`
  - `datasets/uav/labels/{train,val,test}`
- 写数据检查脚本：
  - 坏图
  - 空标
  - 越界框/非法框
  - 类别 id 错
- 可视化随机至少 50 张样本（确认框位置正确）

### 交付物
- `tools/check_dataset.py`
- `docs/DATASET_CARD.md`（数据来源、拆分、标签定义、示例图）
- `reports/dataset_preview/`（网格图）

### 验收门槛
- 关键错误为 0
- 目检确认框对齐

---

## Week 2：baseline + 评估 + 失败案例库 + 分桶评估
### 任务
- 训练 baseline：YOLOv8n @ imgsz=640，开启 AMP
- 评估 baseline：
  - mAP50、mAP50-95、Recall、PR 曲线
  - Params、FLOPs
  - 固定 imgsz 下 FPS/latency
- 建立失败案例库（>=30），并分类：
  - 远距小目标漏检
  - 运动模糊
  - 逆光
  - 背景杂乱
  - 误检
- 实现小目标分桶评估（按 bbox 面积分 Small/Medium/Large）

### 交付物
- `reports/baseline/metrics.json`
- `plots/PR_curve_baseline.png`
- `docs/FAILURE_CASES.md` + `reports/failure_cases/`
- `tools/eval_by_size.py` + `reports/baseline/size_buckets.csv`

### 验收门槛
- 同配置跑两次指标差不多且可解释
- 你能用证据说明主要 2–3 类失败模式

---

## Weeks 3–4：严格消融（注意力 + 轻量卷积）
目标：做 1–2 个“好解释、好测量、代价可控”的改动。

### 严格实验矩阵（控制变量）
- A0：baseline（YOLOv8n）
- A1：+ 注意力（CBAM 风格），只插一个位置
- A2：+ 轻量卷积（Ghost/Depthwise），只替换一个块（比如 head）
- A3：A1 + A2 组合

### 任务
- A0–A3 训练策略保持完全一致
- 每个实验输出：
  - 总体指标
  - 分桶指标
  - 速度/Params/FLOPs
  - 同图同帧可视化对比

### 交付物
- `reports/ablation_A/summary_table.csv`
- `reports/ablation_A/qualitative/`
- `models/` 最小模块代码（注意力/轻量卷积）

### 验收门槛
- A3 小目标分桶指标优于 A0
- 代价（速度/复杂度）记录清楚且可接受

---

## Weeks 5–6：小目标专项增强（只选一个）
为了避免实验爆炸，只选一个方向。

### 选项 1（更像方法）：加高分辨率检测头（P2）
- 小目标通常会涨
- 注意速度成本

### 选项 2（更稳的工程）：训练策略增强
- imgsz 640 → 768（或相近）
- 裁剪策略/小目标 copy-paste（如果你能做）

### 交付物
- `reports/small_object/size_bucket_comparison.csv`
- `docs/DESIGN_CHOICE.md`（为什么选这个）

### 验收门槛
- 小目标提升明确
- 成本有边界（速度下降不能失控）

---

## Weeks 7–8：全面对比 + 最终模型冻结
### 任务
- 同协议对比 2–3 个 baseline
- 冻结最终模型结构与配置（不再随意改架构）
- 速度测试流程统一（warmup + 平均）

### 交付物
- `reports/final/comparison_table.csv`
- `reports/final/speed_benchmark.md`
- `models/final_config_snapshot.*`

### 验收门槛
- 最终模型冻结
- 关键表格可复现

---

## Weeks 8–9：泛化与鲁棒性切片
### 任务
- 换场景测试（可选：自采少量）
- 鲁棒性切片：
  - 逆光
  - 运动模糊
  - 背景杂乱
- 输出同图对比网格

### 交付物
- `reports/generalization/`
- `plots/robustness_slices.png`

---

## Weeks 9–12：写论文（可与复跑并行）
### 推荐写作顺序
1）Experiments  
2）Method  
3）Introduction  
4）Related Work  

### 论文必须有的证据
- 贡献点（2–3 条）
- 消融表（A0–A3 + S*）
- 分桶评估
- 速度/复杂度分析
- 失败案例与局限性

### 交付物
- `paper/`（LaTeX 或 Markdown）
- `paper/figures/`（脚本生成）
- `docs/REPRODUCIBILITY.md`

---

## Weeks 12–14：缓冲与打磨
### 任务
- 最终模型跑 3 个 seed（均值/方差）
- 图表统一、表格格式统一、脚本整理
- Release v1.0

### 交付物
- GitHub Release v1.0
- `RELEASE_NOTES.md`
