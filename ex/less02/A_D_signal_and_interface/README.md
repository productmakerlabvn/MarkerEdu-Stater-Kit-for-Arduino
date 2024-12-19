# Tín hiệu Analog, Digital và các chuẩn giao tiếp cơ bản

## Tín hiệu tương tự (Analog) và tín hiệu số (Digital)

Sau khi mạch điện được cấp nguồn, dòng điện chạy trong mạch qua các linh kiện điện tử sẽ tạo thành hai dạng tín hiệu điện cơ bản là tín hiệu số (Digital) và tín hiệu tương tự (Analog) để các linh kiện điện tử có thể ghi nhận, xử lý, giao tiếp với nhau hoặc giao tiếp với các hệ thống mạch điện khác, điểm khác nhau chính giữa hai dạng tín hiệu là tín hiệu tương tự (Analog) mang tính liên tục là một giải giá trị còn tín hiệu số (Digital) mang tính rời rạc và chỉ là các giá trị cố định.

![](/ex/less02/A_D_signal_and_interface/image/01_Digital-and-Analog-Signal.jpg)  
Biểu thị dạng tín hiệu Analog và Digital theo thời gian.  

Các thông tin của môi trường xung quanh ta hầu hết đều mang tính liên tục theo thời gian: nhiệt độ, độ ẩm, âm thanh, ánh sáng,..., các thông tin này sau khi đi qua các thành phần chuyển đổi (như cảm biến) của mạch điện sẽ được chuyển thành tín hiệu điện ở dạng tương tự (Analog) để mạch điện có thể ghi nhận và xử lý , ngược lại các mạch điện cũng có thể tạo ra các tín hiệu ở dạng tương tự (Analog) để có thể xuất thông tin trở lại môi trường thông qua các linh kiện vật lý như: Loa (âm thanh), đèn (ánh sáng), điện trở nhiệt (nhiệt độ),...., nhược điểm của tín hiệu tương tự (Analog) là dễ bị nhiễu, suy giảm tín hiệu khi truyền đi xa cũng như khả năng lưu trữ, giao tiếp bằng tín hiệu tương tự rất khó và mất nhiều tài nguyên.

![](/ex/less02/A_D_signal_and_interface/image/02_1050px-LM35_vout_chart.jpg)  
Bảng thông số điện áp Analog tại chân tín hiệu OUT tương ứng với nhiệt độ của cảm biến LM35

Tín hiệu số (Digital) là dạng tín hiệu rời rạc được tạo ra dưới dạng các bit 0 (Low) / 1(High), nó ít bị nhiễu hoặc suy giảm tín hiệu khi truyền đi xa, đồng thời việc lưu trữ và giao tiếp thông tin bằng tín hiệu số cũng rất đơn giản.

Để có thể truyền tải đi xa và lưu trữ các tín hiệu điện ở dạng tương tự (Analog) ta thường chuyển (mã hoá) các tín hiệu này thành tín hiệu số (Digital) thông qua các mạch chuyển đổi tín hiệu tương tự sang tín hiệu số ADC (Analog to Digital Converter), ngược tại để phục hồi từ tín hiệu số sang tín hiệu tương tự ta cũng có các mạch chuyển đổi tín hiệu số sang tín hiệu tương tự DAC (Digital to Analog Converter).

![](/ex/less02/A_D_signal_and_interface/image/03_Fig5-an-introduction-to-digital-signal-processing.jpg)
Mô phỏng việc chuyển đổi giữa tín hiệu Analog sang Digital và ngược lại

Khi sử dụng tín hiệu tương tư (Analog) hoặc tín hiệu số (Digital) cần lưu ý về mức điện áp giao tiếp của thiết bị (như Arduino là 0\~5VDC hoặc Raspberry Pi, Micro:bit là 0\~3.3VDC), nếu giao tiếp quá mức điện áp quy định sẽ làm cháy (hư hỏng) thiết bị.

Dưới đây là bảng điện áp định mức chuẩn TTL của thiết bị sử dụng 5VDC (như Arduino Uno), ta thấy trong bảng thì từ 2\~5VDC sẽ được hiểu là mức cao (HIGH) và từ 0\~0.8VDC sẽ được hiểu là mức thấp (LOW):

![](/ex/less02/A_D_signal_and_interface/image/04_518d5681ce395f1e11000000.jpg)
Bảng định mức điệp áp Digital TTL của thiết bị sử dụng 5VDC

## Các chuẩn giao tiếp cơ bản

Các tín hiệu số (Digital) khi được kết hợp với nhau theo một quy chuẩn sẽ tạo thành các chuẩn giao tiếp, mỗi chuẩn giao tiếp khác nhau sẽ có tốc độ, phương thức truyền nhận khác nhau, sau đây là giới thiệu sơ bộ về hai chuẩn giao tiếp cơ bản được sử dụng phổ biến trong lập trình Vi Điều Khiển là UART và I2C:

### Chuẩn giao tiếp UART

![](/ex/less02/A_D_signal_and_interface/image/05_UART_Protocol.jpg)  
Kết nối giữa hai thiết bị sử dụng chuẩn giao tiếp UART.

Chuẩn giao tiếp UART là viết tắt của (Universal Asynchronous Receiver/Transmitter) tức giao tiếp truyền nhận chung không đồng bộ, sở dĩ nói là không đồng bộ vì trong chuẩn giao tiếp này không sử dụng tín hiệu Clock (xung nhịp) như các chuẩn giao tiếp khác, chuẩn giao tiếp UART được sử dụng rất phổ biến hiện nay để giao tiếp giữa 2 thiết bị với nhau với chỉ 2 dây tín hiệu là Tx (Transmitter) và Rx (Receiver). Các lưu ý khi sử dụng chuẩn giao tiếp UART:

Để kết nối giữa hai thiết bị ta đấu nối chéo chân Rx (Receiver) của thiết bị UART1 nối với chân Tx (Transmitter) của thiết bị UART2 và ngược lại, chân Tx (Transmitter) của UART1 nối với chân Rx (Receiver) của thiết bị UART2.
Các thông số cấu hình giao tiếp UART giữa hai thiết bị như: Baudrate, Data bits, Stop bits, Parity,...phải đồng bộ với nhau, đặc biệt là thông số Baudrate (tốc độ truyền).

### Các chuẩn giao tiếp cơ bản

![](/ex/less02/A_D_signal_and_interface/image/06_900px-I2C-SDA-SCL-01.jpg)  
Kết nối giữa các thiết bị sử dụng chuẩn giao tiếp I2C.

Chuẩn giao tiếp I2C là viết tắt của (Inter-Integrated Circuit) tức chuẩn giao tiếp nội bộ giữa các mạch tích hợp ban đầu được phát triển bởi công ty Phillips Corporation (nay là NXP Semiconductors) giúp kết nối giữa nhiều thiết bị tốc độ thấp với nhau thông qua hai dây tín hiệu là SDA (Serial Data) và SCL (Serial Clock), trong chuẩn kết nối I2C sẽ có một thiết bị chủ (Master) kết nối với một hoặc nhiều thiết bị tớ (Slave), các thiết bị tớ (Slave) này sẽ được thiết lập địa chỉ khác nhau, khi cần giao tiếp thiết bị chủ (Master) sẽ gọi địa chỉ của thiết bị tớ (Slave) cần giao tiếp để trao đổi thông tin. Các lưu ý khi sử dụng chuẩn giao tiếp I2C:

Các thiết bị chủ (Master) và tớ (Slave) được kết nối song song qua hai dây tín hiệu là SDA (dữ liệu) và SCL (xung nhịp), hai dây tín hiệu này cần được nối lên mức cao (High) của mức áp giao tiếp bằng điện trở kéo phù hợp.
Các thiết bị tớ (Slave) sẽ được thiết lập địa chỉ duy nhất, không trùng với các thiết bị khác trong mạng sẽ gây ra lỗi.  

## Tham khảo  

- [Logic Levels - SparkFun Learn](https://learn.sparkfun.com/tutorials/logic-levels)
