---
name: gpt-anime-denoise
description: Remove the fish-scale (鱼鳞纹) noise that GPT image models (gpt-image-2/2.5, ChatGPT, Codex image_gen) leave in anime, illustration and TV-anime style images. Use after generating or receiving any GPT-made anime image that will be delivered, or when the user mentions 鱼鳞纹, 鳞片噪点, GPT 图噪点, fish-scale artifacts, or cleaning GPT anime images.
---

# GPT 二次元图去鱼鳞纹

GPT 生图在二次元 / 番剧画风里会留下细密的鱼鳞状纹理。人眼在原尺寸下能看出来，但放大、高通滤波和频谱分析都不容易检测到——**不要自己判断"这张图没有噪点"**，每张要交付的 GPT 二次元图都跑一遍。

## 用法

```bash
python <skill目录>/denoise.py <图片或文件夹> [...]           # 默认方案：ESRGAN 缩 60% + L0 0.001
python <skill目录>/denoise.py <图片> --all                    # 7 种候选 + 4×2 原尺寸对比图
python <skill目录>/denoise.py <图片> --method apisr          # 指定单一方法
```

- 依赖：`torch`、`spandrel`、`numpy`、`pillow`（`requirements.txt`）。模型第一次运行时自动下载到 `~/.cache/gpt-anime-denoise`，有 SHA-256 校验；已有模型目录用 `--models` 指定。
- 输出在 `<图片目录>/<文件名>_denoise/`，尺寸和输入一致，文件名 `<文件名>_<方法>.png`。
- 方法：`esrgan60_l0`（默认）、`esrgan60`、`cugan60_l0`、`cugan60`、`apisr`、`l0_0.001`、`l0_0.0005`。

## 交付规则

1. 默认交付 `esrgan60_l0`。
2. 用户想自己挑时跑 `--all`，把 `<文件名>_sheet.png`（原尺寸并排）发给用户；**给原尺寸并排图，不要发放大的局部**，放大后反而看不出差别。
3. 不要改用扩散重绘、ControlNet、导向/双边滤波来去噪：会重画眼睛或把画面抹糊（见 README「试过但效果不好的方法」）。
4. 写实照片风格不在测试范围内，效果未知。
