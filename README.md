# Firmware_Programming

### 목차
- [`임베디드 회로`](https://github.com/KimUJin3359/Firmware_Programming#%EC%9E%84%EB%B2%A0%EB%94%94%EB%93%9C-%ED%9A%8C%EB%A1%9C)
- [`용어정리`](https://github.com/KimUJin3359/Firmware_Programming#%EC%9A%A9%EC%96%B4%EC%A0%95%EB%A6%AC)
- [`임베디드 시스템`](https://github.com/KimUJin3359/Firmware_Programming#%EC%9E%84%EB%B2%A0%EB%94%94%EB%93%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C)
- [`AP 구성`](https://github.com/KimUJin3359/Firmware_Programming#ap-%EA%B5%AC%EC%84%B1)
- [`라즈베리파이`](https://github.com/KimUJin3359/Firmware_Programming#%EB%9D%BC%EC%A6%88%EB%B2%A0%EB%A6%AC%ED%8C%8C%EC%9D%B4)
- [`STM`](https://github.com/KimUJin3359/Firmware_Programming#stm)
- [`레지스터를 통한 입출력`](https://github.com/KimUJin3359/Firmware_Programming#%EB%A0%88%EC%A7%80%EC%8A%A4%ED%84%B0%EB%A5%BC-%ED%86%B5%ED%95%9C-%EC%9E%85%EC%B6%9C%EB%A0%A5)
- [`Address를 통한 입출력`](https://github.com/KimUJin3359/Firmware_Programming#address%EB%A5%BC-%ED%86%B5%ED%95%9C-%EC%9E%85%EC%B6%9C%EB%A0%A5)
- [`LoRA`](https://github.com/KimUJin3359/Firmware_Programming#lora)

---

### 임베디드 회로
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
- 전자 부품의 특성을 적어 놓은 문서, 제종사에서 배포함
  - 제조사 이름
  - 제품의 모델 번호, 이름, 주문번호
  - 제품의 특성 및 기능 설명
  - 각종 전기적 특성 : 공급 전압, 소모 전류, 최소, 최대값 등
  - 동작환경(온도, 습도 등)
  - 기타
- 모든 장치들은 DataSheet가 존재함 -> 장치에 맞는 설계를 해줘야됨
- 전체 블럭다이어그램
  - 디바이스의 특징, 전원, 클럭, 전체 블럭다이어그램, 메모리 부분 숙지
  - 후에 Peripheral(UART, DMA, I2C 등)을 보는 것이 좋음

#### Reference Manual
- 레지스터 관련 정보 확인
- DataSheet에 비해 더욱더 상세한 정보가 들어가 있음

#### 오실로스코프
- 특정 시간 간격의 전압 변화를 볼 수 있는 장치

#### 용어정리
- `DMA(Direct Memory Access)` : CPU 속도를 올리는(성능 향상) 기술
  - 데이터 보내는 작업을 일일이 관여하는 것이 아닌, 다른 작업을 할 수 있음
  - DMA Controller : 데이터 보내는 작업을 대신 관리하는 장치
- `GPIO(General Purpose Input Output)`
  - **범용 입출력이 가능**한 핀
  - 사용할 Pin에 대해 Input, Output 선택
  - 출력 설정 시 high, low를 택일하여 출력
  - 입력 설정 시 high, low를 입력 받음
    - **GPIO 입출력은 3.3V**
- `IWDG` : Watch Dog Timer
  - 내부적으로 타이머를 돌려, **프로그램이 정상 동작**하는지를 판단
- `NVIC` : Nested Vectored Interrupt Controller
  - **중첩된 인터럽트를 제어**하는 기능
  - 모든 exception에 대해서 우선순위가 설정되어있고 이 우선순위에 따라 interrupt를 처리
  - **숫자가 낮을수록, 우선순위가 높음**
  - 인터럽트 모드
    - External Interrupt Mode with Rising edge trigger detection
    - External Interrupt Mode with Falling edge trigger detection
    - External Interrupt Mode with Rising/Fallig edge trigger detection
    - External Event Mode with Rising edge trigger detection
    - External Event Mode with Falling edge trigger detection
    - External Event Mode with Rising/Fallig edge trigger detection
  - Event 모드 설정 시 sleep mode에 진입한 MCU를 꺠우는 용도로 사용됨
  - 일반적으로 Falling edge trigger detection 사용
  - 콜백 함수
    - OOOit.c : 인터럽트 관련 소스 코드
    ```
    in OOOit.c
    __weak void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
    
    in main.c
    void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
    {
      .. 동작 내용
    }
    ```
- `RCC` : 클럭
- `ADC` : 아날로그를 디지털로 변환
- `RTC` : Real Time Clock
- Connectivity
  - `CAN` : 차량에 쓰이는 통신
  - `I2C`
  - `SPI`
  - `USART`
  - `USB`
- `CRC` : checksum
- `MBED` : 컴파일러를 설치하지 않고 웹에서 개발할 수 있는 개발환경

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
- **`전원 유무`** : 보드에 전원이 공급되는지 유무, 된다면 전압측정 까지
- **`클럭 발진 유무 및 정확한 주기`** : 클럭 발진이 안되면 보드가 동작하지 않음
- **`GPIO 연결 포트`**
- **`점퍼 설정`**

#### 개발 환경
- [다운로드 사이트](https://www.st.com/stm32cube)
- STM32CubeMX
  - STM32 마이크로 컨트롤러 및 마이크로 프로세서를 매우 쉽게 구성할 수 있는 그래픽 도구
  - 단계별 프로세스를 통해 Arm Cortex-M(또는 ARM Cortex-A 코어 용 부분 Linux 장치 트리)에 대한 해당 초기화 C 코드 생성도 포함
- STM32CubeIDE
  - 주의 : 사용자 코드는 주석 안에 입력할 것
    - 코드가 사라질 수 있음
  ```
  /* USER CODE BEGIN x */
  ...
  /* USER CODE END x */
  ```
  - 함수 원형 찾기
    - ctrl + space
    - 함수 위에서 F3
    - 함수 우클릭 후 Open Declaration
- 디버거
  - Trace : CPU의 동작을 멈추면서, 한 단계씩 진행하는 디버깅 방식
  - 임베디드 환경에서 CPU를 일시 중지시키는 일은 어려움
  - 따라서, **디버거용 장비가 필요**
    - ST-Link
  - Nucleo 보드에는 ST-Link/V2가 포함되어 음

---

### 레지스터를 통한 입출력
#### Memory Mapped I/O
- 주기억 장치의 일부주소를 입출력 장치에 할당하는 방법
- 메모리의 x번지에 값을 쓰면 출력포트로 값이 나감
- x번지에 값을 읽으면, 입력 포트의 값을 읽을 수 도 있음

#### 레지스터 설정을 하는 이유
- 개발 관련된 API가 존재하지 않는 경우도 많음
- 레지스터로 설정하는 것이 일반적
  - 레지스터로 설정하는 방법은 모든 MCU

#### 해당 비트 세트/클리어
- CRL : 0 ~ 7번 핀, CRH : 8 ~ 15번 
  ![datasheet](https://user-images.githubusercontent.com/50474972/116590484-c73fde80-a958-11eb-856c-3abdfc2c9be4.png)
  ```
  // PIN A5를 Output Mode, 50MHz로 설정하려면
  GPIO_CRL = 0x003;
  ```
- `IDR` : port input data register
  - 포트가 입력일 경우 들어오는 데이터가 저장되는 레지스터
- `ODR` : port output data register
  - 포트가 출력일 경우 나갈 데이터를 저장
- `BSRR` : port bit set/reset register
  - 해당 비트를 0, 1로 설정할 수 있음
  - BR = RESET, BS = SET 담당
  - ODR을 쓰는것보다 연산 처리가 간단하고, 속도가 빠름
- `BRR` : port bit reset register
  - 오로지 리셋만함
- `LCKR` : port configuration lock register
  - GPIO는 input, output 택 일 사용이 가능, 사용 중 변경도 가능
  - 락을 걸면 input -> output, output -> input 변경이 불가
- 원하는 비트만 세트
  ```
  // GPIOA의 PA5 set
  GPIOA->ODR = 1 << 5;
  // GPIOA의 PA5 reset
  GPIOA->ODR &= ~(1 << 5);
  ```
- 원하는 비트만 클리어

---

### Address를 통한 입출력
- **Memory Mapped I/O**
- 데이터시트를 확인하면 각 레지스터의 주소를 알 수 있음
- 메모리 주소를 컴파일러에게 알려 주어야 함

#### Peripherals 확인
- Datasheet의 Memory Mapping을 확인
- STM의 경우 0x4000 0000

#### MX_GPIO_Init을 확인
- ctrl + click 시 해당 함수로 이동
- 그 중 GPIOA를 보면, (0x4000 0000 + 0x0001 0000) + 0x0000 0800
  - Peripherals Base + APB2 Base + GPIOA Base

#### Offset 확인
- Reference Manual의 data register 확인
- STM의 경우 9.2.4 Port output data register에 GPIOA는 0x0C라고 나와있음

#### ODR레지스터의 주소
- GPIOA + offset = 0x4001 080C임을 알 수 있음

#### 값 변경
```
  // volatile : compiler 최적화 방지
  // unsigned : 주소값이기 때문에
  *((volatile unsigned int *)0x4001080C) = 0xF0;
```

---

### Timer
- 주기적으로 시간을 얻을 때 사용하는 디지털 카운터
- STM32F103RB에는 4개의 Timer가 존재
  ```
  APB2 : TIM1,
  APB1 : TIM2 ~ 4
  ```
- 관련 개념
  - System Clock
  - 주파수와 주기의 관계
  - 분주비(Prescale Ratio Value)
  - 카운트 값(Count Value)

#### 타이머 계산
- 타이머에 입력되는 주파수를 알아야 함

![캡처](https://user-images.githubusercontent.com/50474972/116816016-415fa580-ab9b-11eb-8463-33b08aa98f97.JPG)
![클럭설정](https://user-images.githubusercontent.com/50474972/116816809-75889580-ab9e-11eb-977f-4a8604e42ebb.JPG)

- 위 블록도에서 TIM2는 APB1에 연결되어 있고, APB1의 System Clock은 64MHz
```
Prescaler : 64 - 1
Counter Period : 1000 - 1
System Clock : 64MHz로 설정 시
UpdateEvent = Timer_clock / (Prescaler + 1)(Period + 1)
UpdateEvent = 64000000 / (64 * 1000) = 1000Hz = 0.001sec
```

#### 타이머 모드
- 카운터 모드 : 카운터 값이 증가/감소되면서 0이 될 때 인터럽트 발생
- 외부 입력 카운터 모드 : 외부 입력이 발생할 때 인터럽트 발생
- 펄스폭변조 출력 모드 : PWM 파형 생성 모드
- 입력 캡쳐 모드 : 타이머가 생성하는 PWM 신호를 다른 타이머에서 캡쳐하여 Freq, Duty 비 확인 가능
- 출력 비교 모드 : TCNT 값이 CCRx값과 일치할 때 인터럽트가 발생

---

### USART
- Universal Synchronous Asynchronous Receive Transmit
- **범용 동기/비동기식 송수신**
- 데이터를 **직렬로 전송**하는 프로토콜
- 동기식, 비동기식 2가지 프로토콜이 있지만 거의 비동기식으로 사용
- 데이터 송수신용으로 2개의 선이 사용되며 **전이중**
- 70년대경 개발된 프로토콜
- 용도
  - 디버깅
  - 단순 통신
  - 동작확인
- 장점
  - 역사가 오래되고 편리하여 많이 사용되는 직렬 표준
  - 프로토콜이 쉬움
  - 패리티 비트를 사용한 오류 확인 가능
- 단점
  - 통신 거리가 20m로 애매
  - 데이터 전송속도가 매우 느림
  - 여러 디바이스와 연결할 수 없는 1:1 구조
  - 1:1의 단점을 보완하고자 RS-422, RS-485 등 사용

#### 사용
```
HAL_UART_Transmit(&huart2, (uint8_t *)"UART test\r\n", (uint16_t)strlen("UART test\r\n"), 0xFFFFFFFF);
// &huart2 : uart 핸들 번호
// (uint8_t *)"UART test\r\n" : 출력할 문자열
// (uint16_t)strlen("UART test\r\n") : 문자열 길이
// 0xFFFFFFFF : 타임아웃

HAL_UART_Receive(&huart2, (uint8_t *)buf, size, 0xFFFFFFFF);
// &huart2 : uart 핸들 번호
// (uint8_t *)buf : 입력받을 문자열
// size : 입력받을 길이
// 0xFFFFFFFF : 타임아웃
```
- 디바이스와 PC의 설정을 동일하게 해야함
  - 9600-8-N-1
  ```
  Speed : 9600
  Data : 8(bit)
  Parity : N(none)
  Stop bit : 1(bit)
  ```
- STM32CubeIDE에서 실수 출력 방법
  - Project Properties > C/C++ Build > Settings > Tool settings > MCU GCC Linker > Miscellaneous > Other Flags > "-u \_printf_float"
- 폴링 방식
  - 문제점
    - 효율이 좋지 않음
    - 데이터를 놓칠 때가 있음
- 인터럽트 방식
  - NVIC setting
  ```
  // 인터럽트로 처리하기 때문에 계속 읽을 필요가 없음
  HAL_UART_Receive_IT(&huart2, &buf, 1);
  ```
  
![UART](https://user-images.githubusercontent.com/50474972/116820853-1af93480-abb2-11eb-8e5e-9a8e22ea6d97.png)

#### 분석
- 임베디드 환경에서는 통신이 안되는 경우가 많음

![파형](https://user-images.githubusercontent.com/50474972/116819927-68bf6e00-abad-11eb-8337-48cdbcbc245d.png)

- H/W, S/W
  - 회로도 문제
  - 부품 불량
  - 소스코드 오류
- 송신, 수신
  - 송신이 제대로 되는지
  - 송신은 제대로 되는데 수신부의 문제가 발생한 경우
  - 데이터의 깨짐, 처리 불량
- 간헐적 에러
  - 잡음, 송수신부 F/W 문제 등

---

### 데이터 통신
- 종류

| Wire | Car | Wireless |
| --- | --- | --- |
| Parallel | CAN | FM/AM |
| RS232 | LIN | NFC |
| RS422, RS485 | Flex Ray | Zigbee |
| I2C | Most | Bluetooth |
| SPI | Wave | Wi-Fi |
| 1-Wire 등 | Car-Ethernet 등 | Lora 등 |

#### 분류
- 거리 : 근거리, 원거리
- 선의 유무 : 유선, 무선
- 통신의 방향성 : Simplex, Half-Duplex, Full-Duplex
- 직렬, 병렬
- 동기, 비동기

#### 통신 방향
- Simplex(단방향 전송 방식)
  - 데이터의 흐름이 한 방향으로만 한정되어 있는 통신방식
  - 일반적인 송수신, 데이터를 수신만 할 수 있음
- Half Duplex(반이중 전송 방식)
  - 양쪽 방향으로 송수신이 가능한 양방향 통신, 한 번에 하나의 전송만 이루어짐
  - 송신하고 있을 때는 수신이, 수신하고 있을 때는 송신이 되지 않는 전송방식
- Full Duplex(전이중 전송 방식)
  - 데이터를 양방향으로 동시에 송수신 할 수 있는 방식

#### 직렬, 병렬
- 직렬 통신 : 데이터가 1 bit
- 병렬 통신 : 데이터가 n bits
  - 보통 직렬통신보다 빠르지만, 항상 그런 것은 아님
  - 장거리 및 고속 통신에서는 직렬 > 병렬
    - 고속 통신 : CPU와 RAM간의 통신

#### 동기, 비동기
- 동기 통신 : 클럭이 있음
  - 두 디바이스가 약속된 클럭에 맞추어 통신을 함
  - I2C, SPI 등
- 비동기 통신 : 클럭이 없음
  - USART

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
