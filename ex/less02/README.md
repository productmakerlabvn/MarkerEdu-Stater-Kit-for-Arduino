# Bài 2: Điều khiển đèn Led chớp tắt với Arduino (Digital Out) - MakerEdu Starter Kit for Arduino

# Mô tả dự án

![](/ex/less02/image/01_750px-Blink_Led.jpg)
Blink Led

Trong bài này các bạn sẽ học cách xuất tín hiệu Digital từ các chân tín hiệu của Vietduino Uno, tín hiệu Digital chỉ có 2 mức trạng thái là `HIGH` và `LOW`, tương ứng với mức điện áp `5V` và `0V` tại các chân tín hiệu, chỉ đơn giản như vậy nhưng tín hiệu Digital được sử dụng cho hầu hết các ứng dụng cần có sự lựa chọn có hoặc không như: bật tắt đèn, động cơ, máy bơm,...

> Lưu ý:  
 Các chân tín hiệu Digital của Arduino chỉ có thể cung cấp được điện áp 5VDC với dòng điện tối đa 40mA nên chỉ sử dụng được với những thiết bị công suất nhỏ như đèn Led (15~20mA), các thiết bị công suất lớn như động cơ cần sử dụng thêm mạch công suất đi kèm để điều khiển.  
Các bạn có thể tìm hiểu thêm bài viết về [Tín hiệu Analog, Digital và các chuẩn giao tiếp cơ bản](/ex/less02/A_D_signal_and_interface/README.md).

## Video

[![](/ex/less02/image/02_video.png)](https://youtu.be/wrWSUBqg77A)

## Các bước thực hiện

### Danh sách thiết bị

- [1x Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- [1x Mạch MakerEdu Shield for Vietduino](https://makerlab.vn/vietduinosd)
- [1x Cáp USB-C](https://hshop.vn/cap-usb-type-c)
- [1x Mạch led đơn MKE-M01 10mm single LED module](https://makerlab.vn/mkem01)

### Chuẩn bị trước dự án

- Kết nối mạch MakerEdu Shield với mạch Vietduino Uno [theo hướng dẫn](https://makerlab.vn/vietduinosd).
- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch MakerEdu Shield phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino [theo hướng dẫn tại đây](https://makerlab.vn/vuno).
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.
![](/ex/less02/image/03_.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

### Chương trình

Mở phần mềm IDE Arduino và tạo một chương trình (Sketch) mới.
Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) [theo hướng dẫn tại đây](https://makerlab.vn/vuno).

```ino
// Blink Led Example - MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

# define LED_PIN 9 // Khai báo chân điều khiển đèn Led là D9

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Thiết lập chân D9 là Output để điều khiển Led
  pinMode(LED_PIN, OUTPUT);
}

/*-----------------------------------------------------*/

void loop()
{
  // Bật đèn Led trong 1s và in thông báo "ON"
  digitalWrite(LED_PIN, HIGH);
  Serial.println("ON");
  delay(1000);

  // Tắt đèn Led trong 1s và in thông báo "OFF"
  digitalWrite(LED_PIN, LOW);
  Serial.println("OFF");
  delay(1000);
}
```

### Giải thích code

Chương trình gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`

- `void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)
-
- `Serial.begin(baudrate)`: khởi động cổng kết nối Serial trên mạch Vietduino Uno với tốc độ (baudrate) mong muốn, các tốc độ hỗ trợ thường là: 9600, 115200,...
- `pinMode(Pin, OUTPUT)`: cấu hình chân tín hiệu là chân xuất tín hiệu (OUTPUT) để điều khiển đèn Led.
- `void loop()`: (chứa các câu lệnh chạy lặp đi lặp lại )
-
- `digitalWrite(Pin, HIGH);`: Xuất tín hiệu Digital mức cao (HIGH) ra chân tín hiệu giúp bật đèn Led.
- `Serial.println(data)`: gửi dữ liệu từ mạch Vietduino Uno lên máy tính kèm theo ký tự xuống dòng, nếu dữ liệu là kiểu ký tự thì cần để trong dấu "".
- `delay(time)`: yêu cầu Vietduino Uno chờ (không làm gì cả) trong một khoảng thời gian nhất định, đơn vị là mili giây (ms).
- `digitalWrite(Pin, LOW);`: Xuất tín hiệu Digital mức thấp (LOW) ra chân tín hiệu giúp tắt đèn Led.

### Kết nối phần cứng

<table><thead>
  <tr>
    <th>MakerEDU Shield</th>
    <th>Thiết bị</th>
    <th>Cáp kết nối</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Port D9</td>
    <td><a href="https://makerlab.vn/mkem01">Mạch led đơn MKE-M01 10mm single LED module</a></td>
    <td><a href="https://hshop.vn/products/cap-ket-noi-makeredu-xh2-54-3wires-20cm-cable">MakerEdu XH2.54 3Wires</a></td>
  </tr>
</tbody>
</table>

### Kết quả

Sau khi đã nạp code thành công, nhấn vào *"biểu tượng kính lúp"* hoặc chọn *Tools > Serial* Monitor để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy chương trình hoạt động, Module Led kết nối tại chân D9 sẽ chớp tắt 1s một lần kèm theo trạng thái hiển thị lên Serial Monitor:

![](/ex/less02/image/04_.jpg)
Module Led kết nối với Port D9 trên MakerEdu Shield chớp tắt 1 giây / 1 lần.

![](/ex/less02/image/05_1050px-Screenshot_2023-07-08_at_17.05.57.png)
Serial Monitor trên phần mềm Arduino hiển thị trạng thái của đèn Led.

## Bài tập thêm

<ins>Bài tập 1:</ins>

- Sử dụng **Serial Monitor** để điều khiển đèn Led từ máy tính với ký tự `'1'` là **bật đèn** và `'0'` là **tắt đèn** / [Lời giải](/solution/README.md).

## Tài liệu tham khảo

## Tài liệu tham khảo

- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 1: Xin chào Arduino! (Serial Monitor)](/ex/less01/README.md)
- [Bài 3: Nhận tín hiệu từ nút nhấn với Arduino (Digital In)](/ex/less03/README.md)
