---
weight: 1
title: "Generator từ cộng đồng"
---

# Generator từ cộng đồng

Bài này hướng dẫn sử dụng Yeoman để sinh code bằng một `generator` có sẵn từ hệ sinh thái Yeoman.

## Cài đặt Yeoman

Sử dụng Yeoman trong CLI bằng lệnh `yo`.

```sh
npm install -g yo
```

Sau khi cài đặt bạn có thể gọi lệnh `yo` ở bất cứ đâu trong terminal. Kiểm tra version của Yeoman để xác nhận cài đặt thành công.

```sh
$ yo --version
4.3.1
```

## Cài đặt generator

Bài viết này sử dụng [`generator-license`](https://github.com/jozefizso/generator-license) để sinh file license cho app của bạn.

Cài đặt `generator-license` như sau:

```sh
npm install -g generator-license
```

## Sinh code bằng generator

Để sinh code bằng 1 generator nào đó, ta gọi:

```sh
yo <generator_name>
```

Trong đó `<generator_name>` thường là phần tên bỏ qua tiếp đầu ngữ `generator-`. Áp dụng với `generator-license`, lệnh như sau:

```sh
yo license
```

Trả lời các câu hỏi.

![](/yeoman/generator-license.png)

Kết quả là một file `LICENSE` được sinh ra ở thư mục hiện tại.
