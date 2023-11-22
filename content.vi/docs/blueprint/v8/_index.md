---
weight: 2
bookFlatSection: false
bookCollapseSection: true
title: "Blueprint v8"
---

# Giới thiệu về Blueprint v8

Trong `generator-jhipster`, mỗi thành viên trong `generators` đều là một blueprint.

```txt
generator-jhipster
└───generators
    ├── angular
    ├── app
    ├── base
    ├── base-application
    ├── base-core
    ├── base-entity-changes
    ├── base-workspaces
    ├── bootstrap
    ├── bootstrap-application
    ├── bootstrap-application-base
    ├── bootstrap-application-client
    ├── bootstrap-application-server
    ├── bootstrap-workspaces
    ├── ci-cd
    ├── client
    ├── common
    ├── cucumber
    ├── cypress
    ├── docker
    ├── docker-compose
    ├── entities
    ├── entity
    ├── export-jdl
    ├── gatling
    ├── generate-blueprint
    ├── git
    ├── gradle
    ├── heroku
    ├── info
    ├── init
    ├── java
    ├── jdl
    ├── kubernetes
    ├── kubernetes-helm
    ├── kubernetes-knative
    ├── languages
    ├── liquibase
    ├── maven
    ├── project-name
    ├── react
    ├── server
    ├── spring-cache
    ├── spring-cloud-stream
    ├── spring-data-cassandra
    ├── spring-data-couchbase
    ├── spring-data-elasticsearch
    ├── spring-data-mongodb
    ├── spring-data-neo4j
    ├── spring-data-relational
    ├── spring-websocket
    ├── upgrade
    ├── vue
    └── workspaces
```

## Thay đổi lớn so với v7

Chi tiết tại [trang release 8.0.0](https://www.jhipster.tech/2023/11/02/jhipster-release-8.0.0.html). Ở đây điểm lại các thay đổi đáng chú ý.

### 🖨️ Blueprint

#### Thêm

|Sub-generator|Tình trạng|Ghi chú|
|---|---|---|
|`react`|_New_|Tách ra từ `client`|
|`angular`|_New_|Tách ra từ `client`|
|`vue`|_New_|Tách ra từ `client`|

#### Bỏ

|Sub-generator|Tình trạng|Ghi chú|
|---|---|---|
|`aws`|_Unmaintained_||
|`azure-app-service`|_Unmaintained_||
|`azure-spring-cloud`|_Unmaintained_||
|`cloudfoundry`|_Unmaintained_||
|`gae`|_Unmaintained_||
|`openshift`|_Unmaintained_||
|`openapi-client`|_Deprecated_||
|`page`|_Deprecated_||
|`upgrade-config`|_Deprecated_||
|`spring-controller`|_Deprecated_||
|`spring-service`|_Deprecated_||
|`entity-client`|_Deprecated_|Gộp vào blueprint của client tương ứng: `react`, `angular`, `vue`|
|`entity-server`|_Deprecated_|Gộp vào `server`|
|`entity-i18n`|_Deprecated_|Gộp vào `languages`|

### 💎 Thư viện và tính năng

- ⬆️ Spring Boot 3.1.5
- ⬆️ JDK 20, 21
- ⬆️ Node 18 LTS
- ⬆️ Gradle 8.4
- ⬆️ Maven 3.9.5
- ⬆️ Keycloak 22.0.1
- ⬆️ Hibernate 6.2.x

### 💻 Frontend

- ⬆️ Angular 16
- ⬆️ Vue 3
- ⬆️ Prettier 3

{{< hint info >}}
🙊 React giữ nguyên
{{< /hint >}}

## Đánh giá

v8 có nhiều cải tiến và fix lỗi, code gọn gàng hơn. Type cũng được hỗ trợ tốt hơn. Sinh blueprint bằng `jhipster generate-blueprint` không gặp lỗi với `yeoman-test` như v7.
