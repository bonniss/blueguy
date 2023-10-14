---
weight: 5
title: "Kết hợp generator"
---

# Kết hợp generator

Tại sao phải viết mới một tính năng khi có ai đó đã viết rồi? Yeoman cho phép kết hợp nhiều generator lại với nhau.

## `this.composeWith()`

`this.composeWith()` cho phép generator của bạn chạy song song (side-by-side) với một generator khác (hoặc subgenerator). Cách làm này cho phép bạn dùng được các tính năng đã được ai đó làm tốt thay vì tự viết mới.

```js
// In <my-generator>/generators/turbo/index.js
module.exports = class extends Generator {
  prompting() {
    this.log('prompting - turbo');
  }

  writing() {
    this.log('writing - turbo');
  }
};

// In <my-generator>/generators/zap/index.js
module.exports = class extends Generator {
  prompting() {
    this.log('prompting - zap');
  }

  writing() {
    this.log('writing - zap');
  }
};

// In <my-generator>/generators/app/index.js
module.exports = class extends Generator {
  initializing() {
    this.composeWith(require.resolve('../turbo'));
    this.composeWith(require.resolve('../zap'));
  }
};
```

Bạn vẫn nhớ [run loop và priority](/docs/fundamentals/yeoman/method-priority.md) chứ? Các tầng priority cho phép các generator chung sống với hành vi xác định khi được kết hợp lại với nhau. Thứ tự kết hợp là quan trọng: generator được kết hợp trước bằng `composeWith` sẽ được thực hiện trước _trong từng tầng priority_. Minh họa thứ tự hoạt động cho đoạn code trên:

{{< mermaid class="text-center" >}}
flowchart TD
  CT[generator-turbo]
  CZ[generator-zap]
  PT[prompting-turbo]
  PZ[prompting-zap]
  WT[writing-turbo]
  WZ[writing-zap]

  subgraph ComposeWith
  CT --> CZ
  end

  subgraph Prompting
  PT --> PZ
  end

  subgraph Writing
  WT --> WZ
  end

  Prompting --> Writing
{{< /mermaid >}}

Kết quả khi chạy:

```sh
$ yo my-generator
prompting - turbo
prompting - zap
writing - turbo
writing - zap
```

## Tham khảo

- [[Yeoman] Composability](https://yeoman.io/authoring/composability)
