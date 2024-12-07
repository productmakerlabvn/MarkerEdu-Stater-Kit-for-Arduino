# Lời giải các bài tập thực hành - MakerEdu Starter Kit for Arduino

## Bài 1: Xin chào Arduino! (Serial Monitor)

(B1) - Hàm `Serial.write(number)` được sử dụng để in các ký tự trong **Bảng mã ASCII**, hãy thử in *"Hello Arduino"* bằng hàm này.
```ino
// Exercise 1.1 Arduino - MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

void setup()
{
  // Khởi động chuẩn kết nối Serial với tốc độ Baudrate 115200bps
  Serial.begin(115200);
}

/*-----------------------------------------------------*/

void loop()
{
  /**

* Truy cập link này để xem bảng mã ASCII:
* → <https://wiki.makerlab.vn/index.php/Character>
*
* Để in chuỗi nội dung "Hello Arduino"
* Các bạn in lần lượt dãy số sau bằng hàm Serial.write(number)
* → 72, 101, 108, 108, 111, 32, 65, 114, 100, 117, 105, 110, 111
   */

  // Nội dung này được in ra cứ mỗi 2 giây (2000 mili giây)
  Serial.write(72);  // 'H'
  Serial.write(101); // 'e'
  Serial.write(108); // 'l'
  Serial.write(108); // 'l'
  Serial.write(111); // 'o'
  Serial.write(32);  // ' '
  Serial.write(65);  // 'A'
  Serial.write(114); // 'r'
  Serial.write(100); // 'd'
  Serial.write(117); // 'u'
  Serial.write(105); // 'i'
  Serial.write(110); // 'n'
  Serial.write(111); // 'o'
  delay(1000);
  //
  Serial.println("_Done");
  delay(1000);
}
```
## Bài 2: Điều khiển đèn Led chớp tắt với Arduino (Digital Out)

(B1) - Sử dụng **Serial Monitor** để điều khiển đèn Led từ máy tính với ký tự `'1'` là bật đèn và `'0'` là tắt đèn.
```ino
// Exercise 2.1 Arduino - MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

# define LED_PIN 9 // Khai báo chân điều khiển đèn Led là D9

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Thiết lập chân D9 là Output để điều khiển Led
  pinMode(LED_PIN, OUTPUT);

  // Tắt đèn Led khi mới khởi động
  digitalWrite(LED_PIN, LOW);
}

/*-----------------------------------------------------*/

void loop()
{
  // In ra ký tự được tryền từ máy tính
  while (Serial.available())
  {
    char key = Serial.read();
    Serial.print("Received_");
    Serial.println(key);

    if (key == '1')
    {
      // Bật đèn Led trong 1s và in thông báo "ON"
      digitalWrite(LED_PIN, HIGH);
      Serial.println("ON");
    }
    else if (key == '0')
    {
      // Tắt đèn Led trong 1s và in thông báo "OFF"
      digitalWrite(LED_PIN, LOW);
      Serial.println("OFF");
    }
  }
}
```
## Bài 3: Nhận tín hiệu từ nút nhấn với Arduino (Digital In)

(B2) - Viết chương trình khi **nhấn nút** sẽ bật đèn Led và **nhả nút** ra sẽ tắt đèn Led.
```ino
// Exercise 3.2 Arduino - MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

# define BUTTON_PIN 10 // Chọn chân đọc tín hiệu Nút nhấn là D10
# define LED_PIN 9     // Chọn chân điều khiển đèn Led là D9

// Chọn mức tín hiệu đọc từ Nút nhấn với trạng thái tương ứng
# define PRESS_BUTTON LOW    // Khi "nhấn nút" tín hiệu đọc được là LOW
# define RELEASE_BUTTON HIGH // Khi "nhả nút" tín hiệu đọc được là HIGH

// Chọn khoảng thời gian chờ để xác minh nút đã được nhấn
# define TIME_DEBOUNCE 10 // Đơn vị (ms)

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Thiết lập chân D10 là Input để đọc tín hiệu từ Nút nhấn
  pinMode(BUTTON_PIN, INPUT);

  // Thiết lập chân D9 là Output để điều khiển Led
  pinMode(LED_PIN, OUTPUT);
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
      // Bật đèn Led
      digitalWrite(LED_PIN, HIGH);

      // Chờ cho đến khi "nhả" nút
      while (digitalRead(BUTTON_PIN) == PRESS_BUTTON)
      {
      }

      // In thông báo cho biết nút đã "nhả"
      Serial.println("RELEASE");
      // Tắt đèn Led
      digitalWrite(LED_PIN, LOW);
    }
  }
}
```
## Bài 4: Nhận tín hiệu từ biến trở với Arduino (Analog In)

(B1) - Viết chương trình đọc và hiển thị giá trị của module biến trở ở **dạng điện áp** trên Serial Monitor.
```ino
// Exercise 4.1 Arduino / Analog In - MakerEdu Starter Kit for Arduino

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

  /**

* Độ phân giải mặc định của bộ ADC Arduino Uno là 10-bit
* Tương ứng khả năng đọc dãi điện áp từ 0-5VDC
* Với các giá trị trả về trong khoảng từ 0-1023
*
* Ta sử dụng công thức sau để chuyển đổi giá trị ADC đọc được sang điện áp
* (voltage)   (ADC)
* --------- = -----
*     5       1023

   */
  float voltage = rawValue* 5.0 / 1023.0;

  // In các thông tin lên Serial Monitor và Serial Plotter
  Serial.print("Voltage:");
  Serial.print(voltage);
  Serial.print(" ");
  Serial.print("Min:");
  Serial.print(0);
  Serial.print(" ");
  Serial.print("Max:");
  Serial.print(5);
  Serial.println();
}
```
## Bài 5: Điều kiển độ sáng đèn Led với Arduino (Analog Out)

(B1) - Viết chương trình điều khiển độ sáng đèn Led ***bằng biến trở*** (kết hợp Analog In và Analog Out).
```ino
// Exercise 5.1 / Analog Out - MakerEdu Starter Kit for Arduino
/*-----------------------------------------------------*/

// Chọn chân đọc tín hiệu Biến trở
// Phải chọn chân Analog trên Arduino để dùng tính năng (ADC)
# define POT_PIN A1 // A1

// Chọn chân điều khiển đèn Led
// Các Port Digital trên MakerEDU Shield D9, D10, D11 đều có tính năng PWM
# define LED_PIN 9 // D9

/*-----------------------------------------------------*/

// Biến lưu giá trị đọc từ biến trở
// Giá trị đọc được nằm trong khoảng từ 0-1023
int rawValue = 0;

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
  // Đọc giá trị từ Biến trở
  rawValue = analogRead(POT_PIN);

  /**

* Giá trị ADC trả về trong khoảng từ 0 đến 1023
* Giá trị PWM nhận được trong khoảng từ 0 đến 255
*
* Sử dụng hàm map(value, fromLow, fromHigh, toLow, toHigh)
* Để ánh xạ giá trị từ thang đo ADC sang thang đo PWM
   */
  power = map(rawValue, 0, 1023, 0, 255);

  // Điều khiển độ sáng Led theo giá trị Biến trở
  analogWrite(LED_PIN, power);

  // In các thông tin lên Serial Monitor và Serial Plotter
  Serial.print("Power:");
  Serial.print(power);
  Serial.print(" ");
  Serial.print("Min:");
  Serial.print(0);
  Serial.print(" ");
  Serial.print("Max:");
  Serial.print(255);
  Serial.println();
}
```
## Bài 6: Hiển thị thông tin lên màn hình LCD với Arduino (Library)

(B1) - Viết chương trình **nhận ký tự từ Serial Monitor** của Arduino sau đó hiển thị lên màn hình LCD.
```ino
// Exercise 6.1 / MKE-M07 LCD1602 I2C / MakerEdu Starter Kit for Arduino
/*-----------------------------------------------------*/

// Thêm bộ thư viện LCD LiquidCrystal_I2C
# include <LiquidCrystal_I2C.h>

// Khởi tạo màn hình MKE-M07 LCD1602 I2C với đối tượng là "lcd1"
LiquidCrystal_I2C lcd1(0x27, 16, 2);
// Cài đặt địa chỉ I2C là 0x27
// Mỗi dòng 16 ký tự
// 2 dòng hiển thị

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Khởi động màn hình LCD
  lcd1.init();
  // Xóa toàn bộ nội dung trên màn hình LCD (nếu có)
  lcd1.clear();
  // Bật đèn nền màn hình LCD
  lcd1.backlight();
}

/*-----------------------------------------------------*/

void loop()
{
  // Đợi thông tin người dùng nhập từ bàn phím
  while (Serial.available())
  {
    // Lưu toàn bộ nội dung vừa nhận được
    String text = Serial.readString();

    // In nội dung lên Serial Monitor
    Serial.print("Received_");
    Serial.println(text);

    // Xóa màn hình, trước khi hiển thị nội dung mới
    lcd1.clear();

    // Lưu độ dài của biến "text"
    // Nhớ chọn "No line ending" trên Serial Monitor
    int length = text.length();

    /**
     * Chúng ta sử dụng màn hình LCD 16x02
     * Có kích thước 2 dòng, mỗi dòng hiển thị được 16 kí tự
     *
     * Vậy nên nếu nội dung trong biến "text" lớn hơn 16 kí tự
     * Chúng ta sẽ cho in nội dung đó xuống dòng thứ 2
     * Nếu biến "text" lớn hơn 32 kí tự, ta phải bỏ qua phần kí tự dư đó thôi
     *
     * Sử dụng hàm substring() để lấy bất kì phần nội dung nào bạn muốn trong biến "text"
     */
    if (length <= 16)
    {
      // Tại vị trí cột 0 dòng 0, cho in nội dung...
      lcd1.setCursor(0, 0);
      lcd1.print(text);
    }
    else
    {
      // Tại vị trí cột 0 dòng 0, cho in 16 kí tự đầu
      lcd1.setCursor(0, 0);
      lcd1.print(text.substring(0, 16));
      // Tại vị trí cột 0 dòng 1, cho in kí tự thứ 17 trở đi
      lcd1.setCursor(0, 1);
      lcd1.print(text.substring(16));
    }
  }
}
```
## Bài 7: Bật tắt đèn tự động với cảm biến ánh sáng quang trở và đèn Led (Analog In + Digital Out)

(B1) - Viết chương trình điều chỉnh độ sáng của đèn thích ứng theo môi trường, môi trường càng tối thì đèn càng sáng và ngược lại.
```ino
// Exercise 7.1 / [MKE-S02] LDR Light Sensor / MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

// Chọn chân Analog đọc tín hiệu cảm biến ánh sáng LDR
# define LDR_PIN A2

// Chọn chân điều khiển đèn Led
# define LED_PIN 9 // D9

/*-----------------------------------------------------*/

// Biến lưu giá trị đọc từ Quang trở
// Giá trị đọc được nằm trong khoảng từ 0-1023
int rawValue = 0;

// Lưu giá trị "độ rộng xung" cấp cho đèn Led
// Giá trị (PWM) có độ phân giải 8-bit, nằm trong khoảng 0 - 255
int power = 0;

// Lưu giá trị lớn nhất đọc từ cảm biến Quang trở
// Đây là loại cảm biến 3.3V nên giả định giá trị ban đầu là 675
int max = 675;

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
  // Đọc giá trị từ Quang trở
  rawValue = analogRead(LDR_PIN);

  // Cập nhập nếu phát hiện giá trị ADC lớn hơn hiện tại "max" biết
  if (rawValue > max)
  {
    max = rawValue;
  }

  /**

* Giá trị ADC trả về trong khoảng từ 0 đến 1023
* Giá trị PWM nhận được trong khoảng từ 0 đến 255
*
* Sử dụng hàm map(value, fromLow, fromHigh, toLow, toHigh)
* Để ánh xạ giá trị từ thang đo ADC sang thang đo PWM
*
* Trời càng tối, giá trị đọc từ Quang trở càng cao
* Led càng sáng, với giá trị PWM tăng theo tương ứng
   */
  power = map(rawValue, 0, max, 0, 255);

  // Điều khiển độ sáng Led theo giá trị Quang trở
  analogWrite(LED_PIN, power);

  // In các thông tin lên Serial Monitor và Serial Plotter
  Serial.print("power:");
  Serial.print(power);
  Serial.print(" ");
  Serial.print("Min:");
  Serial.print(0);
  Serial.print(" ");
  Serial.print("Max:");
  Serial.print(255);
  Serial.println();
}
```
## Bài 8: Báo cháy với cảm biến lửa và còi Buzzer (Analog In + Digital Out)

(B1) - Dùng nút nhấn để làm chức năng báo động. Khi **nhấn nút** còi báo, **nhấn lần nữa** để tắt báo động.
```ino
// Exercise 8.1 / [MKE-S04] IR Infrared Flame Sensor / MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

// Chọn chân đọc tín hiệu Nút nhấn
# define BUTTON_PIN 10 // D10

// Chọn mức tín hiệu đọc từ Nút nhấn với trạng thái tương ứng
# define PRESS_BUTTON LOW    // Khi "nhấn nút" tín hiệu đọc được là LOW
# define RELEASE_BUTTON HIGH // Khi "nhả nút" tín hiệu đọc được là HIGH

// Chọn khoảng thời gian chờ để xác minh nút đã được nhấn
# define TIME_DEBOUNCE 10 // Đơn vị (ms)

// Chọn chân điều khiển đèn Còi
# define BUZZ_PIN 11 // D11

/*-----------------------------------------------------*/

// Biến kiểu logic, bật tắt cảnh báo
// True  = có nguy hiểm
// False = bình thường
bool warning = false;

// Lưu thời điểm thay đổi trạng thái Còi
unsigned long timeChange;

// Biến kiểu logic, cho biết trạng thái hiện tại của Còi
// True  = bật còi
// False = tắt còi
bool buzz = false;

/*-----------------------------------------------------*/

// Hàm xử lý phần đọc tín hiệu từ Nút nhấn
void readButton()
{
  // Nghi vấn nút đang được "nhấn" ?
  if (digitalRead(BUTTON_PIN) == PRESS_BUTTON)
  {
    // Chờ qua khoảng thời gian để xác minh
    delay(TIME_DEBOUNCE);

    // Xác thực chắc chắn nút đang được "nhấn"
    if (digitalRead(BUTTON_PIN) == PRESS_BUTTON)
    {
      // Chờ cho đến khi "nhả" nút
      while (digitalRead(BUTTON_PIN) == PRESS_BUTTON)
      {
      }

      // Bật hoặc tắt cảnh báo
      if (warning)
      {
        // Tắt cảnh báo
        warning = false;
        digitalWrite(BUZZ_PIN, LOW);
        Serial.println("NORMAL");
      }
      else
      {
        // Bật cảnh báo
        warning = true;
        Serial.println("WARNING");
      }
    }
  }
}

// Hàm xử lý phần điều hiển Còi báo động
void controlBuzzer()
{
  // Thay đổi trạng thái Còi mỗi 0,1s
  if (millis() - timeChange >= 100)
  {
    // Cập nhập thời điểm mới thay đổi
    timeChange = millis();

    // Đảo trạng thái Còi
    buzz = !buzz;
    digitalWrite(BUZZ_PIN, buzz);
  }
}

/*-----------------------------------------------------*/

void setup()
{
  // Mở cổng Serial Baudrate 115200 bps
  Serial.begin(115200);

  // Thiết lập chân D10 là Input để đọc tín hiệu từ Nút nhấn
  pinMode(BUTTON_PIN, INPUT);

  // Thiết lập chân D11 là Output để điều khiển còi
  pinMode(BUZZ_PIN, OUTPUT);
  // Khi mới khởi động, tắt còi
  digitalWrite(BUZZ_PIN, LOW);
}

/*-----------------------------------------------------*/

void loop()
{
  // Đọc tín hiệu Nút nhấn
  readButton();

  // Còi được bật dựa theo trạng thái biến "warning"
  if (warning)
  {
    controlBuzzer();
  }
}
```
## Bài 9: Đo nhiệt độ độ ẩm bằng cảm biến DHT11 hiển thị lên LCD1602 (Library)

(B1) - Định mức và hiển thị thông báo lên màn hình LCD ký hiệu **"!"** phía sau giá trị của nhiệt độ và độ ẩm khi vượt mức quy định. Ví dụ, đặt ngưỡng nhiệt độ là **30ºC** và ngưỡng độ ẩm là **80%**.
```ino
// Exercise 9.1 / [MKE-S14] DHT11 Temperature and Humidity Sensor / MakerEdu Starter Kit for Arduino

/*-----------------------------------------------------*/

// Thêm bộ thư viện cho cảm biến và LCD
# include <LiquidCrystal_I2C.h>
# include <DHT.h>

/*-----------------------------------------------------*/

// Khởi tạo màn hình MKE-M07 LCD1602 I2C với đối tượng là "lcd"
LiquidCrystal_I2C lcd(0x27, 16, 2);
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

  // Kiểm tra giá trị "nhiệt độ" có vượt ngưỡng ko?
  if (temp >= 30)
  {
    lcd.setCursor(15, 0);
    lcd.print("!");
  }
  else
  {
    lcd.setCursor(15, 0);
    lcd.print(" ");
  }
  // Kiểm tra giá trị "độ ẩm" có vượt ngưỡng ko?
  if (humi >= 80)
  {
    lcd.setCursor(15, 1);
    lcd.print("!");
  }
  else
  {
    lcd.setCursor(15, 1);
    lcd.print(" ");
  }

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
## Bài 10: Mô phỏng hệ thống giám sát môi trường trong nông nghiệp thông minh (Tổng hợp)

(B1) - Viết chương trình hiển thị các giá trị của **DHT11** (độ ẩm, nhiệt độ) và **Quang trở** (ánh sáng) lên màn hình **LCD**.

**Còi** báo động khi một trong hai giá trị *"nhiệt độ"* hoặc *"độ ẩm"* vượt ngưỡng.

Với nhiệt độ ngưỡng giá trị là trên <ins>30ºC</ins>, còn với độ ẩm ngưỡng giá trị là trên <ins>80%</ins>.

Thêm mạch đèn **Led** để tự động bật đèn khi trời tối, độ sáng đèn sẽ được điều chỉnh bằng **Biến trở**.
```ino
// Exercise 10.1

/*------------------------------------------------------------------------- */
/*                                  LIBRARY                                  */
/* -------------------------------------------------------------------------*/

// Thêm bộ thư viện cho cảm biến DHT và LCD
# include <LiquidCrystal_I2C.h>
# include <DHT.h>

/*------------------------------------------------------------------------- */
/*                                 CONFIG PIN                                */
/* -------------------------------------------------------------------------*/

// Chọn chân cho các thiết bị
# define POT_PIN A1  // A1 - Biến trở
# define LDR_PIN A2  // A2 - Quang trở
# define DHT_PIN 9   // D9 - DHT11
# define BUZZ_PIN 10 // D10 - Còi
# define LED_PIN 11  // D11 - Led

# define LEVEL_LIGHT 50 // Ngưỡng trời sáng (%)
# define LEVEL_DARK 20  // Ngưỡng trời tối (%)

# define LEVEL_TEMP 30 // Ngưỡng nhiệt độ (ºC)
# define LEVEL_HUMI 80 // Ngưỡng độ ẩm (%)

# define FREQ_BUZZ 100 // Đơn vị (ms)

/*------------------------------------------------------------------------- */
/*                                   OBJECT                                  */
/* -------------------------------------------------------------------------*/

// Khởi tạo màn hình MKE-M07 LCD1602 I2C với đối tượng là "lcd"
LiquidCrystal_I2C lcd(0x27, 16, 2);
// Cài đặt địa chỉ I2C là 0x27
// Mỗi dòng 16 ký tự
// 2 dòng hiển thị

// Khởi tạo cảm biến MKE-S14 DHT11 với đối tượng là "dht" tại Port D9
DHT dht(DHT_PIN, DHT11);

/*------------------------------------------------------------------------- */
/*                                  VARIABLE                                 */
/* -------------------------------------------------------------------------*/

// Biến lưu giá trị đọc từ Biến trở
int valuePot = 0;

// Biến lưu giá trị đọc từ Quang trở
int valueLDR = 0;

// Các biến lưu giá trị "nhiệt độ" và "độ ẩm" đọc từ cảm biến theo kiểu số thực
float temp, humi;

// Cho phép bật hoặc tắt Còi cảnh báo
bool warning = false;

// Lưu thời điểm thay đổi trạng thái Còi
unsigned long timeChange;

// Biến kiểu logic, cho biết trạng thái hiện tại của Còi
// True  = bật còi
// False = tắt còi
bool buzz = false;

// Lưu giá trị "độ rộng xung" cấp cho đèn Led
int power = 0;

// Cho phép bật hoặc tắt đèn Led
bool enableLed = false;

/*------------------------------------------------------------------------- */
/*                                  FUNCTION                                 */
/* -------------------------------------------------------------------------*/

// Đọc Biến trở
void readPot()
{
  // Đọc giá trị từ Biến trở
  valuePot = analogRead(POT_PIN);

  // Ánh xạ sang giá trị PWM điều khiển Led tương ứng
  power = map(valuePot, 0, 1023, 0, 255);
}

// Đọc cảm biến Quang trở
void readLDR()
{
  // Đọc giá trị từ Quang trở
  valueLDR = analogRead(LDR_PIN);

  // Trời càng tối, giá trị đọc từ Quang trở càng cao
  // Ánh xạ sang giá trị % tương ứng
  valueLDR = map(valueLDR, 0, 1023, 100, 0);

  // Cho phép bật/tắt đèn Led dựa theo giá trị Quang trở
  if (valueLDR < LEVEL_DARK)
  {
    // Trời tối (<20%), cho phép bật đèn Led
    enableLed = true;
  }
  else if (valueLDR > LEVEL_LIGHT)
  {
    // Trời sáng (>50%), tắt đèn Led
    enableLed = false;
    digitalWrite(LED_PIN, LOW);
  }
}

// Đọc cảm biến DHT11
void readDHT()
{
  // Lấy dữ liệu từ cảm biến DHT11
  temp = dht.readTemperature();
  humi = dht.readHumidity();

  // Cho phép bật/tắt Còi dựa theo giá trị DHT11
  if ((temp >= LEVEL_TEMP) || (humi >= LEVEL_HUMI))
  {
    warning = true; // Cho phép bật Còi
  }
  else
  {
    warning = false;
    digitalWrite(BUZZ_PIN, LOW); // Tắt còi
  }
}

// Hiển thị thông tin lên màn hình LCD
void displayLCD()
{
  // In giá trị "nhiệt độ" tại vị trí cột 1 dòng 1
  lcd.setCursor(0, 0);
  lcd.print(temp, 2);
  lcd.write(0xDF); // In kí tự (°) theo bảng mã ASCII
  lcd.print("C ");

  // In giá trị "độ ẩm" tại vị trí cột 9 dòng 1
  lcd.setCursor(8, 0);
  lcd.print(humi, 2);
  lcd.print("%  ");

  // Kiểm tra giá trị "nhiệt độ" có vượt ngưỡng ko?
  if (temp >= LEVEL_TEMP)
  {
    lcd.setCursor(7, 0);
    lcd.print("!");
  }
  else
  {
    lcd.setCursor(7, 0);
    lcd.print(" ");
  }
  // Kiểm tra giá trị "độ ẩm" có vượt ngưỡng ko?
  if (humi >= LEVEL_HUMI)
  {
    lcd.setCursor(15, 0);
    lcd.print("!");
  }
  else
  {
    lcd.setCursor(15, 0);
    lcd.print(" ");
  }

  // In giá trị "ánh sáng" tại vị trí cột 1 dòng 2
  lcd.setCursor(0, 1);
  lcd.print(valueLDR);
  lcd.print("%  ");

  // In giá trị "biến trở" tại vị trí cột 9 dòng 2
  lcd.setCursor(8, 1);
  lcd.print(valuePot);
  lcd.print("  ");
}

// Điều khiển Còi theo DHT11
void controlBuzz()
{
  // Còi được bật dựa theo trạng thái biến "warning"
  if (warning)
  {
    // Thay đổi trạng thái Còi mỗi 0,1s
    if (millis() - timeChange >= FREQ_BUZZ)
    {
      // Cập nhập thời điểm mới thay đổi
      timeChange = millis();

      // Đảo trạng thái Còi
      buzz = !buzz;
      digitalWrite(BUZZ_PIN, buzz);
    }
  }
}

// Điều khiển đèn Led theo Quang trở và Biến trở
void controlLed()
{
  // Led được bật dựa theo trạng thái biến "enableLed"
  if (enableLed)
  {
    // Điều khiển độ sáng Led theo giá trị Biến trở
    analogWrite(LED_PIN, power);
  }
}

/*------------------------------------------------------------------------- */
/*                                RUN ONE TIME                               */
/* -------------------------------------------------------------------------*/

void setup()
{
  // Khởi động màn hình LCD
  lcd.init();
  // Xóa toàn bộ nội dung trên màn hình LCD (nếu có)
  lcd.clear();
  // Bật đèn nền màn hình LCD
  lcd.backlight();

  // Khởi động cảm biến DHT11
  dht.begin();

  // Thiết lập chân D10 là Output để điều khiển Còi
  pinMode(BUZZ_PIN, OUTPUT);
  // Khi mới khởi động, tắt còi
  digitalWrite(BUZZ_PIN, LOW);

  // Thiết lập chân D11 là Output để điều khiển Led
  pinMode(LED_PIN, OUTPUT);
  // Khi mới khởi động, tắt Led
  digitalWrite(LED_PIN, LOW);
}

/*------------------------------------------------------------------------- */
/*                                    MAIN                                   */
/* -------------------------------------------------------------------------*/

void loop()
{
  // Đọc giá trị các thiết bị
  readPot();
  readLDR();
  readDHT();

  // Hiển thị thông tin lên màn hình LCD
  displayLCD();

  // Điều khiển còi theo DHT11
  controlBuzz();

  // Điều khiển đèn Led theo Quang trở và Biến trở
  controlLed();
}
```