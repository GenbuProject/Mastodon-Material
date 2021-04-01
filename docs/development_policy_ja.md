# 開発方針

UI設計は、mastodon側の実装の問題により準拠できない場合を除き、[Material Design Guideline](https://material.io/design/)に基づき、変数名も原則それに合わせたものとしている。

## カスタマイズについて

* サーバーの個性を尊重し、サーバー管理者がテーマをカスタマイズしやすいように心がける。[Material Theming](https://material.io/design/material-theming/)を参考に柔軟性のある設計をする。

## ライセンスについて

* 一部のサーバーからGoogle製Material Iconsフォントの採用について、mastodonで採用されている[AGPL-3.0](https://www.gnu.org/licenses/licenses.html#AGPL)ではなく、部分的に[Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)などを導入しなければならず、抵抗があるという意見をいただいた。 そこで、[`loader.scss`](../src/mastodon-material/loader.scss)の設定によってこの問題を回避できるようにしている。

* Material Iconsフォントは、デフォルトではGitHub上のWebfontを利用しているが、ライセンス問題をクリアする場合にのみ各サーバーでのフォントのホスティングを利用できる。([`base_config.scss`](../src/mastodon-material/theme/base_config.scss)を参照)

# 実装

- [x] タイムライン
- [x] トゥート詳細
- [x] 設定
- [ ] 管理
- [x] about
- [x] about/more
- [ ] アニメーション
