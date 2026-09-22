# 奶蛙 NaiWa · Codex 桌面宠物

一个为 Codex 桌面应用制作的自定义 v2 动画宠物。角色采用圆润的奶黄色 3D 玩具风格，包含待机、跑步、等待、完成反馈、方向注视、参考动作视频还原的“捧腹大笑”交互，以及 Codex 思考处理时显示的静态托腮姿势。

> 这是非官方的同人桌宠项目，与 OpenAI 或相关角色权利方无隶属、授权或背书关系。

![奶蛙完整动画图集预览](qa/contact-sheet.png)

## 动画预览

| 招呼 / 交互大笑 | 鼠标进入 / 跳跃状态大笑 | 思考处理中（静态） |
| --- | --- | --- |
| ![waving belly laugh](qa/previews/waving.gif) | ![jumping belly laugh](qa/previews/jumping.gif) | ![static thinking pose](qa/previews/thinking.png) |

## 功能

- 休息、呼吸和眨眼待机动画
- 向左、向右跑步动画
- 常规工作、等待、失败和完成反馈
- 16 个注视方向
- `waving` 与 `jumping` 两种交互状态均使用捧腹大笑动作
- Codex 思考处理对应的 `running` 状态使用静态托腮姿势，六个有效格逐像素相同
- 透明背景、无阴影残留，适合直接作为 Codex v2 宠物图集

## 安装

### 方法一：下载仓库

1. 下载本仓库并解压。
2. 在 `%USERPROFILE%\.codex\pets\` 下新建 `naiwa-laugh-v4` 文件夹。
3. 将仓库根目录的 `pet.json` 和 `spritesheet.webp` 复制到该文件夹。
4. 打开 Codex 的 **Settings → Pets**，点击 **Refresh**。
5. 选择 **奶蛙 NaiWa（大笑与思考版）**。

### 方法二：PowerShell

```powershell
git clone https://github.com/Swordmynew/naiwa-petdex.git

$target = Join-Path $env:USERPROFILE ".codex\pets\naiwa-laugh-v4"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item -LiteralPath ".\naiwa-petdex\pet.json" -Destination $target -Force
Copy-Item -LiteralPath ".\naiwa-petdex\spritesheet.webp" -Destination $target -Force
```

复制完成后，仍需在 Codex 的宠物设置中刷新并选择奶蛙。更多说明参见 [Codex Pets 官方文档](https://learn.chatgpt.com/docs/pets?translationFallback=zh-Hans)。

## 图集规格

| 项目 | 值 |
| --- | --- |
| 格式 | Codex pet sprite v2 |
| 图集尺寸 | 1536 × 2288 |
| 网格 | 8 列 × 11 行 |
| 单帧尺寸 | 192 × 208 |
| 透明通道 | RGBA |
| SHA-256 | `05401DBE6E75515B6EAB796C0D567D169E668A7AD3406871D95768BEBF993CC1` |

标准动画行如下：

| 行 | 状态 | 帧数 | 用途 |
| ---: | --- | ---: | --- |
| 0 | `idle` | 7 | 待机、呼吸、眨眼 |
| 1 | `running-right` | 8 | 向右跑步 |
| 2 | `running-left` | 8 | 向左跑步 |
| 3 | `waving` | 4 | 招呼 / 捧腹大笑 |
| 4 | `jumping` | 5 | 鼠标进入或互动时的捧腹大笑 |
| 5 | `failed` | 8 | 失败或取消反馈 |
| 6 | `waiting` | 6 | 等待用户输入 |
| 7 | `running` | 6 | 静态托腮思考；六格同图、无动画 |
| 8 | `review` | 6 | 完成、等待查看 |
| 9–10 | `look` | 16 | 16 个注视方向 |

## 修复说明

早期版本只替换了 `waving` 行，但当前 Codex 桌面应用在鼠标进入宠物区域时会切换到 `jumping` 状态，所以新笑动作播放后仍可能回到旧跳跃动作。本版本同时替换了两行：

- 第 3 行保留四帧捧腹大笑；
- 第 4 行改为五帧连续动作：抱腹起势 → 仰头大笑 → 俯身笑 → 扶头回弹 → 站立收势；
- 第 4 行未使用的三个单元格保持全透明。

详细修复记录位于 [`qa/repair-notes.md`](qa/repair-notes.md)。

## 静态思考姿势更新

Codex 接收输入并进行思考或处理时会使用第 7 行 `running` 状态。本版本根据用户提供的参考图，将该状态替换为一只手托腮、另一只手托住手肘的完整全身姿势。

为了确保它完全静止，同时又满足 Codex v2 图集对该状态六个有效格的读取规则，六格使用逐像素相同的图像。应用即使继续轮播帧，宠物也不会发生动作、位移或抖动。

## 验证

最终图集已完成自动检查与独立视觉复核：

- 图集尺寸和 v2 结构正确；
- 每个必需状态的帧数正确；
- 无裁切、跨格或旧跳跃姿势残留；
- 无色键泄漏和透明像素 RGB 残留；
- 校验结果为 `PASS`，无错误或警告。

相关材料：

- [`validation.json`](validation.json)：完整图集验证报告
- [`qa/review-jumping.json`](qa/review-jumping.json)：大笑交互帧检查
- [`qa/review-thinking.json`](qa/review-thinking.json)：静态思考帧一致性检查
- [`qa/run-summary.json`](qa/run-summary.json)：修复与安装摘要
- [`qa/chroma-despill.json`](qa/chroma-despill.json)：边缘去色键报告
- [`qa/video-motion-reference.png`](qa/video-motion-reference.png)：动作参考帧

## 项目结构

```text
.
├── README.md
├── LICENSE
├── pet.json
├── spritesheet.webp
├── validation.json
└── qa/
    ├── contact-sheet.png
    ├── repair-notes.md
    ├── review-jumping.json
    ├── review-thinking.json
    ├── run-summary.json
    ├── chroma-despill.json
    ├── video-motion-reference.png
    └── previews/
        ├── waving.gif
        ├── jumping.gif
        └── thinking.png
```

## License

仓库中的代码与项目文件按 [MIT License](LICENSE) 发布。角色形象、名称及参考素材可能涉及其各自权利方的权益；使用或再分发前请自行确认适用的授权范围。

---

### English summary

NaiWa is a custom Codex desktop pet using the v2 8×11 sprite format. It includes idle, directional running, task states, 16-direction look, video-inspired belly-laugh interactions, and a completely static hand-on-chin thinking pose for the processing state. Copy `pet.json` and `spritesheet.webp` to `%USERPROFILE%\.codex\pets\naiwa-laugh-v4`, refresh Pets in Codex Settings, and select **奶蛙 NaiWa（大笑与思考版）**.
