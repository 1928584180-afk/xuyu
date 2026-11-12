# 叙野 · 手机小说工作台（PWA）

这是一个适配 iPhone 屏幕的本地 PWA 小说创作页。它**不是** `.mobileconfig` 配置描述文件——`.mobileconfig` 只能安装 Wi-Fi、VPN、证书、Web Clip 等系统配置，无法承载可交互的 HTML/JavaScript 应用。

## 文件

- `index.html`：完整应用（书架、设定、手稿、记忆、对话预览、钢笔重写）。
- `manifest.webmanifest`：PWA 应用信息。
- `sw.js`：离线缓存 Service Worker。
- `icon-192.png`、`icon-512.png`、`icon-maskable-512.png`、`apple-touch-icon.png`：主屏幕图标。
- `README.md`：本说明。

## API 配置

页面**不包含任何 API Key 或预置模型**。首次打开后：

1. 点击右上角「设置」。
2. 填写：
   - Base URL，例如 `https://api.example.com/v1`
   - API Key
3. 点「检测模型」，程序会请求 `/models` 并列出全部可用模型。
4. 在模型列表中搜索并选择模型。
5. 保存配置。

配置只保存在当前浏览器的本地存储中。关闭“记住 API Key”后，密钥只存在于当前标签页。

固定参数：Temperature `0.70`、Top P `0.90`、Top K `50`、Frequency Penalty `0.20`、Presence Penalty `0.20`、Repetition Penalty `1.00`。TOP K 与 Repetition Penalty 不被接口支持时会自动省略并重试。

## 在 iPhone 上安装

完整 PWA 需要 HTTPS 页面（本地文件或局域网 HTTP 只能使用网页版，Service Worker 和部分安装能力会受限）。

1. 把这个文件夹部署到任意静态 HTTPS 托管（GitHub Pages、Cloudflare Pages、Netlify、Vercel 等）。
2. 用 iPhone Safari 打开部署后的地址。
3. 点「分享」→「添加到主屏幕」。
4. 从主屏幕打开「叙野」，像 App 一样全屏使用。

页面本身不向除你配置的模型接口之外的服务器上传内容。书稿、记忆和 API 配置都留在本机浏览器中。

## 本地预览

在文件夹中运行：

```powershell
python -m http.server 8080
```

然后打开 `http://localhost:8080`。桌面浏览器可以用它测试布局和 Service Worker；iPhone 要通过可访问的 HTTPS 地址安装。

## 数据说明

- 书籍、设定、记忆与对话历史存放在浏览器的 localStorage 中。
- 设置页可清除全部书籍；清除后不可恢复。
- 更换浏览器、清理站点数据或卸载 PWA 时，本地数据可能丢失，重要内容建议用「下载」导出 TXT。
