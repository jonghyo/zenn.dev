---
title: 'Raycastのおすすめ設定:Alfred・Clipyユーザーに試してほしい!'
emoji: '🚀'
type: 'idea' # tech: 技術記事 / idea: アイデア
topics: ['raycast', 'mac', '効率化']
published: true
published_at: 2025-02-12 07:45
---

## はじめに

Findyさんのテックブログを見て

https://tech.findy.co.jp/entry/2025/02/10/070000#Raycast

ようやく[Raycast](https://www.raycast.com/)の導入に踏み切りました!

- ランチャーアプリ： [Alfred](https://www.alfredapp.com/)
- クリップボード履歴アプリ： [Clipy](https://clipy.softonic.jp/mac)

だった僕が、Raycastに乗り換えておすすめする設定やプラグインを紹介します。

※Freeプランの機能に絞って紹介しています...!

## TL;DR

- RaycastはAlfred・Clipyの代替として優秀！
- [おすすめの拡張機能](#extensions)を参考に、自分だけのランチャー環境を作ろう!
- Windows版も開発中

https://www.raycast.com/windows

## こんな人におすすめ！

- Alfred・Clipyを使っている人
- キーボード操作で作業効率を上げたい人
- マウスを使わずにWindow管理をしたい人
- 複数のアプリを素早く切り替えたい人
- ショートカットを活用して時短したい人

## Raycastとは

Raycastは、Raycast Technologies Ltd.によって開発された**macOS向けランチャーアプリ**です。2020年に登場しました。

- [Spotlight](https://support.apple.com/ja-jp/guide/mac-help/mchlp1008/mac)
- [Alfred](https://www.alfredapp.com/)

の代替となるツールで

- **高速な動作**
- **モダンなUI/UX**

を備えた、非常に使いやすいランチャーアプリです。

![Raycast](/images/raycast-settings/raycast.gif)

https://www.raycast.com/

### Raycastの主な機能

Raycastの主な機能として、以下があります。

- アプリケーション検索
- ファイル検索
- クリップボード履歴
- メモ
- 電卓
- システムコントロール(音量・明るさの変更、休止状態への移行など)
- 単語の定義
- Window管理
- スニペット管理
- カレンダーイベントへのアクセス
- ChatGPTへのクイックアクセス（有料機能）

標準機能としてもリッチですが、**Raycast Storeから拡張機能をインストール**できます。

https://www.raycast.com/store

また、拡張機能を自作可能です。

https://developers.raycast.com/basics/create-your-first-extension

### その他Raycastの特徴

設定できめ細かく挙動を定義できます。

**Web Proxy設定も可能**など、エンタープライズユースで欲しくなる機能も備えています🤩

## 🫡 事前準備

![Delete Alfred](/images/raycast-settings/delete-alfred.png)

Raycastインストール後、Alfred, ClipyのHotkeyが衝突しないように

今まで支えてくれたAlfred, Clipyとお別れします。

1. アクティビティモニタから、Alfred・Clipyを強制終了
2. `Finder > アプリケーション`から、Alfred・Clipyを「ゴミ箱にいれる」

さよなら、Alfred, Clipy。**今はただ君に感謝を🙏**

## 📌 Raycastのインストール

以下コマンドでRaycastをインストールします。

```bash
brew install --cask raycast
```

![Install Raycast](/images/raycast-settings/install-raycast.png)

## 🔧 Raycastのおすすめ設定

### General

Generalは、以下のように設定しました。

![General Setting](/images/raycast-settings/general.png)

- **Startup：有効**
  - Macログイン時に自動で、Raycastが有効になるように
- **Raycast Hotkey：`optionキー (⌥) + space`**
  - Alfred時代から、`optionキー (⌥) + space`派なのでデフォルトから変更なし
- **Menu Bar Icon：無効**
  - 個人的にランチャーアプリのアイコンはMenu Barに不要

その他はデフォルトから特に変えてません。

### Extensions

おすすめの拡張機能やその設定を紹介します。

拡張機能は**5段階の評価で個人的なおすすめ度**とともに紹介しています!

拡張機能のHotKeyは、それぞれ`Record Hotkey`をクリックすれば設定できます。

（最初どこから拡張機能のHotkey設定できるかわからず、一瞬悩んだのはここだけの秘密...）

![Record Hotkey](/images/raycast-settings/record-hotkey.png)

拡張機能は以下からインストールできるので、自分に合う拡張機能を探してみてください!

https://www.raycast.com/store

#### Clipboard History（おすすめ度： ★★★★★）

クリップボート履歴を管理する機能です。

Raycastにデフォルトで入ってます。

この機能で、[Clipy](https://clipy.softonic.jp/mac)を代替可能です。

![Clipbord History](/images/raycast-settings/clipboard-history.png)

- **Hotkey： `commandキー (⌘) commandキー (⌘)`**
  - Clipy時代からcommandキー (⌘)を2回押すと、クリップボート履歴を開くようにしてたので、Hotkeyはこれで
- **Primary Action： Copy to Clipboard**
  - 個人的に、履歴を選択してペーストまでされるのは嫌なのでこの設定に変更

その他はデフォルト設定です。

#### Color Picker（おすすめ度： ★★★★★）

カラーピッカーツールの拡張機能です。

![Color Picker](/images/raycast-settings/color-picker.gif)

https://www.raycast.com/thomas/color-picker

- **Hotkey： `commandキー (⌘) + P`**

Webサイトなどを見ていて、「この色いいな、カラーコードなんだろ？」と思ったときすぐに起動できて非常に便利です!

#### Deep Cast（おすすめ度： ★★★★☆）

[DeepL翻訳](https://www.deepl.com/ja/translator)で選択した文字列を翻訳できます。

DeepL APIのAPIキーが必要になります。

DeepL APIは無料枠でも利用でき、**無料枠上限に到達した場合そこから自動課金されることはありません。**

https://www.raycast.com/mooxl/deepcast

![DeepCast Result](/images/raycast-settings/deepcast-result.png)

以下のような設定にしています。

![DeepCast](/images/raycast-settings/deepcast.png)

- **普段翻訳対象にしない言語はOff**
  - デフォルトだと、さまざまな言語の翻訳メニューが全部表示されて煩わしいので必須設定!
- **On Translation Actions： View in Raycast**
  - DeepLのサイトに飛ばなくても翻訳結果を見たいのでこちらで。

[DeepL Chrome Extensions](https://www.deepl.com/ja/chrome-extension)もありますが、**ブラウザ以外のドキュメントも簡単に訳せるので便利です!**

#### GitHub（おすすめ度： ★★★☆☆）

GitHubと連携するための拡張機能です。

https://www.raycast.com/raycast/github

![GitHub](/images/raycast-settings/github.png)

- **Clone Path：適宜、自分のワークディレクトリを設定**

#### Google Search（おすすめ度： ★★★☆☆）

Google検索をRaycast上で行なう拡張機能です。

https://www.raycast.com/mblode/google-search

![Google Search](/images/raycast-settings/google-search.png)

デフォルト設定のまま使っています。

#### Image Modification（おすすめ度： ★★★★★）

画像の編集ができる拡張機能です。

拡張子の変更、リサイズなどが可能。`webp`を`jpeg`に変換したりで重宝してます。

https://www.raycast.com/HelloImSteven/sips

![Image Modification](/images/raycast-settings/image-modification.png)

- **Image Input：Selection In Finder**
- **Image Output： Copy to Clipboard**
  - ダウンロードした`webp`などの拡張子を変更して、すぐペーストするケースが多いので
- **Image Operation Settings**
  - Lossless Conversions：有効

この設定にしています。

#### Installed Extensions（おすすめ度： ★★★★☆）

インストール済みの拡張機能を一覧表示し、管理できる機能です。

拡張機能が増えてくると、何をインストールしてたかわかりづらくなるのでおすすめ!

![Install Extensions](/images/raycast-settings/install-extensions.png)

https://www.raycast.com/pernielsentikaer/installed-extensions

#### ray.so（おすすめ度： ★★★★☆）

Xなどでよく見かけるおしゃれなコードスニペット画像を生成できます。

![ray.so](/images/raycast-settings/ray-so-export.png)

https://www.raycast.com/garrett/ray-so

#### Search Emoji & Symbols（おすすめ度： ★★★★☆）

EmojiをRaycast上で検索できる拡張機能です。

Raycastにデフォルトで入ってます。

よくEmojiを[Let's EMOJI](https://lets-emoji.com/)で検索してたので、この機能は重宝しています。

![Search Emoji](/images/raycast-settings/search-emoji.png)

#### Visual Studio Code（おすすめ度： ★★★☆☆）

VSCodeのプロジェクトを開いたり、管理ができる拡張機能です。

https://www.raycast.com/thomas/visual-studio-code

### Advanced

Web Proxy環境下で作業することもあるので **「Web Proxy設定」を有効** にしています。

この設定があるの素晴らしい😎

![Advanced Setting](/images/raycast-settings/advanced.png)

## Export機能

設定や拡張機能のExport機能があるので、端末の移行なども簡単にできます。

![Search Emoji](/images/raycast-settings/export.png)

Raycast Proプランなら、Cloud Syncで設定が同期されますがFreeプランでも設定の移行ができて安心!

## まとめ

Alfred, ClipyからRaycastに乗り換えることでかなり快適になりました。

さまざまな拡張機能があり、**かゆいところに手が届くとても体験の良いランチャー**です!

おすすめなので、ぜひ試してみてください!

Windows版も開発中のようなので、興味ある方はWaiting listに登録を!

https://www.raycast.com/windows

---

最後までお読みいただきありがとうございました!

Xでも技術ネタをポストしているので、よかったらフォローしてもらえると嬉しいです😋

→ [X@jonghyo\_](https://x.com/jonghyo_)
