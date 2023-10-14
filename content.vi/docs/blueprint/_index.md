---
weight: 2
bookFlatSection: false
bookCollapseSection: true
title: "JHipster Blueprint"
---

# Giới thiệu về blueprint

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

## Tại sao cần đến blueprint

JHipster đã làm rất tốt nhiệm vụ sinh code của mình. Trong nhiều trường hợp, ta chỉ muốn tùy chỉnh một phần trên nền code sinh ra:

- Tùy chỉnh giao diện: sửa CSS, thay thế UI bằng thư viện khác (mặc định code phía client sử dụng Bootstrap).
- Tùy chỉnh tính năng: tùy chỉnh API và tùy chỉnh giao diện tương ứng.
- Xóa bớt tính năng: xóa bớt màn hình, tính năng không dùng đến.
- Cá nhân hóa code sinh ra: thay thế logo, từ khóa JHipster bằng thương hiệu cá nhân, tổ chức khai thác code.
- Thêm sửa xóa, bổ sung bản dịch ngôn ngữ mà JHipster chưa hỗ trợ hoặc thiếu sót.

Nếu chỉ chỉnh sửa vài chỗ như bản dịch ngôn ngữ, xóa logo, ta có thể sinh code rồi sửa. Nhưng khi các chỉnh sửa giống nhau cho rất nhiều phần, tiêu biểu nhất là code của thực thể: ứng dụng có 10 thực thể với 10 tập trang CRUD, chỉ một nỗ lực sửa nút "Tạo mới" thành màu cam thôi đòi hỏi ta phải sửa ít nhất 10 chỗ ứng với từng trang listing từng thực thể.

Thông qua blueprint ta có thể tùy biến code sinh ra từ JHipster ở phạm vi tùy ý, thậm chí viết lại hoàn toàn code server hoặc client, giúp tiết kiệm thời gian và công sức.

## Blueprint chính thức

Danh sách các blueprint chính thức từ đội ngũ JHipster xem [tại đây](https://www.jhipster.tech/modules/official-blueprints/).

Một số blueprint tiêu biểu:

- [jhipster-kotlin](https://github.com/jhipster/jhipster-kotlin): thay thế backend Java bằng Kotlin
- [jhipster-dotnetcore](https://github.com/jhipster/jhipster-dotnetcore): thay thế backend Java bằng .NET Core.
- [jhipster-nodejs](https://github.com/jhipster/generator-jhipster-nodejs): thay thế backend Java bằng Node.js (framework Nest.js)
- [jhipster-react-native](https://github.com/jhipster/generator-jhipster-react-native): tạo client là ứng dụng React Native.
- [jhipster-svelte](https://github.com/jhipster/generator-jhipster-svelte): tạo client là ứng dụng SvelteKit.

## Chợ Plugin

Bên cạnh các plugin chính thức còn có các plugin đến từ cộng đồng. Tra cứu tại [chợ plugin](https://www.jhipster.tech/modules/marketplace).
