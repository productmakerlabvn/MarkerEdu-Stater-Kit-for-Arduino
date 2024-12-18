# Bài 8: Báo cháy với cảm biến lửa và còi Buzzer (Analog In + Digital Out) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

Trong bài này các bạn sẽ học cách để thực hiện một hệ thống báo cháy sử dụng nguyên lý phát hiện lửa bằng tia hồng ngoại (IR-Infrared) phát ra trong các nguồn nhiệt, ứng dụng trong các hệ thống: chữa cháy tự động, báo cháy từ xa,...

![](/ex/less08/image/01_1050px-Fire-Detectors-–-Infrared-Flame-Detectors-min.jpg)
Infrared Flame Detector

## Video

[![](/ex/less08/image/02_video.png)](https://youtu.be/gSkmenG_54E)

## Các bước thực hiện

### Danh sách thiết bị

- [1x Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- [1x Mạch MakerEdu Shield for Vietduino](https://makerlab.vn/vietduinosd)
- [1x Cảm biến lửa MKE-S04 IR infrared flame sensor](https://makerlab.vn/mkes04)
- [1x Mạch còi báo MKE-M03 buzzer module](https://makerlab.vn/mkem03)
- [1x Cáp USB-C](https://hshop.vn/cap-usb-type-c)

### Chuẩn bị trước dự án

- Kết nối mạch MakerEdu Shield với mạch Vietduino Uno theo hướng dẫn.
- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch MakerEdu Shield phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino theo hướng dẫn tại đây.
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.

![](/ex/less08/image/03_1050px-connect.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

### Chương trình

1. Mở phần mềm IDE Arduino và tạo một chương trình (Sketch) mới.
1. Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) theo hướng dẫn tại đây.

```ino
// Hệ thống báo cháy với cảm biến lửa / MakerEdu Starter Kit for Arduino
/*-----------------------------------------------------*/

#define BUZZ_PIN 11 // Chọn chân điều khiển còi Buzzer là D11

// Biến kiểu logic để lưu giữ trạng thái của Buzzer, 0 là không kêu, 1 là kêu
bool stateBuzz = 0;

#define FLAME_SS A3 // Chọn chân kết nối với cảm biến lửa là A3

/*-----------------------------------------------------*/

// Tạo các biến để lưu giá trị của cảm biến lửa
int rawValue = 0;
int normalValue = 0;
int fireValue = 0;

int count = 0; //Tạo biến đếm
int levelOn = 0; //Tạo biến lưu giá trị mức để Bật / Tắt báo động.

//Biến kiểm tra trạng thái môi trường, 0 là an toàn, 1 là nguy hiểm
bool warning = 0;

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Thiết lập chân D11 là Output để điều khiển còi
  pinMode(BUZZ_PIN, OUTPUT);
  // Khi mới khởi động, tắt còi
  digitalWrite(BUZZ_PIN, LOW);

  //Đo giá trị cảm biến ở môi trường bình thường, lấy mẫu trung bình 5 lần
  Serial.print("Giá trị cảm biến tại ánh sáng môi trường:");
  delay(500); //Chờ 0.5s trước khi đo
  for (count = 5; count >= 0; count--)
  {
    delay(1000);
    //Đọc giá trị cảm biến lửa
    rawValue = analogRead(FLAME_SS);
    normalValue = normalValue + rawValue;
  }
  normalValue = normalValue / 5;
  Serial.println(normalValue);

  //Đo giá trị cảm biến khi có lửa, lấy mẫu trung bình 5 lần
  Serial.print("Giá trị cảm biến khi có lửa:");
  delay(2000); //Chờ 2s trước khi đo để bật lửa
  for (count = 5; count >= 0; count--)
  {
    delay(1000);
    // Đọc giá trị cảm biến lửa
    rawValue = analogRead(FLAME_SS);
    fireValue = fireValue + rawValue;
  }
  fireValue = fireValue / 5;
  Serial.println(fireValue);

  //Mức để bật báo động bằng 1/3 giá trị chênh lệch khi có lửa và không có lửa
  levelOn = (normalValue - fireValue)/3 + fireValue;
  Serial.print("Ngưỡng bật tắt báo động:");
  Serial.println(levelOn);
  delay(3000);
}

/*-----------------------------------------------------*/

void loop()
{
  // Đọc giá trị của cảm biến lửa lưu vào biến rawValue
  rawValue = analogRead(FLAME_SS);

  // Kiểm tra nếu cảm biến ở dưới mức giá trị quy định (có lửa)
  if (rawValue <= levelOn)
  {
    // Bật cảnh báo, khi phát hiện có nguy hiểm
    warning = 1;
  }
  // Nếu cảm biến ở trên mức giá trị quy định (bình thường)
  else if (rawValue >= levelOn)
  {
    // Tắt cảnh báo, khi phát hiện đã an toàn
    warning = false;
    // Tắt còi
    digitalWrite(BUZZ_PIN, LOW);
  }

  // Cho còi kêu tick...tick khi có nguy hiểm !!!
  if (warning)
  {
      stateBuzz = !stateBuzz;
      digitalWrite(BUZZ_PIN, stateBuzz);
  }

  // In các thông tin lên Serial
  Serial.print("Flame:");
  Serial.print(rawValue);
  Serial.print(" ");
  Serial.print("LevelOn:");
  Serial.print(levelOn);
  Serial.println();
  delay(500);
}
```

### Giải thích code

Chương trình chính gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`

`void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)

- Thiết lập chức năng của các chân điều khiển.
- Thực hiện đo lấy mẫu trung bình 5 lần để lấy giá trị môi trường bình thường
- Thực hiện đo lấy mẫu trung bình 5 lần để lấy giá trị môi trường khi có lửa.
- Thiết lập mức giá trị bật / tắt báo động dựa trên giá trị môi trường khi có lửa / không có lửa.
`void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )

- Đo giá trị cảm biến và so sánh với mức giá trị bật / tắt báo động.
- Thực hiện bật / tắt báo động theo giá trị so sánh.
- In giá trị cảm biến lên Serial Monitor và Serial Plotter.

### Sơ đồ kết nối

<table><thead>
  <tr>
    <th>MakerEDU Shield</th>
    <th>Thiết bị</th>
    <th>Cáp kết nối</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Port D11</td>
    <td><a href="https://makerlab.vn/mkem03">Mạch còi báo MKE-M03 buzzer module</a></td>
    <td>MakerEdu XH2.54 3Wires</td>
  </tr>
  <tr>
    <td>Port A3</td>
    <td><a href="https://makelab.vn/mkes04">Cảm biến lửa MKE-S04 IR infrared flame sensor</a></td>
    <td>MakerEdu XH2.54 3Wires</td>
  </tr>
</tbody>
</table>

## Kết quả

Chương trình sẽ tiến hành đo giá trị của môi trường khi có và không có lửa, từ đó định mức ngưỡng bật / tắt báo động, nhấn vào *"biểu tượng kính lúp"* hoặc chọn *Tools > Serial Monitor* để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy giá trị của cảm biến lửa được hiển thị lên Serial Monitor:

![](/ex/less08/image/04_1050px-He_thong_bao_chay.jpg)
Cảm biến lửa nối với Port A3 và Buzzer nối với Port D11 sẽ báo động khi có lửa

![](/ex/less08/image/05_1050px-Screenshot_2023-07-14_at_10.48.13.png)
Serial Monitor trên phần mềm Arduino hiển thị giá trị của cảm biến lửa IR Flame Sensor

Nhấn vào "biểu tượng đồ thị" hoặc chọn Tools > Serial Plotter để mở Serial Plotter lúc này bạn có thể xem các giá trị ở dạng đồ thị trực quan hơn:

![](/ex/less08/image/06_1050px-Screenshot_2023-07-14_at_10.48.50.png)
Hiển thị giá trị của cảm biến lửa IR Flame Sensor bằng đồ thị trên Serial Plotter của Arduino

## Bài tập thêm (nâng cao)

<ins>Bài tập 1</ins>:

- Dùng nút nhấn để làm chức năng báo động. Khi nhấn nút còi báo, nhấn lần nữa để tắt báo động / [Lời giải.](/solution/README.md)

## Nguồn tài liệu (tham khảo thêm)

- [Cảm biến Lửa hoạt động như thế nào?.](https://www.elprocus.com/flame-sensor-working-and-its-applications/)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 7: Bật tắt đèn tự động với cảm biến ánh sáng quang trở và đèn Led (Analog In + Digital Out)](/ex/less07/README.md)
- [Bài 9: Đo nhiệt độ độ ẩm bằng cảm biến DHT11 hiển thị lên LCD1602 (Library)](/ex/less09/README.md)
