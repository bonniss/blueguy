---
weight: 4
title: "Làm việc với filesystem"
---

# Làm việc với filesystem

Yeoman trang bị các hàm tiện ích cho filesystem, với ý tưởng là người dùng làm việc nhiều nhất với hai "ngữ cảnh". "Ngữ cảnh" là khái niệm chỉ các thư mục mà `generator` sẽ thường xuyên đọc/ghi file.

## Ngữ cảnh đích (Destination context)

"Ngữ cảnh đích" được có thể là:

- thư mục đang làm việc hiện tại.
- thư mục cha gần nhất có chứa file `.yo-rc.json`.

Lấy ngữ cảnh đích trong Yeoman:

Chẳng hạn, người dùng muốn sinh code trong thư mục `/var/apps/new-project` như sau:

```sh
cd /var/apps/new-project
yo generatorName
```

Cấu trúc code của `generator` như sau:

```txt
├───package.json
└───generators/
    └───app/
        └───templates/
        │   └───README.ejs
        └───index.js
```

Trong `generators/app/index.js`:

```js
class extends Generator {
  paths() {
    this.destinationRoot();
    // returns '/var/apps/new-project'

    this.destinationPath('README.md');
    // returns '/var/apps/new-project/README.md'
  }
}
```

## Ngữ cảnh biểu mẫu (Template context)

The template context is the folder in which you store your template files. It is usually the folder from which you’ll read and copy.

Ngữ cảnh biểu mẫu là thư mục chưa các biểu mẫu để sinh code. Mặc định thư mục này là `./templates`.

```js
class extends Generator {
  paths() {
    this.sourceRoot();
    // returns './templates'

    this.templatePath('README.ejs');
    // returns './templates/README.ejs'
  }
};
```

{{< hint info >}}
Thay đổi ngữ cảnh biểu mẫu bằng `this.sourceRoot('new/template/path')`.
{{< /hint >}}

Với cấu trúc thư mục như dưới đây thì ngữ cảnh biểu mẫu là `<project-root>/generators/app/templates`.

```txt
├───package.json
└───generators/
    └───app/
        └───templates/
        │   └───README.ejs
        └───index.js
```

## `this.fs` - filesystem trong bộ nhớ

Khi nói đến ghi đè file của người dùng, mọi thao tác ghi đều trải qua quá trình xử lý xung đột nội dung. Quá trình này yêu cầu người dùng phải xác nhận với từng file cách xử lý ghi đè nội dung thế nào. Sự thận trọng này giảm thiểu các rủi ro lỗi, tuy vậy việc ghi file lúc này là bất đồng bộ nên có thể khó sử dụng. Yeoman cung cấp một API filesystem đồng bộ mà ở đó mọi thay đổi với file được áp dụng trong bộ nhớ trước, và chỉ ghi lên đĩa cứng một khi Yeoman đã xử lý xong hết sau tầng `conflict`.

{{< hint info >}}
**Quan trọng**

Khi kết hợp nhiều `generator` lại, filesystem trong bộ nhớ này được dùng chung cho mọi generator.
{{< /hint >}}

API cho filesystem nằm ở `this.fs`, bản chất là một instance của [mem-fs editor](https://github.com/sboudrias/mem-fs-editor).

### Ghi template

Cho nội dung `./templates/index.html` như sau:

```html
<html>
  <head>
    <title><%= title %></title>
  </head>
</html>
```

Method `copyTpl` xử lý nội dung biểu mẫu và sao chép kết quả đến ngữ cảnh đích. `copyTpl` sử dụng [cú pháp biểu mẫu của `ejs`](http://ejs.co/):

```js
class extends Generator {
  writing() {
    this.fs.copyTpl(
      this.templatePath('index.html'),
      this.destinationPath('public/index.html'),
      { title: 'Templating with Yeoman' }
    );
  }
}
```

Kết quả ở `public/index.html`:

```html
<html>
  <head>
    <title>Templating with Yeoman</title>
  </head>
</html>
```

Minh họa đầy đủ hơn khi kết hợp với tầng `prompting`:

```js
class extends Generator {
  async prompting() {
    this.answers = await this.prompt([{
      type    : 'input',
      name    : 'title',
      message : 'Your project title',
    }]);
  }

  writing() {
    this.fs.copyTpl(
      this.templatePath('index.html'),
      this.destinationPath('public/index.html'),
      { title: this.answers.title } // user answer `title` used
    );
  }
}
```

### Ghi đè file

Ghi đè file là một tác vụ không đơn giản. Chiến thuật đáng tin cậy nhất là parse file thành cây cú pháp (AST) rồi mới sửa.

- [Cheerio](https://github.com/cheeriojs/cheerio) với file HTML.
- [Esprima](https://github.com/ariya/esprima) hoặc [AST-Query](https://github.com/SBoudrias/ast-query) (ít abstraction hơn) với JavaScript.
- Method của object JSON với file JSON.

## Tham khảo

- [[Yeoman] Filesystem](https://yeoman.io/authoring/file-system)
