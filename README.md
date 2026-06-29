- **ESP32 컨트롤 보드**: PC와 통신해 파라미터를 받고, 파워 보드를 제어·전원 분배
- **파워 보드**: 게이트 드라이버 + MOSFET로 실제 모터 전류를 구동

---

# 1. ESP32 컨트롤 보드

PC와 USB로 연결해 제어 파라미터를 주고받고, 로터·스테이터 파워 보드로
제어 신호와 전원을 분배하는 인터페이스 보드입니다.

### 주요 기능
- **PC 인터페이스**: USB로 제어 파라미터 입출력
- **전원 관리**: 12V 입력 → LM2596으로 5V 스텝다운하여 ESP32 공급
- **전원 분배**: 파워 보드로 12V / 3.3V 공급 (A89500 게이트 드라이버 · ACS37220 전류 센서용)
- **전원 충돌 방지**: USB와 외부전원 동시 인가 시 ESP32 손상 →
  몰렉스 스위치로 전원 소스를 택일하도록 설계

### 구성
- ESP32-S3 (DevKitC) / LM2596 (12V→5V) / 몰렉스 출력 커넥터 ×2 (로터·스테이터)

| 앞면 (3D) | 뒷면 (3D) |
|:---:|:---:|
| <img src="https://github.com/user-attachments/assets/37b12ed1-b373-474e-b649-16b384df0f0c" alt="ESP32 보드 앞면" width="100%"> | <img src="https://github.com/user-attachments/assets/a8a4e2f9-9cbf-405f-8852-c3e346ecf7b0" alt="ESP32 보드 뒷면" width="100%"> |

| 회로 설계 | PCB 레이아웃 |
|:---:|:---:|
| <img src="https://github.com/user-attachments/assets/4f62e896-a4e1-4341-ac00-725a61c4ef12" alt="ESP32 스키매틱" width="100%"> | <img src="https://github.com/user-attachments/assets/352daa73-483e-43b8-bc63-529c76454738" alt="ESP32 레이아웃" width="100%"> |

---

# 2. 파워 보드 (모터 컨트롤)

게이트 드라이버와 MOSFET로 실제 모터 전류를 구동하는 전력 보드입니다.
A상·B상 풀브리지로 로터·스테이터를 양방향 구동하며, 전류를 센싱해 피드백합니다.

### 구성
- **게이트 드라이버**: A89500 ×4
- **파워 스테이지**: IRFB4115 MOSFET ×8 (풀브리지 채널 구성)
- **전류 센싱**: ACS37220 ×2 (A상 / B상)
- **출력**: XT90 커넥터 (A+/A-/B+/B-), 대용량 벌크 커패시터
- **전원**: 12V 입력

### 설계 포인트
- **고전류 PCB 설계**: 넓은 동박 폴리곤으로 전류 경로·발열 관리
- **전력/신호 분리**: MOSFET 파워부와 게이트·센싱 신호부 영역 구분
- **전류 피드백**: ACS37220로 상별 전류 측정

| 앞면 (3D) | 뒷면 (3D) |
|:---:|:---:|
| <img src="https://github.com/user-attachments/assets/f3e95b27-10df-4552-bc1d-b1b86dfbbd45" alt="파워 보드 앞면" width="100%"> | <img src="https://github.com/user-attachments/assets/d177ceed-2008-4675-ad86-c3e3f5a9058b" alt="파워 보드 뒷면" width="100%"> |

| 회로 설계 | PCB 레이아웃 |
|:---:|:---:|
| <img src="https://github.com/user-attachments/assets/e6769d52-b6c6-4033-bdd6-8dcaac7e3828" alt="파워 보드 스키매틱" width="100%"> | <img src="https://github.com/user-attachments/assets/18330a4d-dd5b-415a-a9b6-c632d9d21aa8" alt="파워 보드 레이아웃" width="100%"> |

---

## 회고
- 제어(MCU)와 구동(파워)을 분리한 2-보드 모터 드라이버 시스템 설계
- 게이트 드라이버·MOSFET·전류 센싱을 통합한 전력전자 보드 설계 경험
- USB/외부전원 충돌 차단 등 실사용을 고려한 안전 설계 적용
