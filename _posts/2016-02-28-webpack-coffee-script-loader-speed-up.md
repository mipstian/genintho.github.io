---
layout: post
title:  "Webpack Coffee Script Loader speed up"
date:   2016-02-28 19:00:00
---

Hipmunk frontend code is mainly written using CoffeScript. We are using the
Webpack loader to transpile it to JavaScript. This process is quick, except when
the Webpack watcher is lunched for the first time.

The [luncher code](https://github.com/webpack/coffee-loader) is small and straightforward.
It is basically a bridge between Webpack and the official npm module.

Each time Webpack need to recompile a CoffeeScript file, it calls the loader,
passing directly the full source code. The loader is simply transpiling the source
into JavaScript and returns it to Webpack.

While in watch mode, the transpilation is extremely quick: Webpack recompile only
the file that got modified. But lunching Webpack can take a very long time as
it is recompiling everything from scratch.


```
Webpack watching...
Compilation starting at: 19:10:31
Hash: 063a8e90a05b83f43d51
Time: 27427ms
```

After [a fork](https://github.com/genintho/coffee-loader) of the loader and the
addition of a file cache, it became much faster.

```
Webpack watching...
Compilation starting at: 19:11:31
Hash: 063a8e90a05b83f43d51
Time: 17760ms
```

10 sec. less. Not too bad. The bigger the code base is, the bigger this is saving.
This will also speed up the testing/CI infrastructure of Hipmunk as we regenerate
the JavaScript for every build.

The next step is to send a pull request with this change. Not sure how it will goes, considering that I used sync file I/O for everything :p
