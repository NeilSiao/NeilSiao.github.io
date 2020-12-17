---
title: 手把手將 Hexo 架設到 GithubPages 上
date: 2020-10-08 16:03
tags:
  - hexo
  - github_pages
---

1. [Hexo 官方文件](https://hexo.io/zh-tw/docs/)
   到 node 官網下載安裝檔，安裝 node.js 他會附帶 npm 工具。
   ```
   //hexo 的命令列工具，安裝完畢可以使用 hexo --help
   npm install -g hexo-cli  //(CMD 需重開。)
   hexo init <folder>
   cd <folder>
   npm install //安裝 hexo 相關的 package
   ```
   執行完上方的指令就等同成功建置完部落格了，剩下的就是相關參數設定，像是網站的標題、網站的風格、文章等等。此時可以直接先使用 `hexo server` ; 即可看到部落格的畫面。也可以使用 `hexo new page about` 先行新增一個關於部落格的介紹頁面玩玩看。
2. 佈署到 githubPages
   在 github 上面，建立一個 Github Repo，Repo 名稱為 <Github 用戶名>.github.io ，由於我是採用 Travis CI 進行佈署，所以還需要 Github 授權 Travis CI 權限，主要分為下方兩個部分。

   1. 前往 Travis 進行 github 帳號連結，且設定要授權給 travis 的 Repository。 (此部分只要登入 [Travis](https://travis-ci.com/) 點一點就 OK 了)
   2. 前往 Github/Settings/Deverloper settings/Personal access tokens 設立一個給 Travis CI 的 token，後續在每次 git commit 到 github 時，travis 會藉由這個 token 來進行 build 的腳本。

   建置給 travis 的 Token 該權限請將 repo 開好開滿。
   ![](/img/travis_ci_github_token.jpg 'Github token generate')

   接著到 Travis 網站貼上產生的 token，token name 可以參照後續的 .travis.yml
   ![](/img/travis_setting_gh.jpg 'Travis setting')

   當 travis 要 build 你的專案時，你還需要告訴 travis 該怎麼做，因此需要在 github repo 新增 `.travis.yml` 檔案，藉由這個檔案告訴 travis 該如何建置環境及 compile，這樣當 push 檔案到 github 時，travis 就會依照你的 .travis.yml 步驟進行 pull 與 build，然後 travis 會自動將 build 好的檔案自動 push 到 gh-pages 的分支之中。

   .travis.yml 內容參考如下

   ```
   // 詳細參數可參考 travis document
   sudo: false
   language: node_js
   node_js:
   - 10 # use nodejs v10 LTS
   cache: npm
   branches:
   only:
       - master # build master branch only
   script:
   - git submodule init
   - git submodule update --remote
   - hexo generate # generate static files
   deploy:
   provider: pages
   skip-cleanup: trueg
   github-token: $GH_TOKEN
   keep-history: true
   on:
       branch: master
   local-dir: public
   ```

   `注意`此時 Github 網站的根目錄還停留在 master branch 所以會看不到任何頁面，需要至 github Repo 的 Setting 進行 Source 切換。

   ![](/img/githubpage_change_branch.jpg)

   這樣就可以輸入 https://\<github username\>.github.io 看到建置好的網站了。

---

其他: Hexo 切換不同的部落格主題(Theme)

本站有使用到 [Hexo fluid theme](https://hexo.io/zh-cn/docs/) 這個主題，此時需要進行一些額外設定，其實主要參考官方網站慢慢看即可。剛好也有卡到 git submodule 的問題，後續再來寫一篇 git submodule...

另外 github pages 其實是有一些限制

- 容量上限建議 1GB 內，不能超過 5GB (Strongly recommended)
- 月流量不能超過 100GB
- 一個小時只能 build 10 次

要超過這些限制好像也挺難的...

> > 參考資源連結

- [Hexo](https://hexo.io/zh-cn/docs/)
- [Hexo Fluid](https://hexo.fluid-dev.com/docs/)
- [GithubPages](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/about-github-pages)
