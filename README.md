# Embedded_Programming

### 구성
- CPU : 컴퓨터는 x86(CISC), AP는 ARM(RISC) 계열로 구성됨
- GPU : Vector 부동 소수점 연산은 CPU 보다 빠름
- 통신 : Wi-Fi 802.11a/b/g/n/ac
- VPU : 4K 재생 담당
- DSP : 오디오, 영상처리, 단순 반복 계산에 특화
- ISP : 카메라 처리 담당

  | ARM | Intel |
  | --- | --- |
  | 모바일 시장에서 강세 | PC 시장에서 강세 |
  
#### ARM
- Cortex A : AP용
- Cortex R : Realtime OS용
- Cortex M : Mobile용

### 장비
#### 라즈베리파이
- 4 Model B CPU : Broadcom BCM2711 SoC
- 3 Model B+ CPU : ARM Cortex A57 1.4GHz
- CPU : Quad Core ARM Cortex-A72 1.5GHz
- GPU : Broadcom VideoCore VI 500MHz

#### Bread Board
- 자유롭게 선을 뽑고 끼울 수 있음
- 세로 줄, 가로 줄은 모두 연결되어있는 상태

#### 저항
- 기기에 들어가는 전류를 맞춰 주기 위해 사용
- 저항을 쓰면 전압, 전류를 조절할 수 있음
- 라즈베리파이 : 5V, 3.3V로 고정

  ![컬러코드](https://user-images.githubusercontent.com/50474972/116275486-a5125900-a7be-11eb-984f-eba6060fa53c.png)


### 확인 사항
#### DataSheet
- 권장 전압, 전류를 알 수 있음
