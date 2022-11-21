# Live Widget Packaging

### JAR Packages

Since Live Widget is mainly a frontend application the creation of a full Java plugin is not necessary but could be convenient when developing Widget Query Handlers and additional features as Web Services as well. As discussed in [WebSetup](../../backend-api/web-setup.md) section of [Backend API](../../backend-api/), the Java Main class needs only the following:

```javascript
import net.intelie.live.*;
import net.intelie.live.util.PluginUtils;

public class Main implements LivePlugin {
    @Override
    public void start(Live live) throws Exception {
        PluginUtils.defaultWebSetup(live);
    }
}
```

### ZIP Packages with manifest.json

The developer could create a ZIP containing a `manifest.json` and ships its`target/bundle.js` (typically compiled to ES6 using [BabelJS](https://babeljs.io) or other transpiler) on the zip file as described in [Plugins](../../plugins.md).

The `manifest.json` must define the following `AddContent` and `AddTag` actions to inform Live which url mappings and scripts it should append to Live web application.

```javascript
{
    "name": "plugin-tutorial-widgets", 
    "version": "1.2.3", 
    "actions":[
        {
            "op": "AddContent",
            "path": "bundle.js",
            "location": "target/bundle.js"
        },
        {
            "op": "AddTag",
            "position": "END",
            "jsFile": {
                "path": "/content/plugin-tutorial-widgets/bundle.js",
            }
        }
    ]
}
```

### Webpack Rules

We recommend the usage of a webpack builder configuration in order to ease the development of widgets and automates its assemble in a single transpiled bundle.js file.

{% code title="webpack.config.js" %}
```typescript
const webpack = require('webpack')
const path = require('path')
const fs = require('fs')
const CopyWebpackPlugin = require('copy-webpack-plugin')

const proxy = 'http://0.0.0.0:8080'

function loadWebpackPlugins() {
    const plugins = [
        new CopyWebpackPlugin([
            {
                from: 'webapp/assets/',
                to: 'webapp/target/assets/'
            }
        ]),
        new webpack.DefinePlugin({
            'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV || 'development')
        })
    ]

    return plugins
}

// main ---------
const entries = {
    'plugin-tutorial-widgets': path.join(__dirname, 'webapp/js/app.js')
}
const entriesNames = Object.keys(entries)

console.log('[Intelie Live Webpack] Building plugins:', Object.keys(entries).join(', '))

const isExternalModule = function(request) {
    return (
        /^live\//.test(request) ||
        (/^react/.test(request) &&
            !/react-redux/.test(request) &&
            !/react-autosuggest/.test(request) &&
            !/react-themeable/.test(request) &&
            !/react-final-form/.test(request) &&
            !/react-autowhatever/.test(request)) ||
        /^prop-types/.test(request) ||
        /^styled-components/.test(request) ||
        /^polished/.test(request) ||
        /^create-react-class/.test(request) ||
        /^highcharts/.test(request) ||
        /lodash$/.test(request) ||
        /^moment/.test(request) ||
        /^codemirror/.test(request)
    )
}

module.exports = {
    entry: entries,
    output: {
        path: path.join(__dirname, '/'),
        filename: 'webapp/target/bundle.js'
    },
    plugins: loadWebpackPlugins(),

    resolve: {
        extensions: ['.ts', '.tsx', '.js', '.jsx']
    },
    externals: [
        function(context, request, callback) {
            if (isExternalModule(request)) return callback(null, 'commonjs ' + request)

            callback()
        }
    ],
    module: {
        rules: [
            {
                test: /\.(ts|js|jsx|tsx)$/,
                loader: 'babel-loader',
                exclude: /node_modules/
            },
            {
                test: /\.s?css$/,
                use: [
                    'style-loader', // creates style nodes from JS strings
                    'css-loader', // translates CSS into CommonJS
                    'sass-loader' // compiles Sass to CSS
                ]
            }
        ]
    },
    devServer: {
        inline: true,
        port: 8084,
        host: '0.0.0.0',
        historyApiFallback: true,
        proxy: [
            {
                path: '/',
                target: proxy,
                onProxyReq(proxyReq, req, res) {
                    // To stop Live from concatenating scripts on `live-plugins.js` on these plugins
                    proxyReq.setHeader('Live-Debug-Plugins', entriesNames.join(', '))
                },
                bypass: (req, res, proxOptions) => {
                    // console.log('debugging request from bypass: ', req.url)
                    const name = entriesNames.find(name => req.url.includes(`${name}/bundle.js`))
                    
                    if (name) return `/webapp/target/bundle.js`
                }
            }
        ]
    }
}

```
{% endcode %}
