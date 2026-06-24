# Home

一个基于 Vue 3 和 Vite 的个人主页项目，适合用作个人首页或轻量展示页。页面包含站点简介、社交链接、一言、天气、时间、时光进度、音乐播放器、壁纸展示和移动端适配。

![主页预览](./screenshots/main.jpg)

## 功能

- 载入动画与随机壁纸
- 站点简介、社交链接
- 实时时间、日期、天气展示
- Hitokoto 一言
- 时光进度条与建站时间统计
- 音乐播放器，支持 Meting API 歌单
- PWA 配置与静态资源压缩
- 响应式布局，支持移动端

## 技术栈

- Vue 3
- Vite
- Pinia
- Element Plus
- IconPark / xicons
- @worstone/vue-aplayer

## 目录结构

```text
.
├── public/                 # 静态资源：字体、背景图、站点图标
├── screenshots/            # README 预览截图
├── src/
│   ├── api/                # 音乐、一言、天气接口
│   ├── assets/             # 社交链接配置
│   ├── components/         # 通用组件
│   ├── store/              # Pinia 状态
│   ├── style/              # 全局样式
│   ├── utils/              # 时间、鼠标等工具函数
│   └── views/              # 页面区域组件
├── .env.example            # 环境变量示例
├── Dockerfile              # Docker 构建配置
└── vite.config.js          # Vite 配置
```

## 本地运行

建议使用 Node.js 18 或更高版本。

```bash
# 安装依赖
pnpm install

# 复制并修改环境变量
cp .env.example .env

# 启动开发环境
pnpm dev

# 构建生产文件
pnpm build

# 本地预览生产构建
pnpm preview
```

构建产物会生成在 `dist/` 目录中，可直接部署到静态托管平台。

## 环境变量

复制 `.env.example` 为 `.env` 后按需修改：

```bash
VITE_SITE_NAME = "Seanの主页"
VITE_SITE_AUTHOR = "Sean"
VITE_SITE_KEYWORDS = "Sean,个人主页"
VITE_SITE_DES = "一个默默无闻的主页"
VITE_SITE_URL = "example.com"
VITE_SITE_LOGO = "/images/icon/favicon.ico"
VITE_SITE_MAIN_LOGO = "/images/icon/logo.png"
VITE_SITE_APPLE_LOGO = "/images/icon/apple-touch-icon.png"

VITE_DESC_HELLO = "Hello World !"
VITE_DESC_TEXT = "一个建立于 21 世纪的小站，存活于互联网的边缘"
VITE_DESC_HELLO_OTHER = "Oops !"
VITE_DESC_TEXT_OTHER = "哎呀，这都被你发现了（ 再点击一次可关闭 ）"

VITE_WEATHER_KEY = ""
VITE_SITE_START = "2020-10-24"
VITE_SITE_ICP = ""

VITE_SONG_API = "https://api-meting.imsyy.top/api"
VITE_SONG_SERVER = "netease"
VITE_SONG_TYPE = "playlist"
VITE_SONG_ID = "7452421335"
```

说明：

- `VITE_WEATHER_KEY`：高德开放平台 Web 服务 Key。留空时会使用备用天气接口。
- `VITE_SITE_START`：建站日期，用于页脚年份和时光统计。
- `VITE_SITE_ICP`：备案号，无备案可留空。
- `VITE_SONG_*`：音乐播放器配置，支持 `netease`、`tencent` 等 Meting API 支持的来源。

## 内容配置

### 社交链接

修改 `src/assets/socialLinks.json`：

```json
{
  "name": "Github",
  "icon": "/images/icon/github.png",
  "tip": "去 Github 看看",
  "url": "https://github.com/your-name"
}
```

图标文件放在 `public/images/icon/`，路径以 `/images/icon/` 开头。

### 背景图

默认使用 `public/images/background1.jpg` 到 `background10.jpg`。如果增删背景图，需要同步调整 `src/components/Background.vue` 中随机数范围：

```js
const bgRandom = Math.floor(Math.random() * 10 + 1);
```

## 部署

### 静态部署

```bash
pnpm build
```

将 `dist/` 目录上传到服务器、对象存储、Vercel、Netlify、Cloudflare Pages 等静态托管平台即可。

### Docker

```bash
docker build -t home .
docker run -p 12445:12445 -d home
```

服务默认监听 `12445` 端口。

### GitHub Actions

仓库包含 `.github/workflows/build.yml`，推送到 `dev` 或 `master` 分支时会自动构建并上传 `dist` 构建产物。

## 常用命令

```bash
pnpm dev       # 开发
pnpm build     # 生产构建
pnpm preview   # 预览构建结果
pnpm lint      # ESLint 修复
pnpm format    # Prettier 格式化 src
```

## 字体

项目使用本地 `Pacifico-Regular.ttf` 和 `UnidreamLED.ttf`，并在 `index.html` 中通过外链加载 HarmonyOS Sans。Logo 字体如需替换，可参考 `public/font/README.md`。

## 许可证

本项目基于 MIT License 开源，详见 [LICENSE](./LICENSE)。
