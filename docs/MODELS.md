# Models

Do not upload model weights to this repository. Download them from the upstream sources below and place them in your local ComfyUI model folders.

## Required files

| Role | Exact filename | ComfyUI folder | Upstream source |
|---|---|---|---|
| H3 FL2VA diffusion model | `minimax_h3_fl2va_pruned_w4a8_mixed.safetensors` | `ComfyUI/models/diffusion_models/` | [`starsfriday/MiniMax-H3-w4a8`](https://huggingface.co/starsfriday/MiniMax-H3-w4a8) |
| Qwen3-VL 4B text encoder | `qwen3vl_4b_int8_convrot.safetensors` | `ComfyUI/models/text_encoders/` | [`Stick9190/qwen3vl_4b_int8_convrot`](https://huggingface.co/Stick9190/qwen3vl_4b_int8_convrot) |
| MiniMax H3 video VAE | `minimax_h3_video_vae_int8_convrot.safetensors` | `ComfyUI/models/vae/` | [`Kijai/MiniMax-H3-experimental`](https://huggingface.co/Kijai/MiniMax-H3-experimental) |
| 4B H3 ClipProj matrix | `mmh3-4b-ClipProj-v3-mlp.safetensors` | `ComfyUI/models/clip_projections/` | [`NicoLab28/ClipProj-MiniMax-H3`](https://huggingface.co/NicoLab28/ClipProj-MiniMax-H3) |

The workflow's loader values are already set to these filenames. If you choose a different compatible quantization, update the corresponding loader in ComfyUI and record the change for reproducibility.

## Folder layout

Your local installation should contain a structure like this:

```text
ComfyUI/
└── models/
    ├── diffusion_models/
    │   └── minimax_h3_fl2va_pruned_w4a8_mixed.safetensors
    ├── text_encoders/
    │   └── qwen3vl_4b_int8_convrot.safetensors
    ├── vae/
    │   └── minimax_h3_video_vae_int8_convrot.safetensors
    └── clip_projections/
        └── mmh3-4b-ClipProj-v3-mlp.safetensors
```

## Compatibility notes

- The H3 diffusion model is loaded by `UNETLoader`.
- The text encoder is loaded by `CLIPLoader` with the `krea2` type, then passed through `ClipProjApply`.
- The 4B ClipProj matrix must be paired with a Qwen3-VL 4B encoder. Do not use an 8B matrix with this workflow without changing the encoder and validating the graph.
- The VAE is the video VAE used by both `MiniMaxH3ImageToVideo` and `VAEDecode`.
- An audio VAE is not required by this video-only graph: the `CreateVideo` audio input is intentionally unconnected.

## Licenses and provenance

The files above are third-party artifacts and are not covered by this repository's MIT license. The W4A8 model card identifies a MiniMax H3 community license; the ClipProj repository identifies its matrices as MIT; the quantized Qwen and VAE artifacts have their own upstream terms and provenance. Read each source repository/model card and any base-model license before use.

The original MiniMax H3 project and official model information are available from [MiniMax-AI/MiniMax-H3](https://github.com/MiniMax-AI/MiniMax-H3). ComfyUI's repackaged H3 model information is available from [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3), but the exact low-VRAM filenames in this workflow come from the third-party sources listed above.
