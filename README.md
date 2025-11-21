![项目Logo](public/logo.webp)

# 当代互联网用户精神状态诊断 (Internet Mental State Diagnosis)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Vue](https://img.shields.io/badge/Vue.js-3.x-green.svg)
![Tailwind](https://img.shields.io/badge/Tailwind_CSS-3.x-38bdf8.svg)

[当代互联网用户精神状态诊断](https://emo.seeyou.cool/)

## 简介 (Introduction)

这是一个基于 **Vue 3** 和 **Tailwind CSS** 构建的趣味 Web 应用程序，旨在为当代互联网用户提供一份“科学”且幽默的精神状态诊断报告。

在这个快节奏的数字时代，每个人或多或少都患有某种“互联网综合症”。本项目通过模拟医疗诊断的流程，从扫描到出具报告，为用户提供一种娱乐化的自我调侃方式。

## 功能特点 (Features)

- **极简主义设计**：干净、现代的界面风格，专注于用户体验。
- **沉浸式诊断流程**：
  - **着陆页**：简单的入口，支持用户输入昵称，营造仪式感。
  - **扫描动画**：复古风格的扫描特效，增加诊断的趣味性和真实感。
  - **诊断报告**：生成一份详尽的“医疗报告”，包含：
    - **核心诊断**：一针见血的精神状态定义。
    - **症状描述**：列举 3 个随机生成的典型症状。
    - **病因分析**：一本正经的胡说八道。
    - **处方建议**：幽默风趣的治疗方案。
- **随机化内容**：基于预设的语料库（Corpus），每次诊断都会生成不同的结果组合。

## 技术栈 (Tech Stack)

- **前端框架**: [Vue 3](https://vuejs.org/) (Script Setup)
- **构建工具**: [Vite](https://vitejs.dev/)
- **样式库**: [Tailwind CSS](https://tailwindcss.com/)
- **图标**: (如有使用，例如 Lucide-vue-next 或 Heroicons)

## 快速开始 (Getting Started)

### 环境要求

确保你的本地环境已安装 [Node.js](https://nodejs.org/) (推荐 v16+)。

### 安装步骤

1.  **克隆项目**

    ```bash
    git clone <repository-url>
    cd cputop
    ```

2.  **安装依赖**

    ```bash
    npm install
    ```

3.  **启动开发服务器**

    ```bash
    npm run dev
    ```

    打开浏览器访问控制台输出的地址 (通常是 `http://localhost:5173`)。

4.  **构建生产版本**

    ```bash
    npm run build
    ```

## 项目结构 (Project Structure)

```
cputop/
├── public/              # 静态资源
├── src/
│   ├── components/      # Vue 组件 (LandingPage, ScanningPage, ReportPage)
│   ├── data/           # 数据语料库 (corpus.js)
│   ├── App.vue         # 根组件
│   ├── main.js         # 入口文件
│   └── style.css       # 全局样式 (Tailwind directives)
├── index.html           # HTML 模板
├── tailwind.config.js   # Tailwind 配置
├── vite.config.js       # Vite 配置
└── package.json         # 项目依赖和脚本
```

## 贡献 (Contribution)

欢迎提交 Issue 或 Pull Request 来丰富诊断语料库！

1.  Fork 本仓库
2.  新建分支 `feat/AmazingFeature`
3.  提交更改
4.  推送到分支
5.  提交 Pull Request

## 许可证 (License)

[MIT](LICENSE)
