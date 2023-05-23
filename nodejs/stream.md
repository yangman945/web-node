# Stream(流)

流是用于Node.js中处理流数据的抽象接口

[文章链接](https://www.runoob.com/nodejs/nodejs-stream.html)

[官方文章链接](https://nodejs.cn/api/stream.html)

#### Stream四种类型

- **Readable** - 可读操作
- **Writable** - 可写操作
- **Duplex**- 可读可写操作
- **Transform** - 操作被写入数据，然后读出结果



#### Stream常用事件

- **data** - 当有数据可读时触发
- **end** - 没有更多的数据可读时触发
- **error** - 在接收和写入过程中发生错误时触发
- **finish** - 所有数据已被写入到底层系统时触发



#### stream常用流操作

##### 从流中读取数据

`createReadStream('fileName')-创建可读取的数据流`

```js
var fs = require("fs");
var data = '';

// 创建可读流
var readerStream = fs.createReadStream('input.txt');

// 设置编码为 utf8。
readerStream.setEncoding('UTF8');

// 处理流事件 --> data, end, and error
readerStream.on('data', function(chunk) {
   data += chunk;
});

readerStream.on('end',function(){
   console.log(data);
});

readerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```



##### 写入流

`createWriteStream('fileName')-创建可写入的流`

`write(data,encoding)-写入数据及编码类型`

```js
var fs = require("fs");
var data = '菜鸟教程官网地址：www.runoob.com';

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');

// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');

// 标记文件末尾 ？？？疑问：为什么要标记文件末尾
writerStream.end();

// 处理流事件 --> finish、error
writerStream.on('finish', function() {
    console.log("写入完成。");
});

writerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```



##### 管道流

读取A文件内容并将内容写入到B文件中（B原始文件会被覆盖）

`pipe('writeFileName')-将内容写入目标文件`

```js
var fs = require("fs");

// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');

// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');

// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);

console.log("程序执行完毕");
```



##### 链式流

通过链接输出流到另一个流，并创建多个流操作链的机制。链式流一般用于管道操作

`zlib.createGzip()-文件压缩`

`zlib.createGunzip()-文件解压`

```js
var fs = require("fs");
var zlib = require('zlib');

// 压缩 input.txt 文件为 input.txt.gz
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'));
  
console.log("文件压缩完成。");
```

```js
var fs = require("fs");
var zlib = require('zlib');

// 解压 input.txt.gz 文件为 input.txt
fs.createReadStream('input.txt.gz')
  .pipe(zlib.createGunzip())
  .pipe(fs.createWriteStream('input.txt'));
  
console.log("文件解压完成。");
```

