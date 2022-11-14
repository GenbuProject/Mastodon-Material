# 開発方針

UI設計は、Mastodon側の実装の問題により準拠できない場合を除き、[Material Design Guideline](https://material.io/design/)に基づき、変数名も原則それに合わせたものとしている。

## カスタマイズについて

- サーバーの個性を尊重し、サーバー管理者がテーマをカスタマイズしやすいように心がける。[Material Theming](https://material.io/design/material-theming/)を参考に柔軟性のある設計をする。

## ライセンスについて

- 一部のサーバーからGoogle製Material Iconsフォントの採用について、Mastodonで採用されている[AGPL-3.0](https://www.gnu.org/licenses/licenses.html#AGPL)ではなく、部分的に[Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)に準拠する必要があるため、抵抗があるという意見をいただいた。これを回避するため、Material Iconsフォントを無効化し、デフォルトフォントのFont Awesomeを利用することができるオプションを提供する。  

- Material Iconsフォントは、デフォルトではGoogle FontsのWebfontを利用しているが、ライセンスに従ってサーバーでホスティングしたフォントを利用できる。

## リリース前の確認リスト

- [ ] [`mastodon-material/config/`](../src/mastodon-material/config/)の設定がデフォルトに戻っているか
- [ ] [`mastodon-material/_index.scss`](../src/mastodon-material/_index.scss)でプラグインが無効になっている
- [ ] プッシュ先ブランチが`develop`になっている
