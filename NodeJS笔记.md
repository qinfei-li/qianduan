### 命令行窗口(小黑屏)、CMD窗口、终端、shell
开始菜单 --> 运行 --> CMD --> 回车
* 常用的指令：
	* dir 列出当前目录下的所有文件
	* cd 目录名 进入到指定的目录
	* md 目录名 创建一个文件夹
	* rd 目录名 删除一个文件夹	
* 目录
	* . 表示当前目录
	* .. 表示上一级目录	
* 环境变量（windows系统中变量）	
	* path
 	* C:\work\jdk\jdk1.7.0_75/bin;
 	* %CATALINA_HOME%/bin;
 	* C:\work\soft\tools\AppServ\Apache24\bin;
 	* C:\work\soft\tools\AppServ\php5;
 	* C:\Users\lilichao\AppData\Local\Programs\Fiddler;
 	* C:\work\environment\Egret\Egret Wing 3\bin;
 	* C:\Users\lilichao\AppData\Roaming\npm;
 	* C:\Program Files\MongoDB\Server\3.2\bin;
 	* C:\Users\lilichao\Desktop\hello
当我们在命令行窗口打开一个文件，或调用一个程序时，系统会首先在当前目录下寻找文件程序，如果找到了则直接打开,如果没有找到则会依次到环境变量path的路径中寻找，直到找到为止
如果没找到则报错
所以我们可以将一些经常需要访问的程序和文件的路径添加到path中，这样我们就可以在任意位置来访问这些文件和程序了
			
### I/O (Input/Output)
I/O操作指的是对磁盘的读写操作
	
### Node
 Node是对ES标准一个实现，Node也是一个JS引擎
 通过Node可以使js代码在服务器端执行
 Node仅仅对ES标准进行了实现，所以在Node中不包含DOM 和 BOM	
 Node中可以使用所有的内建对象
	* String Number Boolean Math Date RegExp Function Object Array
 而BOM和DOM都不能使用
 但是可以使用 console 也可以使用定时器（setTimeout() setInterval()）
 Node可以在后台来编写服务器
 Node编写服务器都是单线程的服务器
* 进程
	* 进程就是一个一个的工作计划（工厂中的车间）
* 线程
	* 线程是计算机最小的运算单位（工厂中的工人）
	* 线程是干活的
				
 传统的服务器都是多线程的
 每进来一个请求，就创建一个线程去处理请求
		
 Node的服务器单线程的
 Node处理请求时是单线程，但是在后台拥有一个I/O线程池
	
 node就是一款使用js编写的web服务器
 node底层是使用c++的编写的
 node的中js引擎使用的chrome的v8引擎
 node的特点：
  1.非阻塞、异步的I/O
  2.事件和回调函数
  3.单线程（主线程单线程，后台I/O线程池）
  4.跨平台
 * 运行hello.js
	* cmd打开
	* node hello.js

### 模块化
 ES5中没有原生支持模块化，我们只能通过script标签引入js文件来实现模块化
 在node中为了对模块管理，引入了CommonJS规范
* 模块的引用
	* 使用 require()函数来引入一个模块
 	* 例子：
		·var 变量 = require("模块的标识");·	
* 模块的定义
 * 在node中一个js文件就是一个模块
 * 默认情况下在js文件中编写的内容，都是运行在一个独立的函数中，外部的模块无法访问
* 导出变量和函数
  * * 使用 exports 
      * 例子：
       * exports.属性 = 属性值;
       * exports.方法 = function(){};
       * exports只能使用.的方式来向外暴露内部变量
  * * 使用module.exports
      * 例子：
       * module.exports.属性 = 属性值;
       * module.exports.方法 = 函数;
       * module.exports = {};
       * module.exports既可以通过.的形式，也可以直接赋值
* 在node中有一个全局对象 global，它的作用和网页中window类似
   * 在全局中创建的变量都会作为global的属性保存
   * 在全局中创建的函数都会作为global的方法保存
* 当node在执行模块中的代码时，它会首先在代码的最顶部，添加:
 * function (exports, require, module, __filename, __dirname) {
	 在代码的最底部，添加:
}
* 实际上模块中的代码都是包装在一个函数中执行的，并且在函数执行时，同时传递进了5个实参
 * exports
  * 该对象用来将变量或函数暴露到外部
 * require
  * 函数，用来引入外部的模块
 * module
  * module代表的是当前模块本身
  * exports就是module的属性
  *  既可以使用 exports 导出，也可以使用module.exports导出
 * __filename
  * C:\Users\lilichao\WebstormProjects\class0705\01.node\04.module.js
  * 当前模块的完整路径
 * __dirname
  * C:\Users\lilichao\WebstormProjects\class0705\01.node
  * 当前模块所在文件夹的完整路径
* 模块的标识
 *  模块的标识就是模块的名字或路径
 * 我们node通过模块的标识来寻找模块的
 * 对于核心模块（npm中下载的模块），直接使用模块的名字对其进行引入
  * var fs = require("fs");
  * var express = require("express");						
 * 对于自定义的文件模块，需要通过文件的路径来对模块进行引入
 * 路径可以是绝对路径，如果是相对路径必须以./或 ../开头
  * var router = require("./router");

###  包（package）
*  将多个模块组合为一个完整的功能，就是一个包
*  包结构
	* bin
		*  二进制的可执行文件，一般都是一些工具包中才有
	* lib
		*  js文件
	* doc
		*  文档
	* test
		*  测试代码
	* package.json
		*  包的描述文件
				
	* package.json	
		*  它是一个json格式的文件，在它里面保存了包各种相关的信息
			* name 包名
			* version 版本
			* dependencies 依赖
			* main 包的主要的文件
			* bin 可执行文件
				
### npm（Node Package Manager node的包管理器）
* 通过npm可以对node中的包进行上传、下载、搜索等操作
* npm会在安装完node以后，自动安装
* npm的常用指令
	* npm -v 查看npm的版本
	* npm version 查看所有模块的版本
	* npm init 初始化项目（创建package.json）
	* npm i/install 包名 安装指定的包
	* npm i/install 包名 --save 安装指定的包并添加依赖
	* npm i/install 包名 -g 全局安装（一般都是一些工具）
	* npm i/install 安装当前项目所依赖的包
	* npm s/search 包名 搜索包	
	* npm r/remove 包名 删除一个包
			
### 文件系统（File System）
* Buffer（缓冲区）
	* Buffer和数组的结构的非常类似，Buffer是用来存储二进制数据的
	* buffer中的一个元素，占用内存的一个字节，buffer中每一个元素的范围是从00 - ff
	* Buffer的大小一旦确定，则不能修改，Buffer实际上是对底层内存的直接操作
	* Buffer的方法
		* Buffer.from(字符串)
			* 将一个字符串中内容保存到一个buffer中
		* buf.toString()
			* 将buffer转换为一个字符串
		* Buffer.alloc(size)
			* 创建一个指定大小的buffer对象
		* Buffer.allocUnsafe(size)
			* 创建一个指定大小的buffer对象，可以包含敏感数据				
* 文件系统简单来说就是通过Node来操作系统中的文件
	* 使用文件系统，需要先引入fs模块，fs是核心模块			
* fs模块
	* 引入fs
	* var fs = require("fs");
* fs模块中的大部分操作都提供了两种方法：同步方法和异步方法
	* 同步方法带sync
	* 异步方法没有sync，都需要回调函数
* 写入文件
	 * 1.同步写入
	 * 2.异步写入√
	 * 3.简单写入
	 * 4.流式写入		
* 读取文件
	 * 1.同步读取
	 * 2.异步读取
	 * 3.简单读取
	 * 4.流式读取
* 方法
	* 打开文件
		 * fs.open(path, flags[, mode], callback)
			* 回调函数两个参数：
				* err 错误对象，如果没有错误则为null
				* fd  文件的描述符
		 * fs.openSync(path, flags[, mode])
				* path 要打开文件的路径
				* flags 打开文件要做的操作的类型
				 * r 只读的
				 * w 可写的
				* mode 设置文件的操作权限，一般不传
			* 返回值：
				* 该方法会返回一个文件的描述符作为结果，我们可以通过该描述符来对文件进行各种操作
	* 读写文件
		 * fs.write(fd, string[, position[, encoding]], callback)
		 * fs.writeSync(fd, string[, position[, encoding]])
				* fd 文件的描述符，需要传递要写入的文件的描述符
 				* string 要写入的内容
 				* position 写入的起始位置
 				* encoding 写入的编码，默认utf-8
		 * fs.read(fd, buffer, offset, length, position, callback)
		 * fs.readSync(fd, buffer, offset, length, position)
				* path 要读取的文件的路径
				* options 读取的选项
				* callback回调函数，通过回调函数将读取到内容返回(err , data)
				* err 错误对象
				* data 读取到的数据，会返回一个Buffer		
	* 保存并关闭文件
		 * fs.close(fd,callback)
		 * fs.closeSync(fd);		
	* 简单文件读取和写入
		 * fs.writeFile(file, data[, options], callback)
		 * fs.writeFileSync(file, data[, options])
				* file 要操作的文件的路径
				* data 要写入的数据
				* options 选项，可以对写入进行一些设置
				* callback 当写入完成以后执行的函数
				* flag
				 * r 只读
				 * w 可写
				 * a 追加		
 例
 fs.writeFile("C:/Users/lilichao/Desktop/hello.txt","这是通过writeFile写入的内容",{flag:"w"} , function (err) {
	if(!err){		
		console.log("写入成功~~~");
	}else{
		console.log(err);
	}
});
  fs.readFile(path[, options], callback)
  fs.readFileSync(path[, options])  					
	* 流式文件读取和写入
		* 流式读取和写入适用于一些比较大的文件，可以分多次将文件读取到内存中
				
				fs.createWriteStream(path[, options])
					* 可以用来创建一个可写流
					* path，文件路径
					* options 配置的参数

				    * 通过监听流的open和close事件来监听流的打开和关闭
					* on(事件字符串,回调函数)
						* 可以为对象绑定一个事件
					* once(事件字符串,回调函数)
						* 可以为对象绑定一个一次性的事件，该事件将会在触发一次以后自动失效

				* fs.createReadStream(path[, options])
				 	* 可以用来创建一个可写流
			        * 如果要读取一个可读流中的数据，必须要为可读流绑定一个data事件，data事件绑定完毕，它会自动开始读取数据
			        * 通过监听流的open和close事件来监听流的打开和关闭

				* pipe()可以将可读流中的内容，直接输出到可写流中
						* rs.pipe(ws);
			
			
			
			
			
			
			
			
						
			
			
			
			
		
		
		
	
	
	
	
	
	
	
	
		
			
		
