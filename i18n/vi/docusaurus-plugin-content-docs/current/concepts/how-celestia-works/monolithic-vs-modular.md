- - -
sidebar_label : So sánh giữa Monolithic Blockchains và Modular Blockchains
- - -

# So sánh mô hình blockchain nguyên khối và mô hình blockchain mô-đun

Blockchains khởi tạo [bản sao của máy trạng thái](https://dl.acm.org/doi/abs/10.1145/98163.98167): các nút không được cấp phép trong mạng phân tán áp dụng một trình tự có thứ tự của các giao dịch xác định đến trạng thái ban đầu dẫn đến một trạng thái cuối cùng. Điều này có nghĩa là các blockchains yêu cầu bốn chức năng sau:

- __ Thực thi __ yêu cầu thực hiện các giao dịch cập nhật trạng thái một cách chính xác. Do đó, việc thực hiện phải đảm bảo rằng chỉ các giao dịch hợp lệ mới được thực hiện, tức là các giao dịch dẫn đến sự chuyển đổi hợp lệ trong máy trạng thái.
- __ Sự dàn xếp __ đòi hỏi một môi trường cho các lớp thực thi để xác minh các bằng chứng, giải quyết các tranh chấp gian lận và làm cầu nối giữa các lớp thực thi khác.
- __ Sự đồng thuận __ đòi hỏi sự đồng ý về thứ tự của các giao dịch.
- __ Sự sẵn sàng của dữ liệu __ (DA) đòi hỏi phải cung cấp dữ liệu giao dịch. Note that execution, settlement, and consensus require DA.

Traditional blockchains, i.e. _monolithic blockchains_, implement all four functions together in a single base consensus layer. The problem with monolithic blockchains is that the consensus layer must perform a lot of different tasks and it cannot be optimized for only one of these functions. As a result, the monolithic paradigm limits the throughput of the system.

![Modular VS Monolithic](/img/concepts/monolithic-modular.png)

As a solution, modular blockchains decouple these functions among multiple specialized layers as part of a modular stack. Due to the flexibility that specialization provides, there are many possibilities in which that stack can be arranged. For example, one such arrangement is the separation of the four functions into three specialized layers.

The base layer consists of DA and consensus and thus, is referred to as the Consensus and DA layer (or for brevity, the DA layer), while both settlement and execution are moved on top in their own layers. As a result, every layer can be specialized to optimally perform only its function and thus, increase the throughput of the system. Furthermore, this modular paradigm enables multiple execution layers, i.e., [rollups](https://vitalik.ca/general/2021/01/05/rollup.html), to use the same settlement and DA layers.
