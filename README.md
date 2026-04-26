# 逆向图片生成器skill

把上传的图片反向生成可复用的 AI 绘图提示词，默认中文优先输出，适合 Midjourney、SDXL、DALL-E 等图像生成工具。

仓库地址：

`https://github.com/Bernie311/ni-xiang-tu-pian-sheng-cheng-qi-skill`

## 功能

- 根据上传图片反推出中文提示词
- 同时生成英文提示词
- 按需生成 Midjourney、SDXL、DALL-E 版本
- 输出负面提示词
- 拆解主体、构图、风格、光线、色彩、材质、文字和版式
- 对看不清的文字或身份信息保持不确定表达

## 适用场景

- 图片反推提示词
- 以图生提示词
- Midjourney 参考图提示词
- SDXL 正向 / 负面提示词
- DALL-E 图片描述
- 海报、杂志封面、拼贴、UI、产品图、二次元图的风格拆解

## Skill 信息

展示名：

`逆向图片生成器skill`

内部名称：

`ni-xiang-tu-pian-sheng-cheng-qi-skill`

目录结构：

```text
ni-xiang-tu-pian-sheng-cheng-qi-skill/
  SKILL.md
  agents/
    openai.yaml
```

## 在 Codex 中安装

使用 GitHub 安装：

```powershell
python install-skill-from-github.py --repo Bernie311/ni-xiang-tu-pian-sheng-cheng-qi-skill --path . --name ni-xiang-tu-pian-sheng-cheng-qi-skill
```

手动安装：

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.codex\skills"
git clone https://github.com/Bernie311/ni-xiang-tu-pian-sheng-cheng-qi-skill.git "$env:USERPROFILE\.codex\skills\ni-xiang-tu-pian-sheng-cheng-qi-skill"
```

安装后重启 Codex。

## 在 Codex 中调用

最短用法：

```text
反推这张图
```

常用短句：

```text
图片转提示词
```

指定平台：

```text
反推这张图，给我 Midjourney 和 SDXL 版本
```

无法自动触发时，再用显式调用：

```text
使用 $ni-xiang-tu-pian-sheng-cheng-qi-skill，把这张图片反向生成一份中文优先、可直接复制使用的提示词。
```

## 在 Claude Code 中安装

个人目录安装：

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills"
git clone https://github.com/Bernie311/ni-xiang-tu-pian-sheng-cheng-qi-skill.git "$env:USERPROFILE\.claude\skills\ni-xiang-tu-pian-sheng-cheng-qi-skill"
```

项目目录安装：

```powershell
New-Item -ItemType Directory -Force -Path ".claude\skills"
git clone https://github.com/Bernie311/ni-xiang-tu-pian-sheng-cheng-qi-skill.git ".claude\skills\ni-xiang-tu-pian-sheng-cheng-qi-skill"
```

安装后重启 Claude Code。

## 在 Claude Code 中调用

直接调用：

```text
/ni-xiang-tu-pian-sheng-cheng-qi-skill 把这张图片反向生成中文提示词、英文提示词、Midjourney 版本和 SDXL 负面提示词。
```

短句触发：

```text
反推这张图
```

指定使用：

```text
请使用 ni-xiang-tu-pian-sheng-cheng-qi-skill 这个 skill，反推这张图。
```

## 默认输出格式

1. 中文提示词
2. 英文提示词
3. 平台版本，按需要输出 Midjourney / SDXL / DALL-E
4. 负面提示词
5. 简短画面拆解

如果只想要最终提示词，可以直接说：

```text
只输出一段可复制的最终提示词。
```

## 测试方法

上传一张图片后输入：

```text
反推这张图
```

如果没有自动触发，再输入：

```text
使用 $ni-xiang-tu-pian-sheng-cheng-qi-skill，反推这张图。
```

合格标准：

- 中文提示词排第一
- 英文提示词排第二
- 有平台专用版本
- 有负面提示词
- 不乱编看不清的文字
- 真人图片不擅自识别身份
- 年龄不明确的性感化角色只输出中性画面描述

## 维护说明

核心逻辑在 `SKILL.md`。

界面展示信息在 `agents/openai.yaml`。

更新后重新提交并推送：

```powershell
git add SKILL.md agents/openai.yaml README.md
git commit -m "Update skill documentation"
git push
```
