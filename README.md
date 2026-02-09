# UAV-Detect-Lite (First Research Project)

A first-research, paper-oriented project: lightweight UAV (drone) detection in RGB images/videos.
Main goal: improve small-object detection while keeping real-time efficiency on consumer GPUs.

## Goal
- Build a reproducible baseline (YOLOv8n/s).
- Propose 1–2 lightweight improvements (attention + lightweight conv) and validate via clean ablations.
- Provide paper-ready experiments: accuracy, speed, complexity, small-object bucket metrics, qualitative cases.

## Scope (for the first paper)
✅ RGB UAV detection (single class or few classes)  
✅ Lightweight + small-object improvements  
✅ Reproducible training/evaluation pipeline  
⛔ RGB-IR fusion / multi-UAV collaboration / heavy tracking (postponed)

## Hardware Target
- RTX 4060 Laptop (8GB VRAM), Windows (WDDM). Training with AMP and conservative batch sizes.

## Repository Structure
See `docs/PROJECT_PLAN.md` for timeline and deliverables.

## Quick Start
1. Install environment
2. Prepare dataset in YOLO format
3. Train baseline
4. Run evaluation and generate reports

## License
MIT (or your preferred license)

