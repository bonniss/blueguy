---
weight: 3
title: "Bắt đầu phát triển"
---

# Bắt đầu phát triển

{{< hint warning >}}
**Phiên bản JHipster: 8.0.0**
{{< /hint >}}

## Tạo mới blueprint

```sh
mkdir my-blueprint && cd my-blueprint
jhipster generate-blueprint
```

Trả lời các câu hỏi như sau:

```txt

        ██╗ ██╗   ██╗ ████████╗ ███████╗   ██████╗ ████████╗ ████████╗ ███████╗
        ██║ ██║   ██║ ╚══██╔══╝ ██╔═══██╗ ██╔════╝ ╚══██╔══╝ ██╔═════╝ ██╔═══██╗
        ██║ ████████║    ██║    ███████╔╝ ╚█████╗     ██║    ██████╗   ███████╔╝
  ██╗   ██║ ██╔═══██║    ██║    ██╔════╝   ╚═══██╗    ██║    ██╔═══╝   ██╔══██║
  ╚██████╔╝ ██║   ██║ ████████╗ ██║       ██████╔╝    ██║    ████████╗ ██║  ╚██╗
   ╚═════╝  ╚═╝   ╚═╝ ╚═══════╝ ╚═╝       ╚═════╝     ╚═╝    ╚═══════╝ ╚═╝   ╚═╝
                            https://www.jhipster.tech
Welcome to JHipster v8.0.0

? What is the base name of your application? my-blueprint
? Do you want to generate a local blueprint inside your application? No
? Which sub-generators do you want to override? (Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to proceed)
❯◉ client
 ◯ gatling
 ◯ generate-blueprint
 ◯ git
 ◯ gradle
 ◯ heroku
 ◯ info
 ◯ init
 ◯ java
 ◯ jdl
 ◯ kubernetes
 ◯ kubernetes-helm
 ◯ kubernetes-knative
 ◯ languages
 ◯ liquibase
 ◯ maven
 ◯ project-name
❯◉ react
 ◯ server
 ◯ spring-cache
 ◯ spring-cloud-stream
 ◯ spring-data-cassandra
 ◯ spring-data-couchbase
 ◯ spring-data-elasticsearch
 ◯ spring-data-mongodb
 ◯ spring-data-neo4j
 ◯ spring-data-relational
 ◯ spring-websocket
 ◯ upgrade
 ◯ vue
 ◯ workspaces
(Move up and down to reveal more choices)
? Comma separated additional sub-generators.
? Add a cli? Yes
? Is client generator a side-by-side blueprint? Yes
? Is client generator a cli command? No
? What task do you want do implement at client generator? (Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to
proceed)
 ◉ initializing
 ◉ prompting
 ◯ configuring
 ◯ composing
 ◯ loading
 ◯ preparing
 ◯ configuringEachEntity
 ◯ loadingEntities
 ◯ preparingEachEntity
 ◯ preparingEachEntityField
 ◯ preparingEachEntityRelationship
 ◯ postPreparingEachEntity
 ◯ default
 ◉ writing
 ◯ writingEntities
 ◉ postWriting
 ◯ postWritingEntities
 ◯ loadingTranslations
 ◯ install
 ◉ postInstall
❯◉ end
? Is react generator a side-by-side blueprint? Yes
? Is react generator a cli command? No
? What task do you want do implement at react generator? initializing, prompting, writing, postWriting, postInstall, end
✔ applying multi-step templates
     info Using existing git repository at parent folder.
   create .prettierignore
   create .prettierrc.yml
✔ prettier configuration files commited to disk
   create .github/workflows/generator.yml
   create package.json
    force .yo-rc.json
   create .eslintrc.json
   create tsconfig.json
   create README.md
   create vitest.config.ts
   create .blueprint/cli/commands.mjs
   create .blueprint/generate-sample/templates/samples/sample.jdl
   create .blueprint/generate-sample/command.mjs
   create .blueprint/generate-sample/index.mjs
   create .blueprint/generate-sample/generator.mjs
   create cli/cli.cjs
   create generators/client/templates/template-file-client.ejs
   create generators/client/index.mjs
   create generators/client/command.mjs
   create generators/react/templates/template-file-react.ejs
   create generators/react/index.mjs
   create generators/client/generator.spec.mjs
   create generators/client/generator.mjs
   create generators/react/generator.mjs
   create .gitignore
   create generators/react/command.mjs
   create .gitattributes
   create generators/react/generator.spec.mjs
   create .editorconfig
✔ files commited to disk

Changes to package.json were detected.

Running npm install for you to install the required dependencies.
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
npm WARN deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142

added 1012 packages, and audited 1013 packages in 53s

238 packages are looking for funding
  run `npm fund` for details

4 moderate severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
INFO!
This is a new blueprint, executing 'npm run update-snapshot' to generate snapshots and commit to git.

> generator-jhipster-myblueprint@0.0.0 update-snapshot
> vitest run --update


 RUN  v0.34.6 /Users/bonniss/works/@gen_code/blueprint-target-app/_blueprints/myblueprint

 ✓ generators/client/generator.spec.mjs (1) 1603ms
 ✓ generators/react/generator.spec.mjs (1) 854ms

  Snapshots  2 written
 Test Files  2 passed (2)
      Tests  2 passed (2)
   Start at  16:45:02
   Duration  3.74s (transform 14ms, setup 0ms, collect 1.15s, tests 2.46s, environment 0ms, prepare 48ms)

##### USAGE #####
To begin to work:
- launch: npm install
- link: npm link
- link JHipster: npm link generator-jhipster
- test your module in a JHipster project:
    - create a new directory and go into it
    - link the blueprint: npm link generator-jhipster-undefined
    - launch JHipster with flags: jhipster --blueprints undefined
- then, come back here, and begin to code!

✔ Application successfully committed to Git from /Users/bonniss/works/@gen_code/blueprint-target-app.

Congratulations, JHipster execution is complete!
If you find JHipster useful consider sponsoring the project https://www.jhipster.tech/sponsors/

Thanks for using JHipster!
```

JHipster sẽ sinh lưu cấu hình vào file `.yo-rc.json`, sinh code, cài đặt dependencies.

Ta muốn override 2 sub-generator là : `client` và `react`. Cấu trúc code blueprint sinh ra như sau:

```txt
my-blueprint
├── README.md
├── cli
│   └── cli.cjs
├── generators
│   ├── client
│   │   ├── __snapshots__
│   │   │   └── generator.spec.mjs.snap
│   │   ├── command.mjs
│   │   ├── generator.mjs
│   │   ├── generator.spec.mjs
│   │   ├── index.mjs
│   │   └── templates
│   │       └── template-file-client.ejs
│   └── react
│       ├── __snapshots__
│       │   └── generator.spec.mjs.snap
│       ├── command.mjs
│       ├── generator.mjs
│       ├── generator.spec.mjs
│       ├── index.mjs
│       └── templates
│           └── template-file-react.ejs
├── package-lock.json
├── package.json
├── tsconfig.json
└── vitest.config.ts
```

File `package.json` được sinh ra với tên project đúng chuẩn JHipster, `keywords` có thêm `jhipster-8` là major version của `jhipster` trên máy.

```json
{
  "name": "generator-jhipster-myblueprint",
  "version": "0.0.0",
  "private": true,
  "description": "Description for Myblueprint",
  "keywords": [
    "yeoman-generator",
    "jhipster-blueprint",
    "jhipster-8"
  ],
  "license": "UNLICENSED",
  "type": "module",
  "bin": {
    "jhipster-myblueprint": "cli/cli.cjs"
  },
  "files": [
    "cli",
    "generators"
  ],
  "scripts": {
    "ejslint": "ejslint generators/**/*.ejs",
    "lint": "eslint .",
    "lint-fix": "npm run ejslint && npm run lint -- --fix",
    "prettier-check": "prettier --check \"{,**/}*.{md,json,yml,html,js,ts,tsx,css,scss,vue,java}\"",
    "prettier-format": "prettier --write \"{,**/}*.{md,json,yml,html,js,ts,tsx,css,scss,vue,java}\"",
    "pretest": "npm run prettier-check && npm run lint",
    "test": "vitest run",
    "update-snapshot": "vitest run --update",
    "vitest": "vitest"
  },
  "dependencies": {
    "generator-jhipster": "8.0.0"
  },
  "devDependencies": {
    "ejs-lint": "2.0.0",
    "eslint": "8.52.0",
    "eslint-config-airbnb-base": "15.0.0",
    "eslint-config-prettier": "9.0.0",
    "eslint-plugin-import": "2.29.0",
    "eslint-plugin-prettier": "5.0.1",
    "prettier": "3.0.3",
    "vitest": "0.34.6",
    "yeoman-test": ">=8.0.0-rc.1"
  },
  "engines": {
    "node": "^18.13.0 || >= 20.6.1"
  },
  "cacheDirectories": [
    "node_modules"
  ]
}
```

Mỗi sub-generator đều có `templates` tương ứng với 1 file rỗng như `template-file-client.ejs`.

## Liên kết blueprint

{{< hint info >}}
Xem lại [bí kíp sử dụng blueprint](/docs/blueprint/v8/bp-cookbook)
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
Welcome to JHipster v8.0.0
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
get [BaseApplicationGenerator.WRITING]() {
  return this.asWritingTaskGroup({
    async writingTemplateTask({ application }) {
      await this.writeFiles({
        sections: {
          files: [{ templates: ['template-file-client'] }],
        },
        context: application,
      });
    },
  });
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
