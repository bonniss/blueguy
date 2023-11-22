---
weight: 2
title: "Chuẩn bị phát triển"
---

# Chuẩn bị phát triển

{{< hint warning >}}
**Phiên bản JHipster: 7.9.4**
{{< /hint >}}

## Mã nguồn JHipster

Khi có ý định tùy biến code, bạn cần khoanh vùng xem cần tác động đến các sub-generator nào trong [`generator-jhipster`](https://github.com/jhipster/generator-jhipster) để sinh được code như ý muốn. Do đó bạn cần clone [`generator-jhipster`](https://github.com/jhipster/generator-jhipster) về máy để:

- Tra cứu mọi thứ nhanh chóng.
- Sao chép code, đặc biệt là biểu mẫu về blueprint rồi sửa trên đó.

{{< hint warning >}}
**Ghi nhớ**

Bạn phải checkout đến đúng tag release tương ứng với phiên bản JHipster đang cài ở máy. Tài liệu này đang dùng bản 7.9.4.
{{< /hint >}}

## Nguyên lý sub-generator

{{< mermaid class="text-center" >}}
flowchart LR
  subgraph JHipster
    PrivateBase --> YeomanGenerator
    BaseGenerator --> PrivateBase
    BaseBlueprintGenerator --> BaseGenerator

    AppGenerator --> BaseBlueprintGenerator
    AwsClientGenerator --> BaseBlueprintGenerator
    AzureAppServiceGenerator --> BaseBlueprintGenerator
    SpringServiceGenerator --> BaseBlueprintGenerator
    OpenApiClientGenerator --> BaseBlueprintGenerator
    HerokuGenerator --> BaseBlueprintGenerator

    ClientGenerator --> BaseBlueprintGenerator
    EntityClientGenerator --> BaseBlueprintGenerator
    ServerGenerator --> BaseBlueprintGenerator
    EntityServerGenerator --> BaseBlueprintGenerator
    LanguagesGenerator --> BaseBlueprintGenerator
    EntityI18nGenerator --> BaseBlueprintGenerator
  end

  subgraph MyBlueprint
    MyClientGenerator --> ClientGenerator
    MyEntityClientGenerator --> EntityClientGenerator
    MyServerGenerator --> ServerGenerator
    MyEntityServerGenerator --> EntityServerGenerator
    MyLanguagesGenerator --> LanguagesGenerator
    MyEntityI18nGenerator --> EntityI18nGenerator
  end
{{< /mermaid >}}

> Danh sách đầy đủ các generator tại đường dẫn: `generators\generator-list.js`

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

JHipster mở rộng số hàng đợi ưu tiên so với Yeoman.

{{< mermaid class="text-center" >}}
flowchart TD
  INI["**initializing**"]
  PRO["**prompting**"]
  CON["**configuring**"]
  COM["**composing**"]
  LOA["**loading**"]
  PRE["**preparing**"]
  DEF["**default**"]
  WRI["**writing**"]
  POW["**postWriting**"]
  INS["**install**"]
  POI["**postInstall**"]
  END["**end**"]

  subgraph JHipster priorities in Run loop
    INI --> PRO --> CON --> COM --> LOA --> PRE --> DEF --> WRI --> POW --> INS --> POI --> END
  end
{{< /mermaid >}}
