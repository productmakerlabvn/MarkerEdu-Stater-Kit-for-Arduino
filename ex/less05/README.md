# Bài 5: Điều kiển độ sáng đèn Led với Arduino (Analog Out) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

Như ta đã biết ở bài trước Arduino có thích hợp bộ chuyển đổi ADC (Analog to Digital) để có thể đọc được các tín hiệu Analog, vậy để xuất tín hiệu Analog, Arduino có làm được không?

Câu trả lời là "gần được" vì Arduino không tích hợp bộ chuyển đổi DAC (Digital to Analog) để có thể xuất được tín hiệu Analog tuy nhiên ta có thể sử dụng một dạng thay thế là PWM (Pulse Width Modulation) hỗ trợ trên Arduino để có thể giả lập tương đối được tín hiệu Analog, mô phỏng kỹ thuật PWM như hình:

![](/ex/less05/image/01_Pulse-width-modulation-signal-diagrams-average.jpg)  
Kỹ thuật điều chế độ rộng xung PWM

Bằng cách điều chỉnh tỷ lệ thời gian giữa xuất tín hiệu `HIGH` và xuất tín hiệu `LOW` ở của tín hiệu Digital ở một <ins>tần số cố định</ins>, ta có thể tạo ra một mức *"điện áp trung bình"*, có thể điều chỉnh được bất kì giá trị nào trong dãi điện áp từ **0 \~ 5VDC**, mang lại kết quả mô phỏng giống như tín hiệu Analog. Chức năng PWM trên mạch Vietduino Uno có độ phân giải '''8-bit''', nghĩa là bạn có thể dùng chân Digital điều chỉnh độ rộng xung với giá trị từ 0 \~ 255 tương ứng với mức điện áp trung bình xuất ra từ 0~5VDC.

<table><thead>
  <tr>
    <th>Độ rộng xung</th>
    <th>0%</th>
    <th>0,39%</th>
    <th>0,78%</th>
    <th>...</th>
    <th>39,21%</th>
    <th>58,82%</th>
    <th>78,43%</th>
    <th>...</th>
    <th>99,21%</th>
    <th>99,6%</th>
    <th>100%</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Giá trị PWM</td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>...</td>
    <td>100</td>
    <td>150</td>
    <td>200</td>
    <td>...</td>
    <td>253</td>
    <td>254</td>
    <td>255</td>
  </tr>
</tbody>
</table>

> **Lưu ý:**  
> Không phải chân Digital nào cũng có tính năng **PWM**. Trên bo Vietduino Uno, những chân Digital có dấu `#` mới sử dụng được tính năng này.  
> Cụ thể là các chân Digital:  
>
> - `D3, D9, D10, D11` có tần số cố định là **490Hz**.  
> - `D5, D6` có tần số cố định là **980Hz**.  
>
> MakerEdu Shield đã ra sẵn các Port D9, D10, D11 có tính năng PWM để các bạn sử dụng.  
> Các bạn có thể tìm hiểu thêm bài viết về [Tín hiệu Analog, Digital và các chuẩn giao tiếp cơ bản](/ex/less02/A_D_signal_and_interface/README.md).

## Video

[![](/ex/less05/image/02_video_less05.png)](https://youtu.be/EiGWIUcm2nM)

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

![](/ex/less05/image/03_connect.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

### Chương trình

1. Mở phần mềm **IDE Arduino** và tạo một chương trình **(Sketch)** mới.
1. Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) theo [hướng dẫn tại đây](https://makerlab.vn/vuno).

```ino
// PWM LED Example / Analog Out - MakerEdu Starter Kit for Arduino
/*-----------------------------------------------------*/

// Chọn chân điều khiển đèn Led
// Các Port Digital trên MakerEDU Shield D9, D10, D11 đều có tính năng PWM

# define LED_PIN 9 // D9

// Chọn khoảng thời gian thay đổi độ sáng của đèn Led

# define TIME_CHANGE 10 // Đơn vị (ms)

/*-----------------------------------------------------*/

// Lưu giá trị "độ rộng xung" cấp cho đèn Led
// Giá trị (PWM) có độ phân giải 8-bit, nằm trong khoảng 0 - 255
int power = 0;

/*-----------------------------------------------------*/

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
  // Điều khiển "tăng dần" độ sáng đèn Led
  for (power = 0; power <= 255; power++)
  {
    analogWrite(LED_PIN, power);
    Serial.println(power);
    delay(TIME_CHANGE);
  }

  // Điều khiển "giảm dần" độ sáng đèn Led
  for (power = 255; power >= 0; power--)
  {
    analogWrite(LED_PIN, power);
    Serial.println(power);
    delay(TIME_CHANGE);
  }
}
```

### Giải thích code

Chương trình gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`

`void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)

- `Serial.begin(baudrate)`: khởi động cổng kết nối Serial trên mạch Vietduino Uno với tốc độ (baudrate) mong muốn, các tốc độ hỗ trợ thường là: 9600, 115200,...
`void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )

- `analogWrite(Pin, Value)`: xuất tín hiệu PWM (mô phỏng Analog) ra chân tín hiệu Digital hỗ trợ chức năng với giá trị từ 0\~255 tương ứng với điện áp từ 0\~5VDC.
- `Serial.println(data)`: gửi dữ liệu từ mạch Vietduino Uno lên máy tính kèm theo ký tự xuống dòng, nếu dữ liệu là kiểu ký tự thì cần để trong dấu "".
- `delay(time)`: yêu cầu Vietduino Uno chờ (không làm gì cả) trong một khoảng thời gian nhất định, đơn vị là mili giây (ms).

<ins>Quá trình vận hành của chương trình như sau</ins>:

- Bắt đầu từ giá trị PWM bằng 0, bo mạch sẽ liên tục xuất xung PWM có giá trị tăng dần đến 255, với tần suất thay đổi mỗi 10ms, đèn Led sẽ sáng dần.
- Sau đó từ giá trị PWM bằng 255, bo mạch cũng liên tục xuất xung PWM có giá trị giảm dần đến 0, với tần suất thay đổi mỗi 10ms, đèn Led sẽ mờ dần.
- Quá trình này sẽ lặp đi lặp lại tạo hiệu ứng Led sáng dần rồi tối dần.

### Sơ đồ kết nối

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
    <td><a href="https://hshop.vn/cap-ket-noi-makeredu-xh2-54-3wires-20cm-cable">MakerEdu XH2.54 3Wires</td>
  </tr>
</tbody>
</table>

### Kết quả

Sau khi đã nạp code thành công, nhấn vào *"biểu tượng kính lúp"* hoặc chọn Tools > Serial Monitor để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy chương trình hoạt động, Module Led kết nối tại chân D9 sẽ sáng dần mờ dần theo giá trị PWM hiển thị lên Serial Monitor:

![](/ex/less05/image/04_1050px-LED_PWM_Arduino.jpg)
Module Led kết nối với Port D9 trên MakerEdu Shield sẽ sáng dần rồi mờ dần.

![](/ex/less05/image/05_1050px-Screenshot_2023-07-10_at_14.59.11.png)
Giá trị PWM sẽ được hiển thị lên Serial Monitor
Các bạn cũng có thể nhấn vào "biểu tượng đồ thị" hoặc chọn Tools > Serial Plotter để mở Serial Plotter lúc này bạn có thể xem giá trị PWM bằng đồ thị trực quan hơn.

![](/ex/less05/image/06_1050px-MakerEdu_Starter_Kit_Bai5.png)
Hiển thị giá trị PWM bằng đồ thị trên Serial Plotter của Arduino

## Bài tập thêm (nâng cao)

Bài tập 1:

Viết chương trình điều khiển độ sáng đèn Led bằng **biến trở** (kết hợp Analog In và Analog Out) / [Lời giải](/solution/README.md).

## Nguồn tài liệu (tham khảo thêm)

- [PWM là gì?.](https://docs.arduino.cc/learn/microcontrollers/analog-output)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 4: Nhận tín hiệu từ biến trở với Arduino (Analog In)](/ex/less04/README.md)
- [Bài 6: Hiển thị thông tin lên màn hình LCD với Arduino (Library)](/ex/less06/README.md)
