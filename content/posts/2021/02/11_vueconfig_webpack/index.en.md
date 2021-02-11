---
title: "Struggled webpack in vue.config"
date: 2021-02-11T01:18:52+01:00
categories: ["node", "vue", "webpack"]
translationKey: "vueconfig_webpack"
draft: false
---

Vue.js version 3 was released in 2020. I want to upgrade quickly in order to use the excellent functions of vue3 series. In fact, the vue ecosystem used in the field would not yet compatible with vue3. We might still have to use vue2 for now. During these times, the development environment often used to unstable and have some troubles.

### Images are no longer bundled with url-loader
In one enviroment, vue files were built and bundled into a single js file.
One day, I replaced webpack with @vue/cli in consideration of other vue, but image file is no longer bundled with js.
It seems that the url-loader doesn't work as intended.

#### Fixed: vue.config.js
@vue/cli has various loaders and it is preconfigured.
But it may necessary to make additional settings with `chainWebpack`.
In my case, I set the url-loader as follows. Base64 encode the image and make it bundle to js.

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

Regarding FIXME:
- `limit: undefined,`: The threshold value for whether to bundle the image. Be careful when handling large images with undefined.
- `esModule: false,`: It goes against the trend of ESM. Disabled it because it becomes the string `[object Module]` in base64.

The settings are case by case. The following setting documents helped me.

- https://cli.vuejs.org/guide/webpack.html#simple-configuration
- https://github.com/neutrinojs/webpack-chain

### Test your config
You can check the config that is finally applied by `@vue/cli` and vue.config.js with [vue inspect](https://cli.vuejs.org/guide/webpack.html#inspecting-the-project-s-webpack-config).
I proceeded while checking with this command one by one.

```sh
$ vue inspect > output.js
```

### Conclusion: Config settings are hard
The frontend has become highly abstracted by the framework, and Vue.js is now easier to get started with @vue/cli.
[vite](https://github.com/vitejs/vite) is also coming up, and Vue.js is evolving quickly.
On the contrary, we need to follow an environment that changes from one to another. And it's so hard that the time would melt if we don't understand the internal.
