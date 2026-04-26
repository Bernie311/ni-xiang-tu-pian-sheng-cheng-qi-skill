---
name: ni-xiang-tu-pian-sheng-cheng-qi-skill
description: Reverse-engineer uploaded images into reusable image-generation prompts. Use when the user provides an image and asks for a prompt, reverse prompt, Midjourney prompt, SDXL prompt, DALL-E prompt, image-to-prompt analysis, visual style extraction, prompt recreation from a reference image, 逆向图片生成器, 图片反推提示词, 以图生提示词, 图片转提示词, or 反推关键词.
---

# 逆向图片生成器skill

## 概览 Overview

中文注释：这个 skill 主要服务中文区用户。用户上传图片后，直接反推出可复制到 Midjourney、SDXL、DALL-E 等工具里的提示词。

Convert uploaded reference images into practical prompts for image-generation systems. Focus on what can be visually supported by the image, then produce a copy-ready prompt that captures subject, composition, style, lighting, materials, typography, and mood.

## 调用方式 Invocation

中文注释：这个 skill 同时兼容 Codex 和 Claude Code 的常见 skill 目录结构。上传 GitHub 后，按目标工具选择安装和调用方式。

### Codex

个人安装目录：

`~/.codex/skills/ni-xiang-tu-pian-sheng-cheng-qi-skill/SKILL.md`

GitHub 安装示例：

`python install-skill-from-github.py --repo 用户名/仓库名 --path ni-xiang-tu-pian-sheng-cheng-qi-skill`

最短用法：

`反推这张图`

常用短句：

`图片转提示词`

指定平台：

`反推这张图，给我 Midjourney 和 SDXL 版本`

无法自动触发时，再用显式调用：

`使用 $ni-xiang-tu-pian-sheng-cheng-qi-skill，把这张图片反向生成一份中文优先、可直接复制使用的提示词。`

中文注释：安装或更新后重启 Codex，再上传图片测试。Codex 会根据 frontmatter 里的 `description` 判断是否自动使用这个 skill；日常优先输入短句，无法触发时再使用 `$ni-xiang-tu-pian-sheng-cheng-qi-skill`。

### Claude Code

个人安装目录：

`~/.claude/skills/ni-xiang-tu-pian-sheng-cheng-qi-skill/SKILL.md`

项目安装目录：

`.claude/skills/ni-xiang-tu-pian-sheng-cheng-qi-skill/SKILL.md`

直接调用示例：

`/ni-xiang-tu-pian-sheng-cheng-qi-skill 把这张图片反向生成中文提示词、英文提示词、Midjourney 版本和 SDXL 负面提示词。`

短句触发：

`反推这张图`

指定使用示例：

`请使用 ni-xiang-tu-pian-sheng-cheng-qi-skill 这个 skill，反推这张图。`

中文注释：Claude Code 会从个人、项目和插件 skill 目录自动发现 skills。`name` 字段会成为可直接调用的 slash command，因此可以用 `/ni-xiang-tu-pian-sheng-cheng-qi-skill` 调用；Claude Code 也会根据 `description` 自动选择相关 skill。更新 skill 后重启 Claude Code；如果没有生效，先让 Claude Code 列出可用 skills，或用 `claude --debug` 查看加载错误。

## 工作流 Workflow

中文注释：先观察图片，再组织提示词。不要先套模板，模板只用于最后整理。

1. Inspect the image before writing. Identify the dominant subject, pose, environment, composition, style, medium, lighting, color palette, texture, and any visible text or graphic design elements.
   - 中文注释：先看主体、动作、场景、构图、风格、光线、颜色、材质和文字元素。
2. Separate factual observation from inference. If a detail is unclear, say "unclear" or omit it from the prompt.
   - 中文注释：看得见的内容优先；看不清的地方直接写“不确定”或省略。
3. Build the prompt from high-impact details first:
   - subject and role
   - pose, expression, clothing, props
   - scene and background
   - visual style, medium, rendering method
   - lighting, color, lens/composition
   - graphic design, typography, paper texture, UI, stickers, stamps, or other layout elements
   - quality modifiers and aspect ratio
   - 中文注释：提示词顺序建议从“主体”到“画面设计”，最后再加质量词和比例。
4. Preserve readable image text only when it is clear. Do not invent exact text. For unclear text, describe the typography or label style instead.
   - 中文注释：图片里的字能读清才复写；读不清时描述“手写体标题”“复古标签排版”等视觉特征。
5. If the image appears to show a real person, do not identify them unless the user already provided the identity. Describe visible traits, styling, pose, and scene.
   - 中文注释：真人图片不要擅自识别身份，只描述外观、服装、动作和场景。
6. If the image contains sexualized minors or ambiguous-age sexualized characters, do not generate erotic or fetishized prompts. Create a neutral, non-sexual visual description instead.
   - 中文注释：涉及未成年人或年龄不明确的性感化角色时，只输出中性画面描述。

## 默认输出 Default Output

中文注释：用户没有指定格式时，按下面顺序输出。中文提示词放第一位，方便中文区用户直接使用。

When the user asks generally, output in this order:

1. Final Chinese prompt
2. Final English prompt
3. Platform version if useful:
   - Midjourney: concise comma-separated prompt with `--ar` and optional `--style raw`
   - SDXL: positive prompt plus negative prompt
   - DALL-E: natural-language scene description with clear layout instructions
4. Negative prompt
5. Short visual breakdown only if it helps reuse or edit the prompt

If the user asks for "only the prompt", output only one polished copy-ready prompt.

## 提示词规则 Prompt Style Rules

中文注释：提示词要像制作说明，少用空泛形容词，多写可被模型执行的画面信息。

- Prefer concrete nouns and visual attributes over vague praise.
  - 中文注释：用“白色泳装、湿发、复古杂志拼贴”这类具体词，少用“很美、很高级”。
- Use "inspired by" for recognizable fictional styles or characters when the goal is a reusable prompt.
  - 中文注释：涉及知名角色或风格时，用 “inspired by” 更适合复用。
- Avoid overclaiming resolution or quality. Use terms like "high detail", "sharp focus", and "polished illustration" only when they match the requested generator style.
  - 中文注释：不要堆砌无意义质量词，质量词应服务于画面目标。
- Keep typography prompts explicit for posters, magazine covers, UI screenshots, packaging, tickets, menus, or collages.
  - 中文注释：海报、杂志封面、票据、包装、UI 类图片要明确写排版、字体、标签、贴纸、条码等元素。
- Include aspect ratio when the image clearly implies one, such as `--ar 2:3` for vertical magazine covers or `--ar 16:9` for cinematic widescreen scenes.
  - 中文注释：画面比例明显时直接给比例，例如竖版封面用 `--ar 2:3`。
- For anime, game art, fashion editorial, product photography, architecture, UI, food, and cinematic stills, adapt the vocabulary to the domain.
  - 中文注释：不同类型图片要换不同词库，二次元、产品摄影、建筑、UI、电影截图不能用同一套表达。

## 常用提示词骨架 Useful Prompt Skeletons

### 通用 General

中文注释：适合大多数单图反推。

`[main subject], [pose/expression/action], [clothing/props], [environment], [composition/camera], [lighting], [color palette], [medium/style], [texture/rendering details], [mood], high detail`

### 杂志或海报 Magazine or Poster

中文注释：适合封面、海报、拼贴、视觉物料。

`[main subject] on a [poster/magazine cover] design, [headline/layout], [background scene], [stickers/tape/barcode/stamps/labels], [paper texture/print grain], [typography style], [color palette], [illustration or photo style], editorial layout, high detail`

### SDXL

中文注释：SDXL 更适合拆成正向提示词和负面提示词。

Positive prompt:
`[subject], [scene], [style], [lighting], [composition], [materials/textures], [quality details]`

Negative prompt:
`low quality, blurry, bad anatomy, extra fingers, distorted face, unreadable text, watermark, logo error, low resolution, oversaturated, flat lighting`

## 回复约束 Response Constraints

中文注释：输出要直接可用。用户只要提示词时，不要额外分析。

- Do not add an unnecessary greeting.
- Put the strongest usable prompt first.
- Keep analysis short unless the user requests detailed breakdown.
- If uncertain about identity, source, exact text, or hidden context, state uncertainty directly.
