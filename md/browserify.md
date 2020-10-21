-- CommonJS 
  - Browserify：是浏览器端，称为js的打包工具
     - 使用： 使用CommonJS规范模块来暴露和引入模块
          即： 暴露  module.exports/exports
              引入  require(文件路径)，第三方直接写包名
     - 引入模块发生时间：
          - 在运行前对模块进行编译/转译/打包的处理（已经将依赖的模块包含进来了）
          - 运行的是打包生成的js,运行时不存在需要从远程引入依赖模块
     
     - 依赖的包
        - npm init -y 初始化
        - 全局: npm install browserify -g （在全局下载）
        - 局部: npm install browserify --save-dev   （下载并且是开发环境）  

   
        
      - 代码写完之后，要进行打包处理，使浏览器可以解析
        browserify js/src/app.js -o js/dist/bundle.js 
          解析： js/src/app.js --- 打包的文件      
                -o  ---  输出
                js/dist/bundle.js  --- 打包后的文件放到路径及文件