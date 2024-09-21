---
weight: 2
title: "Hiểu biết tổng quan"
---

# Hiểu biết tổng quan

{{< hint warning >}}
**Phiên bản JHipster: 8.0.0**
{{< /hint >}}

## Mã nguồn JHipster

Khi có ý định tùy biến code, bạn cần khoanh vùng xem cần tác động đến các sub-generator nào trong [`generator-jhipster`](https://github.com/jhipster/generator-jhipster) để sinh được code như ý muốn. Do đó bạn cần clone [`generator-jhipster`](https://github.com/jhipster/generator-jhipster) về máy để:

- Tra cứu mọi thứ nhanh chóng.
- Sao chép code, đặc biệt là biểu mẫu về blueprint rồi sửa trên đó.

{{< hint warning >}}
**Ghi nhớ**

Bạn phải checkout đến đúng tag release tương ứng với phiên bản JHipster đang cài ở máy.
{{< /hint >}}

## Nguyên lý sub-generator

Kiến trúc và tên gọi class của v8 có nhiều thay đổi so với v7, đáng chú ý là _mọi blueprint sẽ kế thừa từ lớp `BaseApplicationGenerator` chứ không phải từ lớp của sub-generator gốc như v7_.

{{< mermaid class="text-center" >}}
flowchart LR
  subgraph JHipster
    CoreGenerator --> YeomanGenerator
    BaseGenerator --> CoreGenerator
    BaseApplicationGenerator --> BaseGenerator

    CypressGenerator --> BaseApplicationGenerator
    AngularGenerator --> BaseApplicationGenerator
    VueGenerator --> BaseApplicationGenerator
    HerokuGenerator --> BaseApplicationGenerator

    ClientGenerator --> BaseApplicationGenerator
    ReactGenerator --> BaseApplicationGenerator
    LanguagesGenerator --> BaseApplicationGenerator
    ServerGenerator --> BaseApplicationGenerator
    JavaGenerator --> BaseApplicationGenerator
  end

  subgraph MyBlueprint
    MyClientGenerator --> BaseApplicationGenerator
    MyReactGenerator --> BaseApplicationGenerator
    MyLanguagesGenerator --> BaseApplicationGenerator
    MyServerGenerator --> BaseApplicationGenerator
    MyJavaGenerator --> BaseApplicationGenerator
  end
{{< /mermaid >}}

> Danh sách đầy đủ các generator tại: [`generators/generator-list.mjs`](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/generator-list.mjs#L20)

Tổ tiên của mọi sub-generator là `YeomanGenerator`, nên _bạn có thể sử dụng API của Yeoman trong mọi sub-generator_.

{{< hint warning >}}
Ôn lại về Yeoman [tại đây](/docs/fundamentals/yeoman).
{{< /hint >}}

Nguyên tắc khi viết blueprint:

> Khi cần tác động đến code sinh bởi sub-generator nào, bạn cần kế thừa sub-generator đó. Hãy nhìn các sub-generator của `MyBlueprint` ở diagram phía trên.

### BaseGenerator

> Đường dẫn: `generators\generator-base.js`

`BaseGenerator` là lớp cha, mà như tên gọi, đóng vai trò lớp nền tảng của generator JHipster. `BaseGenerator` đã thêm mới cũng như override nhiều method phục vụ cho nghiệp vụ của JHipster, mà các lớp con kế thừa có thể khai thác.

- Thông tin ứng dụng
  - `getFrontendAppName`
- Caching
  - `addEntryToEhcache`
- Build tool
  - `addNpmScript`
  - `buildApplication`
  - `addMavenRepository`
  - `addMavenPlugin`
  - `addGradleProperty`
  - `addGradlePlugin`
- Filesystem
  - `writeFilesToDisk`
  - `editFile`

Danh sách đầy đủ các method các bạn không cần nhớ, hãy tra cứu trực tiếp trong source của `generator-jhipster` khi cần.

### BaseBlueprintGenerator

> Đường dẫn: `generators\generator-base-blueprint.js`

Lớp con của `BaseGenerator` và là lớp cha trực tiếp của mọi sub-generator JHipster. Method quan trọng nhất trong lớp này là [`composeWithBlueprints`](https://github.com/jhipster/generator-jhipster/blob/v7.9.4/generators/generator-base-blueprint.js#L423). Mỗi lớp con trực tiếp đều gọi đến method này.

## Priority

{{< hint info >}}
Xem lại [priority](/docs/fundamentals/yeoman/method-priority.md) trong Yeoman.
{{< /hint >}}

v8 mở rộng lên 21 tầng so với 12 tầng trong v7.

{{< mermaid class="text-center" >}}
flowchart TD
  INI1["**initializing**"]
  PRO2["**prompting**"]
  CON3["**configuring**"]
  COM4["**composing**"]
  LOA5["**loading**"]
  PRE6["**preparing**"]
  CON7["**configuringEachEntity**"]
  LOA8["**loadingEntities**"]
  PRE9["**preparingEachEntity**"]
  PRE101["**preparingEachEntityField**"]
  PRE111["**preparingEachEntityRelationship**"]
  POS121["**postPreparingEachEntity**"]
  DEF131["**default**"]
  WRI141["**writing**"]
  WRI151["**writingEntities**"]
  POS161["**postWriting**"]
  POS171["**postWritingEntities**"]
  LOA181["**loadingTranslations**"]
  INS191["**install**"]
  POS202["**postInstall**"]
  END212["**end**"]

  subgraph JHipster priorities in Run loop
    INI1 --> PRO2 --> CON3 --> COM4 --> LOA5 --> PRE6 --> CON7 --> LOA8 --> PRE9 --> PRE101 --> PRE111 --> POS121 --> DEF131 --> WRI141 --> WRI151 --> POS161 --> POS171 --> LOA181 --> INS191 --> POS202 --> END212
  end
{{< /mermaid >}}
