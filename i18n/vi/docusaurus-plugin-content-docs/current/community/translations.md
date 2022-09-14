- - -
sidebar_label: Bản dịch Tài liệu
- - -

# Hỗ trợ dịch thuật cộng đồng

Nếu bạn là một thành viên cộng đồng Celestia đầy nhiệt huyết muốn đóng góp dịch các trang tài liệu, thì đây là hướng dẫn dành cho bạn.

## Hãy xem qua dự án Crowdin của chúng tôi

Để bắt đầu, hãy đi đến trang dự án Crowdin [tại đây](https://crowdin.com/project/celestia-docs).

Bạn sẽ cần phải tạo một tài khoản và bạn sẽ có thể tham gia vào dự án để bắt đâu hành trình dịch thuật của mình.

Nếu bạn không tìm thấy ngôn ngữ chính của mình, hãy yêu cầu nó tại kênh `#translations` trong [Discord](https://discord.gg/celestiacommunity) của dự án.

Trên trang Crowdin, bạn có thể dịch, bình luận về các bài dịch, đồng thời bỏ phiếu upvotes và downvotes đối với các bản dịch đã tồn tại.

Hãy đưa ra ý kiến của riêng mình đối với các bản dịch đã tồn tại để đảm bảo rằng nó chính xác!

## Tips

Sau đây là một số tips sẽ giúp ích cho bạn trong quá trình dịch thuật.

### Tài liệu của Crowdin

Bạn có thể tìm thấy các tài liệu chính thức của Crowdin [tại đây](https://support.crowdin.com/online-editor).

### Hướng dẫn

#### Code

Một số trang có chưa metadata và các dòng code máy tính.

Hãy nhớ rằng William Shakespeare là một người nói tiếng anh... Và Alan Turing cũng vậy! Đó là lí do vì sao bạn không nên dịch các phần mà bản thân nó là code.

Ví dụ, bạn có thể thấy metadata, ví dụ như `sidebar_label : Hello World`, bản dịch tiếng Pháp nên là `sidebar_label : Salut tout le monde`.

Hãy cùng đến với một ví dụ khác, bạn sẽ không cần phải dịch bất kì thứ gì dưới đây:

```sh
cd $HOME
rm -rf celestia-app
git clone https://github.com/celestiaorg/celestia-app.git
cd celestia-app/
APP_VERSION=$(curl -s \
  https://api.github.com/repos/celestiaorg/celestia-app/releases/latest \
  | jq -r ".tag_name")
git checkout tags/$APP_VERSION -b $APP_VERSION
make install
```

Hơn nữa, bạn không cần phải dịch URLs sang ngôn ngữ địa phương của bạn.

#### Từ ngữ chuyên môn

Khi bạn dịch các khái niệm đổi mới, chẳng hạn như Data Availability Sampling, hãy thảo luận về bản dịch tốt nhất với phần còn lại của cộng đồng.

Ngoài ra, hãy cẩn thận với thứ tự ngày, khoảng thời gian và dấu phẩy liên quan đến số khi dịch từ ngôn ngữ này sang ngôn ngữ khác.
