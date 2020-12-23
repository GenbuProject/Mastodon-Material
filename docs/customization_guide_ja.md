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

<img src="res/search-bar-hover.gif" alt="search-bar hover">
検索バーをホバー(マウスオーバー)時に浮き上がらせ、背景色をフォーカス時と同じ色にします。[`custom_config.scss`](../src/mastodon-material/custom_config.scss)に`$search-bar-hover: true;`と追記してください。

### 文字サイズの変更
トゥート本文やユーザー名の文字サイズを変更できます。`$status-font-size`の値でトゥート本文の文字サイズを、`$name-font-size`の値で表示名の文字サイズを変更できます。

### 背景画像の設定
`$bg-image`の値で背景画像を設定できます。値は相対パスやURLを取ることができ、`""`で囲います。

### 透明度の設定
透明度を設定できます。`$bar-transparency`でトップバーの、`$column-transparency`でカラムの透明度を変更できます。0から1までの間の値で設定します。1で不透明、0で透明です。

## アイコン設定

[`mastodon-material/icon_config.scss`](../src/mastodon-material/icon_config.scss)を編集します。

### お気に入りアイコンを変更する

`// Favorite icon settings`セクションを編集してください。デフォルトは★(星)です。

### アイコンフォントのホストを変更したい

デフォルトではGoogle Fontsを利用する設定になっています。`// Material Design Icon settings`セクションを編集します。

- **GitHub公式リポジトリのフォントを利用したい(非推奨)**
  
  `$icon-font-source`の値を`github`に設定します。

- **自分のサーバーでホスティングしたい**
  
  `$icon-font-source`の値を`self`に設定します。[フォントの公式レポジトリ](https://github.com/google/material-design-icons/tree/master/font)からダウンロードしてきたフォントファイルを`/mastodon/app/javascript/fonts`に配置します。

### アイコンフォントのスタイルを変更したい

アイコンフォントにはFilled, Outlined, Rounded, Two-Tone, Sharpの5種類のスタイルがあります。実際のスタイルは[Icons - Material Design](https://material.io/resources/icons/)から確認できます。`// Material Icon style settings`セクションを編集します。デフォルトはFilledです。

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
