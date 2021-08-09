# カスタマイズガイド

## 言語 | Language
[English (英語)](customization_guide.md)

## 注意事項
ここに書かれているカスタマイズは、すべてソースコードに手を加えることによって行うものです。よって、ソースコードレベルの編集権限のあるユーザーのみがこのカスタマイズを行うことができます。

## プロファイル
プラグインや設定などのカスタマイズはプロファイルとして格納されており、それを切り替えることで複数のテーマとして利用することができます。デフォルトのプロファイルは[`default`](../src/mastodon-material/profiles/default)です。

### プロファイルの新規作成
1. [`default`](../src/mastodon-material/profiles/default)フォルダを複製し、フォルダ名を変更します。この文書では変更後のフォルダ名を`{プロファイル名}`と表記します。
2. [`mastodon-material.scss`](../src/mastodon-material.scss)を複製し、ファイル名を`{プロファイル名}`に変更します。テーマのインストール時と同様、`{プロファイル名}.scss`がMastodonの`app/javascript/styles`に配置されていることを確認します。
3. `{プロファイル名}.scss`を開き、`@import 'mastodon-material/profiles/default/loader';`の行を`@import 'mastodon-material/profiles/{プロファイル名}/loader';`に変更します。
4. これ以降は[README_ja.md](../README_ja.md#導入手順)も参照しながら作業してください。Mastodonの`config/themes.yml`に、以下を参考に追記します。
  ```yml
  default: styles/application.scss
  contrast: styles/contrast.scss
  mastodon-light: styles/mastodon-light.scss
  
  # 追加テーマ
  mastodon-material: styles/mastodon-material.scss
  {プロファイル名}: styles/{プロファイル名}.scss # この1行を追記
  ```
5. テーマの表示名を変更したい場合には、Mastodonの`config/locales/{lang}.yml`に以下を参考に追記してください。(`config/locales/en.yml`は**必須**)
  ```yml
  themes:
   contrast: High contrast
   default: Mastodon
   mastodon-light: Mastodon (light)
  
  # 追加テーマ
   mastodon-material: Mastodon Material
   {プロファイル名}: {任意の表示名} # この一行を追記
  ```

## 基本設定
`mastodon-material/profiles/{プロファイル名}/config.scss`に記述します。ベースファイルである[`mastodon-material/theme/base_config.scss`](../src/mastodon-material/theme/base_config.scss)を参考にしてください。ただし、[`base_config.scss`](../src/mastodon-material/theme/base_config.scss)は**編集しないでください**。

### 配色とレイアウトのプリセット
配色を変更するカラープリセットと、レイアウトなどを変更するレイアウトプリセットの2種類があります。デフォルトのカラープリセットは`v1-light`、レイアウトプリセットは`material-v1`が適用されます。`@import`を使用して変更します。

### 検索バーをホバー時に浮き上がらせる
<img src="res/search-bar-hover.gif" alt="search-bar hover">

検索バーをホバー(マウスオーバー)時に浮き上がらせ、背景色をフォーカス時と同じ色にします。`$search-bar-hover: true;`と追記してください。

### 文字サイズの変更
トゥート本文やユーザー名の文字サイズを変更できます。`$status-font-size`の値でトゥート本文の文字サイズを、`$name-font-size`の値で表示名の文字サイズを変更できます。

### 背景画像の設定
`$bg-image`の値で背景画像を設定できます。値は相対パスやURLを取ることができ、`""`で囲います。

### 透明度の設定
透明度を設定できます。`$bar-transparency`でトップバーの、`$column-transparency`でカラムの透明度を変更できます。0から1までの間の値で設定します。1で不透明、0で透明です。

## アイコン設定
`mastodon-material/profiles/{プロファイル名}/icon_config.scss`に記述します。ベースファイルである[`mastodon-material/theme/base_icon_config.scss`](../src/mastodon-material/theme/base_icon_config.scss)を参考にしてください。ただし、[`base_icon_config.scss`](../src/mastodon-material/theme/base_icon_config.scss)は**編集しないでください**。

### アイコンフォントのホストを変更したい
デフォルトではGoogle Fontsを利用する設定になっています。

- **GitHub公式リポジトリのフォントを利用したい(非推奨)**
  `$icon-font-source: github;`を追記します。

- **自分のサーバーでホスティングしたい**
  `$icon-font-source: self;`を追記します。[フォントの公式レポジトリ](https://github.com/google/material-design-icons/tree/master/font)からダウンロードしてきたフォントファイルをMastodonの`app/javascript/fonts`に配置します。

### アイコンフォントのスタイルを変更したい
アイコンフォントにはFilled, Outlined, Rounded, Two-Tone, Sharpの5種類のスタイルがあります。実際のスタイルは[Icons - Material Design](https://material.io/resources/icons/)から確認できます。`// Material Icon style settings`セクションを編集します。デフォルトはFilledです。

## プラグイン
`mastodon-material/profiles/{プロファイル名}/loader.scss`で必要なものをアンコメントしてください。  
その他のリソースは[こちらのリポジトリ](https://github.com/GenbuProject/Mastodon-Material-Gallery)を参照してください。

### タイムラインの投稿をカード化する
タイムライン上の投稿をカード化します。(デフォルトはリスト型) リスト型と比べ、一覧性が低下します。

`@import 'plugins/cards';`をアンコメントしてください。

### 一覧性を向上させる
このテーマのデフォルト設定では、マテリアルデザインガイドラインに完全準拠しているため、Mastodonデフォルトのテーマと比較して情報密度が低くなっています。ガイドライン非準拠ですが「密モード(denseプラグイン)」を適用することによって、一覧性を向上させることができます。

`@import 'plugins/dense';`をアンコメントしてください。

## (参考)マテリアルデザインアイコンフォントを利用しない
このテーマでは、マテリアルデザインアイコンの表示にGoogle製[Material Iconsフォント](https://google.github.io/material-design-icons/#icon-font-for-the-web)を採用しています。ライセンス上の問題がある場合などに、このテーマの他のUIに影響を及ぼすことなくデフォルトのアイコンフォントである[Font Awesome](https://fontawesome.com/)を利用できます。

`mastodon-material/profiles/{プロファイル名}/loader.scss`を開き、`@import 'theme/material-icons';`をコメントアウトしてください。
