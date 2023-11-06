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
    analogWrite(pin, value);
  }
}

void servoConfigCallback(byte pin, int minPulse, int maxPulse) {
  if (pin == 9 || pin == 10 || pin == 11) {
    Servo.attach(pin);
  }
}

void servoWriteCallback(byte pin, int value) {
  if (pin == 9 || pin == 10 || pin == 11) {
    Servo.write(pin, value);
  }
}
