# MiniMax H3 ComfyUI - 8GB VRAM Workflow

Optimized MiniMax H3 FL2V workflow for ComfyUI, designed and tested for 8GB VRAM GPUs. Includes low-VRAM settings, first/last-frame video generation, model setup, tested resolutions, and performance notes.

This repository contains a custom adaptation created and tested specifically to run MiniMax H3 FL2V with reduced resources. It is a community workflow, not an official MiniMax or ComfyUI release.

## Tested Hardware

- RTX 4060 Laptop
- 8 GB VRAM
- 16 GB system RAM
- Windows
- ComfyUI

8 GB VRAM is the hardware tested for this workflow, not a universal guarantee for every GPU with 8 GB. Results can vary with GPU model, available RAM, drivers, PyTorch version, ComfyUI version, custom-node versions, and background applications.

## Recommended tested configuration

- 480 × 864
- 158 frames
- ~6.6 seconds at 24 FPS
- 20 steps
- Euler
- Simple scheduler

480 × 864 is currently the recommended tested configuration for a good balance between image quality, stability, and generation time. Higher resolutions can also work, although generation time and memory/offload requirements increase significantly. A definitive upper resolution limit has not yet been established.

The included workflow keeps the current working defaults from the source workflow: 608 × 352 and 124 frames, with 20 Euler/Simple steps. Change the resolution and frame count only after confirming that your machine has enough VRAM and system RAM.

## What is included

- First Frame + Last Frame FL2V conditioning.
- 20 sampling steps with Euler and the Simple scheduler.
- `ModelAttentionBackend` configured for ComfyKitchenAttention.
- Spectrum enabled, using system RAM for its history storage.
- A small Qwen3-VL 4B text encoder plus ClipProj instead of the larger H3 text encoder.
- A clean workflow JSON with generic frame placeholders and no bundled media or model weights.

Open [the workflow](workflows/MiniMax_H3_FL2V_8GB_VRAM.json) in ComfyUI after installing the required native/custom nodes and downloading the models listed in [docs/MODELS.md](docs/MODELS.md).

## Quick start

1. Update ComfyUI to a version with native MiniMax H3 FL2V support.
2. Install the two custom-node repositories described in [docs/INSTALLATION.md](docs/INSTALLATION.md): `ComfyUI-ClipProj` and `ComfyUI-Spectrum-MiniMax-H3`.
3. Download the exact model files listed in [docs/MODELS.md](docs/MODELS.md). Do not download them into this repository.
4. Import the workflow into ComfyUI.
5. Upload or select your own first and last frame in the two `LoadImage` nodes. The public copy uses the non-personal placeholders `first_frame.png` and `last_frame.png`; no source images are included.
6. Confirm the model dropdowns, prompt, resolution, frame count, sampler, and scheduler, then queue a generation.

The tested low-VRAM runtime uses `--lowvram`, keeps the video VAE on the GPU with `--fp16-vae`, and does not use `--cpu-vae`. See [docs/INSTALLATION.md](docs/INSTALLATION.md) for setup details and [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) for failure modes.

## Important limitations

This workflow is tuned around one Windows laptop configuration. It may require lower resolution, fewer frames, or other adjustments on a different machine. In particular, 25 steps caused instability in the tested 8 GB system and are not recommended here; start with 20 steps.

The workflow can also be tested at resolutions above 480 × 864. Expect substantially longer generation times and higher memory/offload or system-RAM requirements as the resolution increases. The observed scaling is not necessarily linear with pixel count when an 8 GB GPU is close to its memory limit.

The workflow depends on the node signatures and model formats available in the referenced ComfyUI/custom-node versions. A missing node or model will prevent the graph from running; the repository does not vendor third-party code or weights.

## Documentation

- [Installation](docs/INSTALLATION.md)
- [Models and model paths](docs/MODELS.md)
- [Performance log](docs/PERFORMANCE.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)

## License and third-party components

The original workflow and documentation in this repository are released under the MIT License; see [LICENSE](LICENSE).

This repository is licensed under the MIT License. Model weights and third-party components are NOT covered by this repository's MIT license and remain subject to their respective licenses.

The H3 checkpoint, quantized text encoder, video VAE, ClipProj projection, ComfyUI, and custom nodes are separate projects. Review the license and usage terms at each upstream source before downloading or using them, especially for commercial work.
