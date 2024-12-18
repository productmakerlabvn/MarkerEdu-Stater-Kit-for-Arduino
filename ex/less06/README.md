# Bài 6: Hiển thị thông tin lên màn hình LCD với Arduino (Library) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

Trong bài này chúng ta sẽ cùng tìm hiểu về chuẩn giao tiếp I2C được hỗ trợ trên Arduino với [Mạch hiển thị MKE-M07 LCD1602 I2C module](https://makerlab.vn/mkem07), sơ bộ chuẩn I2C là sự kết hợp của hai chân tín hiệu Digital là SDA (A4) và SCL (A5), các bạn có thể tìm hiểu thêm về [chuẩn giao tiếp I2C tại đây](/ex/less02/A_D_signal_and_interface/README.md).

Với việc nhập xuất các tín hiệu đơn giản như Digital và Analog như ở các bài trước các bạn có thể sử dụng các hàm có sẵn của Arduino, tuy nhiên với các thiết bị sử dụng chuẩn giao tiếp phức tạp hơn các bạn cần phải cài đặt thêm các bộ thư viện hổ trợ của thiết bị (Library) để có thể sử dụng.

![](/ex/less06/image/01_1050px-I2C-SDA-SCL-01.jpg)
Kết nối giữa các thiết bị sử dụng chuẩn giao tiếp I2C.

## Video

[![](/ex/less06/image/02_video_less_06.png)](https://youtu.be/LT-ezKcKiDc)

## Các bước thực hiện

### Danh sách thiết bị

- [1x Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- [1x Mạch MakerEdu Shield for Vietduino](https://makerlab.vn/vietduinosd)
- [1x Cáp USB-C](https://hshop.vn/cap-usb-type-c)
- [1x Mạch hiển thị MKE-M07 LCD1602 I2C module](https://makerlab.vn/mkem07)

### Chuẩn bị trước dự án

- Kết nối mạch MakerEdu Shield với mạch Vietduino Uno [theo hướng dẫn](https://makerlab.vn/vietduinosd).
- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch MakerEdu Shield phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino [theo hướng dẫn tại đây](https://makerlab.vn/vuno).
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.

![](/ex/less06/image/03_connect.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

Cài đặt bộ thư viện (Library)
Tải và cài đặt bộ thư viện [LiquidCrystal_I2C](https://github.com/makerlabvn/LiquidCrystal_I2C) theo [hướng dẫn tại đây]().

### Chương trình

Mở phần mềm **IDE Arduino** và tạo một chương trình **(Sketch)** mới.
Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) theo [hướng dẫn tại đây]().

```ino
// Library Example / MKE-M07 LCD1602 I2C / MakerEdu Starter Kit for Arduino
/*-----------------------------------------------------*/

// Thêm bộ thư viện LCD LiquidCrystal_I2C

# include <LiquidCrystal_I2C.h>

// Khởi tạo màn hình MKE-M07 LCD1602 I2C với đối tượng là "lcd1"
LiquidCrystal_I2C lcd1(0x27,16,2);
// Cài đặt địa chỉ I2C là 0x27
// Mỗi dòng 16 ký tự
// 2 dòng hiển thị

// Taọ biến đếm kiểu số nguyên count
int count = 0;

/*-----------------------------------------------------*/

void setup()
{
  // Khởi động màn hình LCD
  lcd1.init();
  // Xóa toàn bộ nội dung trên màn hình LCD (nếu có)
  lcd1.clear();
  // Bật đèn nền màn hình LCD
  lcd1.backlight();

  // Tại vị trí cột 0 dòng 0, cho in nội dung...
  lcd1.setCursor(0, 0);
  lcd1.print("Hello World ^^");
  // Tại vị trí cột 0 dòng 1, cho in nội dung...
  lcd1.setCursor(0, 1);
  lcd1.print("Lets Start!");
  // Chờ 3 giây
  delay(3000);
}

/*-----------------------------------------------------*/

void loop()
{
  // Xóa màn hình
  lcd1.clear();
   // Tại vị trí cột 0 dòng 0, cho in nội dung...
  lcd1.setCursor(0, 0);
  lcd1.print("MakerEDU");
  // Tại vị trí cột 0 dòng 1, cho in nội dung...
  lcd1.setCursor(0, 1);
  lcd1.print("Starter Kit");

  // Chờ 3 giây
  delay(3000);
  // Xóa màn hình
  lcd1.clear();

  //Cho in số đếm ngược từ 5-0
  for (count = 5; count >= 0; count--)
  {
    // Tại vị trí cột 0 dòng 0, cho in nội dung...
    lcd1.setCursor(0, 0);
    lcd1.print("Count:");
    // In "số đếm" tại vị trí cột 7 dòng 0
    lcd1.setCursor(7, 0);
    lcd1.print(count);
    // Đợi 1s để đếm tiếp
    delay(1000);
  }
  //Hiển thị thông báo sau khi đếm xong...
  lcd1.setCursor(0, 1);
  lcd1.print("Peekaboo!");
  delay(2000);
}
```

### Giải thích code  

Để có thể sử dụng các hàm để điều khiển màn hình [MKE-M07 LCD1602 I2C](https://makerlab.vn/mkem07) đầu tiên ta cần thêm bộ thư viện vào đầu chương trình:

`#include <LiquidCrystal_I2C.h>`: Thêm bộ thư viện [LiquidCrystal_I2C.h](https://github.com/makerlabvn/LiquidCrystal_I2C) với các hàm điều khiển màn hình

Sau đó ta cần khai báo đối tượng cần điều khiển như ở đây ta khai báo là "lcd1", bạn cũng có thể khai báo thêm "lcd2", "lcd3" tuỳ thích, khi đó các hàm điều khiển phía sau sẽ phải gắn với các đối tượng này:

`LiquidCrystal_I2C lcd1(0x27, 16, 2)`: Khai báo đối tượng màn hình "lcd1" với địa chỉ 0x27, 16 ký tự, 2 dòng.

> Lưu ý:  
> Để điều khiển thêm các LCD khác bạn cần phải thiết lập địa chỉ khác với địa chỉ mặc định 0x27, [tham khảo tài liệu sau](https://makerlab.vn/mkem07) để biết cách thiết lập địa chỉ LCD.  

 Chương trình chính gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`  

`void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)

`lcd1.init()`: khởi động màn hình "lcd1", là câu lệnh bắt buộc để bắt đầu điều khiển màn hình.
`lcd1.clear()`: xoá toàn bộ nội dung trên màn hình "lcd1".
`lcd1.backlight()`: bật đèn nền màn hình "lcd1".
`void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )

`lcd1.setCursor(column, row)`: chuyển con trỏ đến vị trí cột và dòng mong muốn của "lcd1" trước khi in ký tự.
`lcd1.print(data)`: In dữ liệu mong muốn tại vị trí con trỏ của "lcd1", nếu là dạng ký tự thì cần bỏ trong dấu "".
`delay(time)`: yêu cầu Vietduino Uno chờ (không làm gì cả) trong một khoảng thời gian nhất định, đơn vị là mili giây (ms).

<ins>Quá trình vận hành của chương trình như sau</ins>

- Khởi động đối tượng "lcd1" tại địa chỉ 0x27 với cấu hình là 16 ký tự, 2 dòng.
- In các thông tin và đếm ngược từ 5 đến 0.
- Hiển thị "Peekaboo" khi kết thúc và lặp lại quá trình.

## Sơ đồ kết nối

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
    <td><a href="https://hshop.vn/cap-ket-noi-makeredu-xh2-54-4wires-20cm-cable">MakerEdu XH2.54 4Wires</td>
  </tr>
</tbody>
</table>

## Kết quả  

Sau khi đã nạp code thành công bạn có thể xem kết quả trực tiếp trên màn hình LCD, phía sau màn hình LCD có một biến trở (màu xanh dương) để chỉnh "độ tương phản" bạn có thể dùng tua vít để điều chỉnh bạn thấy quá đậm hoặc quá mờ.

![](/ex/less06/image/04_1050px-Man_hinh_LCD1602_I2C.jpg)
Màn hình MKE-M07 LCD1602 kết nối với Port I2C của MakerEdu Shield hiển thị thông tin.

## Bài tập thêm

### <ins>Bài tập 1:</ins>

- Viết chương trình nhận ký tự từ Serial Monitor của Arduino sau đó hiển thị lên màn hình LCD / [Lời giải](/solution/README.md).

## Nguồn tài liệu (tham khảo thêm)

- [Tìm hiểu chi tiết về Giao thức I2C.](https://www.circuitbasics.com/basics-of-the-i2c-communication-protocol/)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 5: Điều kiển độ sáng đèn Led với Arduino (Analog Out)](/ex/less05/README.md)
- [Bài 7: Bật tắt đèn tự động với cảm biến ánh sáng quang trở và đèn Led (Analog In + Digital Out)](/ex/less07/README.md)
