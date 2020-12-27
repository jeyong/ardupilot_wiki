.. _common-compass-calibration-in-mission-planner:

===================
컴파스 칼리브레이션(Compass Calibration)
===================

기본적인 컴파스 칼리브레이션 수행하는 방법을 소개한다. Pixhawk 내부에 컴파스가 기본적으로 장착되어 있으며 외부 컴파스를 지원하는 GPS를 추가로 설치할 수 있다. 좀더 상세한 정보는 :ref:`고급 컴파스 셋업 <common-compass-setup-advanced>`를 참고한다.

.. tip::

   ArduPilot은 "world magnetic model"을 포함하고 있어서 새로운 장소로 이동해서 비행하는 경우에도 칼리브레이션을 다시 수행하지 않아도 된다. "world magnetic model"은 칼리브레이션을 다시 하지 않더라도 해당 위치에서의 북쪽을 진북으로 변환하도록 지원한다. 추가로 해당 위치에서의 경향성은 구동 시점에 칼리브레이션이 되고 이륙 후에 바로 칼리브레이션을 수행한다. 이렇게 하는 이유는 최상의 셋업을 가질 수 있도록 컴파일 칼리브레이션이 완료되는 때에 비행체는 3D GPS lock을 가진다. 만약 필요한 경우에는 컴파일 칼리브레이션을 하기 전에 제대로 3D GPS lock을 얻기 위해서 야외로 이동해서 진행할 수 있다.
   
.. note::

   컴파일 칼리브레이션은  비행체가 arm이 되는 동안에는 수행할 수 없다.


칼리브레이션 첫 단계(Calibration first steps)
=======================


.. 경고:: 컴파스 칼리브레이션을 진행하는 장소는 주변에 금속물질이나 자기장을 생성하는 물체(컴퓨터, 핸드폰, 금속 책상, 전원 공급기 등)가 없어야 한다. 이런 물체가 있는 경우 칼리브레이션이 제대로 되지 않는다.


- **SETUP\| Mandatory Hardware** 에서 **Compass**를 선택한다.

   .. figure:: ../../../images/CompassCalibration_Onboard.png
      :target: ../_images/CompassCalibration_Onboard.png

      미션 플래너: 컴파스 칼리브레이션

   arming 시도시에 "inconsistent compasses" 메시지가 계속 나타나는 경우 내부 컴파스를 비활성화시키기를 원할 수 있다. 이 경우 외부 컴파스가 칼브리레이션을 완료했는지 확인해야한다.

.. _onboard_calibration:

온보드 칼리브레이션
===================

"Onboard Calibration"은 Pixhawk에서 실행되는 칼리브레이션 루틴이다. 이 방법은 이전의 "Offboard Calibration"에 ("Live Calibration"이라고 알려진) 비해서 더욱 정확하다. "Offboard Calibration" 칼리브레이션은 offset 추가와 스케일링과 방향이 자동으로 결정된다.

.. note:: :ref:`autopilot board orientation<AHRS_ORIENTATION>` 파라미터가 올바른 값이 아니면 온보드 컴파스에 대해서 칼리브레이션이 실패한다.

모든 컴파스의 온보드 칼리브레이션을 수행하기 위해서 :

- "Onboard Mag Calibration" 섹션의 "Start" 버튼 클릭하기
- 만약 Pixhawk에 버저를 정착한 상태라면 매초 짧은 비프음을 들을 수 있다.
- hold the vehicle in the air and rotate it so that each side (front, back, left, right, top and bottom) points down towards the earth for a few seconds in turn. Consider a full 360-degree turn with each turn pointing a different direction of the vehicle to the ground. It will result in 6 full turns plus possibly some additional time and turns to confirm the calibration or retry if it initially does not pass.

   .. figure:: ../../../images/accel-calib-positions-e1376083327116.jpg
      :target: ../_images/accel-calib-positions-e1376083327116.jpg

- as the vehicle is rotated the green bars should extend further and further to the right until the calibration completes
- upon successful completion three rising tones will be emitted and a "Please reboot the autopilot" window will appear and you will need to reboot the autopilot before it is possible to arm the vehicle.


만약 칼리브레이션에 실패하는 경우:

- 칼리브레이션 실패 음이 발생된다. 녹색 바는 왼쪽으로 리셋될 수도 있다. 칼리브레이션 루틴은 재실행될 수 있다.(사용하는 지상국 SW에 따라 다름) 미션 플래너는 자동으로 재시작되고 위에서 언급한 절차와 같이 비행체 회전을 계속 수행한다.
- 컴파스가 칼리브레이션되지 않으면 자기장 장애로부터 좀 떨어진 곳으로 옮기는 것을 고려하라. 그리고 주머니에는 전자장치가 없어야 한다.
- 여러번 시도하고 난 후에도 컴파스 칼리브레이션을 실패한다면 "Cancel" 버튼을 누르고 "Fitness" 드롭다운을 좀더 유연한 설정으로 변경하고 다시 시도한다.
- 만약 컴파스 칼리브레이션이 계속 실패하면 :ref:`COMPASS_OFFS_MAX <COMPASS_OFFS_MAX>`를 850에서 2000까지 올리거나 아니면 3000까지 올릴 수 있다. 마지막으로 만약 컴파스 하나가 계속 칼리브레이션이 안되는 경우라면 이 컴파스를 비활성화 시키면 다른 컴파스를 비행에 사용하게 된다.

스틱 제스츄어를 사용한 온보드 칼리브레이션(no GCS)
=================================================
ArduPilot는 "RC 제어 스틱 제스츄러를 사용한 Onboard Calibration"를 지원한다. 이 말은 칼리브레이션 루틴이 미션 플래너 없이 Pixhawk에서 실행된다든 뜻이다. 이 방법은 예전의 "Offboard Calibration"보다 더 정확하다. (offset를 추가해서 스케일링을 계산하기 때문에 지상국 SW에서 실행된다.)

- 먼저 RC 칼리브레이션을 해야만 한다.
- 컴파스 칼리브레이션을 시작하려면 쓰로틀 스틱을 최대로 올리고 yaw를 최대한 오른쪽으로 2초간 유지한다.
- 만약 Pixhawk에 부저를 장착한 경우라면 매초 짧은 비프 음이 발생된다. 
- 비행체를 들어서 유지하고 회전시킨다. 각 면(앞, 뒤, 왼쪽, 오른쪽, 위, 아래)을 차례로 아래 방향으로 향하게 몇 초간 유지한다.

   .. figure:: ../../../images/accel-calib-positions-e1376083327116.jpg
      :target: ../_images/accel-calib-positions-e1376083327116.jpg

- 성공적으로 완료되면 성공음이 발생하고 비행체를 arming하기 전에 Pixhawk는 리부팅해야한다.

칼리브레이션이 실패하는 경우에는:

- 실패 음이 발생하면 칼리브레이션 과정을 다시 시작한다.
- 칼리브레이션을 취소하고자 한다면 쓰로틀 스틱을 위로 yaw는 최대한 왼쪽으로 2초간 유지한다.
- 여러번 시도 후에도 컴파스 칼리브레이션이 되지 않는다면 스틱을 이용해서 취소하고 위에서 말한 미션 플래너를 이용한 일반 Onboard Calibration을 사용한다.

큰 기체에서의 MagCal
====================

크거나 무거운 비행체에서 모든 축에 대해서 회전시키는 것은 쉽지 않다. 만약 GPS lock이 활성화 되어 있는 경우고 비행체의 실제 헤딩을 알고 있는 경우라면 꽤 정확한 칼리브레이션이 가능하다. 아니면 미션 플래너 지도에서 andmark 레퍼런스를 사용하거나 다른 컴파스(예로 핸드폰)를 사용해서 비행체의 헤딩을 사용한다.

컴파스 순서
================

이 페이지 맨 위에서 원한다면 장착한 컴파스의 우선순위를 변경할 수 있다. 

추가 정보
======================

컴파스 설정에 관한 추가 정보는 :ref:`Advanced Compass Setup <common-compass-setup-advanced>`에서 찾을 수 있다. 여기에서 추가 컴파스를 설정하는 방법, :ref:`automatic setting of offsets<automatic-compass-offset-calibration>`, non-standard compass alignments, :ref:`compassmot <copter:common-compass-setup-advanced_compassmot_compensation_for_interference_from_the_power_wires_escs_and_motors>` 등이 포함되어 있다.

자기 간섭에 대한 논의와 자기 간섭을 줄이는 방법에 대해서는 :ref:`Magnetic Interference <common-magnetic-interference>`에서 찾을 수 있다.

비디오 데모
===================

컴파스 칼리브레이션 비디오 데모

..  youtube:: CD8EhVDfgnI
    :width: 100%

..  youtube:: DmsueBS0J3E
    :width: 100%

[copywiki destination="copter,plane,rover,planner"]
