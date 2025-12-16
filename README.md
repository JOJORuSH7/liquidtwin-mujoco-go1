# LiquidTwin: Go1 Locomotion with Liquid-Carrying Proxy Metrics

This repo contains a reproducible MuJoCo track for a quadruped “robot dog” (Unitree Go1) walking with a visible cup payload, trained with Soft Actor-Critic (SAC) and evaluated with physics-inspired **proxy metrics** correlated with liquid spill risk.

> **Key idea:** We improve trunk stability (roll/pitch + vertical motion) and report an explainable spill-risk proxy.

---

## What’s inside
- `GO1_clean.ipynb` — the full Colab notebook (MuJoCo setup + XML patching + training + evaluation + 2K video rendering)

---

## Environment
- MuJoCo + Gymnasium (`Ant-v5` wrapper loading Go1 MJCF from MuJoCo Menagerie)
- RL: Stable-Baselines3 SAC

Typical install in Colab:
```bash
pip install -q "gymnasium[mujoco]>=1.0.0" "stable-baselines3[extra]" mujoco tensorboard imageio imageio-ffmpeg shimmy
````

---

## How to run (recommended)

1. Open `GO1_clean.ipynb` in Google Colab
2. Mount Drive
3. Run cells top-to-bottom
4. Outputs:

   * trained checkpoints under Drive (e.g., `best_model.zip`, `sac_go1cup_stable.zip`)
   * evaluation plots (tilt / spill proxy / speed)
   * 2K follow-camera MP4 render

---

## Metrics (proxy for spill risk)

We estimate body tilt from trunk roll/pitch:

* tilt angle:  Θ(t) = sqrt(roll(t)^2 + pitch(t)^2)
* spill proxy: percentage of steps where Θ(t) > 15°

We also report:

* RMS tilt (deg)
* average forward speed (m/s)
* 10-second success rate (200 steps at Δt=0.05s)

---

## Results (fixed 10s horizon, 10 episodes)

Baseline vs stability-shaped SAC:

| Policy   | w_orient |  w_z | Spill ratio (%) | Speed (m/s) | 10s success (%) | RMS tilt (deg) |
| -------- | -------: | ---: | --------------: | ----------: | --------------: | -------------: |
| Baseline |     0.00 | 0.00 |           58.45 |        1.88 |           100.0 |          18.87 |
| Stable   |     0.20 | 0.10 |           54.05 |        2.29 |           100.0 |          18.16 |

Interpretation: enabling stability shaping reduces the spill-risk proxy while preserving long-horizon walking.

---

## Code availability

This repository is linked in the accompanying report as the code release for reproducibility.

---
