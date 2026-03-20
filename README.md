# Seedance Prompt Skill

将剧本、小说或分镜脚本转化为即梦 Seedance 2.0 视频提示词。

## 适用平台

本技能支持以下平台：

| 平台 | 说明 |
|------|------|
| **OpenCode** | Anomaly 推出的开源 AI 编码助手（GitHub 120K stars），支持通过 skill 扩展功能 |
| **MonkeyCode** | OpenCode 的中文版本，与 OpenCode 共享 skill 格式 |
| **Claude Desktop** | 支持 Claude skill 格式，将 `SKILL.md` 放入 `~/.claude/skills/` 目录 |
| **独立使用** | CLI 工具可独立运行，不依赖任何 AI 平台 |

### OpenCode / MonkeyCode 安装

OpenCode 和 MonkeyCode 使用相同的 skill 格式。将 `skills/` 目录复制到配置目录：

```bash
# 克隆仓库
git clone https://github.com/wenwenwennnnn/seedance-prompt-skill.git

# 复制 skills 目录到配置目录
# OpenCode 默认路径
cp -r skills ~/.openclaw/
# 或 MonkeyCode 路径
cp -r skills ~/.monkeycode/

# 或使用链接方式
ln -s $(pwd)/skills ~/.openclaw/skills/seedance-prompt
```

### Claude Desktop 安装

```bash
# 克隆仓库
git clone https://github.com/wenwenwennnnn/seedance-prompt-skill.git

# 创建 skill 目录并复制
mkdir -p ~/.claude/skills/seedance-prompt
cp skills/seedance-prompt/SKILL.md ~/.claude/skills/seedance-prompt/
cp -r skills/seedance-prompt/scripts ~/.claude/skills/seedance-prompt/
cp -r skills/seedance-prompt/references ~/.claude/skills/seedance-prompt/
```

### Submodule 引入

如果你有自己的配置仓库：

```bash
git submodule add https://github.com/wenwenwennnnn/seedance-prompt-skill.git skills/seedance-prompt
```

---

## 使用方法

### 基本语法

当你想生成视频提示词时，直接输入剧本内容：

```
帮我把这个剧本转化成Seedance提示词：
[在这里粘贴你的剧本、小说或分镜脚本]
```

### 可选参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `aspect_ratio` | 视频比例 | `9:16` |
| `model` | 视频模型 | `seedance_2.0_fast` |
| `subtitle` | 是否开启字幕 | `off` |

### 示例

**输入：**
```
/seedance-prompt
夕阳下，一个穿黑色风衣的男人走进老旧的咖啡馆。他点了杯咖啡，望向窗外的雨景。
```

**输出：**
```
视觉风格 赛博朋克, 暗调, 低饱和度, 冷光, 悬疑

片段1 [0-8s] 包含2个镜头
[镜头1: 0-4s] 中近景, 推镜头, 一个穿黑色风衣的男人走在湿漉漉的街道上，@男人 面部冷峻，眼神警惕，油灯暖光从右侧打来，赛博朋克风格，暗调摄影，霓虹色调
[镜头2: 4-8s] 远景, 固定镜头, @男人 推开咖啡馆的木门，@咖啡馆 内烟雾缭绕，复古装潢与霓虹灯交织，冷色调光源，赛博朋克氛围

片段2 [8-15s] 包含2个镜头
[镜头1: 8-11s] 中近景, 跟镜头, @男人 坐在吧台前，@咖啡师 递来一杯咖啡，@男人 手指轻敲吧台，表情沉思，台灯暖光从左侧打来
[镜头2: 11-15s] 中近景, 拉镜头, @男人 转头望向窗外，@雨景 细雨蒙蒙，霓虹灯光在玻璃上折射，冷色调主导，忧郁悬疑氛围
```

---

## 命令行工具

CLI 工具可独立使用，不依赖任何 AI 平台：

```bash
# 生成提示词
python3 skills/seedance-prompt/scripts/generate_prompt.py \
  --input script.txt \
  --output prompts.md \
  --aspect 9:16

# 参数说明
# --input, -i    输入剧本文件路径（必需）
# --output, -o   输出文件路径（默认 prompts.md）
# --aspect       视频比例 16:9/9:16/1:1（默认 9:16）
# --model        视频模型 seedance_2.0/seedance_2.0_fast（默认 seedance_2.0_fast）
# --subtitle     是否开启字幕 on/off（默认 off）
```

---

## 核心概念

| 概念 | 说明 |
|------|------|
| **片段（Segment）** | AI 输出基本单位，包含 1+ 镜头，总时长 ≤15 秒 |
| **镜头（Shot）** | 单个镜头画面，1-8 秒 |
| **资产引用** | 使用 `@资产名` 格式引用角色或物体 |

## 输出格式说明

- **景别术语**：大远景/远景/全景/中景/中近景/特写/大特写
- **镜头运动**：固定/推/拉/摇/跟/升/降/环绕
- **资产引用**：首次出现时用 `@角色名` 标注，后续直接描述动作
- **禁止人称代词**：不使用"他/她/它"，统一用 `@资产` 格式

---

## 项目结构

```
seedance-prompt-skill/
├── skills/
│   ├── manifest.json              # OpenCode/MonkeyCode 注册文件
│   └── seedance-prompt/
│       ├── SKILL.md              # Skill 定义（OpenCode/MonkeyCode/Claude 通用）
│       ├── scripts/
│       │   └── generate_prompt.py  # CLI 工具（独立运行）
│       └── references/
│           └── terms.md          # 术语表
├── README.md
└── LICENSE
```

---

## 许可证

MIT License
