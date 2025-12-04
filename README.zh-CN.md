# Orbit UI 中文指南

Orbit UI 是一个受 shadcn/ui 启发、基于 Tailwind CSS 构建的轻量级 React 组件库——安装简单、易于扩展、开箱即用并面向 npm 发布。

- 最小可组合组件 + 合理的默认样式
- Tailwind First：采用 OKLCH 颜色变量与现代主题机制
- 使用 `class-variance-authority` 与 `tailwind-merge` 实现安全的组件变体
- 通过 Radix `Slot` 支持 `asChild` 组合
- 内置 `lucide-react` 图标支持
- 动效由 `tw-animate-css` 提供

当前状态：早期阶段。仓库已包含可用的 `Button` 组件与全局主题样式，后续将逐步完善更多组件与打包导出。

---

## 环境要求

- React 18+
- Tailwind CSS v4（使用 `@import "tailwindcss"`、`@theme inline` 等新语法）
- Node.js 18+
- 能处理 CSS 与 TypeScript 的构建工具（如 Next.js、Vite 等）

注意：项目内的 `globals.css` 采用 Tailwind v4 的语法特性，请确保你的应用已升级并正确配置 Tailwind v4。

---

## 安装

当 Orbit UI 发布到 npm 后：

- 使用 pnpm：`pnpm add @ooiai/orbit`
- 使用 npm：`npm install @ooiai/orbit`
- 使用 yarn：`yarn add @ooiai/orbit`

在发布之前，可本地路径安装：

- pnpm：`pnpm add /absolute/path/to/orbit`
- npm：`npm install /absolute/path/to/orbit`

组件库内部已声明依赖：
- `@radix-ui/react-slot`
- `class-variance-authority`
- `clsx`
- `tailwind-merge`
- `lucide-react`
- `tw-animate-css`

你的应用需要正确配置 Tailwind v4，并能处理来自 `node_modules` 的 CSS 引入。

---

## 快速开始

1) 在应用入口引入 Orbit 的全局样式：
- `import '@ooiai/orbit/globals.css'`

2) 确认你的应用使用 Tailwind v4：
- 应用级全局 CSS 文件需包含 `@import "tailwindcss";`
- 构建流程能处理从依赖包引入的 CSS（如 Next.js 默认支持）

3) 暗色模式：
- Orbit 使用 `.dark` 类作为暗色模式开关
- 在 `html` 或 `body` 上添加/移除 `dark` 类即可切换主题

---

## 使用指南（以 Button 为例）

当前组件以库内路径导出（后续会提供更稳定的聚合导出）：

- 导入：`import { Button } from '@ooiai/orbit/components/ui/button'`

- 基础用法：`<Button>默认按钮</Button>`
- 指定变体：`<Button variant="secondary">次要按钮</Button>`
- 危险操作：`<Button variant="destructive" size="sm">删除</Button>`
- 大小与事件：`<Button size="lg" onClick={...}>点击我</Button>`
- 组合渲染：`<Button asChild><a href="/docs">文档链接</a></Button>`（通过 Radix Slot 将样式应用于子元素）

支持的变体与属性：
- `variant`：`default | destructive | outline | secondary | ghost | link`
- `size`：`default | sm | lg | icon | icon-sm | icon-lg`
- `asChild`：布尔值，通过 Slot 将样式应用到传入的子元素

---

## 自定义与扩展

1) 自定义主题变量：
- 在应用级的 `:root` 与 `.dark` 中覆写 CSS 变量（如 `--background`、`--foreground`、`--primary`、`--secondary`、`--accent`、`--destructive`、`--border`、`--input`、`--ring`、以及 `--radius` 等）
- Orbit 的样式会引用这些变量，从而与品牌风格一致

2) 扩展组件变体：
- 可引入 `buttonVariants` 与 `cn` 在你的应用中二次封装
- 使用 `buttonVariants({ variant, size, className })` 输出基础类，再通过 `cn(...)` 合并自定义类
- 例如封装 `PrimaryButton`，统一团队内的默认样式与交互行为

3) 图标与动效：
- 在组件中直接使用 `lucide-react` 图标，它们将自动按样式约束对齐与缩放
- 使用 `tw-animate-css` 的类名为组件添加动效（已在 `globals.css` 引入）

---

## 项目结构概览

- `components/ui`：可直接使用的 UI 组件（目前包括 `button.tsx`）
- `components/neoui`：用于实验/下一代组件存放
- `lib/utils.ts`：工具函数（如 `cn`，基于 `clsx + tailwind-merge`）
- `globals.css`：Tailwind v4 主题、颜色变量与基础样式层
- `components.json`：类似 shadcn 的注册表配置与别名
- `package.json`：包信息与依赖
- `tsconfig.json`：TypeScript 配置

当前内部组件使用别名（如 `@/lib/utils`）。在打包发布时，请确保构建产物能正确解析这些别名，或在构建阶段转换为相对路径并提供合理的 `exports`。

---

## 发布到 npm（供维护者参考）

在发布前建议：
- 配置构建流程（如使用 `tsup` 或 `tsc`），输出 ESM/CJS 产物
- 在 `package.json` 中提供稳定的 `exports` 路径，例如：
  - `./dist/index.js`、`./dist/components/ui/button.js` 与 `./globals.css`
- 确认 CSS 被包含（复制或在 `files` 中声明）
- 输出类型声明（`.d.ts`）
- 提升版本号并发布：`npm publish`

---

## 常见问题

- 必须使用哪个 Tailwind 版本？需使用 Tailwind CSS v4。
- 是否支持 Tree Shaking？在提供 ESM 导出后，现代打包器可进行 Tree Shaking。
- 如何切换暗色模式？在根元素添加或移除 `dark` 类即可。

---

## 许可证

采用 `ISC` 许可证。详情见 `package.json`。

---

## 致谢

- 灵感来源于 shadcn/ui 的“用可组合原子构建设计系统”理念
- 使用 Radix UI 的 `Slot` 进行灵活组合
- `lucide-react` 提供一致的图标体验
- Tailwind CSS 提供高效的工具类样式体系
