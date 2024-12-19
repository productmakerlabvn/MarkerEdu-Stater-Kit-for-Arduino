# Bài 3: Nhận tín hiệu từ nút nhấn với Arduino (Digital In) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

Trong bài này các bạn sẽ học cách nhận tín hiệu Digital từ các chân tín hiệu của Vietduino Uno, được sử dụng cho các ứng dụng như nhận tín hiệu từ nút nhấn điều khiển, cảm biến,... với chỉ hai trạng thái là có hoặc không, ở bài trước khi "xuất" tín hiệu Digital ta biết mức HIGH tương ứng với điện áp 5V và LOW tương ứng với 0V, tuy nhiên khi nhận tín hiệu Digital sẽ tuân theo chuẩn TTL với quy định 2\~5VDC sẽ được hiểu là mức cao HIGH và từ 0\~0.8VDC sẽ được hiểu là mức thấp LOW như hình sau:

![](/ex/less03/image/01_518d5681ce395f1e11000000.jpg)
Bảng định mức điệp áp Digital TTL của thiết bị sử dụng 5VDC

> Lưu ý:
Các bạn có thể tìm hiểu thêm bài viết về [Tín hiệu Analog, Digital và các chuẩn giao tiếp cơ bản](/ex/less02/A_D_signal_and_interface/README.md).

## Video

[![](/ex/less03/image/02_video_less03.png)](https://youtu.be/DjCQR_YDwZo)

## Các bước thực hiện

### Danh sách thiết bị

- [1x Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- [1x Mạch MakerEdu Shield for Vietduino](https://makerlab.vn/vietduinosd)
- [1x Cáp USB-C](https://hshop.vn/cap-usb-type-c)
- [1x Mạch nút nhấn MKE-M02 push button tact switch module](https://makerlab.vn/mkem02)

### Chuẩn bị trước dự án

- Kết nối mạch MakerEdu Shield với mạch Vietduino Uno [theo hướng dẫn](https://makerlab.vn/vietduinosd).
- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch MakerEdu Shield phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino [theo hướng dẫn tại đây](https://makerlab.vn/vuno).
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.
![](/ex/less03/image/03_1050px-connect_uno_and_shield_to_computer.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

### Chương trình

Mở phần mềm **IDE Arduino** và tạo một chương trình **(Sketch)** mới.
Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) [theo hướng dẫn tại đây](https://makerlab.vn/vuno).

```ino
// Button Example - MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

// Chọn chân đọc tín hiệu Nút nhấn

# define BUTTON_PIN 10 // D10

// Chọn mức tín hiệu đọc từ Nút nhấn với trạng thái tương ứng

# define PRESS_BUTTON LOW    // Khi "nhấn nút" tín hiệu đọc được là LOW

# define RELEASE_BUTTON HIGH // Khi "nhả nút" tín hiệu đọc được là HIGH

// Chọn khoảng thời gian chờ để xác minh nút đã được nhấn

# define TIME_DEBOUNCE 10 // Đơn vị (ms)

int count = 0; // Tạo biến count kiểu số nguyên (Integer) với giá trị 0

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Thiết lập chân D10 là Input để đọc tín hiệu từ Nút nhấn
  pinMode(BUTTON_PIN, INPUT);
}

/*-----------------------------------------------------*/

void loop()
{
  // Nghi vấn nút đang được "nhấn" ?
  if (digitalRead(BUTTON_PIN) == PRESS_BUTTON)
  {
    // Chờ qua khoảng thời gian để xác minh
    delay(TIME_DEBOUNCE);

    // Xác thực chắc chắn nút đang được "nhấn"
    if (digitalRead(BUTTON_PIN) == PRESS_BUTTON)
    {
      // In thông báo cho biết đang "nhấn"
      Serial.println("PRESS");

      // Chờ cho đến khi "nhả" nút
      while (digitalRead(BUTTON_PIN) == PRESS_BUTTON)
      {
      }

      // In thông báo cho biết nút đã "nhả"
      Serial.println("RELEASE");

      // Đây là 1 Click
      count++;
      Serial.println(count);
      Serial.println("--- --- ---");
    }
  }
}
```

### Giải thích code

Mạch nút nhấn sẽ trả về tín hiệu Digital mức thấp (LOW) khi được nhấn và khi không được nhấn là mức cao (HIGH) nên ta thiết lập hai trạng thái như sau:

```ino
#define PRESS_BUTTON LOW    // Khi "nhấn nút" tín hiệu đọc được là LOW
#define RELEASE_BUTTON HIGH // Khi "nhả nút" tín hiệu đọc được là HIGH
```

Chương trình gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`  

`void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)

- `Serial.begin(baudrate)`: khởi động cổng kết nối Serial trên mạch Vietduino Uno với tốc độ (baudrate) mong muốn, các tốc độ hỗ trợ thường là: 9600, 115200,...
- `pinMode(Pin, INPUT)`: cấu hình chân tín hiệu là chân nhận tín hiệu (INTPUT) để đọc tín hiệu từ nút nhấn.  

`void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )

- `digitalRead(Pin)`: hàm này sẽ trả về trạng thái của chân tín hiệu đang là HIGH (1) hoặc LOW (0).
- `Serial.println(data)`: gửi dữ liệu từ mạch Vietduino Uno lên máy tính kèm theo ký tự xuống dòng, nếu dữ liệu là kiểu ký tự thì cần để trong dấu "".
- `delay(time)`: yêu cầu Vietduino Uno chờ (không làm gì cả) trong một khoảng thời gian nhất định, đơn vị là mili giây (ms).
- `count++`: tăng giá trị của biến count lên 1 đơn vị.  

<ins>Quá trình vận hành của chương trình như sau</ins>:

- Bo mạch sẽ kiểm tra liên tục tín hiệu trên nút nhấn.
- Bình thường khi *"nút không được nhấn"*, tín hiệu đọc được luôn là `HIGH`, bo mạch sẽ không làm gì cả.
- Nhưng khi bo mạch phát hiện *"nút được nhấn"*, tín hiệu đọc được là `LOW`.
- Bo sẽ chờ trong một khoảng thời gian xác minh, sau đó kiểm tra lại một lần nữa có đúng là nút đang được nhấn hay không?
- Nếu nút không được nhấn, thì bo không làm gì cả, ngược lại sẽ cho in thông báo **"PRESS"** lên Serial.
- Sau đó, bo kiểm tra liên tục nút nhấn, chờ cho đến khi nào phát hiện nút được nhả ra, sẽ cho in thông báo **"RELEASE"** lên Serial.
- Cuối cùng là tăng *"số đếm"* lên một đơn vị, và cho biết đây là lần nhấn nút thứ mấy rồi.

### Kết nối phần cứng

<table><thead>
  <tr>
    <th>MakerEDU Shield</th>
    <th>Thiết bị</th>
    <th>Cáp kết nối</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Port D10</td>
    <td><a href="https://makerlab.vn/mkem02">Mạch nút nhấn MKE-M02 push button tact switch module</a></td>
    <td><a href="https://hshop.vn/cap-ket-noi-makeredu-xh2-54-3wires-20cm-cable">MakerEdu XH2.54 3Wires</td>
  </tr>
</tbody>
</table>

### Kết quả

Sau khi đã nạp code thành công, nhấn vào "biểu tượng kính lúp" hoặc chọn Tools > Serial Monitor để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy chương trình hoạt động, Module Nút Nhấn kết nối với Port D10 sẽ hiển thị trạng thái lên Serial Monitor:

![](/ex/less03/image/04_1050px-F72A75C3-EFEF-4195-BD82-ED4CB0F8CD2B_1_201_a.jpg)
Module nút nhấn kết nối với port D10 sẽ hiển thị trạng thái lên Serial Monitor

![](/ex/less03/image/05_1050px-Screenshot_2023-07-09_at_15.30.35.png)
Serial Monitor trên phần mềm Arduino hiển thị trạng thái của module nút nhấn.

## Bài tập thêm

<ins>Bài tập 1</ins>:

- Bạn hãy thử thay đổi thông số `TIME_DEBOUNCE` về **0** và xem kết quả thao tác nút nhấn thế nào khi ta đã bỏ qua khoảng thời gian để xác minh.  

<ins>Bài tập 2</ins>:

- Viết chương trình khi **nhấn nút** sẽ bật đèn Led và **nhả nút** ra sẽ tắt đèn Led / [Lời giải](/solution/README.md).  

## Nguồn tài liệu (tham khảo thêm)

- [Chức năng của điện trở "kéo lên", "kéo xuống" cho Nút nhấn.](https://www.circuitbasics.com/pull-up-and-pull-down-resistors/)
- [Chống nhiễu cho nút nhấn như thế nào?.](https://www.youtube.com/watch?v=e1-kc04jSE4&ab_channel=TexasInstruments)
- [Cách loại bỏ hiện tượng nhiễu của nút nhấn bằng chương trình.](https://docs.arduino.cc/built-in-examples/digital/Debounce)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 2: Điều khiển đèn Led chớp tắt với Arduino (Digital Out)](/ex/less02/README.md)
- [Bài 4: Nhận tín hiệu từ biến trở với Arduino (Analog In)](/ex/less04/README.md)
