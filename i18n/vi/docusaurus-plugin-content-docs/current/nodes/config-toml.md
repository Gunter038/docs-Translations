- - -
sidebar_label : Config.toml Guide
- - -

# Config.toml breakdown

- [Config.toml breakdown](#configtoml-breakdown)
  - [Điều kiện tiên quyết](#pre-requisites)
  - [Understanding config.toml](#understanding-configtoml)
    - [[Core]](#core)
    - [[P2P]](#p2p)
      - [Bootstrap](#bootstrap)
      - [Mutual Peers](#mutual-peers)
    - [[Services]](#services)
      - [TrustedHash and TrustedPeer](#trustedhash-and-trustedpeer)

## Điều kiện tiên quyết

Vui lòng đảm bảo rằng bạn đã cài đặt và khởi chạy node celestia

## Understanding config.toml

Sau khi khởi tạo, đối với bất kỳ loại nút nào, bạn sẽ thấy ` config.toml ` trong đường dẫn sau (vị trí mặc định):

- `$HOME/.celestia-bridge/config.toml` for bridge node
- `$HOME/.celestia-light/config.toml` for light node

Hãy chia nhỏ một số phần được sử dụng nhiều nhất.

### [Core]

Phần này cần thiết cho Celestia Bridge Node. By default, `Remote = false`. Vẫn dành cho devnet, chúng tôi sẽ sử dụng tùy chọn chính từ xa và điều này cũng có thể được thiết lập bằng dòng lệnh ` --core.remote `.

### [P2P]

#### Bootstrap

Bootstrappers giúp các nodes mới tìm thấy các đối tác trong mạng nhanh hơn. By default, the `Bootstrapper = false` and the `BootstrapPeers` is empty. Nếu bạn muốn node của mình là một bootstrapper, hãy kích hoạt ` Bootstrapper = true `. ` BootstrapPeers ` được cung cấp mặc định trong quá trình khởi tạo. Nếu bạn muốn thêm riêng của mình theo cách thủ công, bạn cần cung cấp nhiều địa chỉ ví của các đối tác.

#### Đối tác của nhau

Mục đích của cấu hình này là thiết lập giao tiếp hai chiều. Điều này thường xảy ra đối với các Celestia Bridge Nodes. Ngoài ra, bạn cần thay đổi trường ` PeerExchange ` từ false thành true.

### [Services]

#### TrustedHash and TrustedPeer

` TrustedHash ` là cần thiết để khởi tạo Celestia Bridge Node đúng cách với một node Celestia Core ` Remote ` đã chạy. Celestia Light Node sẽ lấy một mã genesis hash tin cậy, nếu không có hàm hash được cung cấp thủ công trong giai đoạn khởi tạo.

` TrustedPeers ` là mảng của các đối tác Bridge Nodes mà Celestia Light Node tin tưởng. Theo mặc định, các bootstrap trở thành các đối tác đáng tin cậy đối với Celestia Light Nodes nếu người dùng không đặt thông số ngang hàng tin cậy trong tệp cấu hình.

Bất kỳ Celestia Bridge Node nào cũng có thể là một ứng dụng ngang hàng đáng tin cậy cho Light. Tuy nhiên, Light Node theo thiết kế không thể là một ứng dụng ngang hàng đáng tin cậy cho một Light Node khác.
