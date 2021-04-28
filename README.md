# Firmware_Programming

### 목차
- [`임베디드 회로`](https://github.com/KimUJin3359/Firmware_Programming#%EC%9E%84%EB%B2%A0%EB%94%94%EB%93%9C-%ED%9A%8C%EB%A1%9C)
- [`임베디드 시스템`](https://github.com/KimUJin3359/Firmware_Programming#%EC%9E%84%EB%B2%A0%EB%94%94%EB%93%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C)
- [`AP 구성`](https://github.com/KimUJin3359/Firmware_Programming#ap-%EA%B5%AC%EC%84%B1)
- [`라즈베리파이`](https://github.com/KimUJin3359/Firmware_Programming#%EB%9D%BC%EC%A6%88%EB%B2%A0%EB%A6%AC%ED%8C%8C%EC%9D%B4)
- [`STM`](https://github.com/KimUJin3359/Firmware_Programming#stm)
- [`LoRA`](https://github.com/KimUJin3359/Firmware_Programming#lora)

---

### 임베디드 회로
#### GPIO(General Purpose Input Output)
- **범용 입출력이 가능**한 핀
- 사용 규칙
  - 사용할 Pin에 대해 Input, Output 선택
  - 출력 설정 시 high, low를 택일하여 출력
  - 입력 설정 시 high, low를 입력 받음
  - **GPIO 입출력은 3.3V**

#### 저항
- MCU는 High, Low를 알 수 없는 모호한 상태
- 노이즈에 취약함(**플로팅 상태**)
- **풀업 저항**
  - 스위치를 닫았을 때 : Low
  - 스위치를 열었을 때 : High
  <img width="400" alt="pull_up" src="https://user-images.githubusercontent.com/50474972/116412491-b4061380-a871-11eb-814b-395aa90b19ef.png">   

- 풀다운 저항
  - 스위치를 닫았을 때 : High
  - 스위치를 열었을 때 : Low
  
  ![pull_down](https://user-images.githubusercontent.com/50474972/116412357-92a52780-a871-11eb-8d3a-ee2e1654c270.png)
  
- 라즈베리파이, 아두이노 등은 소스 코드를 통한 자체 풀업/풀다운 저항을 제공하지만 기본 임베디드 장비에서는 지원을 하지 않기 때문에 회로를 알아두는 것이 좋음
- MCU에서는 풀업을 더 많이 사용
- 왜 전압에 저항을 달아주는가?
  - **short(단락)** : 단락 회로란 **전류가 의도하지 않은 매우 낮은 임피던스를 갖는 회로의 경로를 흐르는 것**
    - 즉, VCC와 GND가 아무런 저항없이 연결된 것
  - 10V의 전압이 0.1Ω에 흐를 경우, I = 10/0.1 = 100A의 전류가 흐르게 됨
  - 불이 나거나, 터질 수 있음

#### DataSheet
- 모든 장치들은 DataSheet가 존재함 -> 장치에 맞는 설계를 해줘야됨
  - LED : 정격 전압, 정격 전류, 최대 허용 전압, 최대 소모 전류 확인

---

### 임베디드 시스템
- Embedded : 내장된

| `PC` | `Embedded` |
| --- | --- |
| 사용자가 어떠한 용도로 사용할지 모름 | 제작부터 그 **목적**이 정해져 있음 |
| **General Purpose** Computer | |
| 게임, 워드, 인터넷 등 | 전기밥통, 세탁기 등 |

- 분야
  - 하드웨어 : IP(MP, MC), 휘발성 메모리(RAM), 비휘발성 메모리(NRAM, 플래쉬 메모리, NOR, NAND), ASIC, PLD, FPGA, 임베디드 보드
  - 소프트웨어 : OS(Free OS, Linux), 개발 툴(IDE, Linker), 코드 분석툴(SCA, DCA), 디버깅 툴(ICE 등)

| `OS` | `Non-OS` |
| --- | --- |
| OS가 탑재된 임베디드 시스템 | Non-OS 임베디드 시스템 |
| Window-CE, Embedded Linux, FreeRTOS, VxWorks, Android 등 | **보통 Firmware** 프로그램(F/W) |
| | OS가 없어 **소프트웨어가 직접 하드웨어를 제어** |

- 두 시스템은 서로 장단점을 가짐

---

### AP 구성
- **AP** : **모바일 중앙 처리 장치 어플리케이션 프로세서(Application Processor)**
  - AP = CPU + Memory + Graphic IC + Storage
- CPU : 컴퓨터는 x86(CISC), AP는 ARM(RISC) 계열로 구성됨
- GPU : Vector 부동 소수점 연산은 CPU 보다 빠름
- 통신 : Wi-Fi 802.11a/b/g/n/ac
- VPU : 4K 재생 담당
- DSP : 오디오, 영상처리, 단순 반복 계산에 특화
- ISP : 카메라 처리 담당

  | ARM | Intel |
  | --- | --- |
  | 모바일 시장에서 강세 | PC 시장에서 강세 |
  | 저가 | 고가 |
  | 성능 비교적 낮음 | 성능 높음 |
  | 전력 소모가 적음 | 전력 소모가 많음 |
  | 발열량이 낮음 | 최대한의 성능을 내기위해 발열량이 많음 |
  
#### ARM(Advanced RISC Machine)
- **`Cortex A`** : **고성능, 핸드폰 탑재**
- `Cortex R` : Realtime OS용
- **`Cortex M`** : 저성능, 기존 MCU 시장을 장악
- A, R, M 순서로 성능이 좋음

#### 다양한 MCU 벤더
- ARM은 Archon사가 단독으로 설계하여 판매
- 다른 반도체 벤더가 이를 구매한 후  달리하여 판매

---

### 라즈베리파이
- 영국 라즈베리 파이 재단에서 제작한 초소형 초저가 컴퓨터
- [공식 홈페이지](https://www.raspberrypi.org/)
  - Imager 다운로드 : Software -> Download
- 다양한 Raspberry Pi 보드의 종류
  - 외부적인 요구조건 변동 및 CPU 성능에 따라 다양한 모델이 제작됨
  - 4 Model B CPU : **Broadcom BCM2711 SoC**
    - 가장 최신 제품
  - 3 Model B+ CPU : ARM Cortex A57 1.4GHz
- 4 Model B
  - CPU : Broadcom BCM2711 SoC
    - **Quad Core ARM Cortex-A72 1.5GHz**
  - GPU : Broadcom VideoCore VI 500MHz
  - 메모리 2/4/8GB 지원
  - Disk 없음(SD카드로 대체)
  - USB 4EA
  - HDMI 지원
  - BLE 5.0 지원
- 개발환경
  - VNC
    - Thonny IDE
    - 라즈베리파이용 Python IDE
  - SSH
    - mobaXterm 

#### 특징
- 초소형 컴퓨터
  - 강력한 CPU
- 임베디드 보드에서 개발을 일반 사용자들도 할 수 있게함
- 저렴함(\$50)
- 유저층이 넓어 자료를 얻기 쉬움
- 네이티브 개발 환경
  - 대규모 소스코드를 빌드하는데, PC에서 빌드, 개발하는 것이 훨씬 효율적
  - 소규모 소스코드를 개발 시, 네이티브 무방

#### ID/Password(Default)
- id : pi
- password : raspberry

---

### STM
#### STM 특징
- ARM사의 최신 **Cortex-M3**를 사용한 최신의 아키텍쳐
- 우수하고 뛰어난 주변 I/O들
- 저전력/저전압 성능
- 쉬운 개발
- 전 제품군에 걸쳐 제공되는 핀투핀, 주변 I/O와 소프트웨어 호환성이 우수
- 다양한 API/ 샘플 소스코드 제공

#### Device Part Numbering
- STM32/F/407/V/G/T/6/xxx
- Device Type
  - STM32 : ARM-based 32-bit microcontroller
- Product Type
  - F : general purpose
- Device Subfamily
  - 405 : STM32F40x, connectivity
  - 407 : STM32F40x, connectivity, camera VF, Ethernet
- Pin count
  - R : 64 pins
  - O : 90 pins
  - V : 100 pins
  - Z : 144 pins
  - I : 176 pins
- Flash memory size
  - E : 512Kb
  - G : 1024Kb
- Package
  - T : LQFP  
  - H : UFBGA
  - Y : WLCSP
- Temperature range
  - 6 : Industrial temperature range : -40 ~ 85C
  - 7 : Industrial temperature range : -40 ~ 105C
- Options
  - xxx : programmed parts
  - TR : tape and reel

#### 사용 보드
- STM Development Board
  - 보드명은 제조사를 따름 : STM Board
  - 보드의 세 종류
    - Nucleo : 교육 및 초기 개발용(아주 저렴, 스택으로 필요한 모듈을 쌓아서 사용 가능)
    - Discovery : 교육 및 초기 개발용
    - Eval Board : 모든 센스, 기능을 테스트 할 수 있도록 개발된 보드
  - 디바이스가 매우 많음
    - 429 디바이스 : STM32F429 Discovery Board
    - 103 디바이스 : STM32F103 Discovery Board
- 사용 모델
  - ST 개발용 보드 Nucleo
    - 보드 모델명 : Nucleo - F103RB
    - ST사의 CPU 칩셋 이름 : STM32F103RB
    - CPU : ARM Cortex-M3
  - ARm Cortex-M 계열에서 가장 많이 사용하는 Cortex-M3

#### 회로에 대한 최소한의 이해
- ** `전원 유무`** : 보드에 전원이 공급되는지 유무, 된다면 전압측정 까지
- **`클럭 발진 유무 및 정확한 주기`** : 클럭 발진이 안되면 보드가 동작하지 않음
- **`GPIO 연결 포트`**
- **`점퍼 설정`** 등

#### 개발 환경
- [다운로드 사이트](https://www.st.com/stm32cube)
- STM32CubeMX
  - STM32 마이크로 컨트롤러 및 마이크로 프로세서를 매우 쉽게 구성할 수 있는 그래픽 도구
  - 단계별 프로세스를 통해 Arm Cortex-M(또는 ARM Cortex-A 코어 용 부분 Linux 장치 트리)에 대한 해당 초기화 C 코드 생성도 포함
- STM32CubeIDE
- 디버거
  - Trace : CPU의 동작을 멈추면서, 한 단계씩 진행하는 디버깅 방식
  - 임베디드 환경에서 CPU를 일시 중지시키는 일은 어려움
  - 따라서, 디버거용 장비가 필요
    - ST-Link
  - Nucleo 보드에는 ST-Link/V2가 포함되어 음

---

### LoRA
- 바깥(20m 이상)에 있는 노드들을 집에 있는 서버와의 연결 방법
  - weather station 등의 활용
- 원거리(1.5 ~ 2km)의 데이터를 주고받기 위해 만들어짐
- WiFi : 대역폭과 속도가 Bluetooth에 비해 성능이 좋음
- Bluetooth : 근거리 통신(~100m 장애물의 영향을 많이 받음)
  - WiFi와 동일하게 2.4GHz 대역을 사용
  - WiFi에 비해 전력소모/비용이 저렴함(소량의 데이터를 보낼 때 더 유용)
- Celluar : 비용적인 문제

---
