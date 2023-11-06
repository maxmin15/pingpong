import pyfirmata
import time

# 아두이노와 시리얼 통신 설정 (COM 포트 및 속도에 따라 변경)
port = '/dev/ttyACM0'  # 아두이노 포트
board = pyfirmata.Arduino(port)

# Servo 객체 초기화
servo1 = board.get_pin('d:9:s')  # 9번 핀을 서보 모터 1로 설정
servo2 = board.get_pin('d:10:s')  # 10번 핀을 서보 모터 2로 설정
servo3 = board.get_pin('d:11:s')  # 11번 핀을 서보 모터 3로 설정

# 서보 모터 제어 함수
def set_servo_angle(servo, angle):
    servo.write(angle)

try:
    while True:
        set_servo_angle(servo1, 100)
        time.sleep(1)
        set_servo_angle(servo2, 50)
        time.sleep(1)
        set_servo_angle(servo3, 100)
        time.sleep(1)
except KeyboardInterrupt:
    board.exit()
