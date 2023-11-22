---
weight: 2
bookFlatSection: false
bookCollapseSection: true
title: "JHipster Blueprint"
---

# Blueprint trong JHipster

## Tại sao cần đến blueprint

JHipster đã làm rất tốt nhiệm vụ sinh code của mình. Trong nhiều trường hợp, ta chỉ muốn tùy chỉnh một phần trên nền code sinh ra:

- Tùy chỉnh giao diện: sửa CSS, thay thế UI bằng thư viện khác (mặc định code phía client sử dụng Bootstrap).
- Tùy chỉnh tính năng: tùy chỉnh API và tùy chỉnh giao diện tương ứng.
- Xóa bớt tính năng: xóa bớt màn hình, tính năng không dùng đến.
- Cá nhân hóa code sinh ra: thay thế logo, từ khóa JHipster bằng thương hiệu cá nhân, tổ chức khai thác code.
- Thêm sửa xóa, bổ sung bản dịch ngôn ngữ mà JHipster chưa hỗ trợ hoặc thiếu sót.

Nếu chỉ chỉnh sửa vài chỗ như bản dịch ngôn ngữ, xóa logo, ta có thể sinh code rồi sửa. Nhưng khi các chỉnh sửa giống nhau cho rất nhiều phần, tiêu biểu nhất là code của thực thể: ứng dụng có 10 thực thể với 10 tập trang CRUD, chỉ một nỗ lực sửa nút "Tạo mới" thành màu cam thôi đòi hỏi ta phải sửa ít nhất 10 chỗ ứng với từng trang listing từng thực thể.

Thông qua blueprint ta có thể tùy biến code sinh ra từ JHipster ở phạm vi tùy ý, thậm chí viết lại hoàn toàn code server hoặc client, giúp tiết kiệm thời gian và công sức.

{{< hint info >}}
**Phiên bản là quan trọng**

Blueprint là thành phần thường xuyên được tái cấu trúc và cập nhật code ở mỗi bản major release của JHipster. Vì thế lập trình viên phải nắm rõ mình đang làm việc với blueprint phiên bản nào. Mục tài liệu này dành riêng cho blueprint sẽ thống nhất gọi phiên bản blueprint theo phiên bản JHipster.
{{< /hint >}}

Tài liệu được viết cho hai phiên bản:

- [Blueprint v7](/docs/blueprint/v7): JHipster v7, tiêu biểu là phiên bản `7.9.4`
- [Blueprint v8](/docs/blueprint/v8): JHipster v8, tiêu biểu là phiên bản `8.0.0`

## Blueprint chính thức

Danh sách các blueprint chính thức từ đội ngũ JHipster xem [tại đây](https://www.jhipster.tech/modules/official-blueprints/).

Một số blueprint tiêu biểu:

- [jhipster-kotlin](https://github.com/jhipster/jhipster-kotlin): thay thế backend Java bằng Kotlin
- [jhipster-dotnetcore](https://github.com/jhipster/jhipster-dotnetcore): thay thế backend Java bằng .NET Core.
- [jhipster-nodejs](https://github.com/jhipster/generator-jhipster-nodejs): thay thế backend Java bằng Node.js (framework Nest.js)
- [jhipster-react-native](https://github.com/jhipster/generator-jhipster-react-native): tạo client là ứng dụng React Native.
- [jhipster-svelte](https://github.com/jhipster/generator-jhipster-svelte): tạo client là ứng dụng SvelteKit.

## Chợ Plugin

Bên cạnh các plugin chính thức còn có các plugin đến từ cộng đồng. Tra cứu tại [chợ plugin](https://www.jhipster.tech/modules/marketplace).
