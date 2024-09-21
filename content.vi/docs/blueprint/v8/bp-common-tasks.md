---
weight: 6
title: "Tác vụ thường dùng"
---

# Tác vụ thường dùng

{{< hint warning >}}
**Phiên bản JHipster: 8.0.0**
{{< /hint >}}

{{< hint info >}}
Xem lại ["Làm việc với filesystem"](/docs/fundamentals/yeoman/filesystem) trong Yeoman.
{{< /hint >}}

## Mở rộng `package.json`

Method tiện ích [`this.packageJson.merge`](https://yeoman.github.io/generator/Generator.html#packageJson) cho phép mở rộng bất kỳ giá trị nào trong `package.json`. Ở tầng `postWriting`:

```js
get [BaseApplicationGenerator.POST_WRITING_PRIORITY]() {
  return {
    async modifyPackageJson() {
      this.packageJson.merge({
        scripts: {
          'yo:version': 'yo --version'
        },
        dependencies: {
          clsx: '^2.0.0',
          'query-string': '^8.1.0',
        },
        devDependencies: {
          tailwindcss: '^3.3.3',
        },
        peerDependencies: {
          react: "^16.0.0",
        }
      });
    },
  }
}
```

## Xoá nội dung `package.json`

`this.packageJson` về bản chất là một [Yeoman Storage](/docs/fundamentals/yeoman/yeoman-storage).

```js
get [BaseApplicationGenerator.POST_WRITING]() {
  return this.asPostWritingTaskGroup({
    async removeDepsPackageJson() {
      // 1. Đọc JSON vào một object
      const json = this.packageJson.getAll();

      // 2. Xoá các key mong muốn
      const depsToRemove = {
        dependencies: ['@fortawesome/fontawesome-svg-core', '@fortawesome/free-solid-svg-icons', '@fortawesome/react-fontawesome'],
        devDependencies: [],
      };
      Object.keys(depsToRemove).forEach(ns => {
        const items = depsToRemove[ns];
        // eslint-disable-next-line no-unused-expressions
        Array.isArray(items) &&
          items.forEach(x => {
            _.unset(json, `${ns}.${x}`);
          });
      });

      // 3. Lưu object kết quả xuống file JSON
      this.packageJson.writeContent(json);
    },
  });
}
```

## Sao chép thư mục

```js
get [BaseApplicationGenerator.POST_WRITING_PRIORITY]() {
  return {
    async addFolderComponents() {
      await this.fs.copy(
        this.templatePath('react/src/main/webapp/app/shared/components'),
        this.destinationPath('src/main/webapp/app/shared/components')
      );
    },
  }
}
```

## Sao chép file

```js
get [BaseApplicationGenerator.POST_WRITING_PRIORITY]() {
  return {
    async addSharedTemplates() {
      await this.copyTemplate(
        'react/src/main/webapp/app/shared/layout/menus/nav-dropdown.tsx',
        'src/main/webapp/app/shared/layout/menus/nav-dropdown.tsx'
      );
    },
  }
}
```

## Thay thế nội dung file

```js
get [BaseApplicationGenerator.POST_WRITING_PRIORITY]() {
  return {
    async addSharedTemplates() {
      async cleanEntitiesMenu() {
        this.editFile(this.destinationPath('src/main/webapp/app/entities/menu.tsx'), content => content.replaceAll(' icon="asterisk"', ''));
      },
    },
  }
}
```

## Xóa file

```js
get [BaseApplicationGenerator.POST_WRITING_PRIORITY]() {
  return {
    async removeScssFiles() {
      this.deleteDestination('src/main/webapp/app/shared/layout/header/header.scss');
      this.deleteDestination('src/main/webapp/app/shared/layout/footer/footer.scss');
    },
  }
}
```

## Thêm thuộc tính vào entity để dùng trong biểu mẫu

```js
get [BaseApplicationGenerator.POST_PREPARING_EACH_ENTITY]() {
  return this.asPostPreparingEachEntityTaskGroup({
    addDerivedField({ entity, entityName }) {
      const { fields } = entity
      entity.hasStatusField = fields.some(x => x.fieldName === 'status')
    },
  });
}
```

Trong một biểu mẫu dành cho entity, ta có thể dùng `hasStatusField`:

```ejs
<%_ if (hasStatusField) { _%>
  // This entity has status field
<%_ } _%>
```

## Tham khảo

Clone repo [generator-jhipster-native](https://github.com/jhipster/generator-jhipster-native) và đọc code thực hiện các tác vụ:

- Sửa `package.json`.
- Xóa file.
- Sửa `pom.xml`.
- Sửa code Java.
- Sửa Cypress.
