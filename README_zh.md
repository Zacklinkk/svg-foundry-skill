# SVG Foundry (`svg-craft`)

[English](README.md) | [中文](README_zh.md)

**SVG Foundry** 提供名为 `svg-craft` 的智能体技能（Skill），旨在为大语言模型与开发工具提供结构化的 SVG 编写、优化、动画制作与样式重构指导规范。

本技能为矢量图形编写提供指导，支持四种输出格式：

- **独立 SVG**：具备规范命名空间与 `viewBox` 的标准 `.svg` 标记。
- **HTML + SVG**：包含内联 SVG 与 CSS 动画的完整 HTML 文档。
- **React JSX**：支持接收 `size`、`color` 及自定义样式的组件代码。
- **Vue SFC**：包含 `<template>` 与 `<style scoped>` 的单文件组件。

---

## 覆盖范围与技术准则

`svg-craft` 针对 SVG 开发的核心环节提供具体约束与规则：

- **视口与坐标系**：规范 `viewBox` 设置，根据实际嵌入环境处理明确尺寸或响应式流式布局，注意 Y 轴向下的坐标规则。
- **图形与路径**：覆盖基础图形（`<rect>`、`<circle>` 等）与路径指令（`M`、`C`、`S`、`Q`、`A`、`Z`）。
- **样式与复用定义**：规范线性与径向渐变、图案（Pattern）、标记符号（Marker）以及滤镜原语（如 `feDropShadow`、`feGaussianBlur`、`feColorMatrix`）。
- **现代标准优先**：统一采用现代 SVG 2 的 `href` 属性；仅在明确需要向下兼容旧环境时才生成废弃的 `xlink:href`。
- **组件实例隔离**：要求渐变、蒙版、剪切路径及无障碍关联的 `id` 保持唯一，避免同一组件在页面上多次渲染时发生 ID 冲突。
- **动画规范**：支持 CSS `@keyframes` 与 SMIL（`<animate>`、`<animateTransform>`）；指导实现减少动态效果时的行为，不保证硬件 GPU 加速效果。
- **无障碍语义**：信息型内联 SVG 需要可访问的名称，例如用 `aria-labelledby` 关联 `<title>`，并选择适当角色；纯装饰内联 SVG 应避免重复播报。通过 `<img>` 嵌入时，设置符合用途的 `alt`。具体选择取决于使用语境。

---

## 工作流程与验证

本项目提供规则指导和参考资料。渲染与检查使用宿主项目已有工具，仓库未捆绑独立渲染引擎或自动兼容性测试套件。

在宿主环境提供配套工具时，建议对生成的 SVG 进行 XML 语法解析与实际预览。通过 XML 语法校验仅代表代码符合良构规范，并不等同于视觉呈现效果良好或无障碍语义完全适配。

---

## 参考资料与开源协议说明

仓库包含 [Apache-2.0 许可证](LICENSE)。第三方参考资料的来源与许可需要单独确认。

本技能索引了 127 份源自 6 本 SVG 书籍（包括 *SVG Essentials*、*SVG Text Layout*、*SVG Colors Patterns & Gradients*、*SVG Animations*、*Using SVG with CSS3 and HTML5* 与 *Modern SVG*）的参考资料。第三方书籍衍生内容的重新分发授权尚未核实，仓库许可证的存在不代表已获得这些图书内容的再分发授权。再次分发前，应确认许可，或以原创笔记和官方文档链接替换。

---

## 安装与智能体配置

不同智能体宿主环境的调用机制存在差异。部分工具支持 `/svg-craft` 斜杠命令，而其他系统则通过系统提示词（System Prompt）或配置文件进行加载。

### 1. 配置技能路径
通过宿主支持的 skill 机制，将仓库注册为 `svg-craft`。保持 `SKILL.md` 与 `references/` 的相对位置。不同助手没有统一的安装目录。

*多智能体部署建议*：若在同一工作站上运行多个智能体工具，建议使用符号链接（symlink）统一指向同一份物理仓库，避免副本分化与维护脱节。

### 2. 调用说明

- **支持指令路由的环境**：输入 `/svg-craft <需求描述>`。
- **常规对话或 API 宿主**：在上下文或系统提示中引用该技能规则集。

---

## 使用示例

```text
用 svg-craft 做一个 24px 的设置图标，颜色跟随 currentColor。
用 svg-craft 把这份 SVG 改成 React 组件，每个实例的 ID 保持唯一。
用 svg-craft 加入加载动画，并提供减少动态效果的版本。
```

已知项目格式时沿用项目约定，否则默认输出独立 SVG。只有未确定的选择会实质影响结果时才追问。

## 示例代码

### 独立装饰性图标

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
  <circle cx="12" cy="12" r="10"/>
  <polyline points="12 6 12 12 16 14"/>
</svg>
```

### 附带语义标题与渐变的信息型 SVG

下面的 ID 供单个实例使用；封装为可重复使用的组件时，应生成各实例唯一的 ID。曲线仅作示意，不代表实测数据。

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100" height="100" role="img" aria-labelledby="chart-title">
  <title id="chart-title">销售增长趋势曲线</title>
  <defs>
    <linearGradient id="line-grad" x1="0%" y1="100%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#2563eb"/>
      <stop offset="100%" stop-color="#7c3aed"/>
    </linearGradient>
  </defs>
  <path d="M 10 80 Q 40 20 90 20" stroke="url(#line-grad)" stroke-width="4" fill="none" stroke-linecap="round"/>
</svg>
```
