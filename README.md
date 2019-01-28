# wxml-miniapp-loader

wxml loader for webpack

**Please note this
[wxml](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/) is a
markup language for
[Wechat mini programs](https://mp.weixin.qq.com/debug/wxadoc/dev/)**

## Installation

```bash
npm install wxml-miniapp-loader --save-dev
```

## Usage

You may also need to use
[file-loader](https://github.com/webpack-contrib/file-loader) to extract files.

```js
{
  test: /\.wxml$/,
  include: /src/,
  use: [
    {
      loader: 'file-loader',
      options: {
        name: '[name].[ext]',
        useRelativePath: true,
        context: resolve('src'),
      },
    },
    {
      loader: 'wxml-miniapp-loader',
      options: {
        root: resolve('src'),
        enforceRelativePath: true,
      },
    },
  ],
}
```

##### Options

* `root` (String): Root path for requiring sources
* `enforceRelativePath` (Boolean): Should be true if you wish to generate a
  `root` relative URL for each file. **It is recommend to set to `true`**
* `publicPath` (String): Defaults to webpack
  [output.publicPath](https://webpack.js.org/configuration/output/#output-publicpath)
* `transformContent(content, resource)` (Function): Transform content, should
  return a content string
* `transformUrl(url, resource)` (Function): Transform url, should return a url
* `minimize` (Boolean): To minimize. Defaults to `false`
* All
  [html-minifier](https://github.com/kangax/html-minifier#options-quick-reference)
  options are supported


## For Baidu smart programs

This loader is also compatible with
[Baidu smart programs](https://smartprogram.baidu.com/developer/index.html). You
just need to make sure using `test: /\.wxml$/` in
webpack config.

If you're using
[mini-application-webpack-plugin](https://github.com/zhengjiaqi/mini-application-webpack-plugin) and
setting `Targets.Swan` as webpack target, it will automatically transform `.wxml` to `.swan`. That means you could write mini programs once, and build both
Wechat and Baidu mini programs.

###### Example

webpack.config.babel.js

```js
import MiniApplicationWebpackPlugin from 'mini-application-webpack-plugin';
export default env => ({
  // ...other
  target: Targets[env.target || "Wx"],
  module: {
    rules: [
      // ...other,
      {
        test: /\.wxml$/,
        use: [
          {
            loader: "file-loader",
            options: {
              name: `[name].${env.target === "Swan" ? "swan" : "wxml"}`
              useRelativePath: true,
              context: resolve('src'),
            },
          },
          {
            loader: 'wxml-miniapp-loader',
            options: {
              root: resolve('src'),
              enforceRelativePath: true,
            },
          },
        ]
      }
    ]
  },
  plugin: [
    // ...other
    new MiniApplicationWebpackPlugin()
  ]
});
```


## License

MIT
