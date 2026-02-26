# Smart-Weather-Station

# 1. Sơ đồ đấu nối (Wiring)


* **Cảm biến DHT11 (Nhiệt độ & Độ ẩm):**
* Chân VCC -> 5V Arduino.
* Chân GND -> GND Arduino.
* Chân Data -> Pin 2 Arduino.


* **Màn hình LCD 1602 (có module I2C):**
* Chân VCC -> 5V Arduino.
* Chân GND -> GND Arduino.
* Chân SDA -> Pin A4 Arduino.
* Chân SCL -> Pin A5 Arduino.



---

# 2. Cài đặt Thư viện (Library)



1. **DHT sensor library** (của Adafruit).
2. **LiquidCrystal I2C** (của Frank de Brabander).

---

# 3.Code

```cpp
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
#include "DHT.h"

#define DHTPIN 2     // Chân data của DHT11 nối vào chân số 2
#define DHTTYPE DHT11   

DHT dht(DHTPIN, DHTTYPE);
LiquidCrystal_I2C lcd(0x27, 16, 2); // Địa chỉ I2C thường là 0x27 hoặc 0x3F

void setup() {
  Serial.begin(9600);
  dht.begin();
  lcd.init();      // Khởi tạo LCD
  lcd.backlight(); // Bật đèn nền
  lcd.setCursor(0, 0);
  lcd.print("Dang khoi dong...");
  delay(2000);
}

void loop() {
  // Đọc độ ẩm và nhiệt độ
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  // Kiểm tra xem cảm biến có hoạt động không
  if (isnan(h) || isnan(t)) {
    lcd.clear();
    lcd.print("Loi cam bien!");
    return;
  }

  // Hiển thị lên Serial Monitor (để debug)
  Serial.print("Do am: "); Serial.print(h);
  Serial.print("%  Nhiet do: "); Serial.println(t);

  // Hiển thị lên màn hình LCD
  lcd.clear();
  lcd.setCursor(0, 0); // Dòng 1
  lcd.print("Temp: ");
  lcd.print(t);
  lcd.print(" C");

  lcd.setCursor(0, 1); // Dòng 2
  lcd.print("Humi: ");
  lcd.print(h);
  lcd.print(" %");

  delay(2000); // Đợi 2 giây rồi cập nhật tiếp
}



---

