---
sidebar_label: Types
---

# Các loại Wordle

Đối với các bước tiếp theo, chúng tôi sẽ tạo các loại để sử dụng cho các thông điệp chúng tôi đã tạo.

## Các loại Wordle khung

```sh
ignite scaffold map wordle word submitter --no-message
```

Loại này là một bản đồ được gọi là `Wordle` với hai giá trị là `word` và `submitter`. `submitter` là địa chỉ của người đã gửi Wordle.

Loại thứ hai là loại `Guess`. Nó cho phép chúng tôi lưu trữ dự đoán mới nhất cho từng địa chỉ đã gửi giải pháp.

```sh
ignite scaffold map guess word submitter count --no-message
```

Tại đây, chúng tôi cũng đang lưu trữ `count` để đếm xem địa chỉ này đã gửi bao nhiêu lượt đoán.
