---
weight: 3
title: "Method và priority"
---

# Method và priority

Một trong những khái niệm quan trọng nhất cần nắm khi viết generator là method _chạy ra sao_, và trong _ngữ cảnh_ nào.

## Run loop

Chạy task theo thứ tự không phải là vấn đề với một generator, nhưng sẽ trở nên nhức đầu khi kết hợp nhiều generator. Yeoman sử dụng một hệ thống hàng đợi có thứ tự ưu tiên được gọi là run loop để xếp hàng method.

{{< mermaid class="text-center" >}}
flowchart TD
  INI["**initializing**
  initialization methods (checking current project state, getting configs, etc)"]
  PRO["**prompting**
  prompt users for options"]
  CFG["**configuring**
  Saving configurations and configure the project (creating .editorconfig files and other metadata files)"]
  DEF["**default**
  If the method name doesn’t match any priority, push to this group"]
  WRI["**writing**
  Where you write the generator specific files (routes, controllers, etc)"]
  CON["**conflicts**
  Where conflicts are handled (used internally)"]
  INS["**install**
  Where installations are run (npm, yarn)"]
  END["**end**
  Called last, cleanup, say good bye, etc"]

  subgraph Priorities in Run loop
    INI --> PRO --> CFG --> DEF --> WRI --> CON --> INS --> END
  end
{{< /mermaid >}}

## Method === Action

Mỗi method trong prototype của generator là một action được thực hiện tự động bởi Yeoman run loop.

Nếu muốn method không được gọi tự động, hãy đặt tên method bắt đầu bằng `_`, như quy ước rằng method là private.

```js
class extends Generator {
  // được gọi tự động
  method1() {
    console.log('hey 1');
  }

  // KHÔNG gọi tự động
  _private_method() {
    console.log('private hey');
  }
}
```

Method của instance cũng không được gọi tự động.

```js
class extends Generator {
  constructor(args, opts) {
    // Calling the super constructor is important so our generator is correctly set up
    super(args, opts)

    // KHÔNG gọi tự động
    this.helperMethod = function () {
      console.log('won\'t be called automatically');
    };
  }
}
```

## Async task

Cách dễ nhất để thực hiện async task là **trả về promise**. Run loop chỉ tiếp tục khi promise resolved hoặc rejected.

## Minh họa

Trong `generators/app`:

```js
"use strict";
const Generator = require("yeoman-generator");
const chalk = require("chalk");

module.exports = class extends Generator {
  _helperFn() {
    this.log("I won't be called 👅");
  }

  firstDefaultMethod() {
    this.log(chalk.blue("I will be in `default` queue"));
  }

  secondDefaultMethod() {
    this.log(chalk.blue("`default` queue also"));
  }

  initializing() {
    this.log(chalk.green(">> Initializing"));
  }

  async prompting() {
    this.log(chalk.green(">> Prompting"));
    await new Promise(resolve => {
      let count = 0;
      const timer = setInterval(() => {
        count += 1;
        this.log(`Count ${count}`);
        if (count === 5) {
          clearInterval(timer);
          resolve();
        }
      }, 1000);
    });
  }

  configuring() {
    this.log(chalk.green(">> Configuring"));
  }

  writing() {
    this.log(chalk.green(">> Writing"));
  }

  install() {
    this.log(chalk.green(">> Install"));
  }

  end() {
    this.log(chalk.green(">> End"));
  }
};
```

Link global:

```sh
npm link
```

Chạy generator:

```sh
$ yo generatorName
>> Initializing
>> Prompting
Count 1
Count 2
Count 3
Count 4
Count 5
>> Configuring
I will be in `default` queue
`default` queue also
>> Writing
>> Install
>> End
```

## Tham khảo

- [[Yeoman] Running context](https://yeoman.io/authoring/running-context)
