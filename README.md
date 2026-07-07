# NebulaScreen · 星云数据大屏

**NebulaScreen** 是一款面向企业级运维与生产管理的 **数据可视化大屏**。它基于 **Vue 3 + TypeScript + ECharts** 构建，以 **1920×1080** 为设计基准，通过优雅的“星云”视觉风格、实时数据刷新和灵活的 Mock/API 双模式，帮助团队快速建立态势感知、监控关键指标、洞察业务趋势。

---

## ✨ 核心功能

| 模块 | 说明 |
|------|------|
| **关键指标总览** | 设备总数、在线率、当日产量、告警数量 – 四个核心 KPI 以动态数字翻牌器呈现，数值变化带有平滑过渡动画。 |
| **趋势分析** | 24 小时产量与效率双轴折线图，直观展示当日生产节奏，支持 hover 查看详情。 |
| **告警分类占比** | 饼图展示各类告警（设备故障、工艺偏差、环境异常等）的构成比例，中心显示告警总数。 |
| **区域生产排行** | 横向条形图按产量对各车间/区域排序，快速定位高产能与低产能区域。 |
| **综合运行评分** | 六维雷达图（产能、质量、能效、稳定性、安全、交付），全面评估整体运行健康度。 |
| **实时告警流** | 滚动列表滚动展示最新告警信息（时间、区域、级别、内容），鼠标悬停暂停滚动，支持紧急/重要/次要/提示四级标签。 |
| **自适应布局** | 页面自动适配任意屏幕尺寸，始终保持 16:9 比例居中显示，完美用于大屏投放或浏览器全屏。 |
| **自动轮询刷新** | 默认每 5 秒自动拉取最新数据，保证实时性，同时支持手动刷新。 |

---

## 🛠️ 技术栈

| 类别 | 技术 |
|------|------|
| 框架 | Vue 3（组合式 API + `<script setup>`）|
| 语言 | TypeScript（严格模式）|
| 构建工具 | Vite |
| 状态管理 | Pinia |
| 图表引擎 | ECharts 5 + vue-echarts 封装 |
| 样式方案 | SCSS（设计令牌 + 全局样式）|
| 测试框架 | Vitest + jsdom + @vue/test-utils |
| 代码规范 | ESLint + Prettier |
| 日志系统 | 内置统一 Logger（带彩色输出和分级）|

---

## 📁 项目结构（核心）

```
nebula-screen/
├── src/
│   ├── api/                         # 数据服务层（抽象接口 + 双实现）
│   │   ├── types.ts                 # 业务数据类型与 DataService 接口定义
│   │   ├── mock/
│   │   │   ├── mockData.ts          # Mock 数据生成器（带随机波动）
│   │   │   └── mockService.ts       # Mock 服务（300~800ms 延迟，5% 失败模拟）
│   │   └── service/
│   │       ├── DataService.ts       # 接口再导出
│   │       └── apiService.ts        # 真实 API 实现（预留 fetch，暂抛错）
│   ├── components/                  # 可复用组件
│   │   ├── common/                  # 通用 UI（边框、翻牌器、加载、告警滚动）
│   │   └── charts/                  # 图表封装（BaseChart + 4 种图表）
│   ├── composables/                 # 组合式函数
│   │   ├── useScreenAdapt.ts        # 自适应缩放逻辑
│   │   ├── useDataService.ts        # 注入 DataService 的便捷钩子
│   │   └── usePolling.ts            # 轮询辅助
│   ├── stores/
│   │   └── screenStore.ts           # Pinia store – 集中管理所有大屏数据
│   ├── logger/
│   │   └── logger.ts                # 统一日志工具
│   ├── styles/                      # 全局样式与令牌（颜色、字体、混入）
│   ├── views/
│   │   └── MainScreen.vue           # 主大屏页面（布局 + 数据消费）
│   ├── App.vue                      # 根组件
│   └── main.ts                      # 入口 – 根据环境变量注入数据服务
├── tests/                           # 单元测试（服务、自适应钩子等）
├── index.html                       # 入口 HTML
├── package.json
├── tsconfig.json
├── vite.config.ts
├── vitest.config.ts
├── eslint.config.js
└── README.md
```

---

## 🔌 数据源切换（零业务改动）

项目设计为 **纯前端数据驱动**，所有组件和 Store **仅依赖 `DataService` 抽象接口**，不关心数据来自 Mock 还是真实 API。

- **开发阶段**：默认使用 `mockService`，生成含 ±5% 随机波动的仿真数据，并模拟网络延迟和偶发失败，帮助测试加载状态与错误处理。
- **生产对接**：只需在项目根目录创建 `.env` 文件，设置 `VITE_DATA_SOURCE=api`，并在 `src/api/service/apiService.ts` 中替换每个方法的 `fetch` 请求（已预留注释），业务代码 **零改动** 即可切换至真实后端。

```bash
# .env 示例
VITE_DATA_SOURCE=mock   # 默认，使用本地 Mock
# VITE_DATA_SOURCE=api  # 切换真实 API（需实现 apiService）
```

---

## 🚀 快速开始

### 环境要求
- Node.js 18+ / 20+
- npm / yarn / pnpm

### 安装与运行
```bash
# 克隆项目后进入目录
npm install

# 启动开发服务器（默认 http://localhost:5173）
npm run dev

# 运行单元测试
npm run test

# 生产构建（含 TypeScript 类型检查）
npm run build

# 代码检查与格式化
npm run lint
npm run format
```

### 访问地址
- 开发环境：`http://localhost:5173`（Vite 默认）
- 生产部署：将 `dist` 目录部署至任意静态服务器（Nginx、Apache、CDN 等）

---

## 🧪 测试覆盖

- **MockService 单元测试**：验证数据生成、随机性、延迟与失败处理。
- **自适应缩放计算**：测试 `calcScale` 在不同分辨率下的缩放逻辑。
- 测试框架使用 Vitest + jsdom，支持组件与组合式函数的测试扩展。

---

## 🎨 视觉设计

- **色彩系统**：以深蓝星空为背景，搭配 **霓虹蓝（#00F0FF）**、**星云紫（#A855F7）** 等高饱和亮色，营造科技感与沉浸感。
- **边框与光影**：每个卡片配有发光边框、四角装饰和微妙的玻璃态模糊效果，提升视觉层次。
- **动态细节**：翻牌器数字平滑过渡、告警滚动条滚动、时钟实时跳动、脉冲加载动画，增强交互反馈。
- **响应式**：基于 1920×1080 画布等比缩放，无论在大屏或笔记本上都保持布局完整。

---

## 📌 后续扩展方向

- **真实 API 接入**：实现 `apiService` 中的具体接口，对接业务后端。
- **更多图表类型**：可根据需求新增散点图、热力图、仪表盘等。
- **用户配置**：支持主题切换、指标自定义、刷新间隔调整。
- **告警联动**：点击告警项跳转至详情页或触发操作面板。
- **国际化**：预留多语言支持。

---

## 📄 开源协议

本项目仅供学习与内部使用，如有商业需求请联系作者。

---

**NebulaScreen** 是一个开箱即用的数据可视化起点，也是您构建个性化驾驶舱的坚实基础。欢迎探索、修改与扩展！如有问题或建议，请提交 Issue 或 Pull Request。

---

> 截图预览（实际运行效果）  
<img width="3072" height="1693" alt="capture_20260707155322679" src="https://github.com/user-attachments/assets/e9885c28-175f-43a7-a2a0-db77bf0d7e20" />
