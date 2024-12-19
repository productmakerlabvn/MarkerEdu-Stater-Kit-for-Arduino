# Bài 9: Đo nhiệt độ độ ẩm bằng cảm biến DHT11 hiển thị lên LCD1602 (Library) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

Trong bài này các bạn sẽ học các kết hợp giữa cảm biến độ ẩm nhiệt độ MKE-S14 DHT11 và màn hình LCD1602 MKE-M07 để làm một máy đo độ ẩm và nhiệt độ của phòng, cả hai thiết bị này đều phải thêm các bộ thư viện hỗ trợ để có thể sử dụng với Arduino.

![](/ex/less09/image/01_F-971-04b-1000.jpg)  
Máy đo độ ẩm, nhiệt độ FIuke 971

## Video

[![](/ex/less09/image/02_video.png)](https://youtu.be/sGiuZmPzyNk)

## Các bước thực hiện

### Danh sách thiết bị

- [1x Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- [1x Mạch MakerEdu Shield for Vietduino](https://makerlab.vn/vietduinosd)
- [1x Cảm biến độ ẩm nhiệt độ MKE-S14 DHT11 temperature and humidity sensor](https://makerlab.vn/mkes14)
- [1x Mạch hiển thị MKE-M07 LCD1602 I2C module](https://makerlab.vn/mkem07)
- [1x Cáp USB-C](https://hshop.vn/cap-usb-type-c)

### Chuẩn bị trước dự án

- Kết nối mạch MakerEdu Shield với mạch Vietduino Uno theo hướng dẫn.
- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch MakerEdu Shield phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino theo hướng dẫn tại đây.
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.

![](/ex/less09/image/03_connect.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

### Cài đặt bộ thư viện (Library)

- Tải và cài đặt bộ thư viện LiquidCrystal_I2C theo hướng dẫn tại đây.
- Tải và cài đặt bộ thư viện DHT-sensor-library theo hướng dẫn tại đây.
- Tải và cài đặt bộ thư viện Adafruit_Sensor theo hướng dẫn tại đây. (bộ thư viện này cần cài đặt để có thể chạy được bộ thư viện [DHT-sensor-library](https://github.com/adafruit/DHT-sensor-library) ở phía trên).

### Chương trình

Mở phần mềm **IDE Arduino** và tạo một chương trình **(Sketch)** mới.
Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) theo hướng dẫn tại đây.

```ino
// Thêm bộ thư viện cho cảm biến và LCD
#include <LiquidCrystal_I2C.h>
#include <DHT.h>

/*-----------------------------------------------------*/

// Khởi tạo màn hình MKE-M07 LCD1602 I2C với đối tượng là "lcd"
LiquidCrystal_I2C lcd(0x27,16,2);
// Cài đặt địa chỉ I2C là 0x27
// Mỗi dòng 16 ký tự
// 2 dòng hiển thị

// Khởi tạo cảm biến MKE-S14 DHT11 với đối tượng là "dht" tại Port D9
DHT dht(9, DHT11);

// Khởi tạo hai biến temp và humi để lưu giá trị cảm biến theo kiểu số thực
float temp, humi;

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Khởi động màn hình LCD
  lcd.init();
  // Xóa toàn bộ nội dung trên màn hình LCD (nếu có)
  lcd.clear();
  // Bật đèn nền màn hình LCD
  lcd.backlight();

  // Tại vị trí cột 1 dòng 1, cho in nội dung...
  lcd.setCursor(0, 0);
  lcd.print("Temp:");
  // Tại vị trí cột 1 dòng 2, cho in nội dung...
  lcd.setCursor(0, 1);
  lcd.print("Humi:");

  // Khởi động cảm biến DHT11
  dht.begin();
}

/*-----------------------------------------------------*/

void loop()
{
  // Lấy dữ liệu từ cảm biến DHT11
  temp = dht.readTemperature();
  humi = dht.readHumidity();

  // In giá trị "nhiệt độ" tại vị trí cột 7 dòng 1
  lcd.setCursor(6, 0);
  lcd.print(temp, 2);
  lcd.write(0xDF); // In kí tự (°) theo bảng mã ASCII
  lcd.print("C  ");
  // In giá trị "độ ẩm" tại vị trí cột 7 dòng 2
  lcd.setCursor(6, 1);
  lcd.print(humi, 2);
  lcd.print("%  ");

  // In các giá trị này lên Serial
  Serial.print("Humidity: ");
  Serial.print(humi, 2);
  Serial.print("% - Temperature: ");
  Serial.print(temp, 2);
  Serial.println("°C");

  // Chờ 0,5s mới đo lại
  delay(500);
}
```

### Giải thích code

Để có thể sử dụng các hàm để điều khiển màn hình MKE-M07 LCD1602 I2C và cảm biến MKE-S14 DHT11 đầu tiên ta cần thêm bộ thư viện vào đầu chương trình:

`#include <LiquidCrystal_I2C.h>`: Thêm bộ thư viện LiquidCrystal_I2C.h với các hàm điều khiển màn hình

`#include <DHT.h>`: Thêm bộ thư viện DHT.h với các hàm điều khiển cảm biến MKE-S14 DHT11

Sau đó ta cần khai báo đối tượng cần điều khiển của bộ thư viện LiquidCrystal_I2C như ở đây ta khai báo là "lcd", bạn cũng có thể khai báo thêm "lcd1", "lcd2" tuỳ thích, khi đó các hàm điều khiển phía sau sẽ phải gắn với các đối tượng này:

`LiquidCrystal_I2C lcd(0x27, 16, 2)`: Khai báo đối tượng màn hình "lcd" với địa chỉ 0x27, 16 ký tự, 2 dòng.

> **Lưu ý:**  
> Để điều khiển thêm các LCD khác bạn cần phải thiết lập địa chỉ khác với địa chỉ mặc định 0x27, tham khảo tài liệu sau để biết cách thiết lập địa chỉ LCD.

Tương tự ta khai báo đối tượng cần điều khiển của bộ thư viện DHT như ở đây ta khai báo là "dht", bạn cũng có thể khai báo thêm "dht1", "dht2" tuỳ thích, khi đó các hàm điều khiển phía sau sẽ phải gắn với các đối tượng này: DHT dht(9, DHT11);: Khai báo đối tượng cảm biến "dht" tại Port D9.

> Lưu ý:  
> Để điều khiển thêm các cảm biến DHT khác bạn bạn có thể kết nối và khai báo thiết lập ở các Port chưa sử dụng như D10, D11,...

Chương trình chính gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`

`void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)

- `Serial.begin(baudrate)`: khởi động cổng kết nối Serial trên mạch Vietduino Uno với tốc độ (baudrate) mong muốn, các tốc độ hỗ trợ thường là: 9600, 115200,...
- `lcd.init()`: khởi động màn hình "lcd", là câu lệnh bắt buộc để bắt đầu điều khiển màn hình.
- `lcd.clear()`: xoá toàn bộ nội dung trên màn hình "lcd".
- `lcd.backlight()`: bật đèn nền màn hình "lcd".
- `dht.begin()`: khởi động cảm biến DHT.  

`void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )

- `dht.readTemperature()`: đọc giá trị nhiệt độ của cảm biến DHT.
- `dht.readHumidity()`: đọc giá trị độ ẩm của cảm biến DHT.
- `lcd.setCursor(column, row)`: chuyển con trỏ đến vị trí cột và dòng mong muốn của "lcd" trước khi in ký tự.
- `lcd.print(data)`: In dữ liệu mong muốn tại vị trí con trỏ của "lcd", nếu là dạng ký tự thì cần bỏ trong dấu "", với số thực nếu muốn in số chữ số sau dấu phẩy ta thêm số chữ số mong muốn theo dạng lcd.print(float, số chữ số mong muốn sau dấu phẩy).
- `Serial.print(data)`: gửi dữ liệu từ mạch Vietduino Uno lên máy tính, nếu dữ liệu là kiểu ký tự thì cần để trong dấu "", với số thực nếu muốn in số chữ số sau dấu phẩy ta thêm số chữ số mong muốn theo dạng Serial.print(float, số chữ số mong muốn sau dấu phẩy), sử dụng Serial.println(data) để in có xuống dòng.  
- `delay(time)`: yêu cầu Vietduino Uno chờ (không làm gì cả) trong một khoảng thời gian nhất định, đơn vị là mili giây (ms).  

Quá trình vận hành của chương trình như sau:

- Khởi động đối tượng "lcd" tại địa chỉ 0x27 với cấu hình là 16 ký tự, 2 dòng.
- Khởi động cảm biến DHT.
- Đọc và lưu các giá trị nhiệt độ và độ ẩm của cảm biến DHT vào hai biến số thực là Temp và Humi.
- Hiển thị các thông tin độ ẩm và nhiệt độ của cảm biến DHT lên màn hình LCD.
- Hiển thị các thông tin độ ẩm và nhiệt độ của cảm biến DHT lên Serial Monitor.

### Sơ đồ kết nối

<table><thead>
  <tr>
    <th>MakerEDU Shield</th>
    <th>Thiết bị</th>
    <th>Cáp kết nối</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Port I2C</td>
    <td><a href="https://makerlab.vn/mkem07">Mạch hiển thị MKE-M07 LCD1602 I2C module</a></td>
    <td>MakerEdu XH2.54 4Wires</td>
  </tr>
  <tr>
    <td>Port D9</td>
    <td><a href="https://makerlab.vn/mkes14">Cảm biến độ ẩm nhiệt độ MKE-S14 DHT11 temperature and humidity sensor</a></td>
    <td>MakerEdu XH2.54 3Wires</td>
  </tr>
</tbody>
</table>

## Kết quả

Sau khi đã nạp code thành công bạn có thể xem kết quả trực tiếp trên màn hình LCD, phía sau màn hình LCD có một biến trở (màu xanh dương) để chỉnh "độ tương phản" bạn có thể dùng tua vít để điều chỉnh bạn thấy quá đậm hoặc quá mờ.

![](/ex/less09/image/04_1050px-May_do_nhiet_do_do_am.jpg)
Hiển thị thông tin cảm biến nối với Port D9 lên màn hình LCD nối với Port I2C của MakerEdu Shield.

Nhấn vào *"biểu tượng kính lúp"* hoặc chọn *Tools > Serial Monitor* để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy giá trị cảm biến hiển thị lên Serial Monitor.

![](/ex/less09/image/05_1050px-Screenshot_2023-07-15_at_12.27.38.png)
Giá trị cảm biến sẽ được hiển thị lên Serial Monitor

## Bài tập thêm (nâng cao)

<ins>Bài tập 1:</ins>

- Định mức và hiển thị thông báo lên màn hình LCD ký hiệu "!" phía sau giá trị của nhiệt độ và độ ẩm khi vượt mức quy định. / [Lời giải.](/solution/README.md)
- Ví dụ, đặt ngưỡng nhiệt độ là **30ºC** và ngưỡng độ ẩm là **80%**. Các bạn có thể thay đổi tùy theo ý mình.

## Nguồn tài liệu (tham khảo thêm)

- [What is the Human Comfort Zone for Temperature and Humidity?](https://blog.wellcare-global.com/blog/what-is-the-human-comfort-zone-for-temperature-and-humidity)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 8: Báo cháy với cảm biến lửa và còi Buzzer (Analog In + Digital Out)](/ex/less08/README.md)
- [Bài 10: Mô phỏng hệ thống giám sát môi trường trong nông nghiệp thông minh (Tổng hợp)](/ex/less10/README.md)
