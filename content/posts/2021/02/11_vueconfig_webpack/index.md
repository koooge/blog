---
title: "vue.configのwebpack設定苦労した"
date: 2021-02-11T01:18:52+01:00
categories: ["node", "vue", "webpack"]
translationKey: "vueconfig_webpack"
draft: false
---

2020年にVue.jsバージョン3がリリースされました。vue3系の優れた機能を使う為に、さっさとアップグレードしてしまいたい。と思うのもの、現場で利用しているvueエコシステムがまだ3系に対応しておらず、2系を使わないといけないといった過渡期です。こういった時期には開発環境が不安定で、無駄に詰まることが多いです。

### url-loaderで画像がbundleされなくなった
ある環境では、vueで作ったファイル群をbundleし、まるっと1つのjsにbuildされていました。
その環境で、vueの他との兼ね合いで、webpackを@vue/cliに置き換えたところ、画像がjsにbundleされなくなりました。
どうもurl-loaderが狙い通り動かなくなったようです。

#### 修正: vue.config.js
@vue/cliには、良い感じに各種loaderが組み込まれており、良い感じに設定がされています。
このため、`chainWebpack`で追加設定する必要がありました。
今回はurl-loaderの設定を下記のようにしました。画像をbase64エンコードしてjsにbundleにします。

```vue.config.js
module.exports = {
  chainWebpack: (config) => {
    config.module
      .rule('images')
      .test(/\.(otf|eot|svg|ttf|woff|woff2|svg|gif|png)(\?.+)?$/)
      .use('url-loader')
      .loader('url-loader')
      .tap(options => {
        return {
          ...options,
          limit: undefined, // FIXME
          encoding: 'base64',
          esModule: false, // FIXME
        };
      });
  },
};
```

FIXME2箇所について補足。
- `limit: undefined,`: 画像をbundleするかどうかの敷居値。undefinedだと無制限になるため、大きなサイズの画像を扱う時は注意。
- `esModule: false,`: ESM化の世の流れと逆行する。今回はbase64にて `[object Module]` という文字列になってしまうため無効にした。

このあたり欲しい設定は場合に寄ると思います。下記設定ドキュメントを読みつつ調べつつ突破しました。

- https://cli.vuejs.org/guide/webpack.html#simple-configuration
- https://github.com/neutrinojs/webpack-chain

### configを確認しよう
`@vue/cli` とvue.config.jsによって最終的に適用されるconfigは、[vue inspect](https://cli.vuejs.org/guide/webpack.html#inspecting-the-project-s-webpack-config)で確認できます。
例えば下記のように出力可能です。これで逐次確認しつつ進めました。

```sh
$ vue inspect > output.js
```

### まとめ: 環境設定が大変
frontendはframeworkによりどんどん高度に抽象化され、Vue.jsも@vue/cliにより簡単にスタートできるようになりました。
今後は[vite](https://github.com/vitejs/vite)も控えており、Vue.jsがすごい勢いで進化しています。
逆に言うと、コロコロと変わる環境に対応が求められ、その内部理解無しではひたすら時間が溶かされて辛いです。
