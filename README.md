![Dependencies status](https://david-dm.org/tcoopman/image-webpack-loader/status.svg)
![devDependencies status](https://david-dm.org/tcoopman/image-webpack-loader/dev-status.svg)

# image-loader

Image loader module for webpack

> Minify PNG, JPEG, GIF, SVG and WEBP images with [imagemin](https://github.com/kevva/imagemin)

*Issues with the output should be reported on the imagemin [issue tracker](https://github.com/kevva/imagemin/issues).*

## Install

```sh
$ npm install image-webpack-loader --save-dev
```

### libpng issues

Installing on some versions of OSX may raise errors with a [missing libpng dependency](https://github.com/tcoopman/image-webpack-loader/issues/51#issuecomment-273597313): 
```
Module build failed: Error: dyld: Library not loaded: /usr/local/opt/libpng/lib/libpng16.16.dylib
```
This can be remedied by installing the newest version of libpng with [homebrew](http://brew.sh/):

```sh
brew install libpng
```

## Usage

[Documentation: Using loaders](https://webpack.js.org/concepts/loaders/)

In your `webpack.config.js`, add the image-loader, chained after the [file-loader](https://github.com/webpack/file-loader):

```js
loaders: [{
  test: /\.(gif|png|jpe?g|svg)$/i,
  use: [
    'file-loader',
    {
      loader: 'image-webpack-loader',
      options: {
        bypassOnDebug: true,
      },
    },
  ],
}]
```

For each optimizer you wish to configure, specify the corresponding key in options:

```js
loaders: [{
  test: /\.(gif|png|jpe?g|svg)$/i,
  use: [
    'file-loader',
    {
      loader: 'image-webpack-loader',
      options: {
        mozjpeg: {
          progressive: true,
          quality: 65
        },
        // optipng.enabled: false will disable optipng
        optipng: {
          enabled: false,
        },
        pngquant: {
          quality: '65-90',
          speed: 4
        },
        gifsicle: {
          interlaced: false,
        },
        // the webp option will enable WEBP
        webp: {
          quality: 75
        }
      }
    },
  ],
}]
```

Comes bundled with the following optimizers:

- [mozjpeg](https://github.com/imagemin/imagemin-mozjpeg) — *Compress JPEG images*
- [optipng](https://github.com/kevva/imagemin-optipng) — *Compress PNG images*
- [pngquant](https://github.com/imagemin/imagemin-pngquant) — *Compress PNG images*
- [svgo](https://github.com/kevva/imagemin-svgo) — *Compress SVG images*
- [gifsicle](https://github.com/kevva/imagemin-gifsicle) — *Compress GIF images*

And optional optimizers:

- [webp](https://github.com/imagemin/imagemin-webp) — *Compress JPG & PNG images into WEBP*

_Default optimizers can be disabled by specifying `optimizer.enabled: false`, and optional ones can be enabled by simply putting them in the options_

If you are using Webpack 1, take a look at the [old docs](http://webpack.github.io/docs/using-loaders.html) (or consider upgrading).

## Options

Loader options:

#### bypassOnDebug *(all)*

Type: `boolean`
Default: `false`

Using this, no processing is done when webpack 'debug' mode is used and the loader acts as a regular file-loader. Use this to speed up initial and, to a lesser extent, subsequent compilations while developing or using webpack-dev-server. Normal builds are processed normally, outputting optimized files.

For optimizer options, an up-to-date and exhaustive list is available on each optimizer repository:

- [mozjpeg](https://github.com/imagemin/imagemin-mozjpeg#options)
- [optipng](https://github.com/kevva/imagemin-optipng)
- [pngquant](https://github.com/imagemin/imagemin-optipng#options)
- [svgo](https://github.com/imagemin/imagemin-svgo#options)
- [gifsicle](https://github.com/imagemin/imagemin-gifsicle#options)
- [webp](https://github.com/imagemin/imagemin-webp#options)

## Inspiration

* [gulp-imagemin](https://github.com/sindresorhus/gulp-imagemin)
* [file-loader](https://github.com/webpack/file-loader)
* [imagemin-pngquant](https://github.com/imagemin/imagemin-pngquant)

## License

MIT (http://www.opensource.org/licenses/mit-license.php)
