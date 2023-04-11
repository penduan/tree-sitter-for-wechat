# tree-sitter-for-wechat

此项目是将tree-sitter移植到微信小程序环境.

问题:
由于微信小程序的特殊环境,无法通过简单适配使用`web-tree-sitter(npm包)`.并且由于`emscripten`动态链接解决方案使用了小程序不可用的WebAssembly对象(.Module)和Proxy的访问器特性.

简单构想了两种解决方案:
1. 添加适配/重构Emscripten的动态加载功能.
2. 添加自己的移植方案,将`tree-sitter/tree-sitter-*`所需的包集成在一个静态wasm包中,再通过`wasm-split`进行拆分.

## 解决方案1

测试可行程度: __>80%__

- 适配WebAssembly.Module.customSections功能 -> [x]()
  customSections方法: 主动在Module中查找`dylink.0/dylink`列所在位置.
  由于小程序没有Module模块,无法直接实现,需要先在支持的环境中加载一次获取到真实的相应的位置
- 适配加载`SIDE_MODULE`模块时的Import对象
  使用`wasm_objdmp`打印出每个副模块需要的`import`表(用在Proxy polyfill中,替换Proxy对象)
- `SIDE_MODULE`加载
  `SIDE_MODULE`在加载时会被Emscripten检查(模块文件/Buffer)和加载预设表

## 解决方案2

测试可行程序: __100%__

使用`C/C++/Rust`将tree-sitter核心库、高亮库、语言库捆绑在一起呈现。



  


  







