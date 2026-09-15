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

- **Raw Sim** — default Gaussian-Splatting composite (GS background + MuJoCo robot
  foreground), no sim2real translation applied. This is what the policy would see if you
  ran sim eval with no `--color-calibrate`/`--pix2pix`/`--turbo` flag at all.
- **Kaifeng** — the same GS composite with classical color calibration (`--color-calibrate`).
- **Pix2Pix** — the same GS composite translated with a per-task/camera pix2pix GAN.
- **Turbo** — a *raw* MuJoCo render (no GS background) translated with a per-task/camera
  pix2pix-turbo diffusion model (`--turbo_mujoco`); GS is skipped for this baseline because
  the turbo checkpoints were trained to translate directly from the raw render.

All videos show every simulation step. The pix2pix / turbo baselines re-run their
translator on every frame of the underlying composite/raw video, rather than reusing the
model output cached once per policy-prediction chunk (as the sim-eval recording pipeline
does live).

Related: [Sim2Real Baseline Gallery](https://tongmiaoxu.github.io/sim2real-baseline-gallery/)
(per-frame image comparison across many more baseline variants).
