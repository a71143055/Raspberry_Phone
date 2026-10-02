## 1. 라즈베리 폰 & 라즈베리 카드 최종 설계 사양

신용카드 물리 폼팩터(ISO/IEC 7810 ID-1)를 적용한 두뇌(라즈베리 카드)와 몸체(폰 셸)의 역할 분리형 모듈러 스마트폰 시스템 사양입니다.

| 구분 | 라즈베리 카드 (Raspberry Card) | 폰 셸 (Phone Shell) |
| --- | --- | --- |
| **역할** | 연산, 데이터 기억 (Brain)

 | 디스플레이, 배터리, 통신, 센서 (Body)

 |
| **외형 치수** | 85.60 × 53.98 × 3.0~5.0 mm

 | 162.0 × 76.0 × 14.5 mm

 |
| **폼팩터 규격** | ISO/IEC 7810 ID-1 (표준 신용카드)

 | 6.1인치급 슬림 바(Bar) 형 하우징

 |
| **주요 부품** | Raspberry Pi CM5, 8GB RAM, 128GB/256GB eMMC

 | 6.1인치 AMOLED, 4,500mAh 배터리, 5G/LTE 모뎀, GPS, 카메라

 |
| **인터페이스** | 전면 금도금 IC 칩 접점 패드 (IC Contact Array)

 | 포켓 내부 포고핀(Pogo-pin) 커넥터 및 Push-Push 래치

 |
| **방열 구조** | 카드 후면 알루미늄 열전도 패드

 | 폰 셸 알루미늄 통가공 하우징(방열판 역할)

 |

---

## 2. 프로젝트 폴더 및 파일 구조 (Project Workspace)

Autodesk Fusion(KANT Academy Hub) 및 개발 환경에 구축할 프로젝트 파일 디렉터리 구조입니다.

```text
Raspberry_Phone_Project/
├── 📂 01_CAD_Design (Autodesk Fusion 360)
│   ├── Raspberry_Phone_Main_Assembly.f3d       # 최상위 메인 조립품
│   ├── 📂 Components/
│   │   ├── CMP_Phone_Shell_Main_Body.f3d       # 폰 본체 알루미늄 하우징
│   │   ├── CMP_Raspberry_Card_Enclosure.f3d    # 신용카드형 카드 케이싱
│   │   ├── CMP_Display_Panel_6.1in.f3d         # 디스플레이 패널
│   │   ├── CMP_Battery_LiPo_4500mAh.f3d        # 배터리 모듈
│   │   └── CMP_PogoPin_Block_24P.f3d           # 수납부 포고핀 블록
│   └── 📂 Drawings/
│       ├── Drawing_A3_Concept_Layout.dwg       # A3 개념 배치 도면
│       └── Drawing_Card_Slot_Detail.dxf        # 카드 슬롯 가공용 DXF
├── 📂 02_PCB_Design (KiCad Hardware Schematics)
│   ├── 📂 Raspberry_Card_PCB/
│   │   ├── Card_TopLayer_IC_Pad.kicad_pcb      # 카드 전면 IC 접점 패드 (4-Layer)
│   │   └── CM5_Connector_Routing.kicad_sch     # CM5 모듈 연결 회로도
│   └── 📂 Phone_Shell_PCB/
│       ├── Shell_Carrier_Board.kicad_pcb       # 폰 메인 캐리어 보드
│       └── Power_BMS_Boost_5V3A.kicad_sch      # 전원 관리 및 승압 회로도
├── 📂 03_Enclosure_3D_Printing (STL/3MF)
│   ├── Card_Enclosure_Top_SLA.stl              # 레진 출력용 카드 상단 커버
│   ├── Card_Enclosure_Bottom_SLA.stl           # 레진 출력용 카드 하단 커버
│   └── Phone_Chassis_Mockup_FDM.stl            # 목업 검증용 FDM 출력 파일
└── 📂 04_Software_and_Firmware
    ├── 📂 Device_Tree_Overlays/
    │   └── raspberry-phone-display.dtbo        # DSI 디스플레이 오버레이
    └── 📂 Scripts/
        ├── card_detect_hotswap.py              # 카드 삽입/탈착 감지 스크립트
        └── power_management.sh                 # 전원 제어 및 안전 종료 스크립트

```

---

## 3. Autodesk Fusion 360 파라메트릭 치수 (User Parameters)

Fusion 360의 `Modify` > `Change Parameters`에 등록하여 3D 모델링 수치를 제어하는 변수 목록입니다.

| Parameter Name | Value | Unit | Description |
| --- | --- | --- | --- |
| `Shell_Width` | 76.0 | mm | 폰 셸 전체 가로 폭

 |
| `Shell_Height` | 162.0 | mm | 폰 셸 전체 세로 길이

 |
| `Shell_Thickness` | 14.5 | mm | 폰 셸 전체 두께

 |
| `Card_Width` | 53.98 | mm | 라즈베리 카드 가로 폭 (ISO/IEC 7810 ID-1)

 |
| `Card_Height` | 85.60 | mm | 라즈베리 카드 세로 길이 (ISO/IEC 7810 ID-1)

 |
| `Card_Thickness` | 3.0 ~ 5.0 | mm | 라즈베리 카드 두께

 |
| `Card_Corner_R` | 3.18 | mm | 신용카드 모서리 라운딩 R값

 |
| `Card_Clearance` | 0.20 | mm | 카드 수납 포켓 공차 (슬라이딩 조립용)

 |
| `IC_Pad_X_Offset` | 11.0 | mm | 전면 IC 접점 패드 X축 시작 위치 |
| `IC_Pad_Y_Offset` | 19.0 | mm | 전면 IC 접점 패드 Y축 시작 위치 |
| `Pogo_Pin_Pitch` | 1.50 | mm | 접점 핀 간격 피치

 |

---

## 4. 직접 제작(DIY) 실행 로드맵

```text
[1단계: 개발키트 검증] ──► [2단계: 커스텀 PCB 회로] ──► [3단계: 3D 프린팅 체결] ──► [4단계: OS & 전원 제어]
  - CM5 IO 보드 테스트     - 카드/셸 PCB 설계(KiCad)    - SLA 정밀 레진 출력       - 카드 감지 GPIO 스크립트
  - DSI 패널 & 터치 연동    - IC 접점 금도금(ENIG) 패드   - Push-Push 메커니즘       - DSI/터치 드라이버 설정
  - 배터리 승압 회로 확인   - Pogo-pin 수신기 배치       - 0.2mm 슬롯 공차 조율      - 안전 종료(Shutdown) 구성

```

1. **기능 검증 (1단계):** 라즈베리 파이 CM5 IO 보드에 6.1인치 MIPI-DSI 디스플레이, 터치 패널, Li-Po 배터리 승압 회로(5V 3A)를 연결하여 OS 동작을 먼저 확인합니다.
2. **회로 및 PCB 제작 (2단계):**
* **카드 PCB:** 4층(4-Layer) PCB 전면에 신용카드 IC 칩 규격의 금도금 패드(ENIG)를 노출시키고, 내부에 CM5 커넥터와 회로를 통합합니다.
* **폰 셸 PCB:** 포고핀 수신기, BMS 충방전 회로, LTE/GPS 모듈, 디스플레이 커넥터를 배치합니다.


3. **기구부 출력 및 결합 (3단계):**
* 슬롯 유격 공차(0.2mm) 정밀도를 확보하기 위해 **SLA 레진 3D 프린터**로 하우징을 출력합니다.


* 카드 슬롯 하단에 Push-Push 슬롯이나 스냅핏 래치를 장착하여 신용카드가 "딸깍" 소리와 함께 단단히 결합되도록 구성합니다.


4. **소프트웨어 구성 (4단계):** 카드가 탈착될 때 데이터 손상을 방지하기 위해 카드 감지 핀(Card Detect Pin) 기반의 GPIO 전원 차단 스크립트를 작성하고, OS 디스플레이 좌표축 및 DSI 오버레이를 설정합니다.
