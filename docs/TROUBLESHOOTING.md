# Troubleshooting

## CUDA out of memory

- Confirm that `--lowvram` is enabled.
- Keep `--fp16-vae` enabled and do not add `--cpu-vae` for this tested profile.
- Lower resolution first, then lower the frame count. The 8 GB result is hardware-specific.
- Close other GPU applications and restart ComfyUI to clear cached allocations.
- Disable Spectrum temporarily to determine whether its memory history or another node is involved.

## Windows `0xC0000005` crashes

This is a native access-violation-style crash, so the exact cause depends on the driver, PyTorch/CUDA build, ComfyUI revision, and loaded custom nodes. Try the following in order:

1. Restart ComfyUI and run the lower-memory baseline.
2. Update the NVIDIA driver and ComfyUI in a controlled environment.
3. Confirm that the custom-node revisions are compatible with your ComfyUI revision.
4. Reduce resolution and frames, and avoid other GPU-heavy applications.
5. Save the ComfyUI console output and Windows event details before reporting the crash.

## Insufficient system RAM

Spectrum is configured to use `system_ram` history storage, and the tested machine has 16 GB RAM. Close memory-heavy applications, keep the Windows page file enabled, and reduce resolution or frames if the system starts paging heavily. A different machine may need more RAM even with the same VRAM capacity.

## Missing custom nodes

Install and restart ComfyUI with both repositories from [INSTALLATION.md](INSTALLATION.md):

- `ComfyUI-ClipProj` provides `ClipProjApply`.
- `ComfyUI-Spectrum-MiniMax-H3` provides `SpectrumApplyMiniMaxH3`.

`MiniMaxH3ImageToVideo` and `ModelAttentionBackend` are native ComfyUI nodes in a current H3-capable build. If they are missing, update ComfyUI instead of replacing the graph with unrelated nodes.

## Missing models

Check the exact filenames and folders in [MODELS.md](MODELS.md). The most common causes are:

- A file was placed in `models/checkpoints/` instead of `models/diffusion_models/`.
- The text encoder was placed in the wrong folder or loaded with the wrong type.
- The ClipProj matrix was put in `models/vae/` instead of `models/clip_projections/`.
- A filename was changed without changing the corresponding loader.
- ComfyUI was not restarted after adding a model.

## Wrong model paths or unresolved dropdowns

Use the ComfyUI dropdown values, not private absolute paths. The public workflow contains only model filenames and generic frame placeholders. If a dropdown is empty, refresh/restart ComfyUI and verify the folder layout in [MODELS.md](MODELS.md).

## 25 steps cause instability

Do not use 25 steps as the recommendation for this 8 GB profile. That setting caused instability in the tested system. Start with 20 steps, Euler, and the Simple scheduler.

## Generation fails at the maximum configuration

Lower the resolution or frame count. The documented 480x864 / 158-frame configuration is a tested maximum for one RTX 4060 Laptop setup, not a promise for all 8 GB GPUs. Also verify that both first and last frame images are valid and that the required models and nodes load without warnings.
