# Installation Guide

## TOC

> - [Requirements](installation_guide.md#requirements)
> - [Installing using Git submodule](installation_guide.md#installing-using-git-submodule)
>   - [Installing multiple Mastodon Material (by using submodules)](installation_guide.md#installing-multiple-mastodon-material-by-using-submodules)
>   - [Updating Mastodon Material (by using subodules)](installation_guide.md#updating-mastodon-material-by-using-submodules)
> - [Installing files by copying files](installation_guide.md#installing-by-copying-files)
>   - [Installing multiple Mastodon Material (by copying files)](installation_guide.md#installing-multiple-mastodon-material-by-copying-files)
>   - [Updating multiple Mastodon Material (by copying files)](installation_guide.md#updating-mastodon-material-by-copying-files)

## Requirements

- Permission to edit the source code
- Knowledge about Git (basic, submodule)

## Installing using Git submodule

This method makes it easy to update and reuse the code.  
If you want to use Mastodon Material with multiple settings, refer [Install multiple Mastodon Material (by using submodules)](installation_guide.md#installing-multiple-mastodon-material-by-using-submodules).

1. Add submodules to run the following command in the home directory of Mastodon.
   
   ```
   git submodule add -b main https://github.com/GenbuProject/Mastodon-Material app/javascript/styles/mastodon-material
   ```

2. Add the code to `config/themes.yml` of Mastodon like below.
   
   ```yml
   …
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # Added theme
   mastodon-material: styles/mastodon-material/src/mastodon-material.scss # add this line
   …
   ```

3. If you want to change the display theme name in your language, add localization strings to `config/locales/{lang}.yml` like below (At least `config/locales/en.yml` is **REQUIRED**)
   
   ```yml
   …
   themes:
    contrast: High contrast
    default: Mastodon
    mastodon-light: Mastodon (light)
   
    # Added theme
    mastodon-material: Mastodon Material # add this line
    …
   ```

4. If you configure to use the webfont on Google Fonts (default) or on GitHub, you need to add an exception to *CSP (Content Security Policy)*. Make sure to change `config/initializers/content_security_policy.rb` like below.
   
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
   <summary>Before</summary>

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
   <summary>After</summary>

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
     p.font_src        :self, assets_host, github_fonts_host, google_fonts_host
     p.img_src         :self, :https, :data, :blob, assets_host
     p.style_src       :self, :unsafe_inline, assets_host
     p.media_src       :self, :https, :data, assets_host
     p.frame_src       :self, :https
     p.manifest_src    :self, assets_host
   ```

   </details>

> When runnning in a different environment or update Mastodon Material, run `git` command with the `--recursive --remote` option to fetch the submodules source code.

### Installing multiple Mastodon Material (by using submodules)

1. Fork this repository and checkout from `main` branch. This branch name will be written as `{profile_name}` in this guide.

2. Add submodules to run the following command in the home directory of Mastodon. Replace the URL according to the repository you forked.

   ```
   git submodule add -b {profile_name} https://github.com/GenbuProject/Mastodon-Material app/javascript/styles/{profile_name}
   ```

3. Add the code to `config/themes.yml` of Mastodon like below.
   
   ```yml
   …
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # Added theme
   {profile_name}: styles/{profile_name}/src/{profile_name}.scss # Add this line
   …
   ```

4. If you want to change the display theme name in your language, add localization strings to `config/locales/{lang}.yml` like below (At least `config/locales/en.yml` is **REQUIRED**)
   
   ```yml
   …
   themes:
    contrast: High contrast
    default: Mastodon
    mastodon-light: Mastodon (light)
   
   # Added theme
    {profile_name}: Display name # Add this line
   …
   ```

5. Follow the steps from 4. in [Installing using Git submodule](installation_guide.md#installing-using-git-submodule).

### Updating Mastodon Material (by using submodules)

Fetch resources from the repository you set as a submodule to run the following command in the home directory of Mastodon.

```
git submodule update --recursive --remote
```

> When using the forked repository, don't forget to run `git pull` from upstream.

## Installing by copying files

> When using this method, don't forget to display under [license](../LICENSE).

1. Copy those file and folder of this respository to `app/javascript/styles` of Mastodon.

   - [`/src/mastodon-material/`](../src/mastodon-material/)
   - [`/src/mastodon-material.scss`](../src/mastodon-material.scss)

2. Add the code to `config/themes.yml` of Mastodon like below.

   ```yml
   …
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # Added theme
   mastodon-material: styles/mastodon-material.scss # Add this line
   …
   ```

3. Follow the steps from 3. in [Installing using Git submodule](installation_guide.md#installing-using-git-submodule).

### Installing multiple Mastodon Material (by copying files)

1. Copy those files and folders of this respository to `app/javascript/styles` of Mastodon.

   - [`/src/mastodon-material/`](../src/mastodon-material/)
   - [`/src/mastodon-material.scss`](../src/mastodon-material.scss)

2. Change the name of the file and folder you copied in step 1. For convenience, the names of the file and folder are the same. The name will be written as `{profile_name}` in this guide.

3. Add the code to `config/themes.yml` of Mastodon like below.
   
   ```yml
   …
   default: styles/application.scss
   contrast: styles/contrast.scss
   mastodon-light: styles/mastodon-light.scss
   
   # Added theme
   {profile_name}: styles/{profile_name}.scss # Add this line
   …
   ```

4. Edit `{profile_name}.scss` like below.

   Diff

   ```diff
   @use 'application';
   - @use 'mastodon-material';
   + @use '{profile_name}';
   ```
   
   <details>
   <summary>Before</summary>

   ```scss
   @use 'application';
   @use 'mastodon-material';
   ```
   
   </details>

   <details>
   <summary>After</summary>

   ```scss
   @use 'application';
   @use '{profile_name}';
   ```
   
   </details>

5. Follow the steps from 4. in [Installing multiple Mastodon Material (by using submodules)](installation_guide.md#installing-multiple-mastodon-material-by-using-submodules).

### Updating Mastodon Material (by copying files)

Replace the folders in the theme with the corresponding to new ones below.

- [`/src/mastodon-material/color/`](../src/mastodon-material/color/)
- [`/src/mastodon-material/layout/`](../src/mastodon-material/layout/)
- [`/src/mastodon-material/plugins/`](../src/mastodon-material/plugins/)
- [`/src/mastodon-material/theme/`](../src/mastodon-material/theme/)