# SVG Craft

**专业的 SVG 创作与优化 Skill。**

> English version: [README.md](README.md) | Agent metadata: [SKILL_METADATA.yaml](SKILL_METADATA.yaml)

## 简介

SVG Craft 是一个基于知识驱动的 AI Skill，能够从零创建、优化、添加动画、重新设计 SVG 图形。内置完整的 SVG 技术知识体系，涵盖规范的 12 个核心领域，让 AI 在处理 SVG 任务时拥有专家级的技术深度。

## 核心能力

- **Create** — 根据自然语言描述生成 SVG 图标、插画、图表、Logo、加载动画等
- **Optimize** — 优化现有 SVG 代码的性能、可访问性与响应式适配
- **Animate** — 为静态 SVG 添加 CSS 动画、SMIL 动画或描边绘制效果
- **Restyle** — 调整配色方案、渐变、图案、滤镜等视觉风格

## 输出格式

| 格式 | 适用场景 |
|------|---------|
| Pure SVG | 独立 `.svg` 文件，可嵌入任何环境 |
| HTML + SVG | 完整 HTML 页面，含内联 SVG 与 CSS 动画 |
| React 组件 | JSX 组件，支持自定义 props |
| Vue 组件 | SFC 格式，template + scoped styles |

## 技术覆盖

涵盖 SVG 规范的 12 个核心领域：

- 画布与坐标系统（viewBox、preserveAspectRatio、响应式缩放）
- 基础与高级形状（rect、circle、path 等）
- 完整路径命令（M/L/H/V/C/S/Q/T/A/Z）
- 文本排版（text、tspan、textPath、多语言支持）
- 颜色与填充系统
- 线性渐变与径向渐变
- 图案（Pattern）
- 变换（translate、rotate、scale、matrix 等）
- 裁剪与遮罩（clipPath、mask）
- 滤镜（模糊、阴影、颜色矩阵、位移贴图、湍流等）
- 描边属性
- 标记（Marker）

## 知识体系

内置 6 大主题的 SVG 参考知识库（127 个参考文件），按需加载：

| 主题 | 覆盖领域 |
|------|---------|
| SVG 基础 | 形状、坐标、路径、变换、滤镜 |
| 文本排版 | 文本布局、文本路径、国际化 |
| 颜色与视觉 | 颜色、渐变、图案、纹理 |
| 动画 | CSS 动画、精灵图、响应式动画、数据可视化动画 |
| Web 集成 | SVG + CSS3 + HTML5 综合实践 |
| 现代技术 | 现代工具链、高级路径、现代色彩 |

## 安装

**Claude Code：**
```bash
cp -r svg-craft ~/.claude/skills/svg-craft
```

**其他 AI Agent：**
将 `SKILL.md` 作为 system prompt 加载，`references/` 目录作为可检索知识库挂载。详见 [SKILL_METADATA.yaml](SKILL_METADATA.yaml)。

## 使用方式

在对话中使用触发关键词即可自动激活：

```
画SVG、做个SVG、SVG图标、SVG动画、SVG图表、优化SVG
draw SVG、create SVG、SVG icon、SVG animation、SVG chart
```

或通过斜杠命令手动调用：`/svg-craft`

## 设计理念

- **知识驱动** — 通过结构化知识体系指导 AI 输出，而非硬编码脚本
- **按需加载** — 根据当前任务精确读取相关参考章节，避免信息过载
- **结构化工作流** — 5 步标准流程（需求澄清 → 设计 → 动画 → 集成 → 输出）确保质量一致性
- **无障碍优先** — 默认包含 `<title>`、`aria-labelledby`、`prefers-reduced-motion` 等无障碍支持

## 许可证

Apache License 2.0
