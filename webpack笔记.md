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

# WebPack配置
## 配置5个核心
* entry
			
			entry: 入口起点
			1. string --> './src/index.js'
			单入口
			打包形成一个chunk。 输出一个bundle文件。
			此时chunk的名称默认是 main
			2. array  --> ['./src/index.js', './src/add.js']
			多入口
			所有入口文件最终只会形成一个chunk, 输出出去只有一个bundle文件。
			    --> 只有在HMR功能中让html热更新生效
			3. object
			多入口
			有几个入口文件就形成几个chunk，输出几个bundle文件
			此时chunk的名称是 key
			--> 特殊用法
			    {
			    // 多入口文件最终只会形成一个chunk, 输出出去只有一个bundle文件。
			    index: ['./src/index.js', './src/count.js'], 
			    // 形成一个chunk，输出一个bundle文件。
			    add: './src/add.js'
			    }
* ouput
			
			output: {
			    // 文件名称（指定名称+目录）
			    filename: 'js/[name].js',
			    // 输出文件目录（将来所有资源输出的公共目录）
			    path: resolve(__dirname, 'build'),
			    // 所有资源引入公共路径前缀 --> 'imgs/a.jpg' --> '/imgs/a.jpg'
			    publicPath: '/',
			    chunkFilename: 'js/[name]_chunk.js', // 非入口chunk的名称
			    // library: '[name]', // 整个库向外暴露的变量名
			    // libraryTarget: 'window' // 变量名添加到哪个上 browser
			    // libraryTarget: 'global' // 变量名添加到哪个上 node
			    // libraryTarget: 'commonjs'
			},
* loader 	1. 下载   2. 使用（配置loader）

			module: {
			    rules: [
			    // loader的配置
			    {
				test: /\.css$/,
				// 多个loader用use
				use: ['style-loader', 'css-loader']
			    },
			    {
				test: /\.js$/,
				// 排除node_modules下的js文件
				exclude: /node_modules/,
				// 只检查 src 下的js文件
				include: resolve(__dirname, 'src'),
				// 优先执行
				enforce: 'pre',
				// 延后执行
				// enforce: 'post',
				// 单个loader用loader
				loader: 'eslint-loader',
				options: {

				}
			    },
			    {
				// 以下配置只会生效一个
				oneOf: []
			    }
			    ]
			},

* plugins	1. 下载  2. 引入  3. 使用
* mode

* 除了上面五个以外的
* resolve

			    // 解析模块的规则
			    resolve: {
				// 配置解析模块路径别名: 优点简写路径 缺点路径没有提示
				alias: {
				    $css: resolve(__dirname, 'src/css')
				},
				// 配置省略文件路径的后缀名
				extensions: ['.js', '.json', '.jsx', '.css'],
				// 告诉 webpack 解析模块是去找哪个目录
				modules: [resolve(__dirname, '../../node_modules'), 'node_modules']
			    }
* devServer

			    devServer: {
				// 运行代码的目录
				contentBase: resolve(__dirname, 'build'),
				// 监视 contentBase 目录下的所有文件，一旦文件变化就会 reload
				watchContentBase: true,
				watchOptions: {
				// 忽略文件
				ignored: /node_modules/
				}, 
				// 启动gzip压缩
				compress: true,
				// 端口号
				port: 5000,
				// 域名
				host: 'localhost',
				// 自动打开浏览器
				open: true,
				// 开启HMR功能
				hot: true,
				// 不要显示启动服务器日志信息
				clientLogLevel: 'none',
				// 除了一些基本启动信息以外，其他内容都不要显示
				quiet: true,
				// 如果出错了，不要全屏提示~
				overlay: false,

				// 服务器代理 --> 解决开发环境跨域问题
				proxy: {
				// 一旦devServer(5000)服务器接受到 /api/xxx 的请求，就会把请求转发到另外一个服务器(3000)
				'/api': {
				    target: 'http://localhost:3000',
				    // 发送请求时，请求路径重写：将 /api/xxx --> /xxx （去掉/api）
				    pathRewrite: {
				    '^/api': ''
				    }
				}
				}
			    }
* optimization

			    const TerserWebpackPlugin = require('terser-webpack-plugin')

			    optimization: {
				splitChunks: {
				chunks: 'all'
				// 默认值，可以不写
				/*
				minSize: 30 * 1024, // 分割的chunk最小为30kb
				maxSiza: 0, // 最大没有限制
				minChunks: 1, // 要提取的chunk最少被引用1次
				maxAsyncRequests: 5, // 按需加载时并行加载的文件的最大数量
				maxInitialRequests: 3, // 入口js文件最大并行请求数量
				automaticNameDelimiter: '~', // 名称连接符
				name: true, // 可以使用命名规则
				cacheGroups: {
				    // 分割chunk的组
				    // node_modules文件会被打包到 vendors 组的chunk中。--> vendors~xxx.js
				    // 满足上面的公共规则，如：大小超过30kb，至少被引用一次。
				    vendors: {
				    test: /[\\/]node_modules[\\/]/,
				    // 优先级
				    priority: -10
				    },
				    default: {
				    // 要提取的chunk最少被引用2次
				    minChunks: 2,
				    // 优先级
				    priority: -20,
				    // 如果当前要打包的模块，和之前已经被提取的模块是同一个，就会复用，而不是重新打包模块
				    reuseExistingChunk: true
				    } 
				}*/
				},
				// 将当前模块的记录其他模块的hash单独打包为一个文件 runtime
				// 解决：修改a文件导致b文件的contenthash变化
				runtimeChunk: {
				name: entrypoint => `runtime-${entrypoint.name}`
				},
				minimizer: [
				// 配置生产环境的压缩方案：js和css
				//PS D:\woyu\webpack资料\代码> npm i terser-webpack-plugin -D
				new TerserWebpackPlugin({
				    // 开启缓存
				    cache: true,
				    // 开启多进程打包
				    parallel: true,
				    // 启动source-map
				    sourceMap: true
				})
				]
			    }
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
        html文件: 默认不能使用HMR功能，同时会导致问题，html文件不能热更新了。
				解决：修改entry入口，将html文件引入
                entry: ['./index.js', './index.html']
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
#### 优化打包构建速度
##### 1、oneOf
* 作用   ---   oneOf能让文件只匹配一个loader。
* 通常来说每一种类型的文件要被所有的 loader 过一遍，oneOf能让文件只匹配一个loader。  
* 注意：不能有两个配置处理同一种类型文件，重复的写外面。

        oneOf: [
                {
                    test: /\.css$/,
                    use: [...commonCssLoader]
                },
                {
                    test: /\.less$/,
                    use: [...commonCssLoader, 'less-loader']
                },

##### 2、babel缓存   
* 类似于开发环境中的HMR。  
* babel缓存  -->   让第二次打包构建速度更快

        
        缓存：
            babel缓存  cacheDirectory: true
            
            options: {
              presets: [
                [
                  '@babel/preset-env',
                  {
                    useBuiltIns: 'usage',
                    corejs: { version: 3 },
                    targets: {
                      chrome: '60',
                      firefox: '50'
                    }
                  }
                ]
              ],
              // 开启babel缓存
              // 第二次构建时，会读取之前的缓存
              cacheDirectory: true
            }

        server.js

        服务器代码
        启动服务器指令：
            npm i nodemon -g
            nodemon server.js

            node server.js
        访问服务器地址：
            http://localhost:3000

        
        const express = require('express');
        const app = express();
        // express.static向外暴露静态资源
        // maxAge 资源缓存的最大时间，单位ms
        app.use(express.static('build', { maxAge: 1000 * 3600 }));
        app.listen(3000);

##### 多进程打包


        PS D:\woyu\webpack资料\代码> npm i thread-loader -D

        use: [
              /* 
                开启多进程打包。 
                进程启动大概为600ms，进程通信也有开销。
                只有工作消耗时间比较长，才需要多进程打包
              */
              {
                loader: 'thread-loader',
                options: {
                  workers: 2 // 进程2个
                }
              },
              {

              }
        ]

        index.js
        /*
        1. eslint不认识 window、navigator全局变量
            解决：需要修改package.json中eslintConfig配置
            "env": {
                "browser": true // 支持浏览器端全局变量
            }
        2. sw代码必须运行在服务器上
            --> nodejs
            -->
                npm i serve -g
                serve -s build 启动服务器，将build目录下所有资源作为静态资源暴露出去
        */
        // 注册serviceWorker
        // 处理兼容性问题
        if ('serviceWorker' in navigator) {
        window.addEventListener('load', () => {
            navigator.serviceWorker
            .register('/service-worker.js')
            .then(() => {
                console.log('sw注册成功了~');
            })
            .catch(() => {
                console.log('sw注册失败了~');
            });
        });
        }

##### externals
        mode: 'production',
        externals: {
            // 拒绝jQuery被打包进来  要在html里手动引入链接
            //库名：‘包名’
            jquery: 'jQuery'
        }
##### dll
* 使用dll技术，对某些库（第三方库：jquery、react、vue...）进行单独打包  以后不用重复打包了
        当你运行 webpack 时，默认查找 webpack.config.js 配置文件
        需求：需要运行 webpack.dll.js 文件
        PS D:\woyu\webpack资料\代码\4.webpack优化环境配置\28.dll> webpack --config webpack.dll.js
        （先运行webpack.dll.js，生成jquery.js,然后正常用webpack）

        webpack.dll.js
        const { resolve } = require('path');
        const webpack = require('webpack');

        module.exports = {
        entry: {
            // 最终打包生成的name： --> jquery：
            // ['jquery'] --> 要打包的库是jquery
            jquery: ['jquery'],
        },
        output: {
            filename: '[name].js',
            path: resolve(__dirname, 'dll'),
            library: '[name]_[hash]' // 打包的库里面向外暴露出去的内容叫什么名字
        },
        plugins: [
            // 打包生成一个 manifest.json --> 提供和jquery映射
            new webpack.DllPlugin({
            name: '[name]_[hash]', // 映射库的暴露的内容名称
            path: resolve(__dirname, 'dll/manifest.json') // 输出文件路径
            })
        ],
        mode: 'production'
        };


        webpack.config.js
        const webpack = require('webpack');
        //PS D:\woyu\webpack资料\代码> npm i add-asset-html-webpack-plugin -D
        const AddAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin');
        plugins: [
            // 告诉webpack哪些库不参与打包，同时使用时的名称也得变
            new webpack.DllReferencePlugin({
            manifest: resolve(__dirname, 'dll/manifest.json')
            }),
            // 将某个文件打包输出去，并在html中自动引入该资源
            new AddAssetHtmlWebpackPlugin({
            filepath: resolve(__dirname, 'dll/jquery.js')
            })
        ]


#### 优化代码运行的性能
##### 缓存(hash-chunkhash-contenthash)
* 文件资源缓存  -->   让代码上线运行缓存更好使用

            文件资源缓存
            hash: 每次wepack构建时会生成一个唯一的hash值。
                问题: 因为js和css同时使用一个hash值。
                如果重新打包，会导致所有缓存失效。（可能我却只改动一个文件）
                output: {
			        filename: 'bundle.[hash:10].js',
			    },
				plugins: [
					new MiniCssExtractPlugin(
						filename: 'css/bundle.[hash:10].css'
					)
				]
            chunkhash：根据chunk生成的hash值。如果打包来源于同一个chunk，那么hash值就一样
                问题: js和css的hash值还是一样的
                因为css是在js中被引入的，所以同属于一个chunk
                output: {
				   filename: 'bundle.[chunkhash:10].js',
				},
				plugins: [
					new MiniCssExtractPlugin(
						filename: 'css/bundle.[chunkhash:10].css'
					)
				]
        √  contenthash: 根据文件的内容生成hash值。不同文件hash值一定不一样    
                output: {
				   filename: 'bundle.[contenthash:10].js',
				},
				plugins: [
					new MiniCssExtractPlugin(
						filename: 'css/bundle.[contenthash:10].css'
					)
				]
##### tree shaking
        tree shaking：去除无用代码
        前提：1. 必须使用ES6模块化  2. 开启production环境
        作用: 减少代码体积

        在package.json中配置 
        "sideEffects": false 所有代码都没有副作用（都可以进行tree shaking）
            问题：可能会把css / @babel/polyfill （副作用）文件干掉
        √"sideEffects": ["*.css", "*.less"]
##### code split
* 作用：将一个文件分割成多个文件，实现并行加载或者按需加载。

* 做法一（多入口）：不太灵活
		  // 入口文件
          // 多入口：有一个入口，最终输出就有一个bundle
		  entry: {
				index: './index.js',
				test: './test.js'
			},
		  // 输出文件
		  output: {
			// [name]：取文件名
		    filename: '[name].[contenthash:10].js',
		    path: resolve(__dirname, 'dist')
		  }

* 做法二：

	  ①
          // 入口文件
          // 单入口
		  entry: './index.js',
		  // 输出文件
		  output: {       
		    filename: '[name].[contenthash:10].js',
		    path: resolve(__dirname, 'dist')
		  },
		  /*
		  	可以将node_modules中代码单独打包一个chunk最终输出。  
		  */
		  optimization: {
			splitChunks: {
				chunks: 'all'
			}	
		  }
        ②
         /*
         自动分析多入口chunk中，有没有公共的文件（有大小要求，至少要有几十kb）。如果有会打包成单独的一个chunk。
        */
        // 入口文件
		  entry: {
				index: './index.js',
				test: './test.js'
			},
			// 输出文件
		  output: {       
		    filename: '[name].[contenthash:10].js',
		    path: resolve(__dirname, 'dist')
		  },
		  optimization: {
			splitChunks: {
				chunks: 'all'
			}	
		  }

* 做法三：
        
		/*
		通过js代码，让某个文件被单独打包成一个chunk
		import动态导入语法：能将某个文件单独打包
		*/
		index.js
		/* webpackChunkName: 'test' */将打包的名字设定为固定的名字
		import(/* webpackChunkName: 'test' */'./test')  //注释/**/里是要写的，不是省略的 
		.then(({ mul, count }) => {
		    // 文件加载成功~
		    // eslint-disable-next-line
		    console.log(mul(2, 5));
		})
		.catch(() => {
		    // eslint-disable-next-line
		    console.log('文件加载失败~');
		});
##### 懒加载/预加载（兼容性不好）

        index.js
        document.getElementById('btn').onclick = function() {
        // 懒加载：当文件需要使用时才加载  异步
        // 预加载 prefetch：会在使用之前，提前加载js文件 
        // 正常加载可以认为是并行加载（同一时间加载多个文件）  
        // 预加载 prefetch：等其他资源加载完毕，浏览器空闲了，再偷偷加载资源  慎用
        import(/* webpackChunkName: 'test', webpackPrefetch: true */'./test').then(({ mul }) => {
            console.log(mul(4, 5));
        });
        };

##### pwa

        PWA: 渐进式网络开发应用程序(离线可访问)
        workbox --> workbox-webpack-plugin
        PS D:\woyu\webpack资料\代码> npm i workbox-webpack-plugin -D 

        const WorkboxWebpackPlugin = require('workbox-webpack-plugin');

        plugins: [
            new WorkboxWebpackPlugin.GenerateSW({
            /*
                1. 帮助serviceworker快速启动
                2. 删除旧的 serviceworker

                生成一个 serviceworker 配置文件
            */
            clientsClaim: true,
            skipWaiting: true
            })
        ]
