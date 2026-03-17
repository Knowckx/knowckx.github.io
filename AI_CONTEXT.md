# AI Context For This Blog Project

Last updated: 2026-03-17

## Project Overview

- Project type: static personal blog
- Generator: Hugo `0.158.0` extended
- Theme: `github.com/CaiJimmy/hugo-theme-stack/v3` `v3.34.2`
- Go version in repo: `1.25.4`
- Source branch: `master`
- Deployment flow: GitHub Actions builds the site and deploys `public/` to `gh-pages`
- Public URLs:
  - GitHub Pages: `https://knowckx.github.io/`
  - Custom domain: `https://blog.knowckx.top/`

## Current Goal

The current priority is Google indexing.

The site is online and reachable, but as of 2026-03-17 Google Search Console still shows the site as not indexed yet. The main issue is not a deployment failure. Google can crawl the site, but currently does not include it in the index.

## Search Console Status

Observed on 2026-03-17 in Search Console for property `https://blog.knowckx.top/`:

- `0` indexed pages
- `46` not indexed pages
- Main reason: `已抓取 - 尚未编入索引`
- Secondary reason: `404` on a few old taxonomy URLs
- Sitemap `/sitemap.xml` was successfully read

URL inspection results that were verified:

- Homepage `https://blog.knowckx.top/`
  - crawl allowed: yes
  - indexing allowed: yes
  - fetch: success
  - canonical: self-selected and Google-selected are both correct
  - status: not indexed yet
- Post `https://blog.knowckx.top/p/try-to-use-svelte/`
  - crawl allowed: yes
  - indexing allowed: yes
  - fetch: success
  - canonical: correct
  - status: not indexed yet

This means the site is technically crawlable, but Google has not accepted it into the index yet.

## What Has Already Been Done

### Toolchain and Deployment

- Upgraded Go from `1.17` to `1.25.4`
- Pinned Hugo in CI to `0.158.0` instead of using `latest`
- Updated GitHub Actions Go setup to read version from `go.mod`
- Verified local builds with global Hugo `v0.158.0+extended`
- Replaced old Chocolatey Hugo with Scoop-installed Hugo

Relevant files:

- `go.mod`
- `.github/workflows/deploy.yml`

### SEO and Template Fixes

- Fixed pagination canonical so `/page/2/`, `/page/3/` no longer canonicalize to the homepage
- Added generic page-level `robots` handling from front matter
- Added `noindex,follow` for low-value utility pages:
  - `/search/`
  - `/archives/`
  - `/links/`
- Added default `noindex,follow` for taxonomy and term pages
- Added structured data:
  - homepage: `WebSite`
  - posts: `BlogPosting`
  - about page: `ProfilePage`
- Removed leftover theme sample copy on the links page
- Removed debug `console.log` from music player partial

Relevant files:

- `layouts/partials/head/head.html`
- `layouts/partials/head/custom.html`
- `layouts/partials/head/structured-data.html`
- `layouts/partials/music-player.html`
- `content/page/about/index.md`
- `content/page/search/index.md`
- `content/page/archives/index.md`
- `content/page/links/index.md`

### Sitemap and Indexing Signal Improvements

- Reworked sitemap so it no longer contains only post pages
- Homepage is now included in `sitemap.xml`
- Pages marked with `sitemap.disable` remain excluded
- Taxonomy pages are discouraged from indexing via `noindex,follow`

Relevant file:

- `layouts/sitemap.xml`

### Search Console Actions Already Taken

- Re-submitted `/sitemap.xml` in Google Search Console on 2026-03-17
- Requested indexing for the homepage
- Requested indexing for `https://blog.knowckx.top/p/try-to-use-svelte/`
- Requested indexing for `https://blog.knowckx.top/p/shadcn-react.forwardref/`

The request flow returned the success message `已请求编入索引`, meaning the URLs were added to Google's priority crawl queue. This does not guarantee indexing, but it confirms the request was accepted.

## Commits Related To This Work

- `f540d7c` `chore: upgrade hugo toolchain and improve seo`
- `d6d2404` `fix: improve sitemap coverage for indexing`

## Important Notes For Future AI Sessions

- Do not treat the indexing problem as a deployment issue unless the live site is actually broken.
- The live site has already been verified to contain the recent SEO fixes.
- Search Console access required a logged-in browser session; if browser automation is used again, check login state first.
- When using browser automation in Search Console, do not wait only for a generic completion event after clicking `请求编入索引`.
- Search Console shows success through an in-page async success layer with text like `已请求编入索引` and a `关闭` button, not necessarily through a browser-native dialog event.
- If automation is used again, treat the appearance of `已请求编入索引` or the `关闭` button as the success signal and continue immediately.
- There is an unrelated deleted documentation file in the worktree that should not be restored or committed unless explicitly requested by the user:
  - `_doc/关于收录/10 收录的问题.md`

## Recommended Next Verification Steps

When checking again in the future:

1. Confirm the latest deploy is live on `https://blog.knowckx.top/`
2. Open Search Console for `https://blog.knowckx.top/`
3. Check whether indexed page count has increased
4. Inspect at least these URLs:
   - `https://blog.knowckx.top/`
   - `https://blog.knowckx.top/p/try-to-use-svelte/`
   - `https://blog.knowckx.top/p/shadcn-react.forwardref/`
5. Confirm whether the sitemap is still read successfully
6. If pages remain in `已抓取 - 尚未编入索引`, treat it as a Google-side quality/indexing lag issue first, not a robots/canonical issue

## Useful Commands

Local build:

```powershell
hugo --minify --gc
```

Module status:

```powershell
go list -m -u -json github.com/CaiJimmy/hugo-theme-stack/v3
```

## Short Summary

This project has already completed the most important technical SEO cleanup. The remaining state is "waiting for Google indexing feedback", not "site broken". Future work should focus on Search Console re-checks and content quality/indexing follow-up, not redoing the same technical fixes blindly.
