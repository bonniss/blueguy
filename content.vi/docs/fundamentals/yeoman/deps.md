---
weight: 4
title: "Cài đặt dependencies"
---

# Cài đặt dependencies

Khi chạy generators, bạn có thể cần cài đặt dependencies bằng `npm` hoặc `yarn`. Yeoman hỗ trợ sẵn tác vụ này.

## `npm`

Gọi `this.npmInstall()` để cài đặt bằng `npm`. Yeoman đảm bảo `npm install` sẽ chỉ chạy một lần kể cả khi nó được gọi nhiều lần từ nhiều generator.

Chẳng hạn nếu bạn muốn cài đặt `lodash` như `devDependencies`:

```js
class extends Generator {
  installingLodash() {
    this.npmInstall(['lodash'], { 'save-dev': true });
  }
}
```

tương đương với:

```sh
npm install --save-dev lodash
```

### Thêm dependencies trong `package.json`

```js
class extends Generator {
  writing() {
    const pkgJson = {
      devDependencies: {
        eslint: '^3.15.0'
      },
      dependencies: {
        react: '^16.2.0'
      }
    };

    // Extend or create package.json file in destination path
    this.fs.extendJSON(this.destinationPath('package.json'), pkgJson);
  }

  install() {
    this.npmInstall();
  }
};
```

## `yarn`

Tương tự như `npm`, để cài đặt dùng `yarn`, bạn chỉ cần gọi `this.yarnInstall()`.

```js
generators.Base.extend({
  installingLodash: function() {
    this.yarnInstall(['lodash'], { 'dev': true });
  }
});
```

## Sử dụng tool khác

Yeoman đơn giản hóa việc `spawn` bất cứ lệnh CLI nào, bất kể trên Windows, Linux hay MacOS.

Bạn đang sinh code PHP và muốn cài đặt dependencies sử dụng `composer`:

```js
class extends Generator {
  install() {
    this.spawnCommand('composer', ['install']);
  }
}
```

Lưu ý để người dùng không phải đợi cài đặt hoàn tất thì bạn chỉ nên gọi `spawnCommand` trong pha `install`.

## Tham khảo

- [[Yeoman] Dependencies](https://yeoman.io/authoring/dependencies)
