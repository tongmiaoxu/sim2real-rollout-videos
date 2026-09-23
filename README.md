# Sim2Real Rollout Video Gallery

Full-episode policy rollouts rendered under 4 sim2real baselines plus a real-world eval
recording, shown side by side for the stationary and wrist cameras.

Live site: https://tongmiaoxu.github.io/sim2real-rollout-videos/

## Tasks / episodes / checkpoints

| Task | Policy / checkpoint | Sim episode |
| --- | --- | --- |
| Book Shelving | ACT 20k | episode_008 |
| Pick Shoe | pi0.5 1k (batch 1) | episode_004 |
| Place Mug | ACT 20k | episode_004 |
| Pouring | Diffusion Policy 8k | episode_000 |

Real World uses the *same episode index* as the sim episode above, from the corresponding
`data_real_eval_<task>_<policy>_<checkpoint>` recording — sim episode N and real episode N
are the same seeded trial/object placement (see `initial_states_overlay.py`), so each column
is directly comparable frame-for-frame in setup, not just in checkpoint. Exception: **Pick
Shoe**'s real-world clip uses `data_real_eval_pick_shoe_act_010000` episode 4 (ACT 10k)
instead of the pi0.5 1k recording, since that gives a clearer real rollout for this task.

## Baselines

- **Raw Sim** — a genuine from-scratch policy rollout with Gaussian-Splatting compositing
  disabled (`--scene-path` pointed at a nonexistent file, or `--no-composite` for the pi0.5
  remote-server path) and no sim2real translation of any kind — the policy's own visual input
  is the plain raw MuJoCo render. This is a fresh sim eval run for every task, not a
  byproduct of another baseline's pipeline.
- **Kaifeng** — the GS composite with classical color calibration (`--color-calibrate`).
- **Pix2Pix** — the GS composite translated with a per-task/camera pix2pix GAN.
- **Turbo** — a *different* raw MuJoCo render (no GS background), from the `--turbo_mujoco`
  eval run, translated with a per-task/camera pix2pix-turbo diffusion model; GS is skipped
  for this baseline because the turbo checkpoints were trained to translate directly from the
  raw render. Unlike Raw Sim, this rollout's actions were driven by the policy acting on
  turbo-translated observations during that eval, so its trajectory differs from Raw Sim's
  even though both are GS-free.
- **Real World** — the real xArm robot running the same checkpoint on the same seeded
  episode, from `data_real/` (LeRobot v3 dataset, trimmed by frame index from the
  per-camera combined mp4).

The 4 sim baselines show every simulation step; the pix2pix / turbo columns re-run their
translator on every frame of the underlying composite/raw video, rather than reusing the
model output cached once per policy-prediction chunk (as the sim-eval recording pipeline
does live). The Real World column is an unmodified real-robot recording, so its length and
gripper-camera framing differ naturally from the sim columns.

Real World playback speed is adjusted for visual pacing against the sim columns: Book
Shelving is slowed to 0.83x (1.2x slower); Pick Shoe, Place Mug, and Pouring are sped up to
1.2x. Videos in a row are not looped — whichever column finishes first simply freezes on its
last frame until the others (or a manual restart) catch up.

Related: [Sim2Real Baseline Gallery](https://tongmiaoxu.github.io/sim2real-baseline-gallery/)
(per-frame image comparison across many more baseline variants).
