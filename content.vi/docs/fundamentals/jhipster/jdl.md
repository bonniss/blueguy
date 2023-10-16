---
weight: 2
title: "JDL"
---

# JDL

"JHipster Domain Language" hay "JDL" là ngôn ngữ mô tả chuyên dụng (DSL) của JHipster để mô tả:

- Cấu hình ứng dụng - Application
- Thực thể và quan hệ - Entities
- Cấu hình triển khai - Deployment

tất cả chỉ trong một file, có đuôi `.jdl` hoặc `.jh`.

Xem chi tiết [ngôn ngữ trên trang chủ của JHipster](https://www.jhipster.tech/jdl/intro)

## Tại sao nên dùng JDL?

Xét nội dung JDL sau:

```jdl
/*
* Application config
*/
application {
  config {
    baseName maybank
    applicationType monolith
    packageName com.maybank
    authenticationType jwt
    prodDatabaseType postgresql
    clientFramework react
    buildTool gradle
    languages [en, vi]
    nativeLanguage vi
  }
  entities *
}

/*
* Constants
*/
MIN_LENGTH_NAME_DEFAULT = 3
MAX_LENGTH_NAME_DEFAULT = 256
MAX_LENGTH_DESC_DEFAULT = 65535
MAX_LENGTH_NAME_PARAM = 512

/*
* Enums
*/
enum LoginStatus {
  SUCCESS, SESSION_EXPIRED, FAILED
}

enum Gender {
  Male, Female
}

enum Status {
  // 0. Chưa duyệt
  PENDING_NEW ("0")
  // 1. Đã duyệt
  APPROVED_NEW ("1")
  // 2. Sửa chờ duyệt
  PENDING_EDIT ("2")
  // 3. Đã sửa  (sửa đã duyệt)
  APPROVED_EDIT ("3")
  // 4. Xóa chờ duyệt
  PENDING_DELETE ("4")
  // 9. Đã xóa
  APPROVED_DELETE ("9")
}

/*
* Entities
*/
entity Channel {
  id Long
  name String required minlength(MIN_LENGTH_NAME_DEFAULT) maxlength(MAX_LENGTH_NAME_DEFAULT) unique
  desc String maxlength(MAX_LENGTH_DESC_DEFAULT)
  status Status
}

entity LoginEvent {
  id Long
  loginStatus LoginStatus
  status Status
}

entity Customer {
  code UUID
  gender Gender
  status Status
  refId Long
}

relationship OneToOne {
  LoginEvent{channel} to Channel
  LoginEvent{customer} to Customer
  Customer{user} to User
}

/*
* Options
*/
paginate all with pagination
filter all
dto * with mapstruct
```

Các phần đều có comment (giống comment Java) về loại cấu hình. Cú pháp JDL đơn giản, dễ đọc, dễ hiểu, dễ chỉnh sửa. Ta có thể thấy các entity trong hệ thống là `Channel`, `LoginEvent`, `Customer` và mối quan hệ giữa chúng, app được xác thực theo JWT, client viết bằng React, code Java build bằng Gradle.

Khi đã quen với cú pháp JDL thì việc tạo và sửa cấu hình hệ thống trở nên dễ dàng và nhanh chóng hơn so với tương tác prompting trả lời từng câu hỏi một.

Kể cả sinh code bằng prompting hay qua JDL thì JHipster, một generator Yeoman, đều sinh ra một file `.yo-rc.json` để lưu lại toàn bộ cấu hình. So với `yo-rc.json`, nội dung JDL đầy đủ và dễ đọc.

Tài liệu này đi theo hướng sinh code thông qua cấu hình JDL.

## Hệ sinh thái JDL

### JDL mẫu

JHipster cung cấp repo [jdl-samples](https://github.com/jhipster/jdl-samples) có sẵn các file JDL mẫu từ monolith đến microservice. Người dùng có thể sinh code từ JDL mẫu bằng cách gọi đúng tên file mà không cần tải file về. JHipster nếu không tìm thấy file ở thư mục hiện tại sẽ tự động tìm và tải về từ [jdl-samples](https://github.com/jhipster/jdl-samples).

```sh
$ jhipster jdl 21-points.jh
INFO! File not found: 21-points.jh. Attempting download from jdl-samples repository
INFO! Downloading file: https://raw.githubusercontent.com/jhipster/jdl-samples/v7.9.4/21-points.jh
INFO! Error downloading https://raw.githubusercontent.com/jhipster/jdl-samples/v7.9.4/21-points.jh: 404 - Not Found
INFO! Downloading file: https://raw.githubusercontent.com/jhipster/jdl-samples/main/21-points.jh
# etc
```

### IDE Plugin

JHipster có hỗ trợ JDL trên các IDE sau:

- [IntelliJ](https://plugins.jetbrains.com/plugin/19697-jhipster-jdl)
- [Eclipse](https://marketplace.eclipse.org/content/jhipster-ide)
- [VS Code](https://marketplace.visualstudio.com/items?itemName=jhipster-ide.jdl)

### JDL studio

[JDL studio](https://start.jhipster.tech/jdl-studio/) là công cụ trực tuyến giúp tạo thực thể và quan hệ sử dụng JDL. Dán đoạn code JDL trong bài này vào studio và UML của hệ thống sẽ xuất hiện. Công cụ còn cho phép import và export file JDL.

![](/jhipster/jdl-studio.png)

## Tham khảo

- [[JHipster] JDL intro](https://www.jhipster.tech/jdl/intro)