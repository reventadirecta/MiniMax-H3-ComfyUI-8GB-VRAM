# Performance

This log is intentionally conservative. No generation time is invented where an exact measurement has not been recorded.

## Recommended tested configuration

- Resolution: 480x864
- Frames: 158
- FPS: 24
- Duration: ~6.6 seconds
- Steps: 20
- Sampler: Euler
- Scheduler: Simple
- Result: Stable
- Notes: Recommended quality/performance balance

480x864 is currently the recommended tested configuration for a good balance between image quality, stability, and generation time. Higher resolutions can also work, although generation time and memory/offload requirements increase significantly. A definitive upper resolution limit has not yet been established.

## Recorded configurations

| Resolution | Frames | Steps | Result | Approx. time | Notes |
|---|---:|---:|---|---|---|
| 480x864 | 158 | 20 | Stable | pending exact time | Recommended quality/performance balance |
| 544x960 | 158 | 20 | Stable | 10-11+ minutes observed, exact timing pending | Higher-resolution test, significantly slower |

The 544x960 test also completed correctly on the NVIDIA RTX 4060 Laptop / 8 GB VRAM / 16 GB RAM setup. No persistent ComfyUI log containing an unambiguous `Prompt executed in ... seconds` line for this exact run was available, so the table intentionally records the observed range rather than inventing an exact time.

Generation time does not necessarily scale linearly with pixel count. When an 8 GB GPU approaches its memory limit, offload and system-RAM activity can increase disproportionately and make higher-resolution runs substantially slower.

## Workflow defaults

The published workflow preserves the current source defaults of 608x352, 124 frames, 20 steps, Euler, and the Simple scheduler. Those values are part of the copied workflow and are separate from the benchmark configurations recorded above. No exact render time is published for the workflow defaults in this repository.

## How to add a measurement

When recording a new run, keep the following fixed or note the change:

- GPU and available VRAM.
- System RAM and whether other applications were open.
- ComfyUI, PyTorch, CUDA/driver, and custom-node versions.
- Resolution, frame count, FPS, steps, sampler, and scheduler.
- Whether Spectrum was enabled and which attention backend was used.
- Wall-clock time from queueing to saved video.
- Whether the result completed cleanly and was visually usable.
