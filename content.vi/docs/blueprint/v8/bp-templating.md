---
weight: 5
title: "Xử lý biểu mẫu"
---

# Xử lý biểu mẫu

{{< hint warning >}}
**Phiên bản JHipster: 8.0.0**
{{< /hint >}}

{{< hint warning >}}
**Lưu ý**

Bạn cần hoàn thành bài ["Bắt đầu phát triển"](/docs/blueprint/v8/bp-start) trước khi thực hành bài này.
{{< /hint >}}

{{< hint info >}}
Xem lại ["Làm việc với filesystem"](/docs/fundamentals/yeoman/filesystem) trong Yeoman.
{{< /hint >}}

{{< hint info >}}
JHipster sử dụng template engine là [EJS](https://ejs.co/), nên tên biểu mẫu ngầm định kết thúc bằng `.ejs`.
{{< /hint >}}

Khi sinh code sử dụng biểu mẫu, đường dẫn tương đối đến file biểu mẫu sẽ giống với đường dẫn tương đối của file code sinh ra.

Ta sinh code với JDL mẫu `21-points.jh`.

```jdl
/*
 * This is the application and entity model for the 21-Points (https://github.com/mraible/21-points) application from Matt Raible
 */

application {
  config {
    applicationType monolith,
    clientFramework react,
  }
  entities Points, BloodPressure, Weight, Preferences
}
```

Theo như cấu hình frontend sử dụng React nên để biến đổi code dễ hiểu ta cần dùng sub-generator `react`, còn `client` chứa code mà mọi framework đều dùng chung.

```txt
.eslintrc.json.ejs
jest.conf.js.ejs
package.json
package.json.ejs
postcss.config.js.ejs
tsconfig.e2e.json.ejs
tsconfig.json.ejs
tsconfig.test.json.ejs
src
├───main
│   └───webapp
│       ├───app
│       │   │   app.scss.ejs
│       │   │   app.tsx.ejs
│       │   │   index.tsx.ejs
│       │   │   main.tsx.ejs
│       │   │   routes.tsx.ejs
│       │   │   setup-tests.ts.ejs
│       │   │   typings.d.ts.ejs
│       │   │   _bootstrap-variables.scss.ejs
│       │   │
│       │   ├───config
│       │   │       axios-interceptor.spec.ts.ejs
│       │   │       axios-interceptor.ts.ejs
│       │   │       constants.ts.ejs
│       │   │       dayjs.ts.ejs
│       │   │       error-middleware.ts.ejs
│       │   │       icon-loader.ts.ejs
│       │   │       logger-middleware.ts.ejs
│       │   │       notification-middleware.spec.ts.ejs
│       │   │       notification-middleware.ts.ejs
│       │   │       store.ts.ejs
│       │   │       translation.ts.ejs
│       │   │       websocket-middleware.ts.ejs
│       │   │
│       │   ├───entities
│       │   │       menu.tsx.ejs
│       │   │       reducers.ts.ejs
│       │   │       routes.tsx.ejs
│       │   │
│       │   ├───modules
│       │   │   ├───account
│       │   │   │   │   index.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───activate
│       │   │   │   │       activate.reducer.spec.ts.ejs
│       │   │   │   │       activate.reducer.ts.ejs
│       │   │   │   │       activate.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───password
│       │   │   │   │       password.reducer.spec.ts.ejs
│       │   │   │   │       password.reducer.ts.ejs
│       │   │   │   │       password.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───password-reset
│       │   │   │   │   │   password-reset.reducer.ts.ejs
│       │   │   │   │   │
│       │   │   │   │   ├───finish
│       │   │   │   │   │       password-reset-finish.tsx.ejs
│       │   │   │   │   │
│       │   │   │   │   └───init
│       │   │   │   │           password-reset-init.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───register
│       │   │   │   │       register.reducer.spec.ts.ejs
│       │   │   │   │       register.reducer.ts.ejs
│       │   │   │   │       register.spec.tsx.ejs
│       │   │   │   │       register.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───sessions
│       │   │   │   │       sessions.reducer.spec.ts.ejs
│       │   │   │   │       sessions.reducer.ts.ejs
│       │   │   │   │       sessions.tsx.ejs
│       │   │   │   │
│       │   │   │   └───settings
│       │   │   │           settings.reducer.spec.ts.ejs
│       │   │   │           settings.reducer.ts.ejs
│       │   │   │           settings.tsx.ejs
│       │   │   │
│       │   │   ├───administration
│       │   │   │   │   administration.reducer.spec.ts.ejs
│       │   │   │   │   administration.reducer.ts.ejs
│       │   │   │   │   index.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───configuration
│       │   │   │   │       configuration.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───docs
│       │   │   │   │       docs.scss.ejs
│       │   │   │   │       docs.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───gateway
│       │   │   │   │       gateway.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───health
│       │   │   │   │       health-modal.tsx.ejs
│       │   │   │   │       health.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───logs
│       │   │   │   │       logs.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───metrics
│       │   │   │   │       metrics.tsx.ejs
│       │   │   │   │
│       │   │   │   ├───tracker
│       │   │   │   │       tracker.tsx.ejs
│       │   │   │   │
│       │   │   │   └───user-management
│       │   │   │           index.tsx.ejs
│       │   │   │           user-management-delete-dialog.tsx.ejs
│       │   │   │           user-management-detail.tsx.ejs
│       │   │   │           user-management-update.tsx.ejs
│       │   │   │           user-management.reducer.spec.ts.ejs
│       │   │   │           user-management.reducer.ts.ejs
│       │   │   │           user-management.tsx.ejs
│       │   │   │
│       │   │   ├───home
│       │   │   │       home.scss.ejs
│       │   │   │       home.tsx.ejs
│       │   │   │
│       │   │   └───login
│       │   │           login-modal.tsx.ejs
│       │   │           login-redirect.tsx.ejs
│       │   │           login.tsx.ejs
│       │   │           logout.tsx.ejs
│       │   │
│       │   └───shared
│       │       │   DurationFormat.tsx.ejs
│       │       │
│       │       ├───auth
│       │       │       private-route.spec.tsx.ejs
│       │       │       private-route.tsx.ejs
│       │       │
│       │       ├───error
│       │       │       error-boundary-routes.spec.tsx.ejs
│       │       │       error-boundary-routes.tsx.ejs
│       │       │       error-boundary.spec.tsx.ejs
│       │       │       error-boundary.tsx.ejs
│       │       │       error-loading.tsx.ejs
│       │       │       page-not-found.tsx.ejs
│       │       │
│       │       ├───layout
│       │       │   ├───footer
│       │       │   │       footer.scss.ejs
│       │       │   │       footer.tsx.ejs
│       │       │   │
│       │       │   ├───header
│       │       │   │       header-components.tsx.ejs
│       │       │   │       header.scss.ejs
│       │       │   │       header.spec.tsx.ejs
│       │       │   │       header.tsx.ejs
│       │       │   │
│       │       │   ├───menus
│       │       │   │       account.spec.tsx.ejs
│       │       │   │       account.tsx.ejs
│       │       │   │       admin.tsx.ejs
│       │       │   │       entities.tsx.ejs
│       │       │   │       index.ts.ejs
│       │       │   │       locale.tsx.ejs
│       │       │   │       menu-components.tsx.ejs
│       │       │   │       menu-item.tsx.ejs
│       │       │   │
│       │       │   └───password
│       │       │           password-strength-bar.scss.ejs
│       │       │           password-strength-bar.tsx.ejs
│       │       │
│       │       ├───model
│       │       │       user.model.ts.ejs
│       │       │
│       │       ├───reducers
│       │       │       application-profile.spec.ts.ejs
│       │       │       application-profile.ts.ejs
│       │       │       authentication.spec.ts.ejs
│       │       │       authentication.ts.ejs
│       │       │       index.ts.ejs
│       │       │       locale.spec.ts.ejs
│       │       │       locale.ts.ejs
│       │       │       reducer.utils.ts.ejs
│       │       │       user-management.spec.ts.ejs
│       │       │       user-management.ts.ejs
│       │       │
│       │       └───util
│       │               cookie-utils.ts.ejs
│       │               date-utils.ts.ejs
│       │               entity-utils.spec.ts.ejs
│       │               entity-utils.ts.ejs
│       │               pagination.constants.ts.ejs
│       │               url-utils.ts.ejs
│       │
│       └───microfrontends
│               entities-menu.tsx.ejs
│               entities-routes.tsx.ejs
│
└───test
    └───javascript
        │   protractor.conf.js.ejs
        │
        └───e2e
            ├───modules
            │   ├───account
            │   │       account.spec.ts.ejs
            │   │
            │   └───administration
            │           administration.spec.ts.ejs
            │
            ├───page-objects
            │       base-component.ts.ejs
            │       navbar-page.ts.ejs
            │       password-page.ts.ejs
            │       register-page.ts.ejs
            │       settings-page.ts.ejs
            │       signin-page.ts.ejs
            │
            └───util
                    utils.ts.ejs
webpack
└───environment.js.ejs
└───logo-jhipster.png
└───utils.js.ejs
└───webpack.common.js.ejs
└───webpack.dev.js.ejs
└───webpack.microfrontend.js.jhi.react.ejs
└───webpack.prod.js.ejs
```

Vào thư mục `my-project` sinh ra ở bài trước, ta sẽ thấy cấu trúc thư mục giống hệt.

## Ghi đè biểu mẫu có sẵn

{{< hint warning >}}
_Biểu mẫu có sẵn_ được hiểu là các biểu mẫu nằm trong mã nguồn của JHipster, không phải do người dùng thêm vào.
{{< /hint >}}

Vậy làm sao để chỉnh sửa các biểu mẫu có sẵn theo ý mình? Rất đơn giản

### Sao chép biểu mẫu từ `generator-hipster`

Xét file `my-project/src/main/webapp/app/app.scss`:

```scss
// Override Boostrap variables
// Import Bootstrap source files from node_modules
@import '~bootstrap/scss/bootstrap';
body {
  background: #fafafa;
  margin: 0;
}
```

Ta muốn sửa lại để background có màu đỏ.

```scss
// Override Boostrap variables
// Import Bootstrap source files from node_modules
@import '~bootstrap/scss/bootstrap';
body {
  background: red;
  margin: 0;
}
```

### Sao chép đúng biểu mẫu vào blueprint

Sao chép `generator-hipster/generators/client/templates/src/main/webapp/app/app.scss.ejs` vào `my-blueprint/generators/client/templates/src/main/webapp/app/app.scss.ejs`.

### Sửa biểu mẫu trong blueprint

Tìm đến dòng có `background: #fafafa` và sửa lại thành `background: red` trong `my-blueprint/generators/client/templates/src/main/webapp/app/app.scss.ejs`:

```ejs
<%#
 Copyright 2013-2022 the original author or authors from the JHipster project.

 This file is part of the JHipster project, see https://www.jhipster.tech/
 for more information.

 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-%>
// Override Boostrap variables
<%_ if (!clientThemeNone) { _%>
@import "~bootswatch/dist/<%= clientTheme %>/variables";
<%_ } _%>
// Import Bootstrap source files from node_modules
@import "~bootstrap/scss/bootstrap";
<%_ if (!clientThemeNone) { _%>
@import "~bootswatch/dist/<%= clientTheme %>/bootswatch";
<%_ } _%>
body {
<%_ if (clientThemeNone) { _%>
  background: red;
<%_ } _%>
  margin: 0;
}
```

### Sinh code với blueprint

Tại thư mục gốc `my-project`:

```sh
jhipster jdl 21-points.jh --blueprints my-blueprint --skip-jhipster-dependencies --skip-git --skip-install
```

Kiểm tra `my-project/src/main/webapp/app/app.scss` thấy `background: red` là đã thành công.

Nếu bạn đi được tới đây thì **xin chúc mừng, bạn đã có thể override bất kỳ biểu mẫu có sẵn nào của JHipster**.

## Ghi biểu mẫu mới

Trong khi các biểu mẫu có sẵn đã được xử lý bởi sub-generator cha, mọi biểu mẫu mới đều phải có logic xử lý sao chép sang ngữ cảnh đích trong sub-generator của blueprint.

Như đã nói ở cuối bài trước, code blueprint sinh bởi `generate-blueprint` đã có minh họa logic này:

```js
// my-blueprint/generators/client/generator.mjs
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

Ta phải tạo biểu mẫu và sau đó thêm đường dẫn tương đối của biểu mẫu vào trong mảng `templates`.

### Tạo biểu mẫu mới

Tạo file `my-blueprint/generators/client/templates/src/main/webapp/app/author.md.ejs`:

```ejs
Everything not EJS is plain text and keep as-is.

author: <%= author %>
```

### Thêm biến biểu mẫu

{{< hint warning >}}
Dĩ nhiên nếu biểu mẫu không có biến EJS nào thì bạn có thể bỏ qua bước này
{{< /hint >}}

`author.md.ejs` xuất ra giá trị của biến `author`. Với `context.this`, biến này được truyền vào biểu mẫu thông qua `this.author`. Do vậy ta phải tính toán giá trị biến này trước khi xử lý biểu mẫu:

```js
get [BaseApplicationGenerator.WRITING]() {
  this.author = 'bonniss';
  return this.asWritingTaskGroup({
  // ...
}
```

### Thêm đường dẫn biểu mẫu mới

Xóa file biểu mẫu mặc định `template-file-client` do không dùng đến và sửa lại logic ghi biểu mẫu như sau:

```js
// my-blueprint/generators/client/generator.mjs
get [BaseApplicationGenerator.WRITING]() {
  this.author = 'bonniss';
  return this.asWritingTaskGroup({
    async writingTemplateTask({ application }) {
      await this.writeFiles({
        sections: {
          files: [
            {
              path: 'src/main/webapp/app',
              templates: ['author.md'],
            },
          ],
        },
        context: application,
      });
    },
  });
}
```

### Sinh code với blueprint

Tại thư mục gốc `my-project`:

```sh
jhipster jdl 21-points.jh --blueprints my-blueprint --skip-jhipster-dependencies --skip-git --skip-install
```

Kiểm tra tại `my-project/src/main/webapp/app/author.md` nội dung sau là đã thành công:

```md
Everything not EJS is plain text and keep as-is.

author: bonniss
```

Nếu bạn đi được tới đây thì **xin chúc mừng, bạn đã có thể sinh code mới theo biểu mẫu mới**.
