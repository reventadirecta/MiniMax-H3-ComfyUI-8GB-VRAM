# Performance

This log is intentionally conservative. No generation time is invented where an exact measurement has not been recorded.

## Recorded configuration

| Resolution | Frames | Steps | Result | Approx. time |
|---|---:|---:|---|---|
| 480x864 | 158 | 20 | Stable | pending exact time |

This configuration is approximately 6.6 seconds at 24 FPS. It is the current recommended maximum tested configuration for the RTX 4060 Laptop / 8 GB VRAM / 16 GB RAM setup described in the README.

## Workflow defaults

The published workflow preserves the current source defaults of 608x352, 124 frames, 20 steps, Euler, and the Simple scheduler. Those values are part of the copied workflow and are separate from the maximum configuration recorded above. No exact render time is published for either configuration in this repository.

## How to add a measurement

When recording a new run, keep the following fixed or note the change:

- GPU and available VRAM.
- System RAM and whether other applications were open.
- ComfyUI, PyTorch, CUDA/driver, and custom-node versions.
- Resolution, frame count, FPS, steps, sampler, and scheduler.
- Whether Spectrum was enabled and which attention backend was used.
- Wall-clock time from queueing to saved video.
- Whether the result completed cleanly and was visually usable.
