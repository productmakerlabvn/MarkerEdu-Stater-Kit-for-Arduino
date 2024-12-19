# Bài 1: Xin chào Arduino! (Serial Monitor) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

![](/ex/less01/image/01_1050px-Serial-monitor-new-editor.png)
Arduino Serial Monitor
Trong bài thực hành đầu tiên này các bạn sẽ học cách giao tiếp giữa mạch Vietduino Uno và máy tính qua công cụ Serial Monitor trên phần mềm Arduino, qua đó các bạn có thể gửi các dữ liệu từ Vietduino Uno lên máy tính và ngược lại sẽ giúp bạn thực hiện vô số các ứng dụng sau này như:

- Đọc dữ liệu từ cảm biến kết nối với Vietduino Uno lên máy tính.

- Điều khiển Vietduino Uno bằng máy tính.

- Điều khiển máy tính bằng Vietduino Uno.

- Thông báo tiến trình hoạt động của Vietduino lên máy tính để kiểm tra lỗi (Debug),...

> Lưu ý:
Các bạn có thể tìm hiểu thêm bài viết về Cách sử dụng Serial Monitor & Serial Plotter trên phần mềm Arduino.

## Video

[![](/ex/less01/image/02_video.png)](https://youtu.be/PxlqWvR3NM0)

## Các bước thực hiện

### Danh sách thiết bị

- 1x [Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- 1x [Cáp USB-C](https://hshop.vn/cap-usb-type-c)

### Chuẩn bị trước dự án

- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino [theo hướng dẫn tại đây](https://makerlab.vn/vuno).
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.

![](/ex/less01/image/03_1050px-Vietduino_Uno_connect_with_Computer.jpg)
Kết nối Vietduino Uno với máy tính bằng cáp USB-C

### Chương trình

Mở phần mềm IDE Arduino và tạo một chương trình (Sketch) mới.
Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) theo hướng dẫn tại đây.

```ino
// Hello Arduino Example - MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

void setup()
{
  // Khởi động chuẩn kết nối Serial với tốc độ Baudrate 115200bps
  Serial.begin(115200);
}

/*-----------------------------------------------------*/

void loop()
{
  // Nội dung này được in ra cứ mỗi 2 giây (2000 mili giây)
  Serial.print("Hello Arduino!");
  delay(1000);
  Serial.println("_Done");
  delay(1000);

  // In ra ký tự được truyền từ máy tính (nếu có)
  while (Serial.available())
  {
    char key = Serial.read();
    Serial.print("Received_");
    Serial.println(key);
    delay(1000);
  }
}
```

### Giải thích code

Chương trình gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`

- `void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)

- `Serial.begin(baudrate)`: khởi động cổng kết nối Serial trên mạch Vietduino Uno với tốc độ (baudrate) mong muốn, các tốc độ hỗ trợ thường là: 9600, 115200,...
- `void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )

- `Serial.print(data)`: gửi dữ liệu từ mạch Vietduino Uno lên máy tính "không" kèm theo ký tự xuống dòng, nếu dữ liệu là kiểu ký tự thì cần để trong dấu "".
- `Serial.println(data)`: gửi dữ liệu từ mạch Vietduino Uno lên máy tính kèm theo ký tự xuống dòng, nếu dữ liệu là kiểu ký tự thì cần để trong dấu "".
- `delay(time)`: yêu cầu Vietduino Uno chờ (không làm gì cả) trong một khoảng thời gian nhất định, đơn vị là mili giây (ms).
- `Serial.available()`: kiểm tra có dữ liệu gửi về cổng Serial hay không, ví dụ bạn gửi: "abc" thì giá trị của hàm này là 3.
- `Serial.read()`: đọc dữ liệu nhận được từ cổng Serial dưới dạng ký tự.

### Kết quả

Sau khi đã nạp code thành công, nhấn vào "biểu tượng kính lúp" hoặc chọn Tools > Serial Monitor để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy chương trình hoạt động, nhập ký tự "abcd" sẽ nhận được kết quả như sau:

![](/ex/less01/image/04_1050px-Chương_trình_Xin_Chào_Arduino!.png)
Chương trình Xin Chào Arduino!

### Bài tập thêm

Bài tập 1:

- Hàm `Serial.write(number)` được sử dụng để in các ký tự trong **Bảng mã ASCII**, hãy thử in *"Hello Arduino"* bằng hàm này / [Lời giải](/solution/README.md).

## Tài liệu tham khảo

- [Các lệnh chức năng cho giao tiếp Serial.](https://docs.arduino.cc/language-reference/en/functions/communication/serial/)
- [Bảng mã ASCII.](https://computersciencewiki.org/index.php/ASCII)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 2: Điều khiển đèn Led chớp tắt với Arduino (Digital Out)](/ex/less02/README.md)
