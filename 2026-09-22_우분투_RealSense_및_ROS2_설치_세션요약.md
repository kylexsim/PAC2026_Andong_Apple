# 💬 [PAC 2026 / 환경구축] 2026-09-22 우분투 RealSense 및 ROS2 설치 세션 요약

* **작성 일시**: 2026년 9월 22일 22:00
* **작업 환경**: Ubuntu 24.04 LTS (NVIDIA GeForce RTX 4060 Laptop)
* **수행자**: 심준우 & 아티 (A-ti)
* **대상 하드웨어**: Intel RealSense D435 Depth Camera

---

## 1. 오늘 작업 핵심 성과 요약

1. **안동청과 Physical AI & Intel RealSense D435 활용 방안 정립**:
   * 선별 컨베이어 위 사과 3D 체적/기형과 실시간 등급 판별 (Global Shutter IR 센서 활용).
   * 로봇 팔(매니퓰레이터) 피킹 및 난좌(트레이) 6-DoF 파지(Grasping) 궤적 제어.
   * 공판장 콘티 박스 및 출하 박스 스마트 디팔레타이징/팔레타이징 물류 자동화 기획.

2. **우분투 24.04 디스코드(Discord) 공식 패키지 설치 완료**:
   * 최신 2026 배포판 아키텍처(`updater_bootstrap`) 적용 공식 deb 패키지 설치.
   * Ubuntu 24.04의 AppArmor 보안 정책에 맞춘 안전한 권한 등록 및 불필요한 설치 파일 정리 완료.

3. **우분투 24.04 전용 공식 LTS인 ROS 2 Jazzy Jalisco 완벽 구축**:
   * Ubuntu 24.04 Noble Numbat 환경에 맞춘 공식 장기 지원(2024~2029) 버전 선정.
   * `ros-jazzy-desktop` 풀 패키지(코어 라이브러리, RViz2 3D 뷰어, 데모 노드) 및 `ros-dev-tools` 설치 완료.
   * `~/.bashrc` 환경 변수 자동 로드 및 `rosdep` 의존성 데이터베이스 초기화 완료.

4. **실행 치트시트 생성 및 바탕화면 저장 완료**:
   * 카메라 전용 뷰어(`realsense-viewer`), 로봇 3D 시각화 도구(`rviz2`), 그리고 실전 카메라-RViz2 연동 절차를 정리한 메모장 파일(`카메라_및_ROS2_실행명령어.txt`) 바탕화면 생성.

---

## 2. 주요 기술적 이슈 및 트러블슈팅 내역

1. **RealSense 패키지 저장소 누락 (`E: Unable to locate package...`) 해결**:
   * **원인**: `librealsense2`는 우분투 기본 저장소에 없으며, 최신 2026 버전 기준 도메인 및 보안 키가 갱신되어 발생.
   * **해결**: RealSense 공식 공개 키(`/etc/apt/keyrings/librealsenseai.gpg`) 및 noble 전용 저장소를 추가 등록 후 `librealsense2-utils`, `librealsense2-udev-rules` 정상 설치.

2. **Ubuntu 24.04 Wayland 환경과 Wine 카카오톡 간 클립보드 붙여넣기 이슈 분석**:
   * **원인**: GNOME 기본 캡처 도구가 생성하는 리눅스 표준 MIME 타입(`image/png`)과 Wine 위에서 동작하는 카카오톡의 윈도우 비트맵 규격(`CF_DIB`) 간 변환 누락.
   * **해결 가이드**: `~/Pictures/Screenshots` 폴더에서 파일 직접 드래그 앤 드롭 전송 또는 Flameshot / Xorg 세션 활용법 확립.

3. **USB 2.0 vs 3.0 대역폭 및 D435 작동 메커니즘 분석**:
   * USB 2.0(480Mbps, 반이중)과 USB 3.0(5Gbps, 전이중)의 전송량 차이 정리.
   * D435의 대용량 포인트클라우드 및 Full HD 컬러 스트리밍을 위한 USB 3.0(파란색 포트/SS) 및 고속 케이블 필수성 확인.

---

## 3. 핵심 명령어 요약 치트시트

```bash
# 1. RealSense 전용 3D 카메라 뷰어 실행
realsense-viewer &

# 2. ROS 2 환경 수동 로드 (필요시)
source /opt/ros/jazzy/setup.bash

# 3. ROS 2 통신 정상 동작 테스트 (Talker / Listener)
ros2 run demo_nodes_cpp talker
ros2 run demo_nodes_py listener

# 4. 3차원 로봇 시각화 도구 실행
rviz2 &

# 5. [다음 단계] RealSense D435 카메라 ROS 2 드라이버 설치
sudo apt install -y ros-jazzy-realsense2-camera
```

---

## 4. 다음 작업 예정 사항 (Next Steps)

* **Physical AI 비전 센싱 실기 테스트**:
  * `ros-jazzy-realsense2-camera` 패키지를 구동하여 D435 실시간 3D Point Cloud를 RViz2 화면에 띄우기.
* **사과 인식 알고리즘 연동**:
  * 카메라 RGB 영상에 YOLOv8 사과 탐지 모델 결합 ➡️ 검출된 사과의 3D 깊이 중심점 \((X, Y, Z)\) 좌표 추출 파이프라인 제작.
* **GitHub 팀 협업 레포지토리 연동 및 자동 푸시 구성**:
  * Git 환경 설정 후 일일 작업 세션 요약 문서 자동 푸시 체계 확립.
