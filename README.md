# stemstudio-isnet-onnx

IS-Net (DIS, "isnet-general-use") background segmentation model, ONNX, with weights stored as fp16
(computation stays fp32: each fp16 initializer is followed by a `Cast(to=FLOAT)`).

- Original model: Xuebin Qin et al., "Highly Accurate Dichotomous Image Segmentation" (DIS) — https://github.com/xuebinqin/DIS — Apache License 2.0
- ONNX export: `isnet-general-use.onnx` as distributed by rembg (https://github.com/danielgatis/rembg, release v0.0.0)
- Modifications: float32 initializers ≥ 64 KB converted to float16 (178.6 MB → 91.1 MB); MaxPool `ceil_mode=1` (2×2, stride 2) rewritten as `ceil_mode=0` with end padding 1 so it runs on WebGPU (identical output). Max abs difference of the normalized mask vs. the original on a test image: 0.0003.

Input `input_image` [1,3,1024,1024] RGB, (x/255 − 0.5) / 1.0. Output `output_image` [1,1,1024,1024].

Used by StemStudio (https://www.stemstudio.jp) for in-browser background removal. Load it from a commit-pinned URL.
