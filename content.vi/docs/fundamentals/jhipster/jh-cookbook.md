---
weight: 4
title: "Bí kíp sinh code"
---

# Bí kíp sinh code

JHipster sẽ sinh code ở __thư mục hiện tại__, hãy nhớ tạo di chuyển đến đúng thư mục trước khi chạy lệnh.

```sh
mkdir myapp
cd myapp
```

## Trợ giúp

```sh
jhipster --help
```

## Tạo một ứng dụng bằng cách trả lời câu hỏi

```sh
jhipster
```

Từng cấu hình sẽ có câu hỏi tương ứng, khi người dùng trả lời xong hết thì code cho ứng dụng được sinh ra cùng một repo git, dependencies được cài đặt.

Nếu có sử dụng database, dữ liệu giả sẽ được sinh ra.

## Tạo một ứng dụng từ file JDL

Giả sử file JDL nằm cùng thư mục sinh code, sinh ứng dụng từ JDL như sau:

```sh
jhipster jdl ./myapp.jdl
```

Nếu bạn muốn thử nghiệm JDL mà không có sẵn file nào, sử dụng file mẫu từ repo [jdl-samples](https://github.com/jhipster/jdl-samples):

```sh
jhipster jdl bug-tracker.jdl
```

Xem hướng dẫn về JDL [tại đây](/docs/fundamentals/jhipster/jdl).

## Chạy test không ghi file

```sh
jhipster --dry-run
```

## Không tự động tạo repo git

```sh
jhipster --skip-git
```

## Chỉ sinh code phía client hoặc server

Chỉ sinh code server, _không sinh client_:

```sh
jhipster --skip-client
```

Chỉ sinh code client, _không sinh server_:

```sh
jhipster --skip-server
```

## Không tự động cài đặt dependencies

```sh
jhipster --skip-install
```

## Không sinh dữ liệu giả

```sh
jhipster --skip-fake-data
```

## Sinh lại code đã tồn tại

Sinh lại toàn bộ thực thể.

```sh
jhipster --with-entities
```

Do JHipster là một generator Yeoman, bạn có thể sử dụng thêm tùy chọn `--force` để sinh lại toàn bộ file, kể cả đã tồn tại.

```sh
jhipster --force
```

## Kịch bản kết hợp

> Kịch bản 1: Sinh code java bằng trả lời câu hỏi, không sinh dữ liệu giả, không sinh repo git.

```sh
jhipster \
--skip-client \
--skip-fake-data \
--skip-git
```

> Kịch bản 2: Sinh code phía client từ jdl mẫu [`21-points.jh`](https://github.com/jhipster/jdl-samples/blob/v8/21-points.jh), không tự động cài đặt dependencies.

```sh
jhipster \
jdl 21-points.jh \
--skip-server \
--skip-install \
```

## Tham khảo

- [[JHipster] Creating an application](https://www.jhipster.tech/creating-an-app)
