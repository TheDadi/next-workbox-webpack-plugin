# next-workbox-webpack-plugin

> Webpack plugin with workbox, it helps you build a Progressive Web App powered by Next.js. Generating service worker scripts and precache manifest of Next.js's pages and chunks.

<img width="1024" src="https://user-images.githubusercontent.com/124117/36341030-4b040398-142b-11e8-9de7-41d3dbe55427.png">

## Install

```
$ npm install -d next-workbox-webpack-plugin
```

## Usage

```js
const nextWorkboxWebpackPlugin = require('next-workbox-webpack-plugin');

nextWorkboxWebpackPlugin({
  // must, see next.config.js below
  buildId,
  // optional, next.js dist path as compiling. most of cases you don't need to fix it.
  distDir: '.next',
  // optional, which version of workbox will be used in between 'local' or 'cdn'. 'local'
  // option will help you use copy of workbox libs in localhost.
  importWorkboxFrom: 'local',
  // optional ,whether make a precache manifest of pages and chunks of Next.js app or not.
  precacheManifest: true,
  // optional, whether delete workbox path generated by the plugin.
  removeDir: true,
  // optional, path for generating sw files in build, `./static/workbox` is default
  swDestRoot: './static/',
  // optional, path for serving sw files in build, `./static/workbox` is default
  swURLRoot: '/static'
  // optional, you can use workbox-build options. except swDest because of output location is fixed in 'static/workbox',
  ...WorkboxBuildOptions,
});
```

## Usage in next.config.js

```
const NextWorkboxWebpackPlugin = require('next-workbox-webpack-plugin');

module.exports = {
  webpack: (config, {isServer, dev, buildId, config: {distDir}}) => {
    if (!isServer && !dev) {
      config.plugins.push(new NextWorkboxWebpackPlugin({
        distDir,
        buildId
      }))
    }

    return config
  }
}
```

## How it works

### Custom Server

- Only works in `NOT dev mode`. You can't test with `next` and `next start`
- To serve `sw.js`, you need custom server with custom route. See [test server is in this package](https://github.com/ragingwind/next-workbox-webpack-plugin/blob/master/example/hello-pwa/server.js).
- You have to [add script of registering service worker](https://github.com/ragingwind/next-workbox-webpack-plugin/blob/master/examples/hello-pwa/pages/index.js) into part of your application
- All of files will be generated under `/static/workbox` because of exporting. You might need to add the path to gitignore.

```
static/workbox
├── next-precache-manifest-d42167a04499e1887dad3156b93e064d.js
├── sw.js
└── workbox-v3.0.0-beta.0
    ├── workbox-background-sync.dev.js
    ├── ...
    ├── workbox-sw.js
```
- For more information, please refer to test and [Get Started With Workbox For Webpack](https://goo.gl/BFQxuo)

### Now 2.0

[TBD]

### Examples

- [Hello PWA](https://github.com/ragingwind/next-workbox-webpack-plugin/tree/master/examples/hello-pwa): You can learn how to use the webpack plugin basically
- [HNPWA](https://github.com/ragingwind/next-workbox-webpack-plugin/tree/master/examples/hnpwa): Simple HNPWA apps with Next.js

## License

MIT © [Jimmy Moon](https://ragingwind.me)
