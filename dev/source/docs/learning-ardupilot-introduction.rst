.. _learning-ardupilot-introduction:

=================================
ArduPilot 익히기 — 도입
=================================

ArduPilot의 기본 구조에 대해서 알아보자. 시작하기 전에 어떤 코드 탐색 도구를 이용할지 결정해야한다. 그냥 웹브라우져로도 가능하면 그런 경우 https://github.com/ArduPilot/ardupilot/ 를 참고하자. 하지만 :ref:`cloned all of the git repositories <where-to-get-the-code>` 를 가지고 있고 프로그래머를 위한 IDE가 있으면 많은 도움이 된다. :ref:`here <code-editing-tools-and-ides>` 를 참고하자.

기본 구조
===============

.. image:: ../images/ArduPilot_HighLevelArchecture.png
    :target: ../_images/ArduPilot_HighLevelArchecture.png

ArduPilot의 기본 구조는 5개의 파트로 구성된다:

-  vehicle code
-  shared libraries
-  hardware abstraction layer (AP_HAL)
-  tools directories
-  external support code (i.e. mavlink, dronekit)

비행체 코드
------------

비행체 디렉토리는 각 비행체 타입을 위한 펌웨어를 정의하는 최상위 디렉토리다. 현재 5가지 비행체 타입이 있다: Plane, Copter, Rover, Sub, AntennaTracker.

비록 서로 다른 비행체 타입 사이에 많은 공통 요소들이 있지만 그래도 각 비행테 타입은 다른 것이 사실이다. 지금까지 :ref:`Copter 코드의 코드 구조에 대해서 상세 설명 <apmcopter-code-overview>` 하였다.

\*.cpp 파일은 각 비행체 디렉토리는 make.inc 파일을 포함하고 있다. 여기에는 의존하는 라이브러리들의 목록이 있다. Makefiles은 이를 읽어서 빌드를 위해서 -I와 -L flags를 생성한다.

Libraries
---------

`libraries <https://github.com/ArduPilot/ardupilot/tree/master/libraries>`__ 는 4가지 비행체 타입 사이에서 공유된다.  이 라이브러리들은 센서 드라이버, 잣 및 위치 추정, 제어 코드를 포함하고 있다. 제사한 내용은 :ref:`Library Description <apmcopter-programming-libraries>`, :ref:`Library Example Sketches <learning-ardupilot-the-example-sketches>` , :ref:`Sensor Drivers <code-overview-sensor-drivers>` 를 참고하자.

AP_HAL
-------

ArduPilot이 다양한 플랫폼에 포팅이 가능한 것은 AP_HAL 계층(Hardware Abstraction Layer)이 있기 때문이다. libraries/AP_HAL 에는 최상위 AP_HAL이 존재한다. 다양한 보드를 지원하므로 각 보드 타입에 AP_HAL_XXX 하위 디렉토리가 있다. 예로 AP_HAL_AVRsms AVR 기반이고 AP_HAL_PX4는 Pixhawk 보드 그리고 AP_HAL_LINUX는 Linux 기반 보드이다.

Tools 디렉토리
~~~~~~~~~~~~~~~~~

tools 디렉토리는 여러 지원을 담당하는 디렉토리이다. 예제로 tools/autotest는 `autotest.ardupilot.org <https://autotest.ardupilot.org/>`__ 에 있는 autotest 인프라를 제공하고 tools/Replay는 log replay 유틸을 제공한다.

외부 지원 코드
~~~~~~~~~~~~~~~~~~~~~

일부 플랫폼에서 추가 기능이나 보드 지원 제공하기 위한 외부 지원 코드가 필요하다. 현재 외부 tree는 :

-  `PX4NuttX <https://github.com/ArduPilot/PX4NuttX>`__ - Pixhawk 보드에서 사용되는 RTOS
-  `PX4Firmware <https://github.com/ArduPilot/PX4Firmware>`__ - Pixhawk 보드에서 사용되는 PX4 미들웨어와 드라이버
-  `uavcan <https://github.com/ArduPilot/uavcan>`__ - ArduPilot에서 사용되는 uavcan CANBUS 구현
-  `mavlink <https://github.com/mavlink/mavlink>`__ - mavlink protocol과 code 생성기

.. note::

   여기서 다른 대부분은 :ref:`build ArduPilot <building-the-code>` 시에 :ref:`Git Submodules <git-submodules>` 로 import된다.

