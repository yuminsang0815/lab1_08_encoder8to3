# 실험 후 레포트: 8:3 인코더

작성일 2026-09-19.

[실험 전 레포트](../../reports/pre/08_encoder8to3.md) · [해시·입력 기록](../../build/sim/result.json)

## Vivado GUI 과정과 사전 결과 비교

공개 템플릿 `86c15c5`를 새로 clone한 폴더에서 Vivado 2026.1 GUI의 New Project를 사용했습니다. 부품은 `xc7s75fgga484-1`, 설계 top을 `encoder8x3`, 시뮬레이션 top `tb_encoder8x3`을 설정했습니다.

Run Simulation → Run Behavioral Simulation에서 [실제 GUI 시뮬레이션 로그](../../evidence/07/vivado/simulation.log)의 `LAB1_PASS encoder8x3 cases=256`와 16ns 종료를 확인했습니다. 삼항 조건 연산자와 시프트 연산자를 통한 8비트 출력 버스 전송 및 i=0 시 전체 0 유지 동작이 사전 파형과 일치했습니다.

## 합성·구현·bit

Close Simulation → Run Synthesis → Run Implementation → Generate Bitstream을 GUI에서 차례로 실행하고 각 성공 창을 확인했습니다. [GUI 빌드 로그](../../evidence/07/vivado/build.log/)를 보관했습니다.

생성 파일은 `vivado/demux_1x8.runs/impl_1/demux_1x8.bit`, 크기는 3,687,012바이트입니다. 배포 [demux_1x8.bit](../../vivado/demux_1x8.runs/impl_1/demux_1x8.bit)의 SHA-256은 `C80AE4BDC87D7569D44A3AD083416D70FD4C333D9893FB594D502022F493A310`입니다.

오류 및 경고 여부는 실험 시에 기록해두지 못했습니다. 다음 실험 부터 기록하겠습니다.

## 보드 기록·촬영 상태

보드에 비트스트림을 다운로드한 뒤, 푸시버튼 KEY1(데이터 i)과 DIP1~3(선택선 s[2:0]), 출력 LED1~8(o[7:0])을 조작하여 실측했습니다

| 조건 | 시뮬레이션 o[7:0] | 실측 o[7:0] | 사진 |
|---|---|---|---|
| i=0,s=all | 00000000 | 00000000 | [i=0,s=all](../../evidence/07/board/photos/input-i=0,s=all.jpg) |
| i=1,s=000 | 10000000 | 10000000 | [i=1,s=000](../../evidence/07/board/photos/input-i=1,s=000.jpg) |
| i=1,s=111 | 1 | 1 | [i=1,s=111](../../evidence/07/board/photos/input-i=1,s=111.jpg) |
[LED 동장 영상](../../evidence/07/board/videos/demo.mp4)

## 결론

데이터 입력 i=0일 때 모든 출력이 소등 상태를 유지하고, i=1 인가 상태에서 3비트 선택선에 따라 단 하나의 LED만 MSB에서 LSB 방향으로 이동 점등되는 디멀티플렉서의 원-핫 분배 특성을 하드웨어에서 확인했습니다. 
