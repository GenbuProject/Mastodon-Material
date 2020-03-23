# カスタマイズガイド

## 言語 | Language

[English (英語)](customization_guide.md)

## 注意事項

ここに書かれているカスタマイズは、すべてソースコードに手を加えることによって行うものです。よって、ソースコードレベルの編集権限のあるサーバー管理者のみがこのカスタマイズを行うことができます。

## 基本設定

[`mastodon-material/config.scss`](../src/mastodon-material/config.scss)で設定できるもの

### プロファイルの切り替え

プロファイルには、配色を変更する「カラースキーム」と、レイアウトなどを変更する「レイアウトプロファイル」の2種類があります。それぞれ該当項目の`@import`で指定するファイルを変更することによって変更できます。

### マテリアルデザインアイコンの設定

このテーマでは、マテリアルデザインアイコンの表示にGoogle製[Material Iconsフォント](https://google.github.io/material-design-icons/#icon-font-for-the-web)を採用しています。

#### お気に入りアイコンを変更する

`// Favorite icon settings`セクション内の該当箇所4か所をお好みのものに変更してください。デフォルトは★(星)です。

#### アイコンフォントのホストを変更したい

デフォルトではGitHub上の[公式リポジトリ](https://github.com/google/material-design-icons)にあるフォントを利用する設定になっています。

##### 1. Google Fontsのフォントを利用したい

`// Material Design Icon settings`セクション内の`// Google Fonts`の行をアンコメントし、`// GitHub`の行をコメントアウトします。

##### 2. 自分のサーバーでホスティングしたい

1.と同様に`// Self-hosting`の行をアンコメントし、`// GitHub`の行をコメントアウトします。[フォントの公式レポジトリ](https://raw.githubusercontent.com/google/material-design-icons/master/iconfont/MaterialIcons-Regular.woff2)からダウンロードしてきた`MaterialIcons-Regular.woff2`ファイルを`/mastodon/app/javascript/fonts`に配置します。

#### マテリアルデザインアイコンフォントを利用しない

ライセンス上の問題がある場合などに、このテーマの他のUIに影響を及ぼすことなくデフォルトのアイコンフォントである[Font Awesome](https://fontawesome.com/)を利用できます。

[`config.scss`](../src/mastodon-material/config.scss)内の`// Material Design Icon settings`と`// Favorite icon settings`のセクション内**全て**をコメントアウトします。次に、一つ上の階層にある[`mastodon-material.scss`](../src/mastodon-material.scss)内の`@import 'mastodon-material/material-icons';`の行をコメントアウトします。

### 検索バーをホバー時に浮き上がらせる

検索バーをホバー(マウスオーバー)時に浮き上がらせ、背景色をフォーカス時と同じ色にします。`// Search bar hover settings`セクションをアンコメントしてください。

## タイムラインの投稿をカード化する

タイムライン上の投稿をカード化します。(デフォルトはリスト型) リスト型と比べ、一度に表示される情報量が減少します。

[`mastodon-material.scss`](../src/mastodon-material.scss)内の`@import 'mastodon-material/cards';`をアンコメントしてください。
