# Installation

This guide assumes a Windows ComfyUI installation with native MiniMax H3 support. The workflow uses native ComfyUI nodes for the H3 graph and two external node packs:

- [`ComfyUI-ClipProj`](https://github.com/nicolab28/ComfyUI-ClipProj) for `ClipProjApply`.
- [`ComfyUI-Spectrum-MiniMax-H3`](https://github.com/xmarre/ComfyUI-Spectrum-MiniMax-H3) for `SpectrumApplyMiniMaxH3`.

The current workflow also uses `MiniMaxH3ImageToVideo` and `ModelAttentionBackend`, which are native nodes in a current ComfyUI installation. If either native node is missing, update ComfyUI before troubleshooting the workflow.

## 1. Install the custom nodes

From the `custom_nodes` directory of your ComfyUI installation:

```powershell
cd C:\path\to\ComfyUI\custom_nodes
git clone https://github.com/nicolab28/ComfyUI-ClipProj.git
git clone https://github.com/xmarre/ComfyUI-Spectrum-MiniMax-H3.git
```

Restart ComfyUI after installing or updating custom nodes. `ComfyUI-ClipProj` documents that it has no extra `pip` requirements; use the upstream Spectrum instructions if your installation method reports additional requirements.

Use `ComfyUI-ClipProj` 0.1.13 or later for the v3 `-mlp` projection matrix used by this workflow. Older versions do not load that matrix format correctly.

## 2. Install the model files

Download the four files listed in [MODELS.md](MODELS.md) from their upstream sources and put them in the exact ComfyUI model folders. The repository intentionally contains no model weights.

The required ClipProj matrix must match the 4B encoder. The workflow uses the `krea2` loader type and `mmh3-4b-ClipProj-v3-mlp.safetensors`.

## 3. Start ComfyUI for the tested low-VRAM profile

Use the launch options appropriate for your ComfyUI installation. The tested profile is:

```text
--lowvram --fp16-vae
```

Keep the video VAE on the GPU and do not add `--cpu-vae`. If your ComfyUI build exposes ComfyKitchenAttention as a startup option, enable it according to that build's documentation; the workflow itself selects `comfy kitchen attention` through `ModelAttentionBackend`.

Do not treat these flags as a guarantee of success on every 8 GB card. Leave enough system RAM available for ComfyUI and Spectrum's `system_ram` history storage.

## 4. Import and prepare the workflow

1. Open [workflows/MiniMax_H3_FL2V_8GB_VRAM.json](../workflows/MiniMax_H3_FL2V_8GB_VRAM.json) in ComfyUI.
2. In `CARGAR FIRST FRAME`, upload or select the image you want to use as the first frame.
3. In `CARGAR LAST FRAME`, upload or select the image you want to use as the last frame.
4. Check that the loaders resolve to the exact filenames in [MODELS.md](MODELS.md).
5. Start with 20 steps, Euler, Simple, and the resolution/frame count that fit your machine.
6. Queue the workflow.

The published JSON contains the generic filenames `first_frame.png` and `last_frame.png` only to avoid publishing local filenames. Replace them in the UI with your own images; the source images are not part of this repository.

## 5. Verify the graph before a long run

The graph should contain 18 nodes and 19 links, including:

- `UNETLoader` → `ModelAttentionBackend` → `MiniMaxH3SigmaShift` → `SpectrumApplyMiniMaxH3`.
- `CLIPLoader` → `ClipProjApply` → `MiniMaxH3ImageToVideo`.
- `VAELoader` feeding both the H3 conditioning node and `VAEDecode`.
- Two `LoadImage` nodes connected to the first-frame and last-frame inputs.
- `BasicScheduler` set to `simple` with 20 steps and `KSamplerSelect` set to `euler`.
