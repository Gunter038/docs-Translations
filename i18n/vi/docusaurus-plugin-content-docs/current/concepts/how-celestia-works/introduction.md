# Giới thiệu

Celestia là một modular blockchain với mục tiêu xây dựng một [lớp Data Availability](https://blog.celestia.org/celestia-a-scalable-general-purpose-data-availability-layer-for-decentralized-apps-and-trust-minimized-sidechains/) có khả năng mở rộng, mở đường cho thế hệ kiến trúc blockchains có thể mở rộng tiếp theo - [ modular blockchains](https://celestia.org/learn/). Celestia mở rộng bằng cách [tách rời lớp thực thi khởi lớp đồng thuận](https://arxiv.org/abs/1905.09274) và giới thiệu một triết lý mới, [Data Availability sampling](https://arxiv.org/abs/1809.09044).

Điều đầu tiên đồng nghĩa rằng Celestia chỉ chịu trách nhiệm cho việc sắp xếp giao dịch và đảm bảo tính khả dụng của dữ liệu; nó tương tự với việc [giảm sự đồng thuận đối với việc truyền phát ở mức độ nguyên tử](https://en.wikipedia.org/wiki/Atomic_broadcast#Equivalent_to_Consensus).

Điều thứ hai cung cấp một giải pháp hiệu quả cho [vấn đề về tính khả dụng của dữ liệu](https://coinmarketcap.com/alexandria/article/what-is-data-availability) bằng cách chỉ yêu cầu các light node giới hạn về tài nguyên lấy mẫu một số lượng nhỏ các chunk ngẫu nhiên từ mỗi khối để xác minh tính khả dụng của dữ liệu.

Điều thú vị là càng nhiều light node tham gia vào việc lấy mẫu sẽ tăng lượng dữ liệu mà mạng có thể xử lý một cách an toàn, cho phép kích thước khối tăng lên mà không làm tăng chi phí để xác minh chuỗi.
