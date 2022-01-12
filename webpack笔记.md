# WebPack配置
## 配置5个核心
* entry
* ouput
* loader 	1. 下载   2. 使用（配置loader）
* plugins	1. 下载  2. 引入  3. 使用
* mode

		/*
		  webpack.config.js  webpack的配置文件
		    作用: 指示 webpack 干哪些活（当你运行 webpack 指令时，会加载里面的配置）

		    所有构建工具都是基于nodejs平台运行的 模块化默认采用commonjs。
		*/
		// resolve用来拼接绝对路径的方法
		const { resolve } = require('path');

		module.exports = {
			// webpack入口起点文件
		  entry: './index.js',
			// 输出文件
		  output: {       
		    // 输出文件名
		    filename: 'bundle.js',
		    // 输出路径
    		    // __dirname是nodejs的变量，代表当前文件的目录绝对路径
		    path: resolve(__dirname, 'dist')
		  },
		  module: {
		    rules: [
		 	// 详细loader配置
    			// 不同文件必须配置不同loader处理
		    ]
		  },
		  plugins: [
		  // 详细plugins的配置
		  ],
		  mode: 'development'// 开发模式(没压缩，一般我们用来测试，压缩了看不懂)
		  // mode: 'production'
		};
		 
		
	   1. 运行指令：
		    开发环境指令：npx webpack ./src/index.js -o ./build/built.js --mode=development
		       webpack会以 ./src/index.js 为入口文件开始打包，打包后输出到 ./build/built.js
		       整体打包环境，是开发环境
		    生产环境指令：npx webpack ./src/index.js -o ./build/built.js --mode=production
		       webpack会以 ./src/index.js 为入口文件开始打包，打包后输出到 ./build/built.js
		       整体打包环境，是生产环境

	   2. 结论：
		    1. webpack能处理js/json资源，不能处理css/img等其他资源
		    2. 生产环境和开发环境将ES6模块化编译成浏览器能识别的模块化~
		    3. 生产环境比开发环境多一个压缩js代码。【生产给开发压缩了】
	    
		//PS D:\woyu\webpack资料\代码>  npm i webpack-cli -D
		

## 开发环境配置
运行项目指令：  
* webpack 会将打包结果输出出去
* webpack serve 只会在内存中编译打包，没有输出
* PS D:\woyu\webpack资料\代码\2.webpack开发环境配置\03.打包样式资源>  webpack
        
        const { resolve } = require('path');
        const HtmlWebpackPlugin = require('html-webpack-plugin');

        module.exports = {
        entry: './src/js/index.js',
        output: {
            filename: 'js/built.js',
            path: resolve(__dirname, 'build')
        },
        module: {
            rules: [
            // loader的配置
            {
                // 处理less资源
		//PS D:\woyu\webpack资料\代码> npm i less-loader -D
                test: /\.less$/,
                use: ['style-loader', 'css-loader', 'less-loader']
            },
            {
                // 处理css资源
		//PS D:\woyu\webpack资料\代码> npm i css-loader style-loader -D
                test: /\.css$/,
                use: [
			  // use数组中loader执行顺序：从右到左，从下到上 依次执行
			  // 创建style标签，将js中的样式资源插入进行，添加到head中生效
			  'style-loader',
			  // 将css文件变成commonjs模块加载js中，里面内容是样式字符串
			  'css-loader'
			]
            },
            {
                // 处理图片资源
                test: /\.(jpg|png|gif)$/,// 匹配哪些文件
                // 下载 url-loader [file-loader]
		//PS D:\woyu\webpack资料\代码> npm i html-loader url-loader file-loader -D
		loader: 'url-loader',
		//打包图片问题，webpack5x版本已废弃 url-loader、file-loader等
		/*解决：
		{
			test: /\.(jpe|png|gif)$/,
			type: 'asset/inline' //替代url-loader
		      },
		---------------------------------------------------------------------
		以下是官方解读

		资源模块(asset module)是一种模块类型，它允许使用资源文件（字体，图标等）而无需配置额外 loader。

		在 webpack 5 之前，通常使用：

		raw-loader 将文件导入为字符串
		url-loader 将文件作为 data URI 内联到 bundle 中
		file-loader 将文件发送到输出目录
		资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：

		asset/resource 发送一个单独的文件并导出 URL。之前通过使用 file-loader 实现。
		asset/inline 导出一个资源的 data URI。之前通过使用 url-loader 实现。
		asset/source 导出资源的源代码。之前通过使用 raw-loader 实现。
		asset 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 url-loader，并且配置资源体积限制实现。
		当在 webpack 5 中使用旧的 assets loader（如 file-loader/url-loader/raw-loader 等）和 asset 模块时，你可能想停止当前 asset 模块的处理，并再次启动处理，
		这可能会导致 asset 重复，你可以通过将 asset 模块的类型设置为 'javascript/auto' 来解决。*/
                options: {
		// 图片大小小于8kb，就会被base64处理
          	// 优点: 减少请求数量（减轻服务器压力）
         	 // 缺点：图片体积会更大（文件请求速度更慢）
                limit: 8 * 1024,
		 // 给图片进行重命名
         	 // [hash:10]取图片的hash的前10位
       		 // [ext]取文件原来扩展名
                name: '[hash:10].[ext]',
                // 问题：因为url-loader默认使用es6模块化解析，而html-loader引入图片是commonjs
        	// 解决：关闭url-loader的es6模块化，使用commonjs解析
                esModule: false,
                outputPath: 'imgs'
                },
		//使用这些废弃的旧功能，加上type: 'javascript/auto'
                type: 'javascript/auto'
            },
            {
                test: /\.html$/,
		// 处理html文件的img图片（负责引入img，从而能被url-loader进行处理）
                loader: 'html-loader'
            },
            {
                // 处理其他资源
                exclude: /\.(html|js|css|less|jpg|png|gif)$/,
                loader: 'file-loader',
                options: {
                name: '[hash:10].[ext]',
                outputPath: 'media'
                }
            }
            ]
        },
        plugins: [
            // plugins的配置
	    // html-webpack-plugin
	    //PS D:\woyu\webpack资料\代码> npm i html-webpack-plugin -D
	    // 功能：默认会创建一个空的HTML，自动引入打包输出的所有资源（JS/CSS）
	    // 需求：需要有结构的HTML文件
            new HtmlWebpackPlugin({
	     // 复制 './src/index.html' 文件，并自动引入打包输出的所有资源（JS/CSS）
            template: './src/index.html'
            })
        ],
        mode: 'development',
		// 开发服务器 devServer：用来自动化（自动编译，自动打开浏览器，自动刷新浏览器~~）
		// 特点：只会在内存中编译打包，不会有任何输出
		// 启动devServer指令为： webpack serve
		//PS D:\woyu\webpack资料\代码\2.webpack开发环境配置\07.devServer> webpack server
        devServer: {
		// 项目构建后路径
    		//contentBase: resolve(__dirname, 'build'),
   		 //版本更改
            static: resolve(__dirname, 'build'),
	    // 启动gzip压缩
            compress: true,
	    // 端口号
            port: 3000,
	    // 自动打开浏览器
            open: true
        }
        };
        
## 生产环境配置
        
        const { resolve } = require('path');
        //PS D:\woyu\webpack资料\代码> npm i mini-css-extract-plugin -D
        const MiniCssExtractPlugin = require('mini-css-extract-plugin');
        //PS D:\woyu\webpack资料\代码> npm i optimize-css-assets-webpack-plugin -D
        const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');
        const HtmlWebpackPlugin = require('html-webpack-plugin');

        // 定义nodejs环境变量：决定使用browserslist的哪个环境
        process.env.NODE_ENV = 'production';

        // 复用loader
        const commonCssLoader = [
        // 创建style标签，将样式放入'style-loader', 
        // 这个loader取代style-loader。作用：提取js中的css成单独文件
        MiniCssExtractPlugin.loader,
        // 将css文件整合到js文件中
        'css-loader',
         /*
            css兼容性处理：postcss --> postcss-loader postcss-preset-env

            帮postcss找到package.json中browserslist里面的配置，通过配置加载指定的css兼容性样式

            "browserslist": {
              // 开发环境 --> 设置node环境变量：process.env.NODE_ENV = 'development'
              "development": [
                "last 1 chrome version",
                "last 1 firefox version",
                "last 1 safari version"
              ],
              // 生产环境：默认是看生产环境
              "production": [
                ">0.2%",
                "not dead",
                "not op_mini all"
              ]
            }
          */
        //PS D:\woyu\webpack资料\代码> npm i postcss-loader -D
        {
            loader: 'postcss-loader',
            options: {
            ident: 'postcss',
            plugins: () => [require('postcss-preset-env')()]// postcss的插件postcss-preset-env
            }
        }
        ];

        module.exports = {
        entry: './src/js/index.js',
        output: {
            filename: 'js/built.js',
            path: resolve(__dirname, 'build')
        },
        module: {
            rules: [
            {
                test: /\.css$/,
                use: [...commonCssLoader]
            },
            {
                test: /\.less$/,
                use: [...commonCssLoader, 'less-loader']
            },
            /*
                正常来讲，一个文件只能被一个loader处理。
                当一个文件要被多个loader处理，那么一定要指定loader执行的先后顺序：
                先执行eslint 在执行babel
            */
            {
            /*
                语法检查： eslint-loader  eslint
                注意：只检查自己写的源代码，第三方的库是不用检查的
                设置检查规则：
                    package.json中eslintConfig中设置~
                    "eslintConfig": {
                        "extends": "airbnb-base"
                    }
                    airbnb --> eslint-config-airbnb-base  eslint-plugin-import eslint
            //PS D:\woyu\webpack资料\代码> npm i  eslint-config-airbnb-base  eslint-plugin-import eslint eslint-loader -D
            */
                test: /\.js$/,
                //排除第三方的库
                exclude: /node_modules/,
                // 优先执行
                enforce: 'pre',
                loader: 'eslint-loader',
                options: {
                // 自动修复eslint的错误
                fix: true
                }
            },
            {
                /*
                js兼容性处理：babel-loader @babel/core 
                //PS D:\woyu\webpack资料\代码> npm i PS D:\woyu\webpack资料\代码> npm i babel-loader @babel/core @babel/preset-env @babel/polyfill core-js -D
                1. 基本js兼容性处理 --> @babel/preset-env
                    问题：只能转换基本语法，如promise高级语法不能转换 不用
                2. 全部js兼容性处理 --> @babel/polyfill  
                    问题：我只要解决部分兼容性问题，但是将所有兼容性代码全部引入，体积太大了~不用
                3. 需要做兼容性处理的就做：按需加载  --> core-js 用！
                */  
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
                options: {
                // 预设：指示babel做怎么样的兼容性处理
                  presets: [
                    [
                    '@babel/preset-env',
                    {
                        // 按需加载
                        useBuiltIns: 'usage',
                        // 指定core-js版本
                        corejs: {version: 3},
                        // 指定兼容性做到哪个版本浏览器
                        targets: {
                        chrome: '60',
                        firefox: '50'
                        }
                    }
                    ]
                ]
                }
            },
            {
                test: /\.(jpg|png|gif)/,
                loader: 'url-loader',
                options: {
                limit: 8 * 1024,
                name: '[hash:10].[ext]',
                outputPath: 'imgs',
                esModule: false
                }
            },
            {
                test: /\.html$/,
                loader: 'html-loader'
            },
            {
                exclude: /\.(js|css|less|html|jpg|png|gif)/,
                loader: 'file-loader',
                options: {
                outputPath: 'media'
                }
            }
            ]
        },
        plugins: [
            new MiniCssExtractPlugin({
            // 对输出的css文件进行重命名
            filename: 'css/built.css'
            }),
            // 压缩css
            new OptimizeCssAssetsWebpackPlugin(),
            // 压缩html代码
            new HtmlWebpackPlugin({
            template: './src/index.html',
            minify: {
                // 移除空格
                collapseWhitespace: true,
                // 移除注释
                removeComments: true
            }
            })
        ],
        // 生产环境下会自动压缩js代码
        mode: 'production'
        };
        
## webpack性能优化
* 开发环境性能优化
* 生产环境性能优化

### 开发环境性能优化
* 优化打包构建速度
  * HMR
     
        HMR: hot module replacement 热模块替换 / 模块热替换  
        作用：一个模块发生变化，只会重新打包这一个模块（而不是打包所有模块）   
        极大提升构建速度  
        
        样式文件：可以使用HMR功能：因为style-loader内部实现了  
        js文件：默认不能使用HMR功能 --> 需要修改js代码，添加支持HMR功能的代码  
            注意：HMR功能对js的处理，只能处理非入口js文件的其他文件。  
        html文件: 不用做HMR功能  

            ...  
            mode: 'development',
            devServer: {
                contentBase: resolve(__dirname, 'build'),
                compress: true,
                port: 3000,
                open: true,
                // 开启HMR功能
                // 当修改了webpack配置，新配置要想生效，必须重新webpack服务
                hot: true
            }
            ...
        
* 优化代码调试
  * source-map
        
            source-map: 一种提供源代码到构建后代码映射技术 （如果构建后代码出错了，通过映射可以追踪源代码错误）

            [inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map

            source-map：外部
                错误代码准确信息 和 源代码的错误位置
            inline-source-map：内联
                只生成一个内联source-map
                错误代码准确信息 和 源代码的错误位置
            hidden-source-map：外部
                错误代码错误原因，但是没有错误位置
                不能追踪源代码错误，只能提示到构建后代码的错误位置
            eval-source-map：内联
                每一个文件都生成对应的source-map，都在eval
                错误代码准确信息 和 源代码的错误位置
            nosources-source-map：外部
                错误代码准确信息, 但是没有任何源代码信息
            cheap-source-map：外部
                错误代码准确信息 和 源代码的错误位置 
                cheap只能精确的行
            cheap-module-source-map：外部
                错误代码准确信息 和 源代码的错误位置 
                module会将loader的source map加入

        内联 和 外部的区别：1. 外部生成了文件，内联没有 2. 内联构建速度更快

            开发环境：速度快，调试更友好
            速度快(eval>inline>cheap>...)
                eval-cheap-souce-map
                eval-source-map
            调试更友好  
                souce-map
                cheap-module-souce-map
                cheap-souce-map

        综合方案 --> eval-source-map √ / eval-cheap-module-souce-map
                        react默认这个

            生产环境：源代码要不要隐藏? 调试要不要更友好
            内联会让代码体积变大，所以在生产环境不用内联
            nosources-source-map 全部隐藏
            hidden-source-map 只隐藏源代码，会提示构建后代码错误信息

        综合方案--> source-map √ / cheap-module-souce-map
        
### 生产环境性能优化
* 优化打包构建速度
  * oneOf
  * babel缓存
  * 多进程打包
  * externals
  * dll
* 优化代码运行的性能
  * 缓存(hash-chunkhash-contenthash)
  * tree shaking
  * code split
  * 懒加载/预加载
  * pwa
  
