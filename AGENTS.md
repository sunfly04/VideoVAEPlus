# VideoVAEPlus Collaboration Notes

## Goal
- Primary project goal: improve Video VAE reconstruction quality on videos with large camera motion.
- When comparing experiments, prioritize MotionBench reconstruction quality, especially `LPIPS`.

## Environment
- Always use the `vae` conda environment before running training, evaluation, or debugging commands:
  - `conda activate vae`
- Assume future experiment commands should be launched from the repository root:
  - `/project/llmsvgen/feiyang/vae/VideoVAEPlus`

## Training Defaults
- Default experiment setting to keep in mind:
  - use `dataset85`
  - use 2 GPUs for full experiments
- Training configs may change across experiments, so do not assume a single default config.
- For any run, explicitly state the exact config path being used.
- Full runs should be submitted with `sbatch`, not left as interactive `srun` jobs.

## Cluster Workflow
- For short validation / smoke tests, start with `srun`.
- Recommended interactive test command:
  - `srun -p preempt --gres=gpu:1 --time=3:00:00 --pty bash -l`
  - or `srun -p normal --gres=gpu:1 --time=3:00:00 --pty bash -l`
- Partition guidance:
  - `preempt` is cheaper but less stable and only safe for short tests; it is only guaranteed to run for about 15 minutes.
  - `normal` is more expensive but typically easier to get usable GPU time for longer debugging.
- For formal training runs, submit with:
  - `sbatch scripts/run_slurm.sh 3d/config_16z`

## Evaluation Defaults
- Main evaluation entrypoint:
  - `motionbench/evaluation/eval_motionbench.sh`
- Current comparison convention:
  - compare results from 2-GPU `dataset85` runs
  - use the `70000`-step checkpoint for result comparison
  - focus on `LPIPS` first, then review other metrics if needed
- When editing evaluation configs or scripts, preserve a clear mapping between:
  - training config
  - checkpoint step
  - MotionBench output directory

## Working Agreement
- When running future experiments, keep the config path, launch command, checkpoint step, and evaluation output path explicit in notes or responses.
- If a command or config path in discussion conflicts with the repository state, verify the actual file in the repo before running.
- Prefer minimal, testable changes tied to the large-camera-motion reconstruction objective rather than broad refactors.
