import pyfirmata
import threading
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

# 각 서보 모터를 제어할 스레드 함수
def servo_thread(servo, angle):
    while True:
        set_servo_angle(servo, angle)
        time.sleep(2)
        set_servo_angle(servo, angle)  # 초기 위치로 되돌림
        time.sleep(2)

# 서보 모터를 동시에 제어하기 위한 스레드 생성
thread1 = threading.Thread(target=servo_thread, args=(servo1, 100))
thread2 = threading.Thread(target=servo_thread, args=(servo2, 50))
thread3 = threading.Thread(target=servo_thread, args=(servo3, 110))

# 스레드 시작
thread1.start()
thread2.start()
thread3.start()

try:
    # 무한 루프로 스레드 실행 유지
    while True:
        pass

except KeyboardInterrupt:
    # Ctrl+C를 눌러 프로그램을 종료
    board.exit()
