# 博客项目 AI 上下文

最后更新：2026-04-28

## 项目概览

- 项目类型：个人静态博客
- 生成器：Hugo `0.158.0` extended
- 主题：`github.com/CaiJimmy/hugo-theme-stack/v3` `v3.34.2`
- 仓库中的 Go 版本：`1.25.4`
- 源分支：`master`
- 部署流程：GitHub Actions 构建站点，并将 `public/` 发布到 `gh-pages`
- 网站先通过`github pages`部署于地址`https://knowckx.github.io/`
- 然后增加了自定义域名：`https://blog.knowckx.top/` pages 会跳转到此自定义域名


## 参考文档

- `专属文风卡.md`：写新文章时优先参考

## 文档约定

- `user_doc/`：用户手工编辑的文档
- AI 可以读取 `user_doc/` 下的内容，但不要主动改写
- `ai_doc/` 根目录：放项目级 AI 参考文档和约定
