Martin Matin - Netlify + 外部视频存储 v5

这版与 v4 相同的界面、字幕、跟读、句子跳转、单词音频等功能保留不变。
唯一核心变化：12 MB 左右的视频不再放在 Netlify，而是从外部 HTTPS 视频直链播放。

一、部署前必须配置视频地址

打开：
assets/video-config.json

把：
PASTE_YOUR_VIDEO_DIRECT_URL_HERE

替换成你外部视频存储提供的 MP4 直链，例如：
https://你的域名.example/martin-matin-mobile.mp4

二、推荐：Cloudflare R2

1. 登录 Cloudflare Dashboard。
2. Storage & databases → R2 → Create bucket。
3. 上传 martin-matin-mobile.mp4。
4. 在 Public access 中启用公开访问，或绑定自己的 Custom Domain。
5. 复制 MP4 的公开 HTTPS URL。
6. 填入 assets/video-config.json 的 video 字段。
7. 将整个 martin-matin-netlify-v5 文件夹部署到 Netlify。

注意：不要把视频 MP4 再放回这个 Netlify 文件夹，否则视频流量仍会经过 Netlify。

三、当前包中保留的公共资源

assets/subtitles.json
assets/audio-manifest.json
assets/audio/sentence-001.mp3
assets/audio/word-martin.mp3
assets/audio/word-matin.mp3

四、外部视频要求

- 必须是公开 HTTPS URL
- 最好直接返回 video/mp4
- 服务端应支持 HTTP Range 请求，手机拖动进度/分段加载会更稳定
- 不要使用需要登录、临时 Cookie 或 Referer 才能播放的链接

五、为什么这样改

Netlify 只负责网页、字幕和小音频；大视频由外部对象存储/CDN提供。
这样可以显著降低 Netlify 的 Bandwidth credits 消耗。

六、如果以后换视频

只需要把 assets/video-config.json 里的 video URL 换成新 MP4 的公开直链，不需要重新修改网页代码。
