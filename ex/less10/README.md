# Bài 10: Mô phỏng hệ thống giám sát môi trường trong nông nghiệp thông minh (Tổng hợp) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

Trong bài thực hành tổng hợp này các bạn sẽ thực hiện mô phỏng lại một hệ thống giám sát môi trường trong nông nghiệp thông minh với các thông số ánh sáng, nhiệt độ, độ ẩm và báo hiệu khi các thông số vượt quá mức quy định.

![](/ex/less10/image/01_Shutterstock_1347945488-800x450.jpg)  
Hệ thống giám sát môi trường nông nghiệp thông minh

## Video

[![](/ex/less10/image/02_video.png)](https://youtu.be/qPMvq2SYIPY)

## Các bước thực hiện

### Danh sách thiết bị

- [1x Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- [1x Mạch MakerEdu Shield for Vietduino](https://makerlab.vn/vietduinosd)
- [1x Cảm biến độ ẩm nhiệt độ MKE-S14 DHT11 temperature and humidity sensor](https://makerlab.vn/mkes14)
- [1x Cảm biến ánh sáng quang trở MKE-S02 LDR light sensor](https://makerlab.vn/mkes02)
- [1x Mạch còi báo MKE-M03 buzzer module](https://makerlab.vn/mkem03)
- [1x Mạch hiển thị MKE-M07 LCD1602 I2C module](https://makerlab.vn/mkem07)
- [1x Cáp USB-C](https://hshop.vn/cap-usb-type-c)

### Chuẩn bị trước dự án

- Kết nối mạch MakerEdu Shield với mạch Vietduino Uno theo hướng dẫn.
- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch MakerEdu Shield phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino theo hướng dẫn tại đây.
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.

![](/ex/less10/image/03_connect.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

### Cài đặt bộ thư viện (Library)

- Tải và cài đặt bộ thư viện [LiquidCrystal_I2C](https://github.com/makerlabvn/LiquidCrystal_I2C) theo hướng dẫn tại đây.
- Tải và cài đặt bộ thư viện [DHT-sensor-library](https://github.com/makerlabvn/DHT-sensor-library) theo hướng dẫn tại đây.
- Tải và cài đặt bộ thư viện [Adafruit_Sensor](https://github.com/adafruit/Adafruit_Sensor) theo hướng dẫn tại đây. (bộ thư viện này cần cài đặt để có thể chạy được bộ thư viện DHT-sensor-library ở phía trên).

### Chương trình

1. Mở phần mềm IDE Arduino và tạo một chương trình (Sketch) mới.
1. Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) theo hướng dẫn tại đây.

```ino
// Hệ thống giám sát môi trường nông nghiệp thông minh / MakerEdu Starter Kit for Arduino
/*-----------------------------------------------------*/
// Thêm bộ thư viện cho cảm biến DHT và LCD

#include <LiquidCrystal_I2C.h>

#include <DHT.h>

/*-----------------------------------------------------*/

// Chọn chân đọc cảm biến Quang trở

#define LDR_PIN A1 // A1

// Chọn chân điều khiển còi Buzzer

#define BUZZ_PIN 10 // D10

/*-----------------------------------------------------*/

// Khởi tạo màn hình MKE-M07 LCD1602 I2C với đối tượng là "lcd"
LiquidCrystal_I2C lcd(0x27,16,2);
// Cài đặt địa chỉ I2C là 0x27
// Mỗi dòng 16 ký tự
// 2 dòng hiển thị

// Khởi tạo cảm biến MKE-S14 DHT11 với đối tượng là "dht" tại Port D9
DHT dht(9, DHT11);

// Các biến lưu giá trị "nhiệt độ" và "độ ẩm" đọc từ cảm biến theo kiểu số thực
float temp, humi;

// Tạo biến lưu giá trị của cảm biến ánh sáng LDR và biến đếm
int rawLDR = 0;
int minlightValue = 0;
int valueLDR = 0;
int count = 0;

// Tạo các biến trạng thái Logic
bool warning1 = false;
bool warning2 = false;
bool stateBuzz = 0;

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200bps
  Serial.begin(115200);

  // Thiết lập chân D10 là Output để điều khiển còi
  pinMode(BUZZ_PIN, OUTPUT);
  // Khi mới khởi động, tắt còi
  digitalWrite(BUZZ_PIN, LOW);

  // Khởi động màn hình LCD
  lcd.init();
  // Xóa toàn bộ nội dung trên màn hình LCD (nếu có)
  lcd.clear();
  // Bật đèn nền màn hình LCD
  lcd.backlight();

  //Căn chỉnh giá trị của cảm biến ánh sáng
  lcd.setCursor(0, 0); // Cột 0 dòng 0
  lcd.print("Light SS Calib");
  lcd.setCursor(0, 1); // Cột 0 dòng 1
  lcd.print("Cover the sensor");
  Serial.println("Xin để cảm biến ở chỗ tối nhất!");
  delay(2000);
  for (count = 5; count >= 1; count--)
  {
    delay(1000);
    //Đọc giá trị cảm biến ánh sáng LDR
    rawLDR = analogRead(LDR_PIN);
    minlightValue = minlightValue + rawLDR;
    Serial.println(rawLDR);
  }
  //Lấy giá trị trung bình 5 lần đo + 10% sai số
  minlightValue = minlightValue / 4.5;
  Serial.print("Đo giá trị cảm biến lúc tối nhất:");
  Serial.println(minlightValue);

  // Xóa toàn bộ nội dung trên màn hình LCD (nếu có)
  lcd.clear();

  // In khung nội dung mẫu lên LCD
  lcd.setCursor(0, 0); // Cột 0 dòng 0
  lcd.print("T:");
  lcd.setCursor(10, 0); // Cột 10 dòng 0
  lcd.print("H:");
  lcd.setCursor(0, 1); // Cột 0 dòng 1
  lcd.print("LDR: ");

  // Khởi động cảm biến DHT11
  dht.begin();
}

/*-----------------------------------------------------*/

void loop()
{
  // Lấy dữ liệu từ cảm biến DHT11 và cảm biến ánh sáng
  rawLDR = analogRead(LDR_PIN);
  temp = dht.readTemperature();
  humi = dht.readHumidity();
  // Quy đổi dử liệu cảm biến ánh sáng ra dạng 0~100%
  valueLDR = map(rawLDR, 0, minlightValue, 100, 0);

  //Hiển thị các giá trị lên Serial Monitor
  Serial.print("LDR: ");
  Serial.println(rawLDR);
  Serial.print("Temp: ");
  Serial.println(temp);
  Serial.print("Humi: ");
  Serial.println(humi);

  // In giá trị "nhiệt độ" tại vị trí cột 2 dòng 0
  lcd.setCursor(2, 0);
  lcd.print(temp, 1);
  lcd.write(0xDF); // In kí tự (°)
  lcd.print("C ");
  // In giá trị "độ ẩm" tại vị trí cột 12 dòng 0
  lcd.setCursor(12, 0);
  lcd.print(humi, 0);
  lcd.print("% ");
  // In giá trị "quang trở" tại vị trí cột 5 dòng 1
  lcd.setCursor(5, 1);
  lcd.print(valueLDR);
  lcd.print("% - ");
  //So sánh giá trị cảm biến ánh sáng để hiện thông báo Ngày  /  
  if (valueLDR >= 30)
    lcd.print("DAY  ");
  else
    lcd.print("NIGHT ");

  // So sánh giá trị độ ẩm với giá trị định mức bật cảnh báo.
 if (humi > 85)
    {
      warning1 = true; // Bật cảnh báo
      Serial.println("Độ ẩm cao vượt mức quy định (85)");
      lcd.setCursor(15, 0); // Cột 15 dòng 0
      lcd.print("!");
    }
  else if (humi < 40)
    {
      warning1 = true; // Bật cảnh báo
      Serial.println("Độ ẩm thấp dưới mức quy định (40)");
      lcd.setCursor(15, 0); // Cột 15 dòng 0
      lcd.print("!");
    }
  else
    {
      warning1 = false; // Tắt cảnh báo
      lcd.setCursor(15, 0); // Cột 15 dòng 0
      lcd.print(" ");
    }

  // So sánh giá trị nhiệt độ với giá trị định mức bật cảnh báo.
 if (temp > 30)
    {
      warning2 = true; // Bật cảnh báo
      Serial.println("Cảnh báo!: Nhiệt độ cao quá mức quy định (30)");
      lcd.setCursor(8, 0); // Cột 8 dòng 0
      lcd.print("!");
    }
  else if (temp < 25)
    {
      warning2 = true; // Bật cảnh báo
      Serial.println("Cảnh báo!: Nhiệt độ thấp dưới mức quy định (25)");
      lcd.setCursor(8, 0); // Cột 8 dòng 0
      lcd.print("!");
    }
  else
    {
      warning2 = false; // Tắt cảnh báo
      lcd.setCursor(8, 0); // Cột 8 dòng 0
      lcd.print(" ");
    }

  // Bật cảnh báo nếu có giá trị vượt mức quy định.
  if (warning2||warning1)
  {
      stateBuzz = !stateBuzz;
      digitalWrite(BUZZ_PIN, stateBuzz);
  }

  // Chờ 0.5s mới đo lại.
  delay(500);
}
```

### Giải thích code

Để có thể sử dụng các hàm để điều khiển màn hình MKE-M07 LCD1602 I2C và cảm biến MKE-S14 DHT11 đầu tiên ta cần thêm bộ thư viện vào đầu chương trình:

`#include <LiquidCrystal_I2C.h>`: Thêm bộ thư viện LiquidCrystal_I2C.h với các hàm điều khiển màn hình

`#include <DHT.h>`: Thêm bộ thư viện DHT.h với các hàm điều khiển cảm biến MKE-S14 DHT11

Sau đó ta cần khai báo đối tượng cần điều khiển của bộ thư viện   LiquidCrystal_I2C như ở đây ta khai báo là "lcd", bạn cũng có thể khai báo thêm "lcd1", "lcd2" tuỳ thích, khi đó các hàm điều khiển phía sau sẽ phải gắn với các đối tượng này:

`LiquidCrystal_I2C lcd(0x27, 16, 2)`: Khai báo đối tượng màn hình "lcd" với địa chỉ 0x27, 16 ký tự, 2 dòng.

> Lưu ý:
> Để điều khiển thêm các LCD khác bạn cần phải thiết lập địa chỉ khác với địa chỉ mặc định 0x27, tham khảo tài liệu sau để biết cách thiết lập địa chỉ LCD.

Tương tự ta khai báo đối tượng cần điều khiển của bộ thư viện DHT như ở đây ta khai báo là "dht", bạn cũng có thể khai báo thêm "dht1", "dht2" tuỳ thích, khi đó các hàm điều khiển phía sau sẽ phải gắn với các đối tượng này: DHT dht(9, DHT11);: Khai báo đối tượng cảm biến "dht" tại Port D9.

> Lưu ý:
> Để điều khiển thêm các cảm biến DHT khác bạn bạn có thể kết nối và khai báo thiết lập ở các Port chưa sử dụng như D10, D11,...

Chương trình chính gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`

`void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)

- `Serial.begin(baudrate)`: khởi động cổng kết nối Serial trên mạch Vietduino Uno với tốc độ (baudrate) mong muốn, các tốc độ hỗ trợ thường là: 9600, 115200,...
- `pinMode(Pin, OUTPUT)`: cấu hình chân tín hiệu mong muốn là chân xuất tín hiệu (OUTPUT).
- `digitalWrite(Pin, HIGH);`: Xuất tín hiệu Digital mức cao (HIGH) ra chân tín hiệu mong muốn.
- `lcd.init()`: khởi động màn hình "lcd", là câu lệnh bắt buộc để bắt đầu điều khiển màn hình.
- `lcd.clear()`: xoá toàn bộ nội dung trên màn hình "lcd".
- `lcd.backlight()`: bật đèn nền màn hình "lcd".
- `analogRead(Pin)`: hàm này sẽ trả về giá trị của chân Analog cần đọc với dải giá trị điện áp tương ứng 0~5VDC là giá trị số nguyên từ 0~1023.
- `dht.begin()`: khởi động cảm biến DHT.

`void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )

- `dht.readTemperature()`: đọc giá trị nhiệt độ của cảm biến DHT.
- `dht.readHumidity()`: đọc giá trị độ ẩm của cảm biến DHT.
- `map(value,original_min,original_max,new_min,new_max)`: hàm quy đổi giá trị value trong thang đo gốc "original_min,original_max" sang thang đo mới "new_min,new_max".
- `lcd.setCursor(column, row)`: chuyển con trỏ đến vị trí cột và dòng mong muốn của "lcd" trước khi in ký tự.
- `lcd.print(data)`: In dữ liệu mong muốn tại vị trí con trỏ của "lcd", nếu là dạng ký tự thì cần bỏ trong dấu "", với số thực nếu muốn in số chữ số sau dấu phẩy ta thêm số chữ số mong muốn theo dạng lcd.print(float, số chữ số mong muốn sau dấu phẩy).
- `Serial.print(data)`: gửi dữ liệu từ mạch Vietduino Uno lên máy tính, nếu dữ liệu là kiểu ký tự thì cần để trong dấu "", với số thực nếu muốn in số chữ số sau dấu phẩy ta thêm số chữ số mong muốn theo dạng Serial.print(float, số chữ số mong muốn sau dấu phẩy), sử dụng Serial.println(data) để in có xuống dòng.
- `delay(time)`: yêu cầu Vietduino Uno chờ (không làm gì cả) trong một khoảng thời gian nhất định, đơn vị là mili giây (ms).

Quá trình vận hành của chương trình như sau:

- Khởi động đối tượng "lcd" tại địa chỉ 0x27 với cấu hình là 16 ký tự, 2 dòng.
- Đọc và căn chỉnh giá trị của cảm biến ánh sáng LDR.
- Khởi động cảm biến DHT.
- Đọc và lưu các giá trị nhiệt độ và độ ẩm của cảm biến DHT vào hai biến số thực là Temp và Humi.
- Hiển thị các thông tin độ ẩm và nhiệt độ của cảm biến DHT lên màn hình LCD và Serial Monitor.
- Hiển thị thông tin của cảm biến ánh sáng LDR lên màn hình LCD và Serial Monitor.
- So sánh giá trị của cảm biến ánh sáng LDR để hiển thị trời Sáng hay Tối lên màn hình LCD.
- So sánh giá trị nhiệt độ và độ ẩm của cảm biến DHT để hiển thị cảnh báo lên màn hình LCD khi vượt quá mức quy định và kích hoạt còi báo hiệu.

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
  <tr>
    <td>Port A1</td>
    <td><a href="https://makerlab.vn/mkes02">Cảm biến ánh sáng quang trở MKE-S02 LDR light sensor</a></td>
    <td>MakerEdu XH2.54 3Wires</td>
  </tr>
  <tr>
    <td>Port D10</td>
    <td><a href="https://makerlab.vn/mkem03">Mạch còi báo MKE-M03 buzzer module</a></td>
    <td>MakerEdu XH2.54 3Wires</td>
  </tr>
</tbody></table>

## Kết quả

Sau khi đã nạp code thành công bạn có thể xem kết quả trực tiếp trên màn hình LCD, phía sau màn hình LCD có một biến trở (màu xanh dương) để chỉnh "độ tương phản" bạn có thể dùng tua vít để điều chỉnh bạn thấy quá đậm hoặc quá mờ.

![](/ex/less10/image/04_1350px-5048C6D5-A4C2-4507-B1D8-4B7A69862F5A_1_201_a.jpg)
Hiển thị thông tin nhiệt độ (T), độ ẩm (H) và ánh sáng (LDR) lên màn hình LCD
Nhấn vào *"biểu tượng kính lúp"* hoặc chọn *Tools > Serial Monitor* để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy giá trị cảm biến hiển thị lên Serial Monitor.

![](/ex/less10/image/05_1050px-Screenshot_2023-07-15_at_17.55.52.png)
Giá trị cảm biến hiển thị lên Serial Monitor

## Bài tập thêm (nâng cao)

<ins>Bài tập 1</ins>:

- Viết chương trình hiển thị các giá trị của **DHT11** (độ ẩm, nhiệt độ) và **Quang trở** (ánh sáng) lên màn hình **LCD**.
- **Còi** báo động khi một trong hai giá trị *"nhiệt độ"* hoặc *"độ ẩm"* vượt ngưỡng.
- Với nhiệt độ ngưỡng giá trị là trên <ins>30ºC</ins>, còn với độ ẩm ngưỡng giá trị là trên <ins>80%</ins>.
- Thêm mạch đèn **Led** để tự động bật đèn khi trời tối, độ sáng đèn sẽ được điều chỉnh bằng **Biến trở**.  
→ [Lời giải](/solution/README.md).

## Nguồn tài liệu (tham khảo thêm)

- [What is smart farming? Definition from WhatIs.com (techtarget.com)](https://www.techtarget.com/iotagenda/definition/smart-farming)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 9: Đo nhiệt độ độ ẩm bằng cảm biến DHT11 hiển thị lên LCD1602 (Library)](/ex/less09/README.md)
