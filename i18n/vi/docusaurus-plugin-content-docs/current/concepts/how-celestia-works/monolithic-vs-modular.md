- - -
sidebar_label : So sánh giữa Monolithic Blockchains và Modular Blockchains
- - -

# So sánh mô hình blockchain nguyên khối và mô hình blockchain mô-đun

Blockchains khởi tạo [bản sao của máy trạng thái](https://dl.acm.org/doi/abs/10.1145/98163.98167): các nút không được cấp phép trong mạng phân tán áp dụng một trình tự có thứ tự của các giao dịch xác định đến trạng thái ban đầu dẫn đến một trạng thái cuối cùng. Điều này có nghĩa là các blockchains yêu cầu bốn chức năng sau:

- __ Thực thi __ yêu cầu thực hiện các giao dịch cập nhật trạng thái một cách chính xác. Do đó, việc thực hiện phải đảm bảo rằng chỉ các giao dịch hợp lệ mới được thực hiện, tức là các giao dịch dẫn đến sự chuyển đổi hợp lệ trong máy trạng thái.
- __ Sự dàn xếp __ đòi hỏi một môi trường cho các lớp thực thi để xác minh các bằng chứng, giải quyết các tranh chấp gian lận và làm cầu nối giữa các lớp thực thi khác.
- __ Sự đồng thuận __ đòi hỏi sự đồng ý về thứ tự của các giao dịch.
- __ Sự sẵn sàng của dữ liệu __ (DA) đòi hỏi phải cung cấp dữ liệu giao dịch. Lưu ý rằng việc thực thi, giải quyết và đồng thuận yêu cầu sự sẵn sàng của dữ liệu.

Các blockchains truyền thống, tức là _ blockchains nguyên khối _, thực hiện cả bốn hoạt động cùng nhau trong một lớp đồng thuận cơ sở duy nhất. Vấn đề với blockchains nguyên khối là lớp đồng thuận phải thực hiện rất nhiều các nhiệm vụ khác nhau và nó không thể được tối ưu hóa chỉ cho một trong các chức năng này. Kết quả là, mô hình nguyên khối giới hạn thông lượng của hệ thống.

![So sánh giữa Monolithic Blockchains và Modular Blockchains](/img/concepts/monolithic-modular.png)

Như một giải pháp, các blockchains mô-đun tách các chức năng này giữa các nhiều lớp chuyên biệt như một phần của ngăn xếp mô-đun. Bởi vì tính linh hoạt mà sự chuyên môn hóa mang lại, có nhiều khả năng trong đó ngăn xếp đó có thể được sắp xếp. Ví dụ, một trong những cách sắp xếp như vậy là sự tách biệt của bốn chức năng thành ba lớp chuyên biệt.

Lớp cơ sở bao gồm sự sẵn sàng của dữ liệu và sự đồng thuận và do đó, được gọi là dưới dạng lớp đồng thuận và DA (hay nói ngắn gọn là lớp DA), trong khi việc giải quyết và thực hiện được di chuyển trong các lớp của riêng chúng. Kết quả là, mọi lớp đều có thể được chuyên biệt hóa để chỉ thực hiện một cách tối ưu chức năng của nó và do đó, tăng thông lượng của hệ thống. Hơn nữa, mô hình mô-đun này cho phép nhiều lớp thực thi, tức là [ cuộn vào ](https://vitalik.ca/general/2021/01/05/rollup.html), để sử dụng chung lóp giải quyết và DA.
