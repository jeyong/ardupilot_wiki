.. _basic-operation:

======================================
ArduPilot 동작에 대한 간단 개요
======================================

Pixhawk에서 실행되는 ArduPilot 펌웨어의 기능에 대한 간단히 알아보자. 아래 그림은 기본 기능 동작에 대한 블록 다이어그램이다. 시스템을 구성하는 ArduPilot 기본 기능을 이해하는데 도움이 될 것이다.

.. image:: ../../../images/block-diagram.jpg
    :target: ../_images/block-diagram.jpg

기본 목표
----------

이 SW의 기본 목표는 비행체를 제어하는데 있어서 자동 및 조정기를 통한 수동 조정, 지상국 SW, 비행체에 장착된 컴패니온 컴퓨터를 사용할 수 있고 비행체가 자동으로 미션 수행이 가능하게 한다.

입력
------

제어 입력 신호는 라디오 제어 수신기, 텔레메트리나 컴패니온 컴퓨터로부터 MAVLink 통신을 통해서 입력 받을 수 있다. 라디오 제어 수신기는 desired attitude control을 위한 Roll/Pitch/Yaw 입력, 쓰로틀, 비행 모드 제어, 카메라 제어와 같은 추가 기능을 제공한다. Roll/Pitch/Yaw/Throttle를 위한 라디오 제어 입력은 ``RCMAP_x`` 기능을 통해서 조정기 라디오 제어 채널로 할당할 수 있다. 추가 기능(Auxiliary functions)은 ``RCx_FUNCTION`` 파라미터를 이용해서 할당할 수 있다.

출력
-------
출력은 비행체를 제어하기 위해서 서버, 모터, 릴레이(relay) 등을 활성화 시킨다. Pixhawk에서 출력은 ``SERVOx_FUNCTION`` 파라미터를 통해서 원하는 비행체 제어 출력 기능을 할당할 수 있다. 기능이 RCPassThru로 설정되지 않는다면 출력이 RC 입력과 반드시 매핑할 필요는 없다.

Sensors
-------

Pixhawk의 센서를 통해서 attitude, position, power system 모니터링, 비행체 속도를 계산할 수 있다. ArduPilot 호환 비행제어기는 적어도 1개 이상의 가속도, 바로미터, 자이로 센서를 가지고 있다.
일반적으로 GPS와 컴파스 센서는 별도로 필요하다. 보통 Pixhawk 외부에 장착된다. 일부 비행제어기는 이중화를 위해서 여러 센서가 있다. 많은 센서들이 하드웨어 셋업 단계에서 반드시 칼리브레이션이 필요하다.



[copywiki destination="plane,copter,rover"]


