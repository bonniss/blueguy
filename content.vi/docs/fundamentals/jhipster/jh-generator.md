---
weight: 1
title: "Kiến trúc bộ sinh code"
---

# Kiến trúc bộ sinh code

## Giới thiệu

Trái tim của JHipster là một công cụ sinh code (CCSC) [mã nguồn mở](https://github.com/jhipster/generator-jhipster). CCSC này được viết bằng [Yeoman]((https://yeoman.io/)), nói cách khác, JHipster chính là một generator [Yeoman]((https://yeoman.io/)).

Dễ hiểu là tên repo tuân thủ theo quy ước của Yeoman là `generator-jhipster` chứ không đơn thuần là `jhipster`.

{{< hint info >}}
[Tìm hiểu về Yeoman tại đây](/docs/fundamentals/yeoman) nếu bạn chưa biết.
{{< /hint >}}

{{< hint warning >}}
Kể từ đây tên gọi "JHipster" trong bài này được hiểu là nói tới "CCSC của JHipster".
{{< /hint >}}

## Kiến trúc

Như đã biết, Yeoman cho phép kết hợp (composability) nhiều `generator` con (sub-generator) thành một `generator` cha hoàn chỉnh. JHipster hỗ trợ rất nhiều công nghệ ở mọi tầng trong techstack, nếu dồn tất cả logic chỉ trong 1 generator thì sẽ khó tổ chức và bảo trì bởi rẽ nhánh nhiều. Vì thế JHipster giống như một tòa nhà được ghép nối bởi các khối sub-generator theo từng chức năng chuyên biệt.

Khảo sát [cây thư mục `generators` trong mã nguồn JHipster ở phiên bản 7.9.4](https://github.com/jhipster/generator-jhipster/tree/v7.9.4/generators), mỗi một nhánh trong cây chính là một sub-generator có tên gọi ít nhiều gợi mở về tính năng của chúng, ví dụ như:

- `app` là thư mục chính, "lối vào tòa nhà" theo quy ước của Yeoman
- `server` đảm nhiệm sinh code phía server
- `client` đảm nhiệm sinh code phía client
- `heroku` đảm nhiệm sinh code triển khai lên Heroku

```txt
generator-jhipster
└───generators
    ├───add
    ├───app
    ├───aws
    ├───azure-app-service
    ├───azure-spring-cloud
    ├───base
    ├───bootstrap
    ├───bootstrap-application
    ├───ci-cd
    ├───client
    ├───cloudfoundry
    ├───common
    ├───cypress
    ├───database-changelog
    ├───database-changelog-liquibase
    ├───docker-compose
    ├───entities
    ├───entities-client
    ├───entity
    ├───entity-client
    ├───entity-i18n
    ├───entity-server
    ├───export-jdl
    ├───gae
    ├───generate-blueprint
    ├───gradle
    ├───heroku
    ├───info
    ├───init
    ├───java
    ├───kubernetes
    ├───kubernetes-helm
    ├───kubernetes-knative
    ├───languages
    ├───maven
    ├───openapi-client
    ├───openshift
    ├───page
    ├───project-name
    ├───server
    ├───spring-boot
    ├───spring-controller
    ├───spring-service
    ├───upgrade
    ├───upgrade-config
    └───workspaces
```

Kiến trúc này sẽ cần thiết trong phần Blueprint, ở đó ta sẽ tìm cách ghi đè sub-generator mong muốn.
