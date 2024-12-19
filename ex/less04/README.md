# Bài 4: Nhận tín hiệu từ biến trở với Arduino (Analog In) - MakerEdu Starter Kit for Arduino

## Mô tả dự án

Trong các bài trước ta đã học về việc đọc và xuất tín hiệu Digital với chỉ hai mức tín hiệu là HIGH (1) hoặc LOW (0), tuy nhiên các thông tin ngoài môi trường như nhiệt độ, độ ẩm,...không chỉ là HIGH (1) hoặc LOW (0) mà là một dải giá trị liên tục (như từ 20 -> 50 độ), khi đó các mạch cảm biến đọc thông tin môi trường sẽ phải xuất ra một dải giá trị điện áp liên tục từ thấp đến cao để đặc trưng cho những thông tin này gọi là tín hiệu Analog.

![](/ex/less04/image/01_1050px-LM35_vout_chart.jpg)
Đồ thị về mối liên hệ giữa nhiệt độ và điện áp của cảm biến LM35
Để đọc được giá trị điện áp Analog thì Arduino sẽ sử dụng một bộ chuyển đổi ADC (Analog to Digital Converter) được tích hợp dô chỉ một số chân chuyên biệt trên Arduino gọi là chân Analog (trên Arduino Uno là từ A0\~A5) để chuyển từ giá trị điện áp đo được thành tín hiệu số với dải giá trị điện áp tương ứng 0\~5VDC là giá trị số nguyên từ 0\~1023.

<table><thead>
  <tr>
    <th>Mức điện áp</th>
    <th>0 V</th>
    <th>4,8 mV</th>
    <th>9,7 mV</th>
    <th>...</th>
    <th>0,48 V</th>
    <th>0,97 V</th>
    <th>1,46 V</th>
    <th>...</th>
    <th>4,99 V</th>
    <th>4,995 V</th>
    <th>5 V</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Giá trị ADC</td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>...</td>
    <td>100</td>
    <td>200</td>
    <td>300</td>
    <td>...</td>
    <td>1021</td>
    <td>1022</td>
    <td>1023</td>
  </tr>
</tbody>
</table>
Trong bài thực hành này các bạn sẽ sử dụng module biến trở của MakerEdu là một dạng điện trở dạng núm xoay nó thể thay đổi giá trị điện giữa các chân của nó theo góc xoay của trục xoay, giá trị điện trở này đã được MakerLab chuyển thành giá trị điện áp 0\~5VDC, từ việc đọc giá trị điện áp này các bạn có thể biết được góc của trục xoay biến trở, các bạn sẽ thường thấy điều này trên các Amply trong nhà bạn có các núm xoay để điều khiển tăng giảm âm lượng.

> Lưu ý:
Các bạn có thể tìm hiểu thêm bài viết về [Tín hiệu Analog, Digital và các chuẩn giao tiếp cơ bản](/ex/less02/A_D_signal_and_interface/README.md).

## Video

![](/ex/less04/image/02_video_less04.png)

## Các bước thực hiện

### Danh sách thiết bị

- [1x Mạch Vietduino Uno (Arduino Uno Compatible)](https://makerlab.vn/vuno)
- [1x Mạch MakerEdu Shield for Vietduino](https://makerlab.vn/vietduinosd)
- [1x Cáp USB-C](https://hshop.vn/cap-usb-type-c)
- [1x Mạch biến trở MKE-M04 potentiometer module](https://makerlab.vn/mkem04)

### Chuẩn bị trước dự án

- Kết nối mạch MakerEdu Shield với mạch Vietduino Uno [theo hướng dẫn](https://makerlab.vn/vietduinosd).
- Kết nối mạch Vietduino Uno với máy tính qua cáp USB-C sẽ thấy đèn nguồn (ON) trên mạch MakerEdu Shield phát sáng, cài đặt Driver và cấu hình mạch trên phần mềm Arduino [theo hướng dẫn tại đây](https://makerlab.vn/vuno).
- Tìm hiểu về cấu trúc của một chương trình trên phầm mềm Arduino và ngôn ngữ lập trình Arduino tại đây.

![](/ex/less04/image/03_1050px-connect.jpg)
Kết nối mạch Vietduino Uno + MakerEdu Shield với máy tính

### Chương trình

1. Mở phần mềm IDE Arduino và tạo một chương trình (Sketch) mới.
1. Copy đoạn code sau vào chương trình và tiến nạp chương trình (Upload) theo hướng dẫn tại đây.

```ino
// Potentiometer Example / Analog In - MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

// Chọn chân đọc tín hiệu Biến trở
// Phải chọn chân Analog trên Arduino để dùng tính năng (ADC)

# define POT_PIN A1 // A1

/*-----------------------------------------------------*/

// Biến lưu giá trị đọc từ biến trở
// Giá trị đọc được nằm trong khoảng từ 0-1023
int rawValue = 0;

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);
}

/*-----------------------------------------------------*/

void loop()
{
  // Đọc giá trị từ Biến trở
  rawValue = analogRead(POT_PIN);

  // In các thông tin lên Serial Monitor và Serial Plotter
  Serial.print("Raw:");
  Serial.print(rawValue);
  Serial.print(" ");
  Serial.print("Min:");
  Serial.print(0);
  Serial.print(" ");
  Serial.print("Max:");
  Serial.print(1023);
  Serial.println();
}
```

### Giải thích code

Chương trình gồm các câu lệnh được đặt trong 2 hàm bắt buộc của một chương trình Arduino là `void setup()` và `void loop()`  

`void setup()` (chứa các câu lệnh chỉ khởi chạy 1 lần khi khởi động)  

- `Serial.begin(baudrate)`: khởi động cổng kết nối Serial trên mạch Vietduino Uno với tốc độ (baudrate) mong muốn, các tốc độ hỗ trợ thường là: 9600, 115200,...

`void loop()` (chứa các câu lệnh chạy lặp đi lặp lại )  

- `analogRead(Pin)`: hàm này sẽ trả về giá trị của chân Analog cần đọc với dải giá trị điện áp tương ứng 0\~5VDC là giá trị số nguyên từ 0\~1023.
- `Serial.println(data)`: gửi dữ liệu từ mạch Vietduino Uno lên máy tính kèm theo ký tự xuống dòng, nếu dữ liệu là kiểu ký tự thì cần để trong dấu "".

<ins>Quá trình vận hành của chương trình như sau</ins>:

- Vietduino Uno sẽ liên tục đọc tín hiệu Analog từ Module biến trở.
- Sau đó gửi giá trị hiện tại qua Serial Monitor và hiển thị đồ thị trên Serial Plotter.

> **Chú thích thêm:**  
> Nội dung được gửi lên Serial Plotter theo một chuẩn định dạng sau:  
>
> - Chuỗi `"name_1:value_1 name_2:value_2 ... name_N:value_N"` theo sau là "kí tự xuống dòng" (`\n`) được tạo bới hàm `Serial.println()`.  
> - Việc gửi theo chuẩn định dạng này sẽ giúp ta xem được dữ liệu ở dạng đồ thị trên **Serial Plotter**.

### Kết nối phần cứng

<table><thead>
  <tr>
    <th>MakerEDU Shield</th>
    <th>Thiết bị</th>
    <th>Cáp kết nối</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Port A1</td>
    <td><a href="https://makerlab.vn/mkem04">Mạch biến trở MKE-M04 potentiometer module</a></td>
    <td><a href="https://hshop.vn/cap-ket-noi-makeredu-xh2-54-3wires-20cm-cable">MakerEdu XH2.54 3Wires</td>
  </tr>
</tbody>
</table>

### Kết quả

Sau khi đã nạp code thành công, nhấn vào "biểu tượng kính lúp" hoặc chọn Tools > Serial Monitor để mở Serial Monitor, chọn đúng tốc độ Baudrate là 115200bps để thấy chương trình hoạt động, giá trị điện áp của module biến trở sẽ được hiển thị lên Serial Monitor:

![](/ex/less04/image/04_1050px-Analog_in_example.jpg)
Module biến trở kết nối với Port A1 hiển thị giá trị lên Serial Monitor

![](/ex/less04/image/05_1050px-Screenshot_2023-07-10_at_11.16.02.png)
Serial Monitor trên phần mềm Arduino hiển thị giá trị của Module biến trở

> ### **Tip**  
>
> Nhấn vào "biểu tượng đồ thị" hoặc chọn Tools > Serial Plotter để mở Serial Plotter lúc này bạn có thể xem kết quả ở dạng đồ thị trực quan hơn.  
> Bạn nhớ ghi nhận lại giá trị **max** và **min** khi xoay biến trở, ta sẽ cần dùng đến trong những bài sau.

![](/ex/less04/image/06_1050px-MakerEdu_Starter_Kit_Bai4.png)
Hiển thị giá trị điện áp của Module biến trở bằng đồ thị trên Serial Plotter của Arduino  

## Bài tập thêm

<ins>Bài tập 1</ins>:

- Viết chương trình đọc và hiển thị giá trị của module biến trở ở **dạng điện áp** trên Serial Monitor / [Lời giải](/solution/README.md).

## Nguồn tài liệu (tham khảo thêm)

- [Bộ chuyển đổi Analog sang Digital (ADC).](https://www.geeksforgeeks.org/analog-to-digital-conversion/)
- [Cách sử dụng Serial Monitor & Serial Plotter trên phần mềm Arduino](https://wiki.makerlab.vn/index.php/C%C3%A1ch_s%E1%BB%AD_d%E1%BB%A5ng_Serial_Monitor_%26_Serial_Plotter_tr%C3%AAn_ph%E1%BA%A7n_m%E1%BB%81m_Arduino)
- [Getting Started with Arduino | Arduino Documentation](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino)

## Bài viết liên quan

- [Bộ MakerEdu Starter Kit for Arduino - MakerLab Wiki](README.md)
- [Bài 3: Nhận tín hiệu từ nút nhấn với Arduino (Digital In)](/ex/less03/README.md)
- [Bài 5: Điều kiển độ sáng đèn Led với Arduino (Analog Out)](/ex/less05/README.md)
