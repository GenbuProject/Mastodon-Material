# Customization Guide

## Language | 言語

[日本語(Japanese)](customization_guide_ja.md)

## Note

These customizations need to modify source code. So, you need permission to change souce code.

## Basic Settings

Please make reference to [`mastodon-material/theme/base_config.scss`](../src/mastodon-material/theme/base_config.scss) and write your change in [`mastodon-material/custom_config.scss`](../src/mastodon-material/custom_config.scss). **DO NOT** edit [`base_config.scss`](../src/mastodon-material/theme/base_config.scss).

### Switch Profiles

There are two types of profiles, color scheme and layout profile. You can configure them by using `@import`. Default is Material v1(Light).

### Float search bar when cursor hovers

Search bar floats when cursor hovers (mouseover operation) and changes the background color into focusing one. Add `$search-bar-hover: true;` to [`custom_config.scss`](../src/mastodon-material/custom_config.scss).

## Icon settings

Edit [`mastodon-material/icon_config.scss`](../src/mastodon-material/icon_config.scss) to apply below settings.

### Change favorite icon

Edit `// Favorite icon settings` section to change it. Default icon is ★ (star).

### Change icon font's host

Default setting loads the icon font on GitHub [official repository](https://github.com/google/material-design-icons). Edit `// Material Design Icon settings` section to change it.

- **Use the font on Google Fonts**
  
  Set `$icon-font-source` value as `google`.

- **Host the font on your server**
  
  Set `$icon-font-source` value as `self`. Then, download font file from [official font repository](https://raw.githubusercontent.com/google/material-design-icons/master/iconfont/MaterialIcons-Regular.woff2) and put `MaterialIcons-Regular.woff2` file into `/mastodon/app/javascript/fonts`.

## Plugins

Uncomment what you want to enable in [`mastodon-material/loader.scss`](../src/mastodon-material/loader.scss).

### Display statuses on timeline in a card style

Change a default list style statuses in timeline into a card one. If you enable it, the less information are displayed in a card style than in a list one in a same density.

Uncomment `@import 'plugins/cards';` to enable it.

### Improve the browseability

This theme based on Material Design Guideline strictly, the less information are displayed by the default settings than the mastodon default ones in a same density. This plugin (dense plugin) can make the information displayed more by ignoring the guideline.

Uncomment `@import 'plugins/dense';` to enable it.

### (etc) Disable the material design icon font

This theme use [Material Icons Font](https://google.github.io/material-design-icons/#icon-font-for-the-web) by Google to display Material Design icon. If you have some problem about license, you can use [Font Awesome](https://fontawesome.com/), default icon font without any bad effect on other UI in this theme.

Comment out `@import 'theme/material-icons';` to disable it.
