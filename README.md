# Firmware_Programming

### AP 구성
- AP : 모바일 중앙 처리 장치 어플리케이션 프로세서(Application Processor)
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
  | 성능 낮음 | 성능 높음 |
  | 전력 소모가 적음 | 전력 소모가 많음 |
  | 발열량이 낮음 | 최대한의 성능을 내기위해 발열량이 많음 |
  
#### ARM(Advanced RISC Machine)
- Cortex A : AP용
- Cortex R : Realtime OS용
- Cortex M : Mobile용
- A, R, M 순서로 

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

### 임베디드 회로
#### GPIO(General Purpose Input Output)
- 범용 입출력이 가능한 핀
- 사용 규칙
  - 사용할 Pin에 대해 Input, Output 선택
  - 출력 설정 시 high, low를 택일하여 출력
  - 입력 설정 시 high, low를 입력 받음
  - GPIO 입출력은 3.3V

#### 저항
- MCU는 High, Low를 알 수 없는 모호한 상태
- 노이즈에 취약함(플로팅 상태)
- 풀업 저항
  - 스위치를 닫았을 때 : Low
  - 스위치를 열었을 때 : High
  <img width="400" alt="pull_up" src="https://user-images.githubusercontent.com/50474972/116412491-b4061380-a871-11eb-814b-395aa90b19ef.png">   

- 풀다운 저항
  - 스위치를 닫았을 때 : High
  - 스위치를 열었을 때 : Low
  
  ![pull_down](https://user-images.githubusercontent.com/50474972/116412357-92a52780-a871-11eb-8d3a-ee2e1654c270.png)
  
- 라즈베리파이, 아두이노 등은 소스 코드를 통한 자체 풀업/풀다운 저항을 제공하지만 기본 임베디드 장비에서는 지원을 하지 않기 때문에 회로를 알아두는 것이좋음
- MCU에서는 풀업을 더 많이 사용
- 왜 전압에 저항을 달아주는가?
  - short(단락) : 단락 회로란 전류가 의도하지 않은 매우 낮은 임피던스를 갖는 회로의 경로를 흐르는 것
    - 즉, VCC와 GND가 아무런 저항없이 연결된 것
  - 10V의 전압이 0.1Ω에 흐를 경우, I = 10/0.1 = 100A의 전류가 흐르게 됨
  - 불이 나거나, 터질 수 있음

#### DataSheet
- 모든 장치들은 DataSheet가 존재함 -> 장치에 맞는 설계를 해줘야됨
  - LED : 정격 전압, 정격 전류, 최대 허용 전압, 최대 소모 전류 확인

---

### 라즈베리파이
- 영국 라즈베리 파이 재단에서 제작한 초소형 초저가 컴퓨터
- [공식 홈페이지](https://www.raspberrypi.org/)
  - Imager 다운로드 : Software -> Download
- 다양한 Raspberry Pi 보드의 종류
  - 외부적인 요구조건 변동 및 CPU 성능에 따라 다양한 모델이 제작됨
  - 4 Model B CPU : Broadcom BCM2711 SoC
    - 가장 최신 제품
  - 3 Model B+ CPU : ARM Cortex A57 1.4GHz
- 4 Model B
  - CPU : Broadcom BCM2711 SoC
    - Quad Core ARM Cortex-A72 1.5GHz
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
