---
weight: 3
bookCollapseSection: true
title: "JHipster"
---

# Giới thiệu JHipster

## JHipster là gì

- __[JHipster](https://www.jhipster.tech/)__ là nền tảng sinh code, phát triển, và triển khai chuyên dụng cho ứng dụng web.
- JHipster có thể sinh toàn bộ code cho ứng dụng dựa trên cấu hình cho cả server và client, cả web và mobile.
  - Server: __Spring Boot__, Micronaut, Quarkus, Node.js, .NET
  - Client: __Angular__, __React__, __Vue__
  - Mobile: Ionic, React Native
- JHipster hỗ trợ cả kiến trúc microservice và monolith.
- JHipster nhắm tới triển khai ứng dụng lên cloud với Docker và Kubernetes.
  - Các nền tảng cloud: AWS, Azure, Cloud Foundry, Google Cloud Platform, Heroku, OpenShift.

Slide giới thiệu JHipster:

<div class="text-center">
<iframe src="https://www.jhipster.tech/presentation/#/" style="border:0px #ffffff none;" name="myiFrame" scrolling="no" frameborder="1" marginheight="0px" marginwidth="0px" height="450px" width="100%" allowfullscreen></iframe>
</div>

- [Các công ty sử dụng JHipster](https://www.jhipster.tech/companies-using-jhipster/)
- [Showcase](https://www.jhipster.tech/showcase/)

## Cài đặt JHipster

Theo [hướng dẫn chính thức từ trang chủ](https://www.jhipster.tech/installation/), để làm việc với JHipster có 3 cách:

1. Cho người dùng muốn thử nghiệm: [JHipster online](https://start.jhipster.tech/) là một trang dựng cấu hình low-code kiểu như [Spring initializr](https://start.spring.io/).
1. Cho người dùng phổ thông: cài đặt môi trường ở máy cá nhân
1. Cho người dùng nâng cao: cài đặt môi trường bằng Docker

Người đọc tài liệu này sẽ làm việc với blueprint, do đó ta dùng cách 2.

1. Cài Java 11 [Eclipse Temurin builds](https://adoptium.net/temurin/releases/?version=11)
1. Cài Node.js LTS >= 14
1. Cài JHipster: `npm install -g generator-jhipster`
1. Cài Yeoman: `npm install -g yo`

Kiểm tra cài đặt.

```sh
$ java -version
openjdk version "11.0.20.1" 2023-08-24
OpenJDK Runtime Environment Temurin-11.0.20.1+1 (build 11.0.20.1+1)
OpenJDK Client VM Temurin-11.0.20.1+1 (build 11.0.20.1+1, mixed mode)

$ jhipster --version
INFO! Using bundled JHipster
7.9.4

$ yo --version
4.3.1
```
