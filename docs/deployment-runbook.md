# P5 部署与恢复手册

P5 是纯静态 Vue 应用，主部署为 Netlify。GitHub Actions 仅在 `verify-p5` 全绿后发布 `dist`。

## 发布

```powershell
npm ci
npm run check
npm test
npm run build
```

Netlify 配置已包含 SPA fallback。Cloudflare Pages 与 EdgeOne Pages 备用部署均使用：构建命令 `npm run build`、输出目录 `dist`、Node 24；不需要环境变量。

## Smoke test

检查首页 200、标题“星声音乐站”、合成音搜索有结果、重复点歌被拒、空队列安全、外部许可链接新窗口打开。音频是否自动播放不是健康标准，必须由用户点击。

## 恢复

1. 在 GitHub Release 下载上一 Tag 或将 Netlify production 回滚到已知正常 deploy。
2. 再次检查首页与本地合成音主流程。
3. 若只有外部来源不可用，保持站点在线并切回默认合成音，无需整体回滚。

队列保存在浏览器 localStorage，不是服务器业务数据，不需要数据库备份；清除站点数据即可重置。
