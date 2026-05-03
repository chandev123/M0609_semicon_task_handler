# semicon_handler

**반도체 Task Handler 로봇 자동화 공정 (ROS2 기반 제어 및 통합 모니터링 시스템)**

본 프로젝트는 반도체 공정 내 이송 및 정밀 삽입(Peg-in-hole) 작업을 자동화하기 위해 구축된 6축 매니퓰레이터 제어 시스템입니다. 이기종 환경의 모션 제어 코드를 ROS2 아키텍처로 리팩토링 및 이식하여, 시스템의 안정성과 실시간 병렬 모니터링 환경을 확보했습니다.

---

## 💡 1. 주요 기능 (Key Features)
* **6-DoF 복합 모션 시퀀스 제어:** 단일 노드(`task_node`)에서 '접근 ➔ 추출 ➔ 틸팅 ➔ 정밀 삽입'으로 이어지는 총 145개의 고난도 API 시퀀스를 오케스트레이션하여 충돌 없는 자동화 공정 수행.
* **하드웨어 파손 방지(Fail-safe) 위치 제어:** 이기종 통신 환경의 제어 루프 지연(Latency)으로 인한 장비 파손(Overshoot) 리스크를 원천 차단하기 위해, 절대 좌표 기반의 무결점 위치 제어 아키텍처 채택.
* **실시간 하드웨어 병렬 모니터링:** 메인 제어 루프와 완전히 분리된 독립 노드(`hw_node`)를 통해 관절 상태 및 에러를 실시간 모니터링.
* **웹 기반 통합 대시보드 연동:** 사용자가 원격으로 공정을 제어하고 로봇의 상태를 시각적으로 확인할 수 있는 웹 UI 구축.

[🎥 1 Min Demo Video (Click to Watch Website)](https://chandev123.github.io/M0609_semicon_task_handler/)

---

## 🏗️ 2. 시스템 설계 및 플로우차트 (System Architecture)
![System Architecture](docs/images/system_architecture.png)
![Flowchart](docs/images/flowchart.png)
![Workspace](docs/images/workspace.jpeg)
![Node Architecture](docs/images/node_architecture.png)

* **Repository Structure**

```bash
m0609_semicon_task_handler/
.
├── docs
│   ├── images
│   │   ├── 1min_demo_video.mp4
│   │   ├── flowchart.png
│   │   ├── node_architecture.png
│   │   ├── system_architecture.png
│   │   └── workspace.jpeg
│   └── index.html
├── README.md
├── requirements.txt
└── src
    ├── config
    │   └── pose_config.yaml
    ├── force_insertion_algorithm
    │   ├── insert_card_slot.py
    │   └── README.md
    ├── frontend
    │   ├── css
    │   │   └── dashboard.css
    │   └── index.html
    ├── LICENSE
    ├── m0609_semicon_task_handler
    │   ├── hw_node
    │   │   ├── hw_node_force.py
    │   │   └── __init__.py
    │   ├── __init__.py
    │   ├── task_node.py
    │   └── ui_node.py
    ├── package.xml
    ├── resource
    │   └── m0609_semicon_task_handler
    ├── setup.cfg
    ├── setup.py
    └── test
        ├── test_copyright.py
        ├── test_flake8.py
        └── test_pep257.py

```

---

## 🖥️ 3. 운영체제 및 개발 환경 (Environment)
* **OS:** Ubuntu 22.04.5 LTS
* **Middleware:** ROS2 Humble Hawksbill
* **Language:** Python 3.10, HTML5, DART-studio
* **Database/Network:** Firebase DB (UI-ROS2 연동용)

---

## 🛠️ 4. 사용 장비 목록 (Hardware Requirements)
* **Manipulator:** Doosan Robotics M0609 (6-DOF, Payload 6kg)
* **Gripper:** modbus gripper DA_v1

---

## 📦 5. 의존성 (Dependencies)
[requirements.txt](requirements.txt)

---

## 📦 6. 실행순서
```bash
# step1. ros2 패키지 빌드
colcon build --packages-select m0609_semicon_task_handler

# step2. 노드 실행
# 1. bringup
ros2 launch dsr_bringup2 dsr_bringup2_rviz.launch.py mode:=real host:=192.168.1.100 port:=12345 model:=m0609

# 2. task_node
ros2 run m0609_semicon_task_handler task_node

# 3. ui_node
ros2 run m0609_semicon_task_handler ui_node

# step3. ui 페이지 로드 및 실행
cd frontend/
# index.html 파일을 브라우저에서 실행 (Drag & Drop or Double Click)
```
