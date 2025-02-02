# zenn.dev

My zenn.dev articles is here.

<a href="https://zenn.dev/jonghyo" target="_blank"><img alt="Zenn" src="https://img.shields.io/badge/Zenn-3EA8FF.svg?&style=for-the-badge&logo=Zenn&logoColor=white" /></a>

## Zenn latest posts

<!-- BLOG-POST-LIST:START -->
- [Next.js MiddlewareはNode.js runtimeでも使える！【誤解を解消＆今後の展望】](https://zenn.dev/jonghyo/articles/nextjs-middleware)
- [Zennの記事をGitHub連携でカッチリ管理するおすすめ設定](https://zenn.dev/jonghyo/articles/zenn-github-repo)
<!-- BLOG-POST-LIST:END -->

## Local Development

### Install dependencies

```bash
npm ci
```

### Add articles

```bash
npx zenn new:article --slug 記事のスラッグ --title タイトル --type tech --emoji ✨
```

- `--slag`: 記事のスラッグを指定する。`articles/[slug].md`でファイルが生成される。詳細は[コチラ](https://zenn.dev/zenn/articles/what-is-slug)
- `--title`: 記事のタイトル
- `--type`: 記事のタイプ。 `tech` | `idea`
- `--emoji`: アイキャッチ用の絵文字を1つ指定

### Add book

```bash
npx zenn new:book
```

### preview

```bash
npm start
```
