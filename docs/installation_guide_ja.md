# 導入ガイド

## 目次

> - [必要なもの](installation_guide_ja.md#必要なもの)
> - [Gitのサブモジュールを使用して導入する](installation_guide_ja.md#gitのサブモジュールを使用して導入する)
>   - [複数のMastodon Materialを導入する(サブモジュール)](installation_guide_ja.md#複数のMastodon-Materialを導入する(サブモジュール))
>   - [Mastodon Materialのアップデート(サブモジュール)](installation_guide_ja.md#Mastodon-Materialのアップデート(サブモジュール))
> - [ファイルをコピーして導入する](installation_guide_ja.md#ファイルをコピーして導入する)
>   - [複数のMastodon Materialを導入する(ファイルコピー)](installation_guide_ja.md#複数のMastodon-Materialを導入する(ファイルコピー))
>   - [Mastodon Materialのアップデート(ファイルコピー)](installation_guide_ja.md#Mastodon-Materialのアップデート(ファイルコピー))

## 必要なもの

- ソースコードを編集する権限
- Gitの知識(基礎、サブモジュール)

## Gitのサブモジュールを使用して導入する

この方法ではアップデートが容易になる、再利用が簡単になるなどのメリットがあります。  
複数の設定でMastodon Materialを導入する場合、[サブモジュールを使用して複数のテーマをインストールする](installation_guide_ja.md#サブモジュールを使用して複数のテーマをインストールする)を参照してください。

1. Mastodonのリポジトリのホームディレクトリで、下記のコードを実行してサブモジュールを追加します。
   
   ```
   git submodule add -b main https://github.com/GenbuProject/Mastodon-Material app/javascript/styles/mastodon-material
   ```

2. Mastodonの`config/themes.yml`に、以下を参考に追記します。
   
   ```yml
   …
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # 追加テーマ
   mastodon-material: styles/mastodon-material/src/mastodon-material.scss # この行を追記
   …
   ```

3. テーマの表示名を変更したい場合には、Mastodonの`config/locales/{言語}.yml`に以下を参考に追記してください。(`config/locales/en.yml`は**必須**)

   ```yml
   …
   themes:
    contrast: High contrast
    default: Mastodon
    mastodon-light: Mastodon (light)
   
   # 追加テーマ
    mastodon-material: 表示名 # この一行を追記
   …
   ```

4. Material IconsのソースとしてGoogle Fonts(初期設定)やGitHubのWebfontを利用する場合は、*CSP(Content Security Policy)* に例外を追加する必要があります。Mastodonの`config/initializers/content_security_policy.rb`を以下のように編集してください。
   
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
   + google_fonts_host = "https://fonts.gstatic.com" # Google Fonts
   + github_fonts_host = "https://raw.githubusercontent.com" # GitHub

   Rails.application.config.content_security_policy do |p|
    p.base_uri :none
    p.default_src :none
    p.frame_ancestors :none
   - p.font_src :self, assets_host
   + p.font_src :self, assets_host, github_fonts_host, google_fonts_host
    p.img_src :self, :https, :data, :blob, assets_host
    p.style_src :self, :unsafe_inline, assets_host
    p.media_src :self, :https, :data, assets_host
    p.frame_src :self, :https
    p.manifest_src :self, assets_host
   ```
   
   <details>
   <summary>変更前</summary>

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

   </details>

   <details>
   <summary>変更後</summary>

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
   github_fonts_host = "https://raw.githubusercontent.com" # GitHub
   google_fonts_host = "https://fonts.gstatic.com" # Google Fonts
   
   Rails.application.config.content_security_policy do |p|
     p.base_uri        :none
     p.default_src     :none
     p.frame_ancestors :none
     p.font_src        :self, assets_host, github_fonts_host, google_fonts_host
     p.img_src         :self, :https, :data, :blob, assets_host
     p.style_src       :self, :unsafe_inline, assets_host
     p.media_src       :self, :https, :data, assets_host
     p.frame_src       :self, :https
     p.manifest_src    :self, assets_host
   ```

   </details>

> Mastodonを異なる環境で実行する場合や、Mastodon Materialをアップデートする際には`git`コマンドで`--recursive --remote`オプションを付けてsubmoduleのソースコードも取得するようにしてください。

### 複数のMastodon Materialを導入する(サブモジュール)

1. このリポジトリをフォークして、`main`ブランチからチェックアウトします。以下ではこのブランチの名前を`{プロファイル名}`と表記します。

2. Mastodonのリポジトリのホームディレクトリで、以下のコードを実行してサブモジュールを追加します。URLは各自がフォークしたリポジトリに合わせて読み替えてください。
   
   ```
   git submodule add -b {プロファイル名} https://github.com/GenbuProject/Mastodon-Material app/javascript/styles/{プロファイル名}
   ```

3. Mastodonの`config/themes.yml`に、以下を参考に追記します。

   ```yml
   …
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # 追加テーマ
   {プロファイル名}: styles/{プロファイル名}/src/{プロファイル名}.scss # この行を追記
   …
   ```

4. テーマの表示名を変更したい場合には、Mastodonの`config/locales/{lang}.yml`に以下を参考に追記してください。(`config/locales/en.yml`は**必須**)

   ```yml
   …
   themes:
    contrast: High contrast
    default: Mastodon
    mastodon-light: Mastodon (light)
   
   # 追加テーマ
    {プロファイル名}: 表示名 # この一行を追記
   …
   ```

5. [Gitのサブモジュールを使用して導入する](installation_guide_ja.md#gitのサブモジュールを使用して導入する)の4.以降の手順に従ってください。

### Mastodon Materialのアップデート(サブモジュール)

Mastodonのリポジトリのホームディレクトリで以下のコマンドを実行して、サブモジュールとして設定したリポジトリからリソースを取得します。  

```
git submodule update --recursive --remote
```

> フォークしたリポジトリを使用している場合、upstreamからの`git pull`を忘れないようにしてください。

## ファイルをコピーして導入する

> この方法の場合、[ライセンス](../LICENSE)に基づいて表示することを忘れないようにしてください。

1. 本リポジトリの以下のファイルとフォルダを、Mastodonのリポジトリの`app/javascript/styles`へコピーしてください。
   
   - [`/src/mastodon-material/`](../src/mastodon-material/)
   - [`/src/mastodon-material.scss`](../src/mastodon-material.scss)

2. Mastodonのリポジトリの`config/themes.yml`に、以下を参考に追記します。

   ```yml
   …
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # 追加テーマ
   mastodon-material: styles/mastodon-material.scss # この行を追記
   …
   ```

3. [Gitのサブモジュールを使用して導入する](installation_guide_ja.md#gitのサブモジュールを使用して導入する)の3.以降の手順に従ってください。

### 複数のMastodon Materialを導入する(ファイルコピー)

1. 本リポジトリの以下のファイルとフォルダを、Mastodonのリポジトリの`app/javascript/styles`へコピーしてください。
   
   - [`/src/mastodon-material/`](../src/mastodon-material/)
   - [`/src/mastodon-material.scss`](../src/mastodon-material.scss)

2. 1.でコピーした2つのフォルダとファイルの名前を変更してください。ただし、便宜上フォルダとファイルの名前は同じものとします。以下ではその名前を`{プロファイル名}`と表記します。

3. Mastodonのリポジトリの`config/themes.yml`に、以下を参考に追記します。

   ```yml
   …
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # 追加テーマ
   {プロファイル名}: styles/{プロファイル名}.scss # この行を追記
   …
   ```

4. `{プロファイル名}.scss`を以下のように編集してください。
   
   Diff

   ```diff
   @use 'application';
   - @use 'mastodon-material';
   + @use '{プロファイル名}';
   ```
   
   <details>
   <summary>変更前</summary>

   ```scss
   @use 'application';
   @use 'mastodon-material';
   ```
   
   </details>

   <details>
   <summary>変更後</summary>

   ```scss
   @use 'application';
   @use '{プロファイル名}';
   ```
   
   </details>

5. [複数のMastodon Materialを導入する(サブモジュール)](installation_guide_ja.md#複数のMastodon-Materialを導入する(サブモジュール))の4.以降に従ってください。

### Mastodon Materialのアップデート(ファイルコピー)

テーマ内のフォルダを対応する以下の新しいものに置き換えてください。

- [`/src/mastodon-material/color/`](../src/mastodon-material/color/)
- [`/src/mastodon-material/layout/`](../src/mastodon-material/layout/)
- [`/src/mastodon-material/plugins/`](../src/mastodon-material/plugins/)
- [`/src/mastodon-material/theme/`](../src/mastodon-material/theme/)