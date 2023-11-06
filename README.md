#include <Firmata.h>

void setup() {
  Firmata.begin(57600);
  Firmata.attach(ANALOG_MESSAGE, analogWriteCallback);
  Firmata.attach(SERVO_CONFIG, servoConfigCallback);
  Firmata.attach(SERVO_WRITE, servoWriteCallback);
}

void loop() {
  while (Firmata.available()) {
    Firmata.processInput();
  }
}

void analogWriteCallback(byte pin, int value) {
  if (pin == 9 || pin == 10 || pin == 11) {
    // 서보 모터 핀이 아닌 경우에는 analogWrite를 호출하지 않도록 수정
    analogWrite(pin, value);
  }
}

void servoConfigCallback(byte pin, int minPulse, int maxPulse) {
  if (pin == 9 || pin == 10 || pin == 11) {
    Servo.attach(pin, minPulse, maxPulse);  // Servo.attach에 minPulse와 maxPulse 추가
  }
}

void servoWriteCallback(byte pin, int value) {
  if (pin == 9 || pin == 10 || pin == 11) {
    Servo.write(pin, value);
  }
}
주요 수정 사항:

analog
