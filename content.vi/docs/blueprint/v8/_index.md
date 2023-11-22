---
weight: 2
bookFlatSection: false
bookCollapseSection: true
title: "Blueprint v8"
---

# Giới thiệu về Blueprint v8

JHipster được thiết kế để mở rộng được bằng plugin. Trước bản 7.9.0, plugin chia làm 2 loại:

- `module`
- `blueprint`

Từ bản 7.9.0 trở đi, `module` được thay đổi thành một loại `blueprint` độc lập (standalone) với CLI riêng biệt, nên từ giờ khi cần mở rộng/tùy biến JHipster, _ta chỉ cần quan tâm đến `blueprint`_.

Blueprint trong JHipster có các đặc điểm sau:

- Node.js package, nếu được xuất bản là một NPM package
- Yeoman generator
- Tên repo bắt đầu bằng tiền tố `generator-jhipster`
- Mục `keywords` trong `package.json` có 2 phần tử

```json
{
  "keywords": [
    "yeoman-generator",
    "jhipster-blueprint"
  ]
}
```

thay vì chỉ mỗi `yeoman-generator` như generator thông thường.

Trong `generator-jhipster`, mỗi thành viên trong `generators` đều là một blueprint.

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
