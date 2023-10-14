---
weight: 1
bookCollapseSection: true
title: "Yeoman"
---

# Giới thiệu Yeoman

[Yeoman](https://yeoman.io/) là thư viện chuyên dụng để dựng công cụ sinh code. Yeoman tự mô tả mình là ["THE WEB'S SCAFFOLDING TOOL FOR MODERN WEBAPPS"](https://yeoman.io/).

Yeoman giúp khởi tạo nhanh chóng project thông qua một hoặc kết hợp nhiều bộ sinh code (`generator`).

## Cài đặt

Sử dụng Yeoman trong CLI bằng lệnh `yo`.

```sh
npm install -g yo
```

Sau khi cài đặt bạn có thể gọi lệnh `yo` ở bất cứ đâu trong terminal. Kiểm tra version của Yeoman để xác nhận cài đặt thành công.

```sh
$ yo --version
4.3.1
```

## `generator`

`generator` là thuật ngữ trung tâm trong Yeoman.

Về cơ bản `generator` là một plugin có thể chạy được bằng lệnh `yo` để lên khung code. Yeoman cho phép tạo khung hoàn chỉnh cho toàn bộ project chỉ với 1 `generator`, hoặc kết hợp nhiều `generator`, mỗi cái đảm nhiệm sinh một phần project.

Trước khi tự xây dựng `generator` cho riêng mình, bạn nên thử tìm kiếm nhu cầu trong [hệ sinh thái `generator` của Yeoman](https://yeoman.io/generators/). Là một package Node.js kỳ cựu với hơn 10 năm tuổi đời, cộng đồng Yeoman đóng góp tạo nên bộ sưu tập đa dạng công cụ áp dụng cho nhiều công nghệ và mục đích. Ở thời điểm viết bài, JHipster là [generator phổ biến nhất của Yeoman](https://yeoman.io/generators/).

Một số use case tiêu biểu:

- Lên khung nhanh cho toàn bộ project (ví dụ: jhipster, [loopback](https://github.com/strongloop/generator-loopback))
- Tạo một phần tử trong hệ thống, như một controller mới đi kèm unit test, hoặc một service (ví dụ: [angular-module](https://github.com/jackrabbitsgroup/generator-angular-module))
- Tạo module hoặc package (ví dụ: [node](https://github.com/yeoman/generator-node))
- Áp dụng code standard, style guide, best practice (ví dụ: [eslint](https://github.com/eslint/generator-eslint))
- Sample app cho user dùng thử (ví dụ [simple-crud-generator](https://github.com/paflopes/crud-generator))

## Quy ước tên gọi

Yeoman `generator` phải có tên repo bắt đầu bằng `generator-`. Đó là lý do vì sao bộ sinh code của Jhipster có tên repo là [generator-jhipster](https://github.com/jhipster/generator-jhipster).

Khi gọi `generator` bằng `yo`, chỉ phần tên phía sau `generator-` được tính đến. Ví dụ tên là [`generator-office`](https://github.com/officedev/generator-office) thì để sinh code chạy:

```sh
yo office [arguments] [options]
```

## Các lệnh thường dùng

```sh
yo --help # Màn hình trợ giúp
yo --generators # Danh sách các generator đã cài
yo doctor # Kiểm tra "sức khỏe" của Yeoman ở máy hiện tại
```
