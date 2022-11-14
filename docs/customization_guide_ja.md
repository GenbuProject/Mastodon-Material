# カスタマイズガイド

## 言語 | Language

[English (英語)](customization_guide.md)

## 配色とレイアウトのプリセット

配色を設定するカラープリセットとレイアウトプリセットの2種類があります。  
カラープリセットは[`mastodon-material/config/_config_color.scss`](../src/mastodon-material/config/_config_color.scss)の`@forward '../color/v1-light'`で参照するファイルを設定します。  
レイアウトプリセットは[`mastodon-material/config/_config_layout.scss`](../src/mastodon-material/config/_config_layout.scss)の`@forward '../layout/material-v1'`で参照するファイルを設定します。

## 基本設定

[`mastodon-material/config/_config_basic.scss`](../src/mastodon-material/config/_config_basic.scss)に記述します。ベースファイルである[`mastodon-material/theme/_config_basic.scss`](../src/mastodon-material/theme/_config_basic_.scss)を参考にしてください。ただし、ベースファイルは**編集しないでください**。

### 検索バーをホバー時に浮き上がらせる

<img src="res/search-bar-hover.gif" alt="search-bar hover">

`$search-bar-hover: true,`

検索バーをホバー(マウスオーバー)時に浮き上がらせ、背景色をフォーカス時と同じ色にします。`$search-bar-hover: true,`で有効化します。  
デフォルト: `$search-bar-hover: false,`

### 文字サイズの変更

トゥート本文やユーザー名の文字サイズを変更できます。`$status-font-size`の値でトゥート本文の文字サイズを、`$name-font-size`の値で表示名の文字サイズを変更できます。  
デフォルト:  
`$status-font-size: 15px,`  
`$name-font-size: 15px,`

### 背景画像の設定

`$bg-image`の値で背景画像を設定できます。値は相対パスやURLを`""`で囲って記述します。  
デフォルト: `$bg-image: none,`

### 透明度の設定

透明度を設定できます。`$bar-transparency`でトップバーの、`$column-transparency`でカラムの透明度を変更できます。0から1までの間の値で設定します。1で不透明、0で透明です。  
デフォルト:  
`$bar-transparency: 1,`  
`$column-transparency: 1,`

### アイコンフォントのホストを変更する

デフォルトではGoogle Fontsを利用する設定になっています。

- **GitHubリポジトリのフォントを利用する(非推奨)**
  
  `$icon-font-source: github,`を追記します。

- **自分のサーバーでホスティングする**
  
  `$icon-font-source: self,`を追記します。[フォントの公式レポジトリ](https://github.com/google/material-design-icons/tree/master/font)からダウンロードしてきたフォントファイルをMastodonの`app/javascript/fonts`に配置します。

### アイコンフォントのスタイルを変更したい

アイコンフォントにはFilled, Outlined, Rounded, Two-Tone, Sharpの5種類のスタイルがあります。実際のスタイルは[Material Symbols and Icons - Google Fonts](https://fonts.google.com/icons)から確認できます。`// Material Icon style settings`セクションを編集します。デフォルトはFilledです。

## プラグイン

[`mastodon-material/_index.scss`](../src/mastodon-material/_index.scss)で必要なものをアンコメントしてください。  
その他のリソースは[こちらのリポジトリ](https://github.com/GenbuProject/Mastodon-Material-Gallery)を確認してください。

### タイムラインの投稿をカード化する

タイムライン上の投稿をカード化します。(デフォルトはリスト型) リスト型と比べ、一覧性が低下します。

`@use 'plugins/cards';`をアンコメントしてください。

<details>
<summary>スクリーンショットの表示/非表示</summary>

![cards](res/cards.png)
</details>

### 一覧性を向上させる

このテーマのデフォルト設定では、マテリアルデザインガイドラインに厳密に準拠しているため、Mastodonデフォルトのテーマと比較して情報密度が低くなっています。ガイドラインに非準拠ですが「密モード(denseプラグイン)」を適用することによって、一覧性を向上させることができます。

`@use 'plugins/dense';`をアンコメントしてください。

<details>
<summary>スクリーンショットの表示/非表示</summary>

デフォルト
![before](res/mastodon-light.png)

密モード
![after](res/dense.png)
</details>

## マテリアルデザインアイコンフォントを利用しない(非推奨)

このテーマでは、マテリアルデザインアイコンの表示にGoogle製[Material Iconsフォント](https://fonts.google.com/icons)を採用しています。ライセンス上の問題がある場合などに、このテーマの他のUIに影響を及ぼすことなくデフォルトのアイコンフォントである[Font Awesome](https://fontawesome.com/)を利用できます。

[`mastodon-material/theme/icons.scss`](../src/mastodon-material/theme/icons.scss)を開き、`@use 'material-icons';`をコメントアウトしてください。
