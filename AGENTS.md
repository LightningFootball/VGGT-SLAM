# Repository Guidelines

## Project Structure & Module Organization
VGGT-SLAM is a Python 3.11 project built around the SL(4) optimization pipeline in `vggt_slam/`. Core modules such as `solver.py`, `submap.py`, and `frame_overlap.py` drive map construction, loop closure, and overlap estimation. The `evals/` package bundles dataset-specific runners and log processors, while `scripts/` holds one-off utilities for alignment and dataset prep. `main.py` is the CLI entrypoint for experiments, `app.py` exposes a Gradio UI, and `assets/` stores demo media including `office_loop.zip` for local smoke tests.

## Build, Test, and Development Commands
Create the recommended environment and install local dependencies:
- `conda create -n vggt-slam python=3.11` then `conda activate vggt-slam`
- `./setup.sh` clones third-party repos (Salad, VGGT) and installs this package in editable mode
Run the pipeline on the sample loop: `python main.py --image_folder office_loop --max_loops 1 --vis_map`. Launch the Gradio demo with `python app.py` if you need an interactive interface. Keep CUDA drivers up to date; the solver auto-detects GPU vs. CPU.

## Coding Style & Naming Conventions
Follow the existing PEP 8 conventions: four-space indentation, `snake_case` for functions and variables, `CamelCase` for classes, and module-level constants in `UPPER_SNAKE_CASE`. Type hints are encouraged for new public functions, and concise docstrings should state inputs, outputs, and side effects. Mirror current logging patterns (`print`, `tqdm`) and prefer pure-Python utilities before shelling out from scripts.

## Testing & Evaluation Guidelines
There is no dedicated unit-test suite yet. Validate changes by replaying `office_loop` and confirming stable loop closure timing, logged poses, and absence of regressions in the viser view. For quantitative checks, run `./evals/eval_tum.sh 32` or `./evals/eval_7scenes.sh 32` and summarize the metrics emitted by the corresponding `process_logs_*.py` scripts. When adding new evaluation flows, document required dataset paths and cache usage to avoid surprises in CI later.

## Commit & Pull Request Guidelines
Write imperative, low-noise commit messages (`fix:`, `feat:` prefixes are common but optional) and limit the subject to ~50 characters. Each PR should explain the motivation, list key changes, and include reproduction commands or log snippets (e.g., `python main.py --image_folder ...`). Link related issues, attach visual diffs or screenshots when UI-facing, and note any follow-up work so reviewers can queue it for subsequent iterations.
