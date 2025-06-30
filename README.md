# Project-IT4210
Robot system moves and interacts based on the operator's hand gestures

# Robot Điều Khiển Bằng Cử Chỉ Tay 🤖✋

## 📝 Giới thiệu

Dự án này là một mô hình robot tương tác thông minh, có khả năng:
- Nhận dạng cử chỉ tay của người dùng thông qua camera.
- Phân loại cử chỉ bằng mô hình AI (CNN).
- Thực hiện các hành động tương ứng như: tiến, lùi, ngồi, nằm, dừng,...
- Hiển thị biểu cảm khuôn mặt qua màn hình OLED.
- Phát nhạc nền hoặc âm thanh phản hồi bằng buzzer.

Hệ thống được chia làm hai phần:
- **Server xử lý AI** (chạy trên laptop)
- **Vi điều khiển ESP32** điều khiển robot (Arduino IDE)

---

## 🧰 Phần cứng sử dụng

| Thành phần              | Vai trò chính                                          |
|-------------------------|---------------------------------------------------------|
| ESP32 DevKit V1         | Vi điều khiển trung tâm, điều khiển robot              |
| ESP32-CAM               | Thu nhận hình ảnh tay người dùng                       |
| Servo motor (SG90)      | Di chuyển tay/chân/tư thế robot                        |
| Màn hình OLED SH1106    | Hiển thị biểu cảm mặt robot                            |
| Buzzer (PWM hoặc UART)  | Phát âm thanh/phản hồi/nhạc nền                        |
| Nguồn 5V từ Laptop      | Cấp nguồn cho toàn bộ hệ thống                         |
| Breadboard, dây nối,... | Kết nối linh kiện                                      |

---

## 🛠️ Công cụ & môi trường phát triển

- **Arduino IDE 1.8.19** (https://www.arduino.cc/en/software)
- **Python 3.9+** (cho xử lý ảnh và AI)
- **Firebase Realtime Database + Storage**
- **Trình duyệt / Firebase Console** để cấu hình Firebase

---

## 🔧 Hướng dẫn cài đặt & lập trình

### A. Cài đặt Arduino IDE cho ESP32

1. Mở Arduino IDE → Preferences → Thêm URL board:
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
2. Vào **Tools > Board > Board Manager** → Tìm **esp32 by Espressif Systems** → **Install**.
3. Chọn board: **ESP32 Dev Module**.

### B. Cài đặt thư viện Arduino cần thiết

Vào **Sketch > Include Library > Manage Libraries**, cài các thư viện sau:

- `Adafruit GFX`
- `Adafruit SH1106 (hoặc U8g2 nếu bạn dùng U8g2)`
- `ESP32Servo`
- `WiFi.h`
- `Firebase_ESP_Client` (nếu dùng Firebase)
- `DFPlayer_Mini_Mp3` *(nếu dùng module âm thanh)*

> **Lưu ý:** Kiểm tra cổng COM và chọn đúng trong **Tools > Port**.

---

## 📂 Cấu trúc thư mục project

GestureRobot/
├── ArduinoCode/
│ ├── main.ino # ESP32 điều khiển servo, OLED, buzzer
│ └── config.h # Thông tin WiFi + Firebase
├── CNN_Model/
│ ├── model.h5 # Mô hình AI nhận diện cử chỉ
│ ├── predict.py # Script Python đọc ảnh từ Firebase và phân loại
│ └── requirements.txt # Thư viện Python cần cài
├── README.md

---

## ⚙️ Chạy hệ thống

1. **ESP32-CAM**:
   - Flash code chụp ảnh và gửi lên Firebase Storage.

2. **Laptop (Python):**
   - Chạy `predict.py`: sử dụng OpenCV và mô hình CNN để nhận diện cử chỉ tay từ ảnh Firebase.
   - Cập nhật lệnh điều khiển (ví dụ: `"FORWARD"`, `"STOP"`) lên Firebase Realtime DB.

3. **ESP32 DevKit (Robot):**
   - Đọc lệnh từ Firebase.
   - Thực hiện hành động tương ứng, hiển thị biểu cảm, phát âm thanh.

---

## 📦 Python: Cài đặt môi trường AI

```bash
cd CNN_Model
pip install -r requirements.txt

## requirements.txt:
tensorflow
opencv-python
firebase-admin
numpy

## 📷 Demo
