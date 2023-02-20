## dependecias a usar con react

npm install react react-dom -S

## para trabjar con webpack

npm install @babel/core @babel/preset-env @babel/preset-react babel-loader -D

## ..crear y configuar .babelrc..

{
"presets": [
"@babel/preset-env",
"@babel/preset-react"
]
}

## instalar webpack

npm install webpack webpack-cli webpack-dev-server -D

## ..crear y configurar webpack.config.js..

const path = require('path');

module.exports = {
entry: './src/index.js',
output: {
path: path.resolve(**dirname, 'dist'),
filename: 'bundle.js'
},
resolve: {
extensions: ['.js', '.jsx']
},
module: {
rules:[
{
test: /\.(js|jsx)$/,
exclude: /node_modules/,
use: {
loader: 'babel-loader',
}
}
]
},
devServer: {
contentBaser: path.join(**dirname, 'dist'),
compress: true,
port: 8080,
open: true,
}
}

## instalar plugin html

npm install html-loader html-webpack-plugin -D

## instalar plugin css

npm install mini-css-extract-plugin css-loader style-loader sass sass-loader -D

## ..configurar plugin html y css

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
entry: './src/index.js',
output: {
path: path.resolve(\_\_dirname, 'dist'),
filename: 'bundle.js'
},
resolve: {
extensions: ['.js', '.jsx']
},
module: {
rules:[

            {
                test: /\.js$|jsx/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                }
            },
            {
                test: /\.html$/,
                use: [
                    {
                        loader: 'html-loader',
                    }
                ]
            },
            {
                test: /\.s[ac]ss$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'sass-loader'
                ]
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            filename: './index.html'
        }),
        new MiniCssExtractPlugin({
            filename: '[name].css'
        })
    ],

    devServer: {
        static: path.join(__dirname, 'dist'),
        compress: true,
        port: 8080,
        open: true,
    }

}

## configure package.json

"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"start": "webpack serve --mode development --config webpack.config.js",
"build": "webpack --mode production"
},

## dependecias que ayudan a trabajar con el css, optimizar el js y limpiar los recursos

npm install css-minimizer-webpack-plugin terser-webpack-plugin clean-webpack-plugin -D

## añadir la configuracion al proyecto

..crear webpack.config.dev.js (agregar mode:'development') y copiar todo lo de webpack.config.js.. 
..modifica webpack.config.js..

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
const TerserPlugin = require('terser-webpack-plugin');
const {cleanWebpackPlugin} = require('clean-webpack-plugin');

module.exports = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
        publicPaht: "/"
    },
    resolve: {
        extensions: ['.js', '.jsx'],
        alias: {
            '@components': path.resolve(__dirname, 'src/components/'),
            '@styles': path.resolve(__dirname, 'src/styles/')
        }
    },
    mode: 'production',
    module: {
        rules:[
            
            {
                test: /\.js$|jsx/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                }
            },
            {
                test: /\.html$/,
                use: [
                    {
                        loader: 'html-loader',
                    } 
                ]
            },
            {
                test: /\.s[ac]ss$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'sass-loader'
                ]
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            filename: './index.html'
        }),
        new MiniCssExtractPlugin({
            filename: '[name].css'
        }),
        new cleanWebpackPlugin(),
    ],
    optimization: {
        minimize: true,
        minimizer: [
            new CssMinimizerPlugin(),
            new TerserPlugin(),
        ]
    }
}


## package.json

"start": "webpack serve --config webpack.conf.dev.js",
"build": "webpack --config webpack.config.js"


