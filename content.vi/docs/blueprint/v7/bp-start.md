---
weight: 3
title: "Bắt đầu phát triển"
---

# Bắt đầu phát triển

{{< hint warning >}}
**Phiên bản JHipster: 7.9.4**
{{< /hint >}}

## Tạo mới blueprint

```sh
mkdir my-blueprint && cd my-blueprint
jhipster generate-blueprint
```

Trả lời các câu hỏi như sau:

```txt
INFO! Using bundled JHipster

        ██╗ ██╗   ██╗ ████████╗ ███████╗   ██████╗ ████████╗ ████████╗ ███████╗
        ██║ ██║   ██║ ╚══██╔══╝ ██╔═══██╗ ██╔════╝ ╚══██╔══╝ ██╔═════╝ ██╔═══██╗
        ██║ ████████║    ██║    ███████╔╝ ╚█████╗     ██║    ██████╗   ███████╔╝
  ██╗   ██║ ██╔═══██║    ██║    ██╔════╝   ╚═══██╗    ██║    ██╔═══╝   ██╔══██║
  ╚██████╔╝ ██║   ██║ ████████╗ ██║       ██████╔╝    ██║    ████████╗ ██║  ╚██╗
   ╚═════╝  ╚═╝   ╚═╝ ╚═══════╝ ╚═╝       ╚═════╝     ╚═╝    ╚═══════╝ ╚═╝   ╚═╝
                            https://www.jhipster.tech
Welcome to JHipster v7.9.4

⬢ Welcome to the JHipster Project Name ⬢
? What is the base name of your application? my-blueprint
? What is the project name of your application? My Blueprint Application
? Do you want to generate a local blueprint inside your application? No
? Which sub-generators do you want to override?
 ( ) base
 ( ) bootstrap
 ( ) bootstrap-application
 ( ) ci-cd
 ( ) ci-cd
 (*) client
 ( ) cloudfoundry
 ( ) common
 ( ) cypress
 ( ) database-changelog
 ( ) database-changelog-liquibase
 ( ) docker-compose
 ( ) entities
 ( ) entities-client
 ( ) entity
>(*) entity-client
 ( ) entity-i18n
 ( ) entity-i18n
 ( ) entity-server
 ( ) export-jdl
 ( ) gae
 ( ) generate-blueprint
 ( ) gradle
 ( ) heroku
 ( ) info
 ( ) init
 ( ) java
 ( ) kubernetes
 ( ) kubernetes-helm
 ( ) kubernetes-knative
? Comma separated additional sub-generators.
? Add a cli? Yes
? Is client generator a side-by-side blueprint? Yes
? Is client generator a cli command? No
? What task do you want do implement at client generator? writing, end
 ( ) initializing
 ( ) prompting
 ( ) configuring
 ( ) composing
 ( ) loading
 ( ) preparing
 ( ) default
 (*) writing
 ( ) postWriting
 ( ) install
 ( ) postInstall
>(*) end
? Is entity-client generator a side-by-side blueprint? Yes
? Is entity-client generator a cli command? No
? What task do you want do implement at entity-client generator? writing, end
 ( ) initializing
 ( ) prompting
 ( ) configuring
 ( ) composing
 ( ) loading
 ( ) preparing
 ( ) default
 (*) writing
 ( ) postWriting
 ( ) install
 ( ) postInstall
>(*) end
? What is the default indentation? 2
```

JHipster sẽ sinh lưu cấu hình vào file `.yo-rc.json`, sinh code, cài đặt dependencies.

Ta muốn override 2 sub-generator là : `client` và `entity-client`. Cấu trúc code blueprint sinh ra như sau:

```txt
my-blueprint
|   .editorconfig
|   .eslintrc.json
|   .gitattributes
|   .gitignore
|   .mocharc.cjs
|   .prettierignore
|   .prettierrc.yml
|   .yo-rc.json
|   package.json
|   README.md
|
+---.github
|   \---workflows
|           generator.yml
|
+---cli
|       cli.mjs
|
+---generators
|   +---client
|   |   |   generator.mjs
|   |   |   generator.spec.mjs
|   |   |   index.mjs
|   |   |
|   |   \---templates
|   |           template-file-client.ejs
|   |
|   \---entity-client
|       |   generator.mjs
|       |   index.mjs
|       |
|       \---templates
|               template-file-entity-client.ejs
|
\---test
        utils.mjs
```

File `package.json` được sinh ra với tên project đúng chuẩn JHipster, `keywords` có thêm `jhipster-7` là major version của `jhipster` trên máy.

```json
{
  "name": "generator-jhipster-my-blueprint",
  "version": "0.0.0",
  "private": true,
  "description": "My Blueprint Application",
  "keywords": [
    "yeoman-generator",
    "jhipster-blueprint",
    "jhipster-7"
  ],
  "license": "UNLICENSED",
  "type": "module",
  "imports": {
    "#test-utils": "./test/utils.mjs"
  },
  "bin": {
    "jhipster-my-blueprint": "cli/cli.mjs"
  },
  "files": [
    "cli",
    "generators"
  ],
  "scripts": {
    "ejslint": "ejslint generators/**/*.ejs && ejslint generators/**/*.ejs -d '&'",
    "lint": "eslint .",
    "lint-fix": "npm run ejslint && npm run lint -- --fix",
    "mocha": "mocha generators --no-insight --forbid-only",
    "prettier-format": "prettier --write \"{,**/}*.{md,json,yml,html,js,cjs,mjs,ts,tsx,css,scss,vue,java}\"",
    "prettier:check": "prettier --check \"{,src/**/}*.{md,json,yml,html,js,ts,tsx,css,scss,vue,java}\"",
    "prettier:format": "prettier --write \"{,src/**/}*.{md,json,yml,html,js,ts,tsx,css,scss,vue,java}\"",
    "pretest": "npm run prettier-check && npm run lint",
    "test": "npm run mocha --",
    "update-snapshot": "npm run mocha -- --no-parallel --updateSnapshot"
  },
  "dependencies": {
    "chalk": "4.1.2",
    "generator-jhipster": "7.9.4"
  },
  "devDependencies": {
    "ejs-lint": "1.2.2",
    "eslint": "8.23.0",
    "eslint-config-airbnb-base": "15.0.0",
    "eslint-config-prettier": "8.5.0",
    "eslint-plugin-import": "2.26.0",
    "eslint-plugin-mocha": "10.1.0",
    "eslint-plugin-prettier": "4.2.1",
    "expect": "28.1.3",
    "mocha": "9.2.2",
    "mocha-expect-snapshot": "4.0.0",
    "prettier": "2.7.1",
    "prettier-plugin-java": "1.6.2",
    "prettier-plugin-packagejson": "2.2.18",
    "yeoman-test": "6.3.0"
  },
  "engines": {
    "node": ">=16.13.0"
  },
  "cacheDirectories": [
    "node_modules"
  ]
}
```

Mỗi sub-generator đều có `templates` tương ứng với 1 file rỗng như `template-file-client.ejs`.

{{< hint danger >}}
Lệnh sinh blueprint có thể gặp lỗi khi cài đặt dependencies.

```sh
npm ERR! Could not resolve dependency:
npm ERR! peerOptional mem-fs@"^3.0.0" from @yeoman/types@1.1.1
npm ERR! node_modules/@yeoman/types
npm ERR!   peer @yeoman/types@"^1.0.1" from yeoman-generator@6.0.1
npm ERR!   node_modules/yeoman-generator
npm ERR!     peer yeoman-generator@"*" from yeoman-test@6.3.0
npm ERR!     node_modules/yeoman-test
npm ERR!       dev yeoman-test@"6.3.0" from the root project
```

Nguyên nhân do xung đột phiên bản các dependencies. Trong `package.json` sửa lại

```txt
"yeoman-test": "^5.0.0"
```

sau đó cài đặt lại:

```sh
npm install
```

{{< /hint >}}

## Liên kết blueprint

{{< hint info >}}
Xem lại [bí kíp sử dụng blueprint](/docs/blueprint/v7/bp-cookbook)
{{< /hint >}}

Tại thư mục gốc của blueprint:

```sh
npm link
```

Liệt kê danh sách các package global để kiểm tra, thấy `generator-jhipster-my-blueprint` là thành công.

```sh
$ npm ls -g
.\
+-- cowsay@1.5.0
+-- generator-generator@5.1.0
+-- generator-jhipster-my-blueprint@0.0.0 -> .\D:\works\goline\blueprint\my-blueprint
+-- npm@8.19.4
+-- yarn@1.22.19
`-- yo@4.3.1
```

## Chạy thử blueprint

Tạo và di chuyển đến một thư mục trống để chạy thử `my-blueprint`:

```sh
$ jhipster jdl 21-points.jh --blueprints my-blueprint --skip-jhipster-dependencies --skip-git --skip-install
INFO! No custom sharedOptions found within blueprint: generator-jhipster-my-blueprint at C:/ProgramData/nvm/v16.20.1/node_modules/generator-jhipster-my-blueprint
INFO! No custom commands found within blueprint: generator-jhipster-my-blueprint at C:/ProgramData/nvm/v16.20.1/node_modules/generator-jhipster-my-blueprint

        ██╗ ██╗   ██╗ ████████╗ ███████╗   ██████╗ ████████╗ ████████╗ ███████╗
        ██║ ██║   ██║ ╚══██╔══╝ ██╔═══██╗ ██╔════╝ ╚══██╔══╝ ██╔═════╝ ██╔═══██╗
        ██║ ████████║    ██║    ███████╔╝ ╚█████╗     ██║    ██████╗   ███████╔╝
  ██╗   ██║ ██╔═══██║    ██║    ██╔════╝   ╚═══██╗    ██║    ██╔═══╝   ██╔══██║
  ╚██████╔╝ ██║   ██║ ████████╗ ██║       ██████╔╝    ██║    ████████╗ ██║  ╚██╗
   ╚═════╝  ╚═╝   ╚═╝ ╚═══════╝ ╚═╝       ╚═════╝     ╚═╝    ╚═══════╝ ╚═╝   ╚═╝
                            https://www.jhipster.tech
Welcome to JHipster v7.9.4
INFO! Executing import-jdl 21-points.jh
INFO! The JDL is being parsed.
warn: An Entity name 'User' was used: 'User' is an entity created by default by JHipster. All relationships toward it will be kept but any attributes and relationships from it will be disregarded.
INFO! Found entities: Points, BloodPressure, Weight, Preferences.
INFO! The JDL has been successfully parsed
INFO! Generating 1 application.
INFO! No custom sharedOptions found within blueprint: generator-jhipster-my-blueprint at C:/ProgramData/nvm/v16.20.1/node_modules/generator-jhipster-my-blueprint
     info Using blueprint generator-jhipster-my-blueprint for client subgenerator
     info Creating changelog for entities Points,BloodPressure,Weight,Preferences
     info Using blueprint generator-jhipster-my-blueprint for entity-client subgenerator
     info Using blueprint generator-jhipster-my-blueprint for entity-client subgenerator
     info Using blueprint generator-jhipster-my-blueprint for entity-client subgenerator
     info Using blueprint generator-jhipster-my-blueprint for entity-client subgenerator
```

Để ý các dòng log bắt đầu bằng `info`.

- Sub-generator `client` phụ trách sinh code chung cho phía client nên được thực thi 1 lần. JHipster đã phát hiện và sử dụng `client` từ `my-blueprint`.
- Sub-generator `entity-client` phụ trách sinh code riêng cho _từng thực thể_. Hệ thống có 4 thực thể là `Points`, `BloodPressure`, `Weight`, `Preferences` nên `entity-client` được thực thi tương ứng 4 lần. JHipster đã phát hiện và sử dụng `entity-client` từ `my-blueprint` 4 lần.

Kiểm tra code sinh ra ta thấy 2 file biểu mẫu từ 2 sub-generator đã được xử lý và sao chép sang.

```txt
my-new-project
│   21-points.jh
│   ...
│   template-file-client
│   template-file-entity-client
│   ...
```

Di chuyển tới `my-blueprint/generators/client/generator.mjs`, thấy logic xử lý và sao chép file ở method writing:

```js
get [WRITING_PRIORITY]() {
  return {
    async writingTemplateTask() {
      await this.writeFiles({
        sections: {
          files: [{ templates: ['template-file-client'] }],
        },
        context: this,
      });
    },
  };
}
```

Nếu bạn đi được tới đây thì __xin chúc mừng, bạn đã tạo thành công blueprint đầu tay__.

## Cài đặt `lodash` (tùy chọn)

[Lodash](https://lodash.com/) là thư viện hàm tiện ích đã quá nổi tiếng trong thế giới Javascript.

```sh
npm install lodash
```

Một số hàm tiện ích `lodash` toả sáng khi làm biểu mẫu:

- Đọc và ghi object nhiều tầng:
  - `_.get`
  - `_.set`
- Chuyển đổi loại ký tự như tên biến theo quy ước các ngôn ngữ lập trình:
  - `_.upperFirst`
  - `_.camelCase`
  - `_.snakeCase`
- Thao tác biến đổi object:
  - `_.omit`
  - `_.pick`


## Tham khảo

- [[JHipster] Blueprint basics](https://www.jhipster.tech/modules/extending-and-customizing/)
- [[JHipster] Creating a blueprint](https://www.jhipster.tech/modules/creating-a-blueprint/)
