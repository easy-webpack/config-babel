# @easy-webpack/config-babel
Transform next generation JavaScript using [babel] and [babel-loader].

Transform on `.jsx` file is also supported by default.

# Installation
```
npm install --save-dev @easy-webpack/config-babel babel-core

# Default config presets and plugins
npm install --save-dev babel-preset-env babel-plugin-transform-decorators-legacy babel-plugin-transform-class-properties
```
[easy-webpack](https://github.com/easy-webpack/core) is also required.

[Babel presets/plugins](http://babeljs.io/docs/plugins/) is also required to be installed for different transformation.

# Usage
```js
// webpack.config.js
const generateConfig = require('@easy-webpack/core').generateConfig;

const baseConfig = { ... }; // project-specific config like the entry file

module.exports = generateConfig(
  baseConfig,

  require('@easy-webpack/config-babel')
    ({/* Options object */ options: { presets: ['es2015'], cacheDirectory: true } })
);

// This config will transform your ES2015 JS code into ES5 using babel es2015 presets
```

# Options
### options
Type: `Object` Default: 
```js
{
  plugins: [
    "transform-decorators-legacy",
     "transform-class-properties"
  ],
  presets: [
    [
      [
        "env",
        {
          "targets": {
            "browser": [
              "last 2 versions",
              "not ie <= 11"
            ]
          },
          "loose": true,
          "modules": false,
          "whitelist": [
            "transform-es2015-literals",
            "transform-es2015-template-literals" // required for uglify
          ]
        }
      ]
    ]
  ],
  cacheDirectory: true
}
```
Babel options. Please refer to [babel API options](https://babeljs.io/docs/usage/api/#options) and [babel-loader options](https://github.com/babel/babel-loader#options).

It is recommended to use `.babelrc` file. Please check the [tips](#using-babelrc) in the bottom section. 

### exclude
Type: `(Array of) webpack Condition object` Default: `absolute path of node_modules`

JavaScript files that would be excluded from babel transformation.

Check [webpack documentation](https://webpack.js.org/configuration/module/#condition) for details of Condition object.

If you use bower, it is recommended to change this to `/(node_modules|bower_components)/`. 

# Tips
## Using .babelrc
Using `.babelrc` file can separate babel's config from webpack config. This is recommended as this can increase readability of webpack config.

```js
// webpack.config.js
const generateConfig = require('@easy-webpack/core').generateConfig;

generateConfig(
  require('@easy-webpack/config-babel')
    ({ options: { cacheDirectory: true } })  // Remember to enable cacheDirectory for better build performance
)

// .babelrc
{
  "presets": ["es2015"]
}
```

You can then use [.babelrc format](https://babeljs.io/docs/usage/babelrc/) in `.babelrc`.

(`cacheDirectory` is an option for [babel-loader], therefore it cannot exist in `.babelrc`)
 
[babel]: http://babeljs.io/
[babel-loader]: https://github.com/babel/babel-loader