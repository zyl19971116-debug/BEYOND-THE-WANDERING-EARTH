# THE EARTH MIGRATION PROJECT — SOURCE REPLACEMENT REPORT

生成时间：2026-10-16（Asia/Shanghai）

## 结论

当前工作目录已经整体替换为 GitHub 原仓库源码，而不是基于旧 React/Vite 项目重新实现。原仓库的 HTML、CSS、JavaScript、Three.js、Shader、模型、纹理、动画与静态资源均保持原目录结构。除本报告及验收截图外，没有修改原仓库运行代码或视觉资源。

## 路径与版本

- 当前项目：`C:\Users\21194\Documents\Codex\2026-10-15\new-chat`
- 旧项目完整备份：`C:\Users\21194\Documents\Codex\2026-10-15\EARTH-MIGRATION-BACKUP`
- 旧项目 Git bundle：`C:\Users\21194\Documents\Codex\2026-10-15\EARTH-MIGRATION-BACKUP\earth-migration-pre-source-replacement.bundle`
- 独立克隆校验目录：`C:\Users\21194\Documents\Codex\2026-10-15\earth-migration-source`
- 上游仓库：`https://github.com/oxomkbk/earth-migration.git`
- 上游 Commit SHA：`fc129932a990a45a4ca501aaeac74d4ff39ae089`
- 当前分支：`main`
- 旧项目备份 Commit：`bbc7d5a4e443b2287ca15f8fe412ba453cc2849f`

## 备份验证

- 旧项目代码、资源、配置与 `.git` 历史已复制到独立目录。
- 排除了可重新生成的 `node_modules`、`dist`、`.vite`、`test-results` 与 `*.tsbuildinfo`。
- Git bundle 已通过 `git bundle verify`，可恢复完整 Git 历史及 refs。
- 替换前旧工作区为干净状态。

## 源码与资源检查

- 原仓库共 123 个 Git 跟踪文件。
- 顶层目录完整：`assets/`、`css/`、`js/`、`screenshot/`、`tools/`、`vendor/`、`wallpaper/`。
- 顶层文件完整：`index.html`、`beyond.html`、`serve.py`、`README.md`、`LICENSE`、`.gitignore`。
- Git LFS 文件：0。
- Git 子模块：0。
- `assets/sky/stars.bin`、Three.js 本地 vendor、GLB、纹理及 Shader 入口均可访问。

## 安全检查

- 许可证：MIT。
- `serve.py` 仅使用 Python 标准库静态文件服务器、MIME 配置及打开本地浏览器，没有安装步骤或 shell 下载行为。
- 未发现 `eval`、`new Function`、WebSocket、`sendBeacon`、`XMLHttpRequest`、子进程执行或数据上传逻辑。
- 运行时 `fetch` 仅请求本地 `assets/sky/stars.bin`。
- 存储仅用于 HUD/壁纸设置的 `sessionStorage` 与 `localStorage`。
- 唯一外部运行资源为 Google Fonts CSS/字体请求；无用户数据提交。
- 未执行 `tools/` 中的构建脚本或任何陌生安装脚本。

## 本地运行

原仓库推荐 `python3 serve.py`，但本机当前没有可用的 Python/`py` 命令。因此使用无依赖的 Node.js 24 静态 HTTP 服务提供完全相同的源文件，没有执行 `npm install`，也没有构建或转译源码。

- 第一部：`http://127.0.0.1:8080/index.html`
- 第二部：`http://127.0.0.1:8080/beyond.html`
- 根路径：`http://127.0.0.1:8080/`（指向第二部）

三个地址均返回 HTTP 200。核心 JS、Three.js vendor 与星表二进制资源也返回 HTTP 200。

## 浏览器实测

### 第一部 `index.html`

- 首页启动画面：通过。
- `EXPLORE FREELY`：通过。
- 1–9 共九个真实镜头入口：全部可点击并成功切换。
- 地球、发动机、转向阵列、太空电梯、Apex、Habitat、Mining、Array、Luna：入口遍历通过。
- HUD 与实时数据更新：通过。
- 控制台 error/warn：0。

### 第二部 `beyond.html`

- 首页启动画面：通过。
- `FREE CAMERA` / Explore：通过。
- `FILM` 模式切换：通过。
- 地球、太空电梯、地球同步环、旗舰、护航舰队、深空观测站：通过。
- 太阳系总览、木星、土星：通过。
- 银河系：通过。
- 目的地列表、航行时间轴、轨道速度按钮：已加载。
- 控制台致命错误：0。
- WebGL 编译警告：2 组，为 Three.js/显卡驱动关于 `isnan` 和潜在未初始化采样变量的 warning；Shader 成功渲染，未出现编译失败或黑屏。

### 交互实现核验

- 鼠标拖拽旋转、滚轮跨尺度缩放、标记/目的地聚焦：事件监听与实际 UI 均存在。
- 第一部自由探索和场景切换已实际操作。
- 第二部 Explore、Film 和代表性目的地已实际操作。
- 全屏按钮和 `requestFullscreen` 处理已接入；自动化侧栏视口下按钮被响应式布局隐藏，未能完成浏览器级全屏状态确认。

## 截图

真实运行截图位于：`screenshot/validation/`

- `index-home.png`
- `index-elevator.png`
- `beyond-home.png`
- `beyond-solar-system.png`
- `beyond-solar-system-desktop.png`
- `beyond-milky-way.png`

## 未解决问题

1. 本机缺少 Python，因此未直接运行 `serve.py`；当前 Node 静态服务不改变任何页面行为或资源内容。
2. 自动化侧栏是窄视口，第二部的目的地信息面板在该宽度会拥挤；这是原仓库当前响应式布局表现，不是资源缺失。
3. 全屏按钮因自动化窄视口隐藏，未完成自动化点击验证；源码事件绑定存在。
4. 第二部存在非致命 WebGL driver warning，但无 Shader 编译失败、资源 404 或页面崩溃。

## 原版保留状态

第一轮未对布局、3D 模型、银河系 Shader、地球纹理、星云、太空电梯、空间站、飞船、摄像机、动画时间线、HUD 或导航进行任何重设计。当前网站主体即上游原始实现。
