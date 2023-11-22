---
weight: 4
title: "Sao lưu cấu hình và `.yo-rc.json`"
---

# Sao lưu cấu hình và `.yo-rc.json`

Chia sẻ cấu hình chung giữa các generator là một tác vụ phổ biến: các câu trả lời của người dùng ở tầng `prompting`, cấu hình cho các generator con (sub-generator)...

Các cấu hình này được lưu trong file `.yo-rc.json` thông qua [API Yeoman Storage](https://yeoman.github.io/generator/Storage.html) tại `this.config`.

## Tác vụ thường dùng của Yeoman Storage

### `this.config.save()`

Ghi cấu hình vào `.yo-rc.json`, tự động tạo file nếu không tồn tại.

### `this.config.set()`

Đặt giá trị cho key. Cho phép đặt nhiều key khi truyền vào một object.

Lưu ý chỉ chấp nhận các giá trị có thể serialize thành JSON được (number, string, object không đệ quy).

### `this.config.get()`

### `this.config.getAll()`

### `this.config.delete()`

### `this.config.defaults()`

Nhận vào một object để dùng như giá trị mặc định. Bỏ qua các cặp `key/value` đã tồn tại, các `key` không tồn tại sẽ lấy từ object mặc định.

{{< hint info >}}
Tra cứu đầy đủ [API Yeoman Storage](https://yeoman.github.io/generator/Storage.html).
{{< /hint >}}

## `.yo-rc.json`

Là file JSON lưu chung cấu hình từ nhiều generator, mỗi generator có namespace riêng:

```json
{
  "generator-backbone": {
    "requirejs": true,
    "coffee": true
  },
  "generator-gruntfile": {
    "compass": false
  }
}
```

Nội dung file là dễ đọc, dễ hiểu, dễ sửa. Với các cấu hình phức tạp, người dùng có thể sửa trực tiếp trên file không cần thông qua Storage API để tránh phải trả lời nhiều câu hỏi.

## Tham khảo

- [[Yeoman docs] Managing Configuration](https://yeoman.io/authoring/storage)
