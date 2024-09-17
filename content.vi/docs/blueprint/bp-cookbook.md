---
weight: 1
title: "Bí kíp sử dụng blueprint"
---

# Bí kíp sử dụng blueprint

## Nguyên tắc

Để sinh code cùng với blueprint, về cơ bản bạn cần:

1. Cài đặt blueprint với scope global
2. Sinh code với tùy chọn `--blueprints`

```sh
jhipster --blueprints <blueprint_name>
```

Dĩ nhiên, bạn thoải mái kết hợp với các [tùy chọn khác](/docs/fundamentals/jhipster/jh-cookbook.md):

```sh
jhipster jdl ./app.jdl --blueprints <blueprint_name> --skip-client --skip-install
```

## Blueprint đã xuất bản trên NPM

Cài đặt package blueprint với scope global bằng `npm` hoặc `yarn`, ví dụ với blueprint chính thức [`entity-audit`](https://github.com/jhipster/generator-jhipster-entity-audit):

```sh
npm install -g generator-jhipster-entity-audit
# or
yarn global add generator-jhipster-entity-audit
```

Sau đó sinh code:

```sh
jhipster --blueprints entity-audit
```

## Blueprint đang phát triển

Với blueprint đang phát triển và chưa xuất bản lên NPM, hoặc blueprint riêng tư chỉ chạy ở local, ta không thể dùng `npm install -g` được.

`npm` cho phép liên kết package local để sử dụng như một NPM package global: tại thư mục của blueprint, gọi [`npm link`](https://docs.npmjs.com/cli/v10/commands/npm-link) để tạo liên kết.

Bước 2 có một điểm khác: tùy chọn `--skip-jhipster-dependencies` là cần thiết để JHipster không thêm blueprint đang phát triển vào `dependencies` của code sinh ra:

```sh
jhipster --blueprints my-local-blueprint --skip-jhipster-dependencies
```

## Blueprint có CLI riêng

Ngoài cách gọi như một tùy chọn `--blueprints`, một số blueprint còn cung cấp một CLI riêng biệt, như với trường hợp của [jhipster-nodejs](https://github.com/jhipster/generator-jhipster-nodejs).

```sh
# Step 1
npm install -g generator-jhipster-nodejs

# Step 2
jhipster --blueprints nodejs
# or
nhipster
```

`nhipster` không đơn giản là một shortcut cho cách gọi chính quy, mà có thể thực hiện nhiều tác vụ khác:

```sh
# Sinh controller
nhipster spring-controller <controller-name>

# Sinh entity
nhipster entity <entity-name>

# Sinh CI/CD
nhipster ci-cd
```

Người dùng cần bám tài liệu của blueprint để nắm được các tác vụ sử dụng.
