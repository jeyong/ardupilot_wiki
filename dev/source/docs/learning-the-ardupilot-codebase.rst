.. _learning-the-ardupilot-codebase:

===============================
ArduPilot 코드베이스 익히기
===============================

ArduPilot 코드 베이스는 대략 700k 라인이다. 따라서 새로운 사용자에게는 겁나게 많다. 여기서는 코드를 빠르게 익힐 수 있는 방법을 제안한다. 이미 C++의 핵심 개념과 많은 예제들에 익숙하다고 가정하고 리눅스에서 해당 코드를 분석해 보자.

이 페이지와 아래 링크 페이지는 튜토리얼로 사용할 수 있게 만들어졌다. 각 페지이를 순서대로 스스로 따라가보자. 만약 중요한 정보가 빠졌거나 개선이 필요한 부분에 대해서 알려주면 최대한 수정할 예정이다.

튜터리얼 단계
--------------

.. toctree::
    :maxdepth: 1

    소개 <learning-ardupilot-introduction>
    Library 설명 <apmcopter-programming-libraries>
    Library 예제 개요 <learning-ardupilot-the-example-sketches>
    Sensor Drivers <code-overview-sensor-drivers>
    Threading <learning-ardupilot-threading>
    UARTs and the Console <learning-ardupilot-uarts-and-the-console>
    RC Input and Output <learning-ardupilot-rc-input-output>
    Storage and EEPROM management <learning-ardupilot-storage-and-eeprom-management>
    EKF <ekf>
    Copter - Vehicle Code introduction <apmcopter-code-overview>
    Copter - Attitude Control <apmcopter-programming-attitude-control-2>
    Copter - Adding Parameters <code-overview-adding-a-new-parameter>
    Copter - Adding a new flight mode <apmcopter-adding-a-new-flight-mode>
    Copter - Scheduling your new code to run intermittently <code-overview-scheduling-your-new-code-to-run-intermittently>
    Copter - Motors Library <code-overview-copter-motors-library>
    Copter - PosControl and Navigation <code-overview-copter-poscontrol-and-navigation>
    Copter - Object Avoidance <code-overview-object-avoidance>
    Rover - Adding a new drive mode <rover-adding-a-new-drive-mode>
    Rover - L1 navigation controller <rover-L1>
    Plane - Architecture overview <plane-architecture>
    Adding a new Log message <code-overview-adding-a-new-log-message>
    Adding a new MAVLink message <code-overview-adding-a-new-mavlink-message>
    Adding a new MAVLink Gimbal <code-overview-adding-support-for-a-new-mavlink-gimbal>

.. note::

   현재 ArduPilot에서는 4개 비행체가 있지만 서로 다른 비행체 타입 사이에 공통된 요소가 많다. 지금까지 Copter 코드에 대한 코드 구조만 상세하게 설명했었다.

다른 튜터리얼
---------------

비록 엄격히 말하면 ArduPilot 프로젝트는 아니지만 여기 튜터리얼도 도움이 될 것이다.

- `DroneKit <https://dronekit-python.readthedocs.io/en/latest/>`__
