---
layout: post
title: "Mongoose快速指南"
date: 2015-10-17 00:09:53 +0800
comments: true
categories: 
---

首先确保安装了[mongodb](http://www.mongodb.org/downloads)和[nodejs](http://nodejs.org/)

安装Mongoose

```
$npm install mongoos

```

连接一个本地mongo数据库，数据库名为test

```
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');
```

我们可以对连接状态进行捕获并进行相应处理

```
var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function (callback) {
  // yay!
});
```
一旦数据库被连接，我们的回调就被调用

现在定义数据结构

```
var kittySchema = mongoose.Schema({
    name: String
});
```

定义了数据结构后，编译到我们的model中

```
var Kitten = mongoose.model('Kitten', kittySchema);
```

使用方法

```
var silence = new Kitten({ name: 'Silence' });
console.log(silence.name); 
```

模型中可以定义方法

```
kittySchema.methods.speak = function () {
  var greeting = this.name
    ? "Meow name is " + this.name
    : "I don't have a name";
  console.log(greeting);
}

var Kitten = mongoose.model('Kitten', kittySchema);

```
调用方法

```
var fluffy = new Kitten({ name: 'fluffy' });
fluffy.speak(); // "Meow name is fluffy"
```

保存数据

```
fluffy.save(function (err, fluffy) {
  if (err) return console.error(err);
  fluffy.speak();
});
```

查找数据

```
Kitten.find(function (err, kittens) {
  if (err) return console.error(err);
  console.log(kittens);
})
```

查找符合条件的数据

```
Kitten.find({ name: /^Fluff/ }, callback);
```

