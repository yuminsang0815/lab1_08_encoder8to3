# 실험 후 레포트: 8:3 인코더

작성일 2026-09-19.

[실험 전 레포트](../../reports/pre/08_encoder8to3.md) · [해시·입력 기록](../../build/sim/result.json)

## Vivado GUI 과정과 사전 결과 비교

공개 템플릿 `86c15c5`를 새로 clone한 폴더에서 Vivado 2026.1 GUI의 New Project를 사용했습니다. 부품은 `xc7s75fgga484-1`, 설계 top을 `encoder8x3`, 시뮬레이션 top `tb_encoder8x3`을 설정했습니다.

Run Simulation → Run Behavioral Simulation에서 [실제 GUI 시뮬레이션 로그](../../evidence/08/vivado/simulation.log)의 `LAB1_PASS encoder8x3 cases=256`와 2560ns 종료를 확인했습니다. 8개의 원-핫 입력 패턴에 대한 3비트 2진 코드 변환과 무입력 및 다중 입력(Multi-hot) 조건에서의 default: a = 0; 예외 처리가 사전 파형과 완벽히 일치했습니다.

## 합성·구현·bit

Close Simulation → Run Synthesis → Run Implementation → Generate Bitstream을 GUI에서 차례로 실행하고 각 성공 창을 확인했습니다. [GUI 빌드 로그](../../evidence/07/vivado/build.log/)를 보관했습니다.

생성 파일은 `vivado/encoder8x3.runs/impl_1/encoder8x3.bit`, 크기는 3,687,013바이트입니다. 배포 [encoder_8x3.bit](../../vivado/encoder8x3.runs/impl_1/encoder8x3.bit)의 SHA-256은 `AD67E8407B0D6A80B271DF6FDB96EF63DF88B38C0B29CFBFB0F2E7F284521F9D`입니다.

오류 및 경고 여부는 실험 시에 기록해두지 못했습니다. 다음 실험 부터 기록하겠습니다.

## 보드 기록·촬영 상태

보드에 비트스트림을 다운로드한 후, 푸시버튼 KEY1\~8(i[7:0])과 출력 LED1\~3(a[2:0])의 상태를 대조했습니다

| 조건(입력 i[7:0]) | 시뮬레이션 a[2:0] | 실측 a[2:0] | 사진 |
|---|---|---|---|
| 10000000 | 000 | 000 | [10000000](../../evidence/08/board/photos/input-10000000.jpg) |
| 00000001 | 111 | 111 | [00000001](../../evidence/08/board/photos/input-00000001.jpg) |
| 00000011 | 000 | 000 | [00000011](../../evidence/08/board/photos/input-00000011.jpg) |
[LED 동장 영상](../../evidence/08/board/videos/demo.mp4)

## 결론

8개 입력선 중 단 하나의 활성 비트 위치를 3비트 2진 신호로 변환하는 기능을 확인했습니다. 특히 우선순위 판정이 없는 회로 특성에 따라 버튼 2개를 동시에 누르면 default 분기에 의해 모든 LED가 즉각 소등되는 비원-핫 예외 처리 동작이 보드 상에서도 정확히 실현됨을 검증했습니다.
