---
weight: 2
bookFlatSection: false
bookCollapseSection: true
title: "JHipster v7"
---

## Cài đặt

Theo [hướng dẫn chính thức từ trang chủ](https://www.jhipster.tech/installation/), để làm việc với JHipster có 3 cách:

1. Cho người dùng muốn thử nghiệm: [JHipster online](https://start.jhipster.tech/) là một trang dựng cấu hình low-code kiểu như [Spring initializr](https://start.spring.io/).
1. Cho người dùng phổ thông: cài đặt môi trường ở máy cá nhân
1. Cho người dùng nâng cao: cài đặt môi trường bằng Docker

### 1. JHipster online

JHipster online chỉ giúp sinh code cho ứng dụng bằng một giao diện "thân thiện với người mới bắt đầu", sau đó _để chạy ứng dụng bạn vẫn phải làm theo một trong hai cách sau đây_.

### 2. Cài đặt môi trường ở máy cá nhân (khuyến nghị)

Người đọc tài liệu này sẽ làm việc với blueprint, do đó ta dùng cách 2.

1. Cài Java 11 [Eclipse Temurin builds](https://adoptium.net/temurin/releases/?version=11)
1. Cài Node.js LTS >= 14
1. Cài JHipster: `npm install -g generator-jhipster@7`
1. Cài Yeoman: `npm install -g yo`

Kiểm tra cài đặt.

```sh
$ java -version
openjdk version "11.0.20.1" 2023-08-24
OpenJDK Runtime Environment Temurin-11.0.20.1+1 (build 11.0.20.1+1)
OpenJDK Client VM Temurin-11.0.20.1+1 (build 11.0.20.1+1, mixed mode)

$ jhipster --version
7.9.4

$ yo --version
4.3.1
```

### 3. Sử dụng Docker (Cho người dùng nâng cao)

Chuẩn bị:

1. [Docker desktop (khuyến nghị)](https://docs.docker.com/desktop/)
2. [Docker Engine](https://docs.docker.com/engine/install/)

Sau đó làm theo [hướng dẫn](https://www.jhipster.tech/installation/#docker-installation-for-advanced-users-only).

Docker image chính thức của JHipster [tại đây](https://hub.docker.com/r/jhipster/jhipster/).
