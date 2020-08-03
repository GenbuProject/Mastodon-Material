# カスタマイズガイド

## 言語 | Language

[English (英語)](customization_guide.md)

## 注意事項

ここに書かれているカスタマイズは、すべてソースコードに手を加えることによって行うものです。よって、ソースコードレベルの編集権限のあるユーザーのみがこのカスタマイズを行うことができます。

## 基本設定

[`mastodon-material/custom_config.scss`](../src/mastodon-material/custom_config.scss)に記述します。ベースファイルである[`mastodon-material/theme/base_config.scss`](../src/mastodon-material/theme/base_config.scss)を参考にしてください。ただし、[`base_config.scss`](../src/mastodon-material/theme/base_config.scss)は**編集しないでください**。

### プロファイルの切り替え

プロファイルには、配色を変更する「カラースキーム」と、レイアウトなどを変更する「レイアウトプロファイル」の2種類があります。それぞれ`@import`で変更できます。デフォルトではMaterial v1のライトテーマが適用されます。

### 検索バーをホバー時に浮き上がらせる

検索バーをホバー(マウスオーバー)時に浮き上がらせ、背景色をフォーカス時と同じ色にします。[`custom_config.scss`](../src/mastodon-material/custom_config.scss)に`$search-bar-hover: true;`と追記してください。

## アイコン設定

[`mastodon-material/icon_config.scss`](../src/mastodon-material/icon_config.scss)を編集します。

### お気に入りアイコンを変更する

`// Favorite icon settings`セクションを編集してください。デフォルトは★(星)です。

### アイコンフォントのホストを変更したい

デフォルトではGitHub上の[公式リポジトリ](https://github.com/google/material-design-icons)にあるフォントを利用する設定になっています。`// Material Design Icon settings`セクションを編集します。

- **Google Fontsのフォントを利用したい**
  
  `// Google Fonts`の行をアンコメントし、`// GitHub`の行をコメントアウトします。

- **自分のサーバーでホスティングしたい**
  
  1.と同様に`// Self-hosting`の行をアンコメントし、`// GitHub`の行をコメントアウトします。[フォントの公式レポジトリ](https://raw.githubusercontent.com/google/material-design-icons/master/iconfont/MaterialIcons-Regular.woff2)からダウンロードしてきた`MaterialIcons-Regular.woff2`ファイルを`/mastodon/app/javascript/fonts`に配置します。

## プラグイン

[`mastodon-material/loader.scss`](../src/mastodon-material/loader.scss)で必要なものをアンコメントしてください。

### タイムラインの投稿をカード化する

タイムライン上の投稿をカード化します。(デフォルトはリスト型) リスト型と比べ、一覧性が低下します。

`@import 'plugins/cards';`をアンコメントしてください。

### 一覧性を向上させる

このテーマのデフォルト設定では、マテリアルデザインガイドラインに完全準拠しているため、Mastodonデフォルトのテーマと比較して情報密度が低くなっています。ガイドライン非準拠ですが「密モード(denseプラグイン)」を適用することによって、一覧性を向上させることができます。

`@import 'plugins/dense';`をアンコメントしてください。

### (参考)マテリアルデザインアイコンフォントを利用しない

このテーマでは、マテリアルデザインアイコンの表示にGoogle製[Material Iconsフォント](https://google.github.io/material-design-icons/#icon-font-for-the-web)を採用しています。ライセンス上の問題がある場合などに、このテーマの他のUIに影響を及ぼすことなくデフォルトのアイコンフォントである[Font Awesome](https://fontawesome.com/)を利用できます。

`@import 'theme/material-icons';`をコメントアウトしてください。
