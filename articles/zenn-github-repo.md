---
title: 'Zennの記事をGitHub連携でカッチリ管理するおすすめ設定'
emoji: '🔐'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['zenn', 'git', 'github', 'githubactions', 'npm']
published: true
published_at: 2025-01-29 07:00
---

## はじめに

はじめまして、[jonghyo](https://zenn.dev/jonghyo)と申します!

Zennの[GitHub連携機能](https://zenn.dev/zenn/articles/connect-to-github)とっても便利ですよね!

記事を書くにあたって、GitHubリポジトリの設定を色々やったのでおすすめ設定を共有します。

記載内容などを**カッチリ管理したい方**必見です👀

内容の詳細は以下リポジトリをご覧ください。

https://github.com/jonghyo/zenn.dev

## こんな人におすすめ!

- 記事の**内容や品質を楽に・カッチリチェック**したい!
- **コミットメッセージ**もしっかり書いて、かっこいいリポジトリにしたい!
- Zennの**記事をきれい**に管理したい!
- **これからZennとGitHubの連携設定**を行う方

## TL;DR

- GitHubリポジトリの`Automatically delete head branches Loading`, `Dependabot`有効化
- textlint, markdownlint-cli2, commitlint, secretlint, lefthookの導入
- GitHubActionsの導入

## 前提

この記事は以下を前提としています。

- GitHub上にZennの記事管理用のリポジトリ作成済み
- ZennとGitHub連携済み
- Node.js v22系の環境
- VSCode上で記事を執筆

## おすすめ設定

### GitHubリポジトリSettings

#### Automatically delete head branches Loading

Zenn記事管理用リポジトリの `Settings > General > Pull Requests`の`Automatically delete head branches Loading`を有効にします。

![Automatically deletehead branches Loading](/images/zenn-github-repo/auto-delete-branch.png)

新しく記事を作成する際は、featureブランチを切って管理した方が作業途中の記事が公開されずに済むのでベターですが**mainにマージ後、古いfeatureブランチを自動で削除してくれるようになり便利です!**

#### Dependabotの有効化

`Settings > Code security > Dependabot`の`Dependabot version updates`を有効化します。

![Dependabot](/images/zenn-github-repo/dependabot.png)

Dependabotの定義は以下です。

```yml:dependabot.yml
# To get started with Dependabot version updates, you'll need to specify which
# package ecosystems to update and where the package manifests are located.
# Please see the documentation for all configuration options:
# https://docs.github.com/code-security/dependabot/dependabot-version-updates/configuration-options-for-the-dependabot.yml-file

version: 2
updates:
  - package-ecosystem: 'github-actions'
    directory: '/'
    schedule:
      interval: weekly
      time: '10:00'
      timezone: Asia/Tokyo
    open-pull-requests-limit: 10
    commit-message:
      prefix: 'Common'
      include: 'scope'
    labels:
      - 'dependencies'
  - package-ecosystem: 'npm'
    directory: '/'
    schedule:
      interval: daily
      time: '10:00'
      timezone: Asia/Tokyo
    open-pull-requests-limit: 10
    commit-message:
      prefix: 'Common'
      include: 'scope'
    labels:
      - 'dependencies'
```

後述する、GitHub Actionsやnode_modulesを自動でアップデートしてくれるようにします。

### Node modules

記事の管理を楽にするため、いくつかの`node_modules`を追加します。

- [Linter](#linter)
- [Formatter](#formatter)
- [Git hook](#git-hook)

の設定を入れます。

#### Linter

Linterは

- [textlint](https://github.com/textlint/textlint)
- [markdownlint-cli2](https://github.com/DavidAnson/markdownlint-cli2)

を導入します。

##### textlint

textlintは文章構成ツールです。

今回は以下プラグインを追加して利用します。

| プラグイン | 詳細 |
| --- | --- |
| [textlint-rule-preset-smarthr](https://github.com/kufu/textlint-rule-preset-smarthr) | SmartHRさんが提供するtextlintルールプリセット |
| [textlint-filter-rule-comments](https://github.com/textlint/textlint-filter-rule-comments) | textlintのルールを適用させたくないファイルなどをフィルターするためのプラグイン |
| [textlint-filter-rule-allowlist](https://github.com/textlint/textlint-filter-rule-allowlist) | 特定の文言にルールを適用させたくない場合にフィルターするためのプラグイン |
| [textlint-rule-preset-ja-spacing](https://github.com/textlint-ja/textlint-rule-preset-ja-spacing) | 日本語周りにおけるスペースの有無を決定するtextlintルールプリセット |
| [textlint-rule-preset-ja-technical-writing](https://github.com/textlint-ja/textlint-rule-preset-ja-technical-writing) | 技術文書向けのtextlintルールプリセット |
| [textlint-rule-spellcheck-tech-word](https://github.com/azu/textlint-rule-spellcheck-tech-word) | 技術用語の表記揺れをチェックするルール |
| [textlint-rule-prh](https://github.com/textlint-rule/textlint-rule-prh) | textlintのルールセットを自前で定義するためのプラグイン |
| [textlint-plugin-mdx](https://github.com/textlint/textlint-plugin-mdx) | textlintをmdxにも対応させるプラグイン。zennはmdx非対応なのでいらなかった... |

以下コマンドでインストールします。

```bash
npm install -D textlint textlint-rule-preset-smarthr textlint-filter-rule-comments textlint-rule-preset-ja-spacing textlint-rule-preset-ja-technical-writing textlint-rule-spellcheck-tech-word textlint-rule-prh
```

textlintのルールは使ってみた結果、以下で運用しています。

```yml:.textlintrc.yml
---
plugins:
  mdx: true
filters:
  comments: true
rules:
  # SmartHR のプリセットルールを使用（句点混在のルールはオフ）
  preset-smarthr:
    # 文末の句点が混在していてもエラーにしない
    ja-no-mixed-period: false
  preset-ja-spacing:
    # 全角と半角の間のスペースのルールをオフ
    ja-space-between-half-and-full-width: false
  preset-ja-technical-writing:
    # 文の長さの制限をオフ（長い文章が必要な場合に対応）
    sentence-length: false
    # 文末の句点が混在していてもエラーにしない
    ja-no-mixed-period: false
    # 漢字の連続使用制限をオフ（専門用語や固有名詞が多い場合に対応）
    max-kanji-continuous-len: false
    # 感嘆符（!）や疑問符（?）の使用制限をオフ
    no-exclamation-question-mark: false
  ja-technical-writing:
    # カンマ(,)の上限をオフ
    max-comma: false
  # 技術用語のスペルチェックを有効化
  spellcheck-tech-word: true
  # カスタムの置換ルールを適用
  prh:
    rulePaths:
      - ./rules.yml
```

`rules.yml`は任意で表記揺れを防ぐ定義を作成できます。サーバーの表記揺れなどを定義しておくと防ぐことができます。

```yml:rules.yml
version: 1
rules:
  - expected: サーバー
    pattern: サーバ
```

VSCodeの[vscode-textlint](https://marketplace.visualstudio.com/items?itemName=taichi.vscode-textlint)をインストールします。

以下設定を`.vscode/setting.json`に追加すると、エディタ上でリアルタイムにチェックできます!

```json:.vscode/setting.json
{
  "textlint.languages": [
    "mdx",
    "markdown",
    "plaintext",
    "html"
  ],
}
```

![textlint](/images/zenn-github-repo/textlint.png)

npmコマンドにもtextlintを追加します。

```json:package.json
"scripts": {
    "start": "zenn preview",
    "lint:docs": "textlint --cache **/*.{md,mdx}",
    "lint:docs:fix": "textlint --cache --fix **/*.{md,mdx}"
  },
```

コマンドを実行することにより、textlintのチェックをCLI上でもかけることができます。

GitHub Actionsと連携する際便利になります!

![textlint](/images/zenn-github-repo/npm-run-lint.png)

#### markdownlint-cli2

markdownlint-cli2はmarkdown記法のlintをかけてくれるチェックツールです。

markdownの記法自体もきれいに書きたいので導入します。

以下コマンドでインストールします。

```bash
npm i -D markdownlint-cli2
```

使ってみた結果、以下設定で利用しています。

```jsonc:.markdownlint-cli2.jsonc
{
  "config": {
    "line-length": false, // MD013  行の長さ
    "no-hard-tabs": false, // MD010 Markdown中にTSVを書くとエラーになるため
    "no-trailing-punctuation": false, // MD026 ヘディングに句読点（.,;:!?）を許容したい
    "no-inline-html": false, // MD033 HTMLは許容
    "no-bare-urls": false, // MD034 URLのリンク化条件
    "no-space-in-emphasis": false, // MD037  $$など数式で問題になったため外す
    "link-fragments": false, // MD051 相対パス設定
    "first-line-h1": false, //MD041 first-line-heading
    "no-duplicate-heading": false //MD024 no-duplicate-heading
  },
  "ignores": [".git", "node_modules"] // 無視するディレクトリ
}
```

導入すると、markdownの書き方についてチェックしてくれるようになります。

![markdownlint](/images/zenn-github-repo/markdownlint.png)

npmコマンドにもmarkdownlint-cli2を追加します。

```json:package.json
"scripts": {
    "start": "zenn preview",
    "markdownlint": "markdownlint-cli2 \"**/*.{md,mdx}\"",
    "markdownlint:fix": "markdownlint-cli2 --fix \"**/*.{md,mdx}\"",
    "lint:docs": "textlint --cache **/*.{md,mdx}",
    "lint:docs:fix": "textlint --cache --fix **/*.{md,mdx}"
  },
```

コマンドを実行することにより、markdownlint-cli2のチェックをCLI上でもかけることができます。

![npm run markdownlint](/images/zenn-github-repo/npm-run-markdownlint.png)

#### Formatter

コードフォーマッタは[Prettier](https://prettier.io/)を採用しました。

:::message

動作の軽快な[Biome](https://biomejs.dev/)を使いたいところだが、markdownのFormattingは未対応...

[Biome | Language support](https://biomejs.dev/internals/language-support/)

:::

:::message

2025/1/31 追記

動作の軽快な[dprint](https://dprint.dev/plugins/markdown/)がmarkdown対応しており、高速に動作するコードフォーマッタとしては適切かもです!

:::

以下コマンドでインストールします。

```bash
npm i -D prettier
```

設定は以下としました。

```json:.prettierrc
{
  "printWidth": 90,
  "tabWidth": 2,
  "useTabs": false,
  "semi": false,
  "singleQuote": true,
  "jsxSingleQuote": false,
  "quoteProps": "as-needed",
  "trailingComma": "none",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "requirePragma": false,
  "insertPragma": false,
  "htmlWhitespaceSensitivity": "css",
  "embeddedLanguageFormatting": "auto",
  "singleAttributePerLine": false,
  "proseWrap": "never"
}
```

`.vscode/setting.json`に以下を追加することでフォーマッタが効くようになります。

```json:.vscode/setting.json
{
  "textlint.languages": [
    "mdx",
    "markdown",
    "plaintext",
    "html"
  ],
  //追加
  "[markdown]": {
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "prettier.configPath": ".prettierrc"
}
```

#### Git hook

GitHubへプッシュする前に、textlintやmarkdownlintのチェックを潰し込めているか事前チェックできるようにします。

今回は動作が軽快な[lefthook](https://github.com/evilmartians/lefthook)を採用しました。

Git hookで以下をチェックすることにしました。

| フェーズ | チェック | 詳細 |
| --- | --- | --- |
| pre commit | [secretlint](https://github.com/secretlint/secretlint) | シークレット情報を誤ってGitHubにpushしないようにチェック |
|  | formatting | prettierフォーマットの修正を適用 |
|  | textlint | textlintのチェック |
|  | markdownlint | markdown記法をチェック |
| commit-msg | [commitlint](https://github.com/conventional-changelog/commitlint) | コミットメッセージが[conventional commit format](https://conventionalcommits.org/)に従っているかチェック |

以下コマンドでインストールします。

```bash
npm i -D lefthook secretlint @secretlint/secretlint-rule-preset-recommend @commitlint/cli @commitlint/config-conventional
```

##### secretlint

secretlintは以下設定とします。

```yml:.secretlintrc.yml
rules:
  - id: '@secretlint/secretlint-rule-preset-recommend'
```

##### commitlint

commitlintは以下設定とします。

```yml:.commitlintrc.yml
extends:
  - '@commitlint/config-conventional'
rules:
  subject-case:
    - 2
    - 'never'
    - - 'start-case'
      - 'sentence-case'
      - 'pascal-case'
```

conventional commit formatに従って

```text
<type>(<scope>): <subject>
```

上記のようなフォーマットでコミットメッセージを入力しないとエラーとなります。subjectは小文字以外エラーになります。

e.g. 以下のようなコミットメッセージを求めるようになります

```text
docs(zenn-github-repo): add articles
```

##### lefthook

lefthookの設定を以下のように行ないます。

```yml:lefthook.yml
# Refer for explanation to following link:
# https://github.com/evilmartians/lefthook/blob/master/docs/configuration.md

pre-commit:
  parallel: true
  commands:
    # common rules
    secretlint:
      glob: '*'
      run: npx secretlint --maskSecrets --secretlintignore .gitignore "{staged_files}"
    formatting:
      glob: '**/*.{js,ts,jsx,tsx,md,mdx,json,yml}'
      run: npx prettier --write "{staged_files}"
    # for docs edit rules
    textlint:
      glob: '**/*.{md,mdx}'
      run: npx textlint --cache {staged_files}
    markdownlint:
      glob: '**/*.{md,mdx}'
      run: npx markdownlint-cli2 {staged_files}
commit-msg:
  parallel: true
  commands:
    commitlint:
      run: npx commitlint -e
```

これで、Git hookで各種lintをかけられるようになりました。

この例では**コミットメッセージがconventional commit formatに従ってない**ので、エラーになり`git commit`が抑止されています。

![lefthook](/images/zenn-github-repo/lefthook.gif)

### GitHub Actions

基本的にGit hookで各lintのチェックはかけられますが、GitHub上でササッと記事編集してPRを出すケースもあるのでCI側でもlintをかけられるようにします。

GitHub Actionsは**PRをmainブランチに出したとき && mdファイルが編集されたとき**のみ実行されるようにします。

```yml:.github/workflows/lint.yml
# This is a basic workflow to help you get started with Actions

name: reviewdog

# Controls when the action will run.
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  pull_request:
    branches: [main]
    paths:
      - '**/*.md'

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  reviewdog-github-check:
    name: reviewdog (github-check)
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repo
        uses: actions/checkout@master

      - name: Setup Node/npm
        uses: actions/setup-node@v4
        with:
          node-version: 22

      # キャッシュリストア
      - name: Restore cache
        id: cache-restore
        uses: actions/cache/restore@v4
        with:
          key: ${{ runner.os }}-cache
          path: |
            ~/.npm
            .textlintcache

      # 依存モジュールインストール
      - name: Install node modules
        if: ${{ steps.cache_dependency.outputs.cache-hit != 'true' }}
        run: 'npm ci --no-audit --progress=false --silent'

      # textlint
      - name: Exec textlint-github-pr-check
        uses: tsuyoshicho/action-textlint@v3
        with:
          github_token: ${{ secrets.github_token }}
          reporter: github-pr-review
          level: warning
          textlint_flags: '**/*.{md,mdx}'

      # markdownlint
      - name: Exec markdwonlint
        run: 'npm run markdownlint'

      # キャッシュセーブ
      - name: Always Save Cache
        id: cache-save
        if: always() && steps.cache-restore.outputs.cache-hit != 'true'
        uses: actions/cache/save@v4
        with:
          key: ${{ steps.cache-restore.outputs.cache-primary-key }}
          path: |
            ~/.npm
            .textlintcache
```

これでPRを出したときにmarkdownファイルの変更を検知してGitHub Actions上でlintのチェックがかかるようになります。

![GitHub Actions](/images/zenn-github-repo/github-actions.png)

## まとめ

textlintやmarkdownlintを入れることできれいに読みやすい記事がかけるようになりました。

**「です・ます調」「だ・である調」は記事書いているときに混在しがち**だったので助かってます。

ただ、たまにtextlintさん厳しい...😇 ※その際は、ルールのチューニングを..

皆さんもぜひ参考にしてみてください!

また、皆さんのおすすめの設定をぜひコメントで教えてください🙇

## 参考リンク

https://zenn.dev/eguchi244_dev/articles/github-zenn-linkage-20230501

https://zenn.dev/yuta28/articles/blog-lint-ci-reviewdog

---

最後までお読みいただきありがとうございました!

Xでも技術ネタをポストしているので、よかったらフォローしてもらえると嬉しいです😋

→ [X@jonghyo\_](https://x.com/jonghyo_)
