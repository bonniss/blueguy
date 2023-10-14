---
weight: 2
title: "Tự viết generator"
---

# Tự viết generator

Khi không có `generator` cộng đồng nào đáp ứng được nhu cầu, hãy tự viết một `generator` để đáp ứng tốt nhất workflow của bạn.

## Lên khung code cho generator mới

`generator` về bản chất là một package Node.js. Vì thế để tạo  `generator`, bạn có thể tạo một package Node.js từ đầu hoặc sinh khung code tự động với trợ giúp của Yeoman.

{{< hint info >}}
Yeoman cung cấp `generator-generator` để lên khung code cho một... `generator` mới.

Cài đặt với scope global:

```sh
npm i -g generator-generator
```

Lên khung code cho `generator` mới:

```sh
yo generator
```

Sau khi trả lời các câu hỏi, một thư mục được tạo ra mới tên `generator-name` (trong đó `name` là tên của generator). Yeoman sẽ dựa trên `name` để định vị `generator`.

![](/yeoman/gen-gen.png)

{{< /hint >}}

Dù tạo theo cách nào thì khung code vẫn tuân thủ theo quy định về kiến trúc của Yeoman.

## Kiến trúc generator

Ta sẽ tạo một generator từ đầu, và diễn giải từng thành phần tạo nên một Yeoman generator.

### Tạo thư mục gốc

Tạo một thư mục với tên `generator-name` (trong đó `name` là tên của generator). Yeoman sẽ dựa trên `name` để định vị `generator`.

### Tạo `package.json`

```json
{
  "name": "generator-name",
  "version": "0.1.0",
  "description": "",
  "files": [
    "generators"
  ],
  "keywords": ["yeoman-generator"],
  "dependencies": {
    "yeoman-generator": "latest"
  }
}
```

`files` phải là một mảng chứa các file và thư mục sẽ được dùng bởi generator.

Kể cả khi đã cài `yo` toàn cục, bạn vẫn phải đảm bảo phiên ban mới nhất của `yeoman-generator` xuất hiện trong `dependencies`.

```sh
npm install yeoman-generator
```

### Cây thư mục

Ta khai báo `files` trong `package.json` chứa nội dung của thư mục `generators`. Giả sử ta có cấu trúc thư mục như sau:

```txt
├───package.json
└───generators/
    ├───app/
    │   └───index.js
    └───router/
        └───index.js
```

Yeoman căn cứ theo thư mục này tạo ra các lệnh sau:

```sh
yo name  # -> generators/app, đầu vào mặc định của generator
yo name:router  # -> generators/router
```

Bạn tùy ý thay đổi cấu trúc thư mục như ý, chỉ nhớ là hãy cấu hình `files` trong `package.json` tương ứng:

```txt
├───package.json
├───app/
│   └───index.js
└───router/
    └───index.js
```

```json
// package.json
{
  "files": [
    "app",
    "router"
  ]
}
```

### Logic chính

Yeoman offers a base generator which you can extend to implement your own behavior. This base generator will add most of the functionalities you’d expect to ease your task.

Yeoman cung cấp sẵn một class có vai trò base generator, có sẵn hầu hết các tính năng mà bạn có thể mở rộng để thực thi các task thường gặp.

```js
// generators/app.js
var Generator = require('yeoman-generator');

module.exports = class extends Generator {
  // The name `constructor` is important here
  constructor(args, opts) {
    // Calling the super constructor is important so our generator is correctly set up
    super(args, opts);

    // Next, add your custom code
    this.option('babel'); // This method adds support for a `--babel` flag
  }

  method1() {
    this.log('method 1 just ran');
  }

  method2() {
    this.log('method 2 just ran');
  }
};
```

### Chạy generator

Tại thư mục gốc của generator:

```sh
npm link
```

Lệnh này sẽ cài đặt dependecies rồi tạo 1 symlink từ global packages đến generator hiện tại. Lúc này bạn có thể gọi generator như một global package:

```sh
$ yo name
method 1 just ran
method 2 just ran
```

Tada! Bạn đã viết xong generator đầu tiên.

## Tham khảo

- [[Yeoman docs] Writing Your Own Yeoman Generator](https://yeoman.io/authoring/)
