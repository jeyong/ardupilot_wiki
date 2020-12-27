.. _common-sensor-offset-compensation:

[copywiki destination="copter,plane,rover"]

===================================
센서 위치 Offset 보상 (Sensor Position Offset Compensation)
===================================

ArduPilot은 비행체에 센서 장착 위치에 따른 보상이 가능하다. 여기서는 어떤 파라미터를 어떻게 설정해야하는지에 대해서 정리한다.

.. note::

     대부분의 비행체내에 모든 센서들(IMU, GPS, optical flow 등)이 서로 15cm 내에 위치하는 경우에는 offset을 제공하더라도 차이날만한 성능개선을 얻기는 어렵다.

.. image:: ../../../images/sensor-position-offsets-xyz.png

센서의 위치 offset은 3개 값(X, Y, Z)로 설정한다. 이 값은 IMU가 Pixhawk의 가운데 있다고 가정하거나 비행체의 무게 중심으로부터 해당 센서의 떨어진 거리를 의미한다. (단위는 m)

- X : IMU나 무게 중심의 정면 방향의 거리. 양수 값은 비행체의 정면 방향이고 음수 값은 뒤 방향이다.
- Y : IMU나 무게 중심의 오른쪽 방향의 거리. 양수 값은 비행체의 오른쪽 방향이고 음수는 왼쪽 방향이다.
- Z : IMU나 무게 중심의 아래 방향의 거리. 양수 값은 *낮은* 방향이고 음수 값은 *높은* 방향이다.

Pixhawk가 비행체의 무게 중심과 떨어진 곳에 장착하는 경우 실제로 센서까지의 거리는 Pixhawk의 중심으로부터 측정할 수 있다. 이 경우 IMU position offset은 별도로 지정할 수 있고 다른 센서의 위치는 비행체의 무게 중심으로부터의 거리는 지정할 수 있다.

파라미터 상세
=================

**IMU (aka INS):**

최상의 결과를 얻기 위해서 Pixhawk는 비행체의 무게 중심에 둬야만 한다. 만약 물리적으로 그렇게 설치하기가 불가능한 경우에는 offset은 다음 파라미터를 서절해서 부분적으로 보상할 수 있다.

- :ref:`INS_POS1_X <INS_POS1_X>`, :ref:`INS_POS1_Y <INS_POS1_Y>`, :ref:`INS_POS1_Z <INS_POS1_Z>` 비행체의 무게 중심으로부터의 첫번째 IMU의 위치
- :ref:`INS_POS2_X <INS_POS2_X>`, :ref:`INS_POS2_Y <INS_POS2_Y>`, :ref:`INS_POS2_Z <INS_POS2_Z>` 비행체의 무게 중심으로부터의 두번째 IMU의 위치
- :ref:`INS_POS3_X <INS_POS3_X>`, :ref:`INS_POS3_Y <INS_POS3_Y>`, :ref:`INS_POS3_Z <INS_POS3_Z>` 비행체의 무게 중심으로부터의 세번째 IMU의 위치

보상은 *부분적으로* 적용된다. 왜냐하면 ArduPilot은 비행체의 속도와 위치 추정을 할 수 있지만 가속도 추정은 보정할 수 없다.
예제로 만약 Pixhawk가 비행체의 앞쪽에 설치하고 offset 보상을 하지 않고 비행체가 갑자기 뒤로 기울어지면(nose가 올라가도록 회전하는 경우), 비행체의 속도 추정은 모멘텀적으로 비행체가 상승하는 것을 볼 수 있다. 위치 offset을 비행체에 추가하면 이런 모멘텀 상승은 나타나지 않는다. EKF는 여전히 모멘텀 수직 가속도를 보여준다. Altitude hold 제어기에서 가속도를 사용하기 때문에 이렇게 하면 비행체는 모멘텀적으로 쓰로톨을 줄이게 된다.

비록 개별 position offset은 각 IMU에 대해서 설정할 수 있다. Pixhawk 보드 내부에서 IMU 센서들의 위치 차이는 아주 작아서 모든 IMU에 대해서 동일한 값을 사용한다.

**GPS:**

- :ref:`GPS_POS1_X <GPS_POS1_X>`, :ref:`GPS_POS1_Y <GPS_POS1_Y>`, :ref:`GPS_POS1_Z <GPS_POS1_Z>` 비행체의 IMU나 무게 중심으로부터의 첫번째 GPS 위치
- :ref:`GPS_POS2_X <GPS_POS2_X>`, :ref:`GPS_POS2_Y <GPS_POS2_Y>`, :ref:`GPS_POS2_Z <GPS_POS2_Z>` - :ref:`GPS_POS2_X <GPS_POS2_X>`, :ref:`GPS_POS2_Y <GPS_POS2_Y>`, :ref:`GPS_POS2_Z <GPS_POS2_Z>` 비행체의 IMU나 무게 중심으로부터의 두번째 GPS 위치

**Range Finder (Sonar or Lidar):**

- :ref:`RNGFND1_POS_X <RNGFND1_POS_X>`, :ref:`RNGFND1_POS_Y <RNGFND1_POS_Z>`, :ref:`RNGFND1_POS_Z <RNGFND1_POS_Z>` 비행체의 IMU나 무게 중심으로부터의 첫번째 RangeFinder의 위치
- :ref:`RNGFND2_POS_X <RNGFND2_POS_X>`, :ref:`RNGFND2_POS_Y <RNGFND2_POS_Z>`, :ref:`RNGFND2_POS_Z <RNGFND2_POS_Z>` 비행체의 IMU나 무게 중심으로부터의 두번째 RangeFinder의 위치

**Optical Flow:**

- :ref:`FLOW_POS_X <FLOW_POS_X>`, :ref:`FLOW_POS_Y <FLOW_POS_Y>`, :ref:`FLOW_POS_Z <FLOW_POS_Z>` 비행체의 IMU나 무게 중심으로부터의 거리

