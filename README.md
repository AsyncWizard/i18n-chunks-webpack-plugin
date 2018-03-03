# i18n-chunks-webpack-plugin
Look in all chunks for files that match a rule and then, associate and generate translations files with it.
If you have lazy loaded files, the plugin will generate separates files for each chunks and each langs defined by the rules.

## Installation

Install with npm:

```
npm i i18n-chunks-webpack-plugin
```

## Usage

**webpack.config.js**

```js
const I18NChunksWebpackPlugin = require('i18n-chunks-webpack-plugin')

module.exports = {
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new I18NChunksWebpackPlugin({
      // Modules requiring translation
      modules: [
        {
          // Identigying module
          rule: /src\/module1\.js$/i,

          // Defining langs for it
          languagesFiles: {
            'en-US': '@/i18n/en.js', // Filename doesen't matter but key does.
            'fr-FR': '@/i18n/fr.js'
          }
        },
        // ... you can add other rules or import a ones of package
      ],

      /**
       * Directory for temporary files (i18n chunks)
       **/
      tmpEntriesPath: './webpack-i18n-entries'
    })
  ]
}
```

## Results

The plugin will generate i18n chunks next to the code chunks:

| Filename| Description |
| --- | --- |
| *Code chunks:* |
| app.js | Main app chunk |
| 0.js | Lazy loaded chunk |
| 1.js | Lazy loaded chunk |
| index.html | App html |
| *I18N Chunks:* |
| app-en-US.js | App english chunk |
| app-fr-FR.js | App french chunk |
| 0-en-US.js | Lazy loaded I18N chunk |
| 0-fr-FR.js | Lazy loaded I18N chunk |
| 1-en-US.js | Lazy loaded I18N chunk |
| 1-fr-FR.js | Lazy loaded I18N chunk |
| webpack-i18n-manifest.js | I18N Chunk manifest |

The app chunk is necessary if the user wants to change the language of the page.
Each I18N chunk will contains translation for all files in the associated chunk.
I will publish a project to import and use these i18n chunks on the client side and update this doc later.
Stay tuned!
