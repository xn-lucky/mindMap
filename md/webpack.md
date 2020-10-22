-- webpack
    -- Webpack是一个模块打包器(bundler)。
    -- 五个核心概念
       -- Entry：入口起点(entry point)指示 ,通俗来说就是想要构建的文件。
       -- Output：output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。
       -- Loader：loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只能解析 JavaScript/JSON）。（详细了解看md文件笔记）
       -- Plugins：插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量等。（详细了解看md文件笔记）
       -- Mode：模式，有生产模式production和开发模式development(5以上版本没有这个核心概念)



   -- 启动项目
         - npm init -y
         - 安装
           * npm install webpack@4.44.2 webpack-cli@4.0.0 -g  //全局安装,作为指令使用
           * npm install webpack@4.44.2  webpack-cli@4.0.0  -D //本地安装,作为本地依赖使用
         - 运行
           * 开发配置指令：webpack ./src/js/app.js -o ./dist/js --mode=development
              --功能：webpack能够编译打包js和json文件，并且能将es6的模块化语法转换成浏览器能识别的语法
           * 生产配置指令：webpack ./src/js/app.js -o ./dist/js --mode=production
               --功能: 在开发配置功能上加上一个压缩代码

            不写默认是开发环境
        - 结论：
           - webpack能够编译打包js和json文件
           - 能将es6的模块化语法转换成浏览器能识别的语法
           - 能压缩代码
        - 缺点：
           - 不能编译打包css、img等文件
           - 不能将js的es6基本语法转化为es5以下语法

        改善：使用webpack配置文件解决，自定义功能
   -- 配置文件：
       目的：在项目根目录定义配置文件，通过自定义配置文件，还原以上功能，文件名称webpack.config.js
       (详细看md文件笔记)


   