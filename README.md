# Sim2Real Rollout Video Gallery

Full-episode ACT policy rollouts (20k checkpoint) rendered under 4 sim2real baselines,
shown side by side for the stationary and wrist cameras.

Live site: https://tongmiaoxu.github.io/sim2real-rollout-videos/

## Tasks / episodes

| Task | Episode |
| --- | --- |
| Book Shelving | episode_008 |
| Pick Shoe | episode_002 |
| Place Mug | episode_004 |
| Pouring | episode_005 |

## Baselines

- **Raw Render** — raw MuJoCo render, no Gaussian Splatting background, no sim2real translation.
- **Kaifeng** — Gaussian-Splatting composite with classical color calibration (`--color-calibrate`).
- **Pix2Pix** — Gaussian-Splatting composite translated with a per-task/camera pix2pix GAN.
- **Turbo** — raw render translated with a per-task/camera pix2pix-turbo diffusion model.

All videos show every simulation step. The pix2pix / turbo baselines re-run their
translator on every frame of the underlying composite/raw video, rather than reusing the
model output cached once per policy-prediction chunk (as the sim-eval recording pipeline
does live).

Related: [Sim2Real Baseline Gallery](https://tongmiaoxu.github.io/sim2real-baseline-gallery/)
(per-frame image comparison across many more baseline variants).
