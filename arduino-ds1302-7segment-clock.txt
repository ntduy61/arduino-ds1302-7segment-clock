#include <DS1302.h>

//================= DS1302 =================//
// Define pins for DS1302 RTC module
// Định nghĩa chân kết nối với module RTC DS1302
#define CE_PIN   A3   // RST
#define IO_PIN   A2   // DAT
#define SCLK_PIN A1   // CLK
DS1302 rtc(CE_PIN, IO_PIN, SCLK_PIN);


// ==================== PIN CONFIGURATION ====================
// Pins connected to 7-segment display
// Các chân nối với LED 7 đoạn
const int segmentPins[8] = {2,3,4,5,6,7,8,9}; // a,b,c,d,e,f,g,dp
const int digitPins[4]   = {10,11,12,13};     // D1,D2,D3,D4

// ==================== 7-SEGMENT CODE TABLE ====================
// Common cathode
// segCode[digit][segment] — 1 = ON, 0 = OFF
// Thứ tự: a,b,c,d,e,f,g,dp
const byte segCode[10][8] = {
  {1,1,1,1,1,1,0,0}, // 0
  {0,1,1,0,0,0,0,0}, // 1
  {1,1,0,1,1,0,1,0}, // 2
  {1,1,1,1,0,0,1,0}, // 3
  {0,1,1,0,0,1,1,0}, // 4
  {1,0,1,1,0,1,1,0}, // 5
  {1,0,1,1,1,1,1,0}, // 6
  {1,1,1,0,0,0,0,0}, // 7
  {1,1,1,1,1,1,1,0}, // 8
  {1,1,1,1,0,1,1,0}  // 9
};

bool showHourMinute = true;       // Flag to switch between HH:MM and MM:SS
// Cờ để chuyển hiển thị giữa HH:MM và MM:SS
unsigned long lastSwitch = 0;     // Last time we switched display
// Thời điểm lần cuối chuyển hiển thị

void setup() {
  // Configure segment and digit pins as OUTPUT
  // Cấu hình các chân 7 đoạn và digit là OUTPUT
  for (int i = 0; i < 8; i++) pinMode(segmentPins[i], OUTPUT);
  for (int i = 0; i < 4; i++) pinMode(digitPins[i], OUTPUT);
  
  rtc.writeProtect(false); // Disable write protection
  // Tắt bảo vệ ghi dữ liệu
  rtc.halt(false);         // Start RTC
  // Bật RTC (không dừng)
}

void loop() {
  Time t = rtc.time(); // Get current time from RTC
  // Lấy giờ hiện tại từ module RTC
  
  int h = t.hr;
  int m = t.min;
  int s = t.sec;

  int num[4]; // Array to hold 4 digits to display
  // Mảng lưu 4 số sẽ hiển thị

  if (showHourMinute) {
    // Display HH:MM
    // Hiển thị giờ:phút
    num[0] = h / 10;
    num[1] = h % 10;
    num[2] = m / 10;
    num[3] = m % 10;
  } else {
    // Display MM:SS
    // Hiển thị phút:giây
    num[0] = m / 10;
    num[1] = m % 10;
    num[2] = s / 10;
    num[3] = s % 10;
  }

  // Scan 4 digits multiple times for stable display
  // Quét 4 digit nhiều lần để hiển thị sáng đều
  for (int i = 0; i < 50; i++) {  // scan 50 times
    for (int d = 0; d < 4; d++) {
      showDigit(d, num[d], (d == 1)); // Light dot on 2nd digit (colon effect)
      // Bật chấm thập phân cho digit thứ 2 (hiệu ứng dấu :)
      delay(2); // ~2ms per digit
      // Thời gian sáng mỗi digit ~2ms
    }
  }

  // Switch between HH:MM and MM:SS every 3 seconds
  // Chuyển hiển thị giữa HH:MM và MM:SS mỗi 3 giây
  if (millis() - lastSwitch > 3000) {
    showHourMinute = !showHourMinute;
    lastSwitch = millis();
  }
}

// Function to display a single digit on 7-segment
// Hàm hiển thị 1 digit lên LED 7 đoạn
void showDigit(int digit, int num, bool dot) {
  // Turn off all digits first
  // Tắt tất cả các digit trước
  for (int i = 0; i < 4; i++) digitalWrite(digitPins[i], HIGH);

  // Write segment values for the number
  // Ghi giá trị các đoạn của số num
  for (int i = 0; i < 8; i++) {
    int state = segCode[num][i];
    if (i == 7 && dot) state = 1; // Turn on dot if needed
    // Bật chấm thập phân nếu cần
    digitalWrite(segmentPins[i], state);
  }

  // Turn on the selected digit
  // Bật digit tương ứng
  digitalWrite(digitPins[digit], LOW);
}
