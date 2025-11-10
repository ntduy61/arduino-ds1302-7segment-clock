# Arduino ds1302 7segment clock
Real-time digital clock using Arduino, DS1302 RTC module, and 4-digit 7-segment display. It alternates between showing hours:minutes and minutes:seconds every few seconds.

This Arduino project demonstrates how to build a real-time digital clock using the DS1302 RTC module and a 4-digit 7-segment display.
The program reads the current time from the DS1302 chip and displays it on the 7-segment display. Every 3 seconds, it automatically switches between showing HH:MM and MM:SS formats.

**Features:**
  - Uses DS1302 RTC for accurate timekeeping.
  - Displays time on a 4-digit common cathode 7-segment LED.
  - Alternates display between hours:minutes and minutes:seconds.
  - Simple and compact multiplexing code for smooth brightness.

**🔧 Hardware Required / Phần cứng cần có:**
  - Arduino Uno (hoặc tương thích)
  - DS1302 RTC module
  - 4-digit 7-segment LED (common cathode)
  - Jumper wires & breadboard

**⚙️ Connections / Sơ đồ kết nối:**

**Component -> Arduino Pin**
  - DS1302 RST -> A3
  - DS1302 DAT -> A2
  - DS1302 CLK -> A1
  - Segment a–g, dp -> 2–9
  - Digit D1–D4 ->  10–13
![hq720](https://github.com/user-attachments/assets/ade2a9e3-efc7-416d-a4cf-a74eeb73a73b)

**⚙️Manual Installation (if Library Manager doesn’t find it)**

- **Download the DS1302 library from GitHub:**
- 👉 https://github.com/msparks/arduino-ds1302

- **Then in Arduino IDE:**

- Go to Sketch → Include Library → Add .ZIP Library…

- Select the downloaded .zip file.

- Wait for it to install — you’ll see “Library added to your libraries”.

- **✅ Now you can include it in your code with:**
- #include <DS1302.h>


📸 Optional preview:
![IMG_20251108_162823_121](https://github.com/user-attachments/assets/530b191e-ff03-4981-8517-884fdf73d27a)
