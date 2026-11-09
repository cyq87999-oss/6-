---
name: comfyui-model-placement
description: 把下载的模型文件（safetensors/gguf/ckpt）正确归位到本机 ComfyUI 对应目录。当下载了新模型不知道放哪、ComfyUI 下拉框找不到模型、或需要批量整理模型时使用。通过读取权重文件头判定模型真实类型（不靠文件名），并按共享库+软链接体系归位。
agent_created: true
---

# ComfyUI 模型归位

## 何时用
- 下载了模型不知道放哪个目录
- ComfyUI 下拉框里找不到刚下的模型
- 批量整理 Downloads 里的模型文件

## 本机环境（已确认，长期有效）
- **主力 ComfyUI**：`/Users/qy/ComfyUI-Installs/ComfyUI/ComfyUI`
- **自带 Python（含 PyYAML）**：`/Users/qy/ComfyUI-Installs/ComfyUI/standalone-env/bin/python3`
- **共享模型库**：`/Users/qy/ComfyUI-Shared/models` —— 主力实例通过 `extra_model_paths.yaml` 自动映射，**文件放这里即可被扫到**
- 其他实例：`/Users/qy/ComfyUI/ComfyUI`、`/Users/qy/ComfyUI/ComfyUI-shamo/ComfyUI`、`/Users/qy/Documents/comfy/ComfyUI`

### 归位策略
1. 默认 `mv` 到 `/Users/qy/ComfyUI-Shared/models/<子目录>/`，让所有实例共享，不重复占盘
2. **例外**：`gguf` 目录不在共享库映射内 → 放共享库 `gguf/` 后，需软链到 `models/gguf/`
3. 同卷 mv 是瞬时重命名，不额外占磁盘

## 第一步：判定模型真实类型（关键，别信文件名）
文件名极不可靠。实测案例：`Flux2-Klein-9B-consistency-V2.safetensors` 只有 316MB，实为 LoRA 而非 9B 主模型。

用 `probe.py` 读 safetensors 权重头（只需 8 字节 + JSON，不加载权重）：

```python
import json, struct, sys, os
from collections import Counter
for f in sys.argv[1:]:
    with open(f,'rb') as fh:
        n = struct.unpack('<Q', fh.read(8))[0]
        hdr = json.loads(fh.read(n))
    keys = [k for k in hdr if k != "__metadata__"]
    md = hdr.get("__metadata__", {})
    print(f"\n== {os.path.basename(f)}  张量{len(keys)}")
    if md: print("  metadata:", json.dumps(md, ensure_ascii=False)[:300])
    c = Counter('.'.join(k.split('.')[:2]) for k in keys)
    for p, cnt in c.most_common(10): print(f"    {cnt:>5}  {p}")
```

### 判定规则
| 权重 key 特征 | 类型 | 目标目录 |
|---|---|---|
| `lora_A.weight` / `lora_B.weight` | LoRA | `loras/` |
| `lokr_w1` / `lokr_w2` / `alpha` | **LoKr**（原生 LoraLoader 不支持，需 LyCORIS 节点） | `loras/` |
| `model.diffusion_model.*` + `cond_stage_model` + `first_stage_model` | SD 完整 checkpoint | `checkpoints/` |
| `transformer.rotary_pos_emb.inv_freq` 或 `model.model.*`+`pretransform.model.*` | Stable Audio | `checkpoints/` |
| `encoder.*`+`decoder.*`+`per_channel_statistics` | VAE（视频） | `vae/` |
| `audio_vae.*`+`vocoder.*` | VAE（音频） | `vae/` |
| `blocks.N.*`（Wan 风格）、`double_blocks`/`single_blocks`（Flux 风格） | DiT 主模型 | `diffusion_models/` |
| `vision_model.encoder.*` | CLIP Vision | `clip_vision/` |
| `encoder.block.*`+`decoder.block.*`+`shared.weight` | T5 文本编码器 | `text_encoders/` |
| `text_embedding_projection.*` | LTX-2 connector | `checkpoints/` |

**强烈建议先看 metadata**：`ss_base_model_version` 出现 = ai-toolkit 训练产物（必是 LoRA/LoKr，不是主模型）；`training_info.step` 同理。

## 第二步：目录映射速查
| 模型用途 | 目录 |
|---|---|
| SD1.5/SDXL 完整 checkpoint、Stable Audio、LTX-2 connector | `checkpoints/` |
| DiT 主模型（Wan / Flux / LTX / Kandinsky）、Unet GGUF | `diffusion_models/` |
| LoRA / LoKr / LoHa | `loras/` |
| VAE（图像/视频/音频） | `vae/` |
| 文本编码器（T5 / CLIP / Qwen / Gemma safetensors） | `text_encoders/` |
| CLIP Vision | `clip_vision/` |
| ControlNet | `controlnet/` |
| 文本编码器 GGUF | `text_encoders/` 或 `clip/` |
| LTX-2 的 Gemma GGUF | `gguf/`（需软链，见下） |

## 第三步：执行归位
```bash
S=/Users/qy/ComfyUI-Shared/models
mv ~/Downloads/xxx.safetensors "$S/diffusion_models/"
```
**先检查目标是否已存在同名**（`ls` 一下），已存在就别覆盖——本机大量模型是指向 HF cache 或其他目录的软链接，覆盖会破坏。

GGUF 额外步骤（共享库 gguf 不被扫描）：
```python
import os
os.symlink("/Users/qy/ComfyUI-Shared/models/gguf/模型.gguf",
           "/Users/qy/ComfyUI-Installs/ComfyUI/ComfyUI/models/gguf/模型.gguf")
```

## 第四步：验证可见性
模拟 ComfyUI 扫描（本地 models + 共享库映射）确认模型会出现在下拉框。用主力实例自带的 PyYAML 解析 `extra_model_paths.yaml`：

`/Users/qy/ComfyUI-Installs/ComfyUI/standalone-env/bin/python3` 读 yaml → 拼路径 → `os.listdir` 检查目标文件名是否在内。

## 各节点目录注册（源码级，已确认）
- `ComfyUI-GGUF/nodes.py:32` → `unet_gguf`: `["diffusion_models", "unet"]`
- `ComfyUI-GGUF/nodes.py:33` → `clip_gguf`: `["text_encoders", "clip"]`
- `ltx2_sm/LTX2_node.py:22-26` → 自建 `models/gguf`；节点下拉：dit=diffusion_models / gguf=gguf / clip=gguf / connector=checkpoints / vae+audio_vae=vae / lora=loras
- ComfyUI 核心 `folder_paths.py`：text_encoders 同时扫 `text_encoders` 和 `clip`；diffusion_models 同时扫 `unet` 和 `diffusion_models`
- Stable Audio 走 `CheckpointLoaderSimple`，`comfy/model_detection.py:144` 按 `transformer.rotary_pos_emb.inv_freq` 识别
- `llm-toolkit` 是**纯云端 API 节点，不读本地模型**

## 踩坑清单
- **别用全盘 `find`**：`find /Users/qy -name "*.gguf"` 会超时被 SIGTERM（exit 137）。改用 `mdfind "kMDItemFSName == '*.gguf'c"`，秒出
- **别手写 YAML 解析** `extra_model_paths.yaml`：`base_path` 是缩进行、`diffusion_models` 用 `|` 块标量，手写解析器极易漏。用 ComfyUI 自带 venv 的 PyYAML
- **别信文件名**：316MB 不可能是 9B 模型，一定先读权重头或 metadata
- **Chrome 下载中的文件**叫 `未确认 xxxxx.crdownload`，下完自动改名；此时不该去动它
- **改完模型目录后 ComfyUI 要刷新页面**（Ctrl+R 或菜单 Refresh）才能看到新模型
- 本机 `models/vae/` 有历史遗留错误：`wan2.2_i2v_lightx2v_4steps_lora_v1_*` 是 LoRA 却放在 vae 目录
