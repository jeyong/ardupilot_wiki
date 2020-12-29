.. _autotrim:

========
오토트림(AutoTrim)
========

바람은 비행에 많은 영향을 미쳐서 비행체를 밀어내기도 한다.
스테빌라이즈 모드에서 비행하더라도 이런 현상은 발생할 수 있다. 심지어 바람이 없는 환경에서도 비행체가 동일한 방향으로 드리프트 되는 경향이 발생할 수 있다.
이 경우 주로 "Save Trim" 혹은 "Automatic Trim" 기능을 사용해서 보정할 수 있다.

.. note::

    대부분 사용자에게 이런 과정이 필요하지는 않다. 왜냐하면 :ref:`accelerometer calibration <common-accelerometer-calibration>` 으로 trim 값 설정하기 충분한 경우가 대부분이다.

Trim 저장(Save Trim)
~~~~~~~~~

Save Trim은 가장 간단한 방법으로 본질적으로 조정기의 trim값이 Pixhawk로 전송되는 방식이다. (`비디오 데모 <https://www.youtube.com/watch?v=ayA0uYOqKX4>`__)

1. CH7 스위치가 1800 이상인지 미션 플래너의 Hardware > Mandatory Hardware > Radio Calibration 화면에서 확인한다.

.. image:: ../images/MP_SaveTrim_Ch7PWMCheck.png
    :target: ../_images/MP_SaveTrim_Ch7PWMCheck.png

2. CH7 옵션을 Save Trim으로 설정한다. 설정 방법은 Software > Copter Pids 화면에서 "Write params" 버튼을 누른다.

.. image:: ../images/MP_SaveTrim_Ch7.png
    :target: ../_images/MP_SaveTrim_Ch7.png

3. CH7 스위치는 off 상태로 하고, 스테빌라이저 모드로 비행체를 날리고 조정기의 roll과 pitch trim을 사용하여 비행체가 수평 비행이 되도록 한다.

4. 비행체를 착륙시키고 쓰로틀을 0으로 한다.

5. roll과 pitch 스틱을 놓고 CH7 스위치를 적어도 1초 이상 high 상태로 올린다. 미션 플래너의 Flight Data 화면의 메시지 탭에 "Trim saved"라는 문구가 나타나야만 한다.

6. 조정기의 roll과 pitch trim을 다시 가운데로 리셋한다. 그리고 다시 비행시킨다. 이제 수평으로 비행이 되어야만 한다. 만약 수평 비행이 안된다면 동일한 과정을 3, 4, 5회 반복한다.

자동 Trim(Auto Trim)
~~~~~~~~~

자동 trim과 roll과 pitch trim은 안정적으로 호버링 비행을 해야 설정할 수 있다.

1. 충돌 없이 비행이 가능한 공간이면서 바람이 없는 환경을 찾는다.

2. 비행체를 스테빌라이저 모드로 설정한다.

3. 쓰로틀을 내린 상태로 러더를 오른쪽으로 15초 동안 누르고 있으면서 LED가 빨강, 파랑, 노랑 패턴으로 바뀌는 때를 기다린다.

4. 안정적인 호버링 상태로 대략 25초 동안 비행체를 날린다.

5. 착륙시키고 쓰로틀을 0으로 맞춘다. 그리고 몇 초 동안 기다린다. (이 시점에 trim 파라미터가 저장된다.)

6. 스테빌라이저 모드에서 다시 이륙하고 이제 비행체가 수평 비행이 되는지 확인한다. 수평 비행이 안되면 2, 3, 4회 반복한다.

.. note::

    위에 절차들을 테스트도 할 수 있는데 지상에서 운영할 때는 배터리 연결 해제한 상태로 진행한다. Pixhawk를 미션 플래너에 연결하고 해당 단계들을 위에 단계들을 완료하는 시뮬레이션처럼 Flight Data 화면을 관찰한다.

.. image:: ../images/MP_SaveTrim_FlightDataScreen.jpg
    :target: ../_images/MP_SaveTrim_FlightDataScreen.jpg

.. note::

    :ref:`AHRS_TRIM_X <AHRS_TRIM_X>` 와 :ref:`AHRS_TRIM_Y <AHRS_TRIM_Y>` 를 수정해서 trim을 수동으로 설정할 수 있다. Roll trim은 :ref:`AHRS_TRIM_X <AHRS_TRIM_X>` 이고 Pitch trim은 :ref:`AHRS_TRIM_Y <AHRS_TRIM_Y>` 이다. 두 값은 rad 단위를 사용하고 왼쪽으로 roll과 앞쪽 pitch는 음수가 된다.

.. note::

    비행체가 입력이 없으면 가만히 떠 있도록 하기 위해 모든 drift를 제거하기는 사실상 불가능하다.

Save Trim과 Auto Trim 비디오
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  youtube:: 5z0zuwicZh8
    :width: 100%

..  youtube:: e3OF9ih50PU
    :width: 100%

데스크탑 방법
~~~~~~~~~~~~~~

trim은 비행체 수평 설정으로 업데이트할 수 있다. 미션 플래너에 연결하고 Initial Setup, Mandatory Hardware, Accel Calibration을 선택하고 아래쪽에 "Calibrate Level" 버튼을 누른다. 

.. image:: ../images/AccelCalibration_MP.png
    :target: ../_images/AccelCalibration_MP.png

비행체가 지상에 있는 동안 HUD 수평을 만든다는 의미는 수평으로 drift가 바 drift 
Please note though that making the HUD level while the vehicle is on the
ground does not necessarily mean it won't drift horizontally while
flying because of other small frame issues including the flight
controller not being perfectly level on the frame and slightly tilted
motors.

.. |MP_SaveTrim_Ch7| image:: ../images/MP_SaveTrim_Ch7.png
    :target: ../_images/MP_SaveTrim_Ch7.png
