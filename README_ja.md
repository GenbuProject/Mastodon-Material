# Mastodon Material

## 言語 | Language

[English (英語)](README.md)

## 概要

Mastodon Materialは、[Material Design](https://material.io)準拠のMastodon向けネイティブテーマです。開発方針は[こちら](docs/development_policy_ja.md)

> お知らせ: このテーマは現在ベータ版です。様々な要素が不安定または未実装の場合があります。

## 動作環境

- [Mastodon](https://github.com/tootsuite/mastodon) v3.1以上

- [Sass](https://sass-lang.com) 1.25.x

## 導入手順

1. 本リポジトリの以下のファイルを、Mastodonの`app/javascript/styles`へコピーしてください。
   
   * `/src/mastodon-material/`
   
   * `/src/mastodon-material.scss`

2. Mastodonの`config/themes.yml`に、以下のコードを追記してください。
   
   ```yml
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # 追加テーマ
   mastodon-material: styles/mastodon-material.scss # この一行を追記
   ```

3. テーマの表示名を変更したい場合には、Mastodonの`config/locales/{lang}.yml`に以下を参考に追記してください。(`config/locales/en.yml`は**必須**)
   
   ```yml
   themes:
    contrast: High contrast
    default: Mastodon
    mastodon-light: Mastodon (light)
   
    # 追加テーマ
    mastodon-material: Mastodon Material # この一行を追記
   ```

4. GitHubのWebfontを使用する設定(初期設定)やGoogle FontsのWebfontを利用する設定にしている場合は、*CSP(Content Security Policy)* に例外を追加する必要があります。Mastodonの`config/initializers/content_security_policy.rb`を以下のように変更してください。
   
   変更前 …
   
   ```ruby
   def host_to_url(str)
     "http#{Rails.configuration.x.use_https ? 's' : ''}://#{str}" unless str.blank?
   end
   
   base_host = Rails.configuration.x.web_domain
   
   assets_host   = Rails.configuration.action_controller.asset_host
   assets_host ||= host_to_url(base_host)
   
   media_host   = host_to_url(ENV['S3_ALIAS_HOST'])
   media_host ||= host_to_url(ENV['S3_CLOUDFRONT_HOST'])
   media_host ||= host_to_url(ENV['S3_HOSTNAME']) if ENV['S3_ENABLED'] == 'true'
   media_host ||= assets_host
   
   Rails.application.config.content_security_policy do |p|
     p.base_uri        :none
     p.default_src     :none
     p.frame_ancestors :none
     p.font_src        :self, assets_host
     p.img_src         :self, :https, :data, :blob, assets_host
     p.style_src       :self, :unsafe_inline, assets_host
     p.media_src       :self, :https, :data, assets_host
     p.frame_src       :self, :https
     p.manifest_src    :self, assets_host
   ```
   
   変更後…
   
   ```ruby
   def host_to_url(str)
     "http#{Rails.configuration.x.use_https ? 's' : ''}://#{str}" unless str.blank?
   end
   
   base_host = Rails.configuration.x.web_domain
   
   assets_host   = Rails.configuration.action_controller.asset_host
   assets_host ||= host_to_url(base_host)
   
   media_host   = host_to_url(ENV['S3_ALIAS_HOST'])
   media_host ||= host_to_url(ENV['S3_CLOUDFRONT_HOST'])
   media_host ||= host_to_url(ENV['S3_HOSTNAME']) if ENV['S3_ENABLED'] == 'true'
   media_host ||= assets_host
   
   # custom host
   github_host = "https://raw.githubusercontent.com" # GitHub
   google_fonts_host = "https://fonts.gstatic.com" # Google Fonts
   
   Rails.application.config.content_security_policy do |p|
     p.base_uri        :none
     p.default_src     :none
     p.frame_ancestors :none
     p.font_src        :self, assets_host, github_host, google_fonts_host
     p.img_src         :self, :https, :data, :blob, assets_host
     p.style_src       :self, :unsafe_inline, assets_host
     p.media_src       :self, :https, :data, assets_host
     p.frame_src       :self, :https
     p.manifest_src    :self, assets_host
   ```

## カスタマイズ

テーマのカスタマイズについては、[カスタマイズガイド](docs/customization_guide_ja.md)をご覧ください。

## Stylish/Stylus版テーマ

任意のサーバーでもこのテーマを利用できるよう、ブラウザ拡張機能の[Stylish](https://userstyles.org/)や[Stylus](https://add0n.com/stylus.html)向けのテーマを公開しています。

## ライセンス

このテーマ及びStylish/Stylus版テーマは[AGPL-3.0](LICENSE)に基づいて公開されています。また、Google製[Material Iconsフォント](https://google.github.io/material-design-icons/#icon-font-for-the-web)については、[Apache license version 2.0](https://www.apache.org/licenses/LICENSE-2.0.html)で提供されています。(このリポジトリにMaterial Iconsフォントファイルは含まれていません)

## スクリーンショット

<img src="docs/res/timeline/v1-light.png" alt="material-v1-light" width=50%>
<img src="docs/res/timeline/v2-dark.png" alt="material-v2-dark" width=50%>
