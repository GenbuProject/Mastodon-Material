# Mastodon Material

![Mastodon Material](docs/res/top.png)

## 言語 | Language

[English (英語)](README.md)

## 概要

Mastodon Materialは、[Material Design](https://material.io)準拠のMastodon向けネイティブテーマです。開発方針は[こちら](docs/development_policy_ja.md)

## スクリーンショット
<details>
<summary>表示/非表示</summary>

v1-light + material-v1
![v1-light](docs/res/v1-light.png)

v1-dark + material-v1
![v1-dark](docs/res/v1-dark.png)

black + material-v1
![black](docs/res/black.png)

v2-light + material-v2
![v2-light](docs/res/v2-light.png)

v2-dark + material-v2
![v2-dark](docs/res/v2-dark.png)

mastodon-light + material-v1
![mastodon-light](docs/res/mastodon-light.png)

mastodon-dark + material-v1
![mastodon-dark](docs/res/mastodon-dark.png)
</details>

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

4. Google Fonts(初期設定)やGitHubのWebfontを利用する設定にしている場合は、*CSP(Content Security Policy)* に例外を追加する必要があります。Mastodonの`config/initializers/content_security_policy.rb`を以下のように変更してください。
   
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

   Diff
   
   ```diff
   def host_to_url(str)
    "http#{Rails.configuration.x.use_https ? 's' : ''}://#{str}" unless str.blank?
   end
   base_host = Rails.configuration.x.web_domain
   assets_host = Rails.configuration.action_controller.asset_host
   assets_host ||= host_to_url(base_host)
   media_host = host_to_url(ENV['S3_ALIAS_HOST'])
   media_host ||= host_to_url(ENV['S3_CLOUDFRONT_HOST'])
   media_host ||= host_to_url(ENV['S3_HOSTNAME']) if ENV['S3_ENABLED'] == 'true'
   media_host ||= assets_host

   + # custom host
   + github_host = "https://raw.githubusercontent.com" # GitHub
   + google_fonts_host = "https://fonts.gstatic.com" # Google Fonts

   Rails.application.config.content_security_policy do |p|
    p.base_uri :none
    p.default_src :none
    p.frame_ancestors :none
   - p.font_src :self, assets_host
   + p.font_src :self, assets_host, github_host, google_fonts_host
    p.img_src :self, :https, :data, :blob, assets_host
    p.style_src :self, :unsafe_inline, assets_host
    p.media_src :self, :https, :data, assets_host
    p.frame_src :self, :https
    p.manifest_src :self, assets_host
   ```

## カスタマイズ

テーマのカスタマイズについては、[カスタマイズガイド](docs/customization_guide_ja.md)をご覧ください。

## Stylus/Stylishテーマ

任意のサーバーでもこのテーマを利用できるよう、ブラウザ拡張機能の[Stylus](https://add0n.com/stylus.html)や[Stylish](https://userstyles.org/)向けのテーマを公開しています。

- **自分でビルドしたテーマを利用する**
  
  1. [Sass](https://sass-lang.com)を導入します。バージョンは[動作環境](#動作環境)を参照してください。
  2. このリポジトリをクローンまたはダウンロードしてください。
  3. カスタマイズする場合、[カスタマイズガイド](docs/customization_guide_ja.md)を参照してください。
  4. [build.bat (Windows)](build/build.bat)または[build.sh (macOS/Linux)](build/build.sh)を実行してください。結果が[build.css](build/build.css)に出力されます。
  5. StylusまたはStylishでテーマを新規作成し、[build.css](build/build.css)の内容をコピペします。利用しているサーバーのドメインを追加して、テーマを保存/有効化してください。

## ライセンス

このテーマ及びStylish/Stylus版テーマは[AGPL-3.0](LICENSE)に基づいて公開されています。また、Google製[Material Iconsフォント](https://google.github.io/material-design-icons/#icon-font-for-the-web)については、[Apache license version 2.0](https://www.apache.org/licenses/LICENSE-2.0.html)で提供されています。(このリポジトリにMaterial Iconsフォントファイルは含まれていません)

[ヘッダー画像](docs/src/top.png)は[Noto Sans](https://www.google.com/get/noto/#sans-lgc)と[mastodon.privacyfilter.user.styl](https://github.com/eai04191/userscript-graveyard#mastodonprivacyfilteruserstyl)を使用して作りました。
