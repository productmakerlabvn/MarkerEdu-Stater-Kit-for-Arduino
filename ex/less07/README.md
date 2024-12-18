# Bài 7: Bật tắt đèn tự động với cảm biến ánh sáng quang trở và đèn Led (Analog In + Digital Out) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

Trong bài này các bạn sẽ học cách để làm một chiếc đèn tự động bật khi trời tối, các bạn sẽ thấy ứng dụng này chính là những cột đèn đường, đèn cổng tự động phát sáng khi trời tối rất tiện dụng đúng không?, hãy cùng làm thử với các phần cứng MakerEdu nhé!

![](/ex/less07/image/01_1050px-Auto_street_light.jpg)
Automatic street lights

## Video

[![](/ex/less07/image/02_video.png)](https://youtu.be/qOHlDC1ItIo)

## Các bước thực hiện

### Danh sách thiết bị

- [1x Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- [1x Mạch MakerEdu Shield for Vietduino](https://makerlab.vn/vietduinosd)
- [1x Cảm biến ánh sáng quang trở MKE-S02 LDR light sensor](https://makerlab.vn/mkes02)
- [1x Mạch led đơn MKE-M01 10mm single LED module](https://makerlab.vn/mkem01)
- [1x Cáp USB-C](https://hshop.vn/cap-usb-type-c)

### Chuẩn bị trước dự án

- Kết nối mạch MakerEdu Shield với mạch Vietduino Uno theo hướng dẫn.
- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch MakerEdu Shield phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino theo hướng dẫn tại đây.
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.

![](/ex/less07/image/03_connect.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

### Chương trình

1. Mở phần mềm IDE Arduino và tạo một chương trình (Sketch) mới.
1. Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) theo hướng dẫn tại đây.

```ino
// Bật tắt đèn tự động bằng cảm biến ánh sáng / MakerEdu Starter Kit for Arduino
/*-----------------------------------------------------*/

// Chọn chân điều khiển đèn Led

# define LED_PIN 9 // D9

// Chọn chân Analog đọc tín hiệu cảm biến ánh sáng LDR

# define LDR_PIN A2

/*-----------------------------------------------------*/

// Tạo biến lưu giá trị của cảm biến ánh sáng LDR
int rawValue = 0;
int lightValue = 0;
int darkValue = 0;

//Tạo biến đếm bổ trợ và giá trị mong muốn để Bật / Tắt đèn
int count = 0;
int levelOn = 0;

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Thiết lập chân D9 là Output để điều khiển Led
  pinMode(LED_PIN, OUTPUT);

  //Đo giá trị sáng nhất bằng cách lấy kết quả trung bình 5 lần đo khi sáng
  Serial.println("Xin để cảm biến ở chỗ sáng nhất!");
  Serial.print("Đo giá trị cảm biến khi trời sáng:");
  for (count = 5; count >= 0; count--)
  {
    delay(1000);
    //Đọc giá trị cảm biến ánh sáng LDR
    rawValue = analogRead(LDR_PIN);
    lightValue = lightValue + rawValue;
  }
  lightValue = lightValue / 5;
  Serial.println(lightValue);

  //Đo giá trị tối nhất bằng các lấy trung bình 5 lần đo khi tối
  Serial.println("Xin để cảm biến ở chỗ tối nhất!");
  Serial.print("Đo giá trị cảm biến khi trời tối:");
  delay(1000); //Chờ 1s trước khi đo
  for (count = 5; count >= 0; count--)
  {
    delay(1000);
    //Đọc giá trị cảm biến ánh sáng LDR
    rawValue = analogRead(LDR_PIN);
    darkValue = darkValue + rawValue;
  }
  darkValue = darkValue / 5;
  Serial.println(darkValue);

  //Mức sáng để bật đèn bằng 1/3 giá trị chênh lệch khi sáng và tối
  levelOn = (darkValue - lightValue)/3 + lightValue;
  Serial.print("Ngưỡng bật tắt đèn:");
  Serial.println(levelOn);
  delay(3000);
}

/*-----------------------------------------------------*/

void loop()
{
  //Đọc giá trị của cảm biến ánh sáng LDR lưu vào biến rawValue
  rawValue = analogRead(LDR_PIN);

  //So sánh giá trị đọc được với mức sáng mong muốn để Bật / Tắt đèn
  if (rawValue >= levelOn)
  {
    //Bật đèn khi giá trị đo được lớn hơn giá trị quy định
    digitalWrite(LED_PIN, HIGH);
    Serial.println("Led On");
  }
  else if (rawValue <= levelOn)
  {
    //Tắt đèn khi giá trị đo được nhỏ nhơn giá trị quy định
    digitalWrite(LED_PIN, LOW);
  }
  //In giá trị của cảm biến ánh sáng LDR lên Serial Monitor và Serial Plotter
  Serial.print("LDR:");
  Serial.print(rawValue);
  Serial.print(" ");
  Serial.print("Level On:");
  Serial.println(levelOn);
  delay(1000);
}
```

### Giải thích code

Chương trình chính gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`

`void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)

- Thiết lập chức năng của các chân điều khiển.
- Thực hiện đo lấy mẫu trung bình 5 lần để lấy giá trị môi trường khi sáng.
- Thực hiện đo lấy mẫu trung bình 5 lần để lấy giá trị môi trường khi tối.
- Thiết lập mức giá trị bật / tắt đèn dựa trên giá trị môi trường khi sáng / tối.
`void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )

- Đo giá trị cảm biến và so sánh với mức giá trị bật / tắt đèn.
- Thực hiện bật / tắt đèn theo giá trị so sánh.
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
    <td>Port D9</td>
    <td><a href="https://makerlab.vn/mkes02">Cảm biến ánh sáng quang trở MKE-S02 LDR light sensor</a></td>
    <td>MakerEdu XH2.54 3Wires</td>
  </tr>
  <tr>
    <td>Port A2</td>
    <td><a href="https://makerlab.vn/mkem01">Mạch led đơn MKE-M01 10mm single LED module</a></td>
    <td>MakerEdu XH2.54 3Wires</td>
  </tr>
</tbody>
</table>

### Kết quả

Chương trình sẽ tiến hành đo mức sáng tối của môi trường, từ đó định mức ngưỡng bật / tắt đèn, nhấn vào *"biểu tượng kính lúp"* hoặc chọn *Tools > Serial Monitor* để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy giá trị của cảm biến ánh sáng LDR được hiển thị lên Serial Monitor:

![](/ex/less07/image/04_1050px-Bat_tat_den_tu_dong.jpg)
Cảm biến ánh sáng LDR nối với Port A2 & module Led nối với Port D9 của MakerEdu Shield bật tắt tự động khi môi trường sáng / tối.

![](/ex/less07/image/05_1050px-Bat_den_tu_dong.png)
Serial Monitor trên phần mềm Arduino hiển thị giá trị của cảm biến ánh sáng LDR

Nhấn vào "biểu tượng đồ thị" hoặc chọn Tools > Serial Plotter để mở Serial Plotter lúc này bạn có thể xem các giá trị ở dạng đồ thị trực quan hơn:

![](/ex/less07/image/06_1050px-Screenshot_2023-07-12_at_13.52.15.png)
Hiển thị giá trị của cảm biến ánh sáng LDR bằng đồ thị trên Serial Plotter của Arduino

## Bài tập thêm (nâng cao)

<ins>Bài tập 1</ins>:

Viết chương trình điều chỉnh **độ sáng của đèn thích ứng theo môi trường**, môi trường càng tối thì đèn càng sáng và ngược lại / [Lời giải.](/solution/README.md)

## Nguồn tài liệu (tham khảo thêm)

- [Tìm hiểu đặc tính của Quang trở (Photoresistor).](https://eepower.com/resistor-guide/resistor-types/photo-resistor/#)
- [Sự khác nhau giữa "Photoresistor" và "Photodiode".](https://www.rfwireless-world.com/Terminology/Photoresistor-vs-Photodiode.html)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](/README.md)
- [Bài 6: Hiển thị thông tin lên màn hình LCD với Arduino (Library)](/ex/less06/README.md)
- [Bài 8: Báo cháy với cảm biến lửa và còi Buzzer (Analog In + Digital Out)](/ex/less08/README.md)
