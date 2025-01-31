---
title: 'Next.jsのMiddlewareはNode.js runtimeでも使えます!'
emoji: '🙆'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['nextjs', 'typescript', 'javascript', 'nodejs']
published: true
published_at: 2025-02-03 07:00
---

## はじめに結論から

たまにEdge runtimeでしか使えないと誤解されている方がいるので...

**Next.jsのMiddlewareはNode.js runtimeでも使えます!**

:::message alert

ただし、Node.js runtimeのフルサポートではなく、`fs`, `net`, `child_process`, `crypto`などの特定のNode APIは使えません。

:::

## Next.js Middlewareとは？

Next.jsアプリへのリクエストが完了する前にコードを実行できる仕組みです。

送られてきたリクエストに基づいて、

- レスポンスの書き換え
- リダイレクト
- リクエストやレスポンスのヘッダーを変更

などの処理を加えることができるようになります。

https://nextjs.org/docs/app/building-your-application/routing/middleware

大まかなイメージですが、以下図のようにリクエストを受けてから実際のアプリの処理に入るまでに任意の処理を挟むことができるようになります。

この図では、アクセスログの出力と認証状態の楽観チェックを行なう例です。

![Middleware overview](/images/nextjs-middleware/middleware-overview.png)

:::message

**Next.jsのmiddlewareでは、認証状態の楽観チェックのみがベター**

パフォーマンスの観点から認証状態のチェックはCookieからセッションを読み取る楽観的なチェックのみに行なうことが重要とされています。

https://nextjs.org/docs/app/building-your-application/authentication#optimistic-checks-with-middleware-optional

:::

## Edge runtimeサポート

> Middleware currently only supports APIs compatible with the Edge runtime. APIs exclusive to Node.js are unsupported.

Middlewareは便利な仕組みですが、公式ドキュメントにこのように記載されておりNode.js runtimeでは使えない?? と勘違いしてしまうこともありそうです。

https://nextjs.org/docs/app/building-your-application/routing/middleware#runtime

ただ、公式ドキュメントの[Runtime](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes)を見ると

> - The Node.js Runtime (default), which has access to all Node.js APIs and compatible packages from the ecosystem.
> - The Edge Runtime which contains a more limited [set of APIs](https://nextjs.org/docs/app/api-reference/edge).

と記載があり、**Middleware上ではNode.js runtimeが提供するAPIは使えない**ということです。※Node.js runtime上でも動作はする

Middlewareのページにも

> APIs exclusive to Node.js are unsupported.

と書いており、別にNode.js runtimeで動作しないとは言ってないんですよね.. 😇

※ただ、英語弱いのでパッと見動作しないように受け取っちゃった..

### Node.js runtimeが提供するAPIを使えないとは？

Node.js runtimeが提供するAPIを使えないというのはどういうことでしょうか🤔

Node.js runtimeが提供するAPIは代表的なもので、`fs`や`http`があります。

Node.js runtimeが提供するAPIの詳細はこちら。

https://nodejs.org/api/

Node.js runtimeが提供するAPIを使えないということは

**Middlewareの処理でアクセスログを`fs`を使ってファイルに書き出す**

といったことはできません。

また

**Middlewareの処理でアクセスログをLoggerライブラリ[Pino](https://github.com/pinojs/pino)を使って出力する際はbrowser周りの設定を記載する必要があります。**

詳細：[【Next.js】全てのログをターミナルに出力する方法](https://qiita.com/P-man_Brown/items/0f0e0613fd9bb3e8c99c)

## MiddlewareのNode.js runtimeの対応状況

MiddlewareのNode.js runtime対応の要望は多く、以下ディスカッションではNode.js runtimeのフルサポートをするために開発を進める旨が明言されています。

> We are working on allowing using the Node.js runtime for Middleware, and it will be included in an upcoming Next.js release. We appreciate your feedback and patience on this issue. We are also exploring a potential new API, called Request Interceptors, that would address some of the feedback here for supporting nested, file-system based middleware. If this is something you are interested in, please comment on the proposal. To prevent additional GitHub notifications and emails, we have a new discussion where you can opt-into notifications as progress is made. Thank you 🙏

https://github.com/vercel/next.js/discussions/46722#discussioncomment-11030344

Middlewareの制約や問題を解消するために、`Interceptors`という仕組みの開発も進められています。※やや停滞気味..MiddlewareのNode.js runtime対応のほうが優先度高い？

https://github.com/vercel/next.js/pull/70961

最新のディスカッションは以下で行なわれています。

https://github.com/vercel/next.js/discussions/71727

## まとめ

- Next.jsのMiddlewareはEdge runtime, Node.js runtimeのどちらでも使えます
  - ただし、Node.js runtimeのフルサポートではなく、`fs`, `net`, `child_process`, `crypto`などの特定のNode APIは使えません
- MiddlewareのNode.js runtimeのフルサポートに向けて開発が進められています
  - 早く対応してほしい...!

## 参考リンク

https://zenn.dev/azoookid/scraps/5c05e850e40849

---

最後までお読みいただきありがとうございました!

Xでも技術ネタをポストしているので、よかったらフォローしてもらえると嬉しいです😋

→ [X@jonghyo\_](https://x.com/jonghyo_)
