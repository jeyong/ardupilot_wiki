.. _common-apm-navigation-extended-kalman-filter-overview:

============================
Extended Kalman Filter (EKF)
============================

.. image:: ../../../images/advanced-configuration-ekf.png

ArduPilot은 EKF 알고리즘을 이용하여 자이로스코프, 액셀로미터, 컴파스, GPS, airspeed, 바로미터 센서로부터 측정값으로 비행체의 위치, 속도, 방향을 추정한다.

단순한 상보 필터 알고리즘을 사용하는 것보다 EKF를 사용할때의 장점은 측정 가능한 모든 센서의 값을 퓨전하고 심각한 에러가 있는 경우 측정값을 거절할 수 있다. 여러 센서 중에 하나의 센서가 문제를 발생시키는 경우에 이 센서의 영향을 적게 받도록 하는 것이 가능하다. EKF는 optical flow나 거리센서로 부터의 측정값을 이용하여 비행의 성능을 높일 수 있다.

현재 ArduPilot의 안정 버전은 EKF2로 비행체의 자세와 위치 추정에 사용된다.  Pixhawk에 2개 IMU 센서가 있다면 각 IMU 센서에서 대해서 각 EKF 모듈이 병렬로 실행되므로 2개의 EKF 모듈이 병렬로 실행되게 된다. 이 중에서 하나의 EKF에서 계산되어 나오는 추정값을 사용한다. 잘 동작하고 있는 EKF를 선택하는 것은 센서 데이터가 제대로 동작하는지를 체크를 통해서 결정된다.

대부분 사용자들에게 있어서는 EKF 파라미터를 직접 수정하는 일이 드물다. 하지만 아래 나열한 파라미터들은 그래도 자주 변경될 수 파라미터들이다. 좀더 상세한 정보는 :ref:`developer EKF wiki page <dev:extended-kalman-filter>` 를 참고하라.

EKF2와 EKF3 중에 어떤 것을 사용해야할까?(Should the EKF2 or EKF3 be used?)
--------------------------------

일반적으론 EKF2 사용을 추천한다. 하지만 일부 경우에서는 EKF3를 사용해야만 한다. 아래 각각의 장점에 대해서 정리했다. :

- 대부분 사용자는 EKF2를 기본적으로 사용한다. 가장 많이 테스트가 되었고 안정적이다.
- EKF2는 Vicon 시스템이나 ROS SLAM으로 부터 외부 위치 추정을 수신하여 처리할 수 있다. EKF3의 경우 `이 PR <https://github.com/ArduPilot/ardupilot/pull/8730>`__ 을 머지할 예정이다.
- EKF3는 tailsitter 나 위/아래로 time pointing하는 기타 기체인 경우에 사용해야만 한다. EKF2는 가속도 Z축 offset만 추정하는 반면에 EKF3는 3축 모두에 대해서 추정한다.
- EKF3는 Beacon, Wheel Encoder, Visual Odometry를 포함하는 새로운 센서를 지원한다.
- EKF2는 gyro scale factors를 추정하지만 EKF3는 그렇지 않다. 일반적으로 이것은 중요하지 않은데 그 이유는 gyro scale factors는 거의 항상 1.0에 가깝기 때문이다. 하지만 매우 빠르게 회전하는 비행체에게는 중요하다.

EKF와 core의 수 선택하기 (Choosing the EKF and number of cores)
------------------------------------

:ref:`AHRS_EKF_USE <dev:extended-kalman-filter_ahrs_ekf_use>`: "1"로 설정하면 EKF 사용, "0"은 자세 제어는 DCM 혹은 위치 제어는 ahrs dead reckoning 사용. Copter-3.3 이상 버전에서는 이 파라미터가 강제로 "1"로 설정되며 변경 불가.

:ref:`AHRS_EKF_TYPE <AHRS_EKF_TYPE>`: "2"로 설정하면 자세와 위치 추정에 EKF2를 사용하고 "3"으로 설정하면 EKF3 사용

:ref:`EK2_ENABLE <EK2_ENABLE>`, :ref:`EK3_ENABLE <EK3_ENABLE>`: 각각 "1"을 설정하면 EKF2 혹은 EKF3 활성화

:ref:`EK2_IMU_MASK <EK2_IMU_MASK>`, :ref:`EK3_IMU_MASK <EK3_IMU_MASK>`: 사용할 IMU(가속도/자이로) 지정. EKF "core"(단일 EKF 인스턴스)는 지정한 IMU를 위해서 구동시킨다.

-  1: 첫번째 IMU를 사용하여 단일 EKF core 구동시키기
-  2: 두번째 IMU만 사용하여 단일 EKF core 구동시키기
-  3: 첫번째와 두번째 IMU 각각을 사용하여 2개의 별도 EKF core를 구동시키기

.. note::

   고정익과 Rover는 EKF가 불안정하게 동작하거나 GPS 3D lock이 되었는데도 GPS 데이터 퓨젼을 못하는 경우 EKF2나 EKF3에서 DCM으로 전환시킨다.
   EKF3에서 EKF2로의 전환은 되지 않는다.(EKF2에서 EKF1으로도 전환 없음)

.. warning::

   위에 파라미터를 이용하면 병렬로 5개까지 AHRS를 실행시킬 수 있다.(DCM x 1, EKF2 x 2, EKF3 x 2) 이렇게 되면 성능에 문제가 될 수 있다. EKFx의 계산량이 많으므로. 이 경우 IMU_MASK를 설정하여 실행되는 전체 core 수를 줄여야 한다.

Affinity and Lane Switching
----------------------------

EKF3는 sensor affinity 기능을 제공한다. 이는 EKF cores가 비주류 센서들(airspeed, barometer, compass, GPS 등)의 인스턴스를 사용할 수 있게 한다. 이렇게 하면 비행체는 좋은 품질의 센서 관리가 향상되고 가장 성능이 좋은 센서의 정보를 사용하기 위해서 lane 변경이 가능하다. 더 상세한 정보는 :ref:`EKF3 Affinity and Lane Switching <dev:ek3-affinity-lane-switching>`를 참고하자.

GPS / Non-GPS Transitions
-------------------------

EKF3는(ArduPilot 4.1 이상) 비행 중에 센서 전환을 지원한다. 따라서 GPS와 GPS가 동작하지 않는 환경 사이에 전환에 유용하게 사용할 수 있다.  자세한 내용은 :ref:`GPS / Non-GPS Transitions <common-non-gps-to-gps>`를 참고하자.

자주 수정하는 파라미터들(Commonly modified parameters)
----------------------------

:ref:`EK2_ALT_SOURCE <EK2_ALT_SOURCE>` 고도를 결정할 때 사용하는 센서 선택

-  0 : barometer 사용 (기본값)
-  1 : 거리센서 사용.  **지면이 평평한 실내환경에서만 이 옵션을 사용한다.**. 지형 추적은 :ref:`copter <terrain-following>` 와 :ref:`plane specific terrain following instructions <common-terrain-following>` 를 참고하며 이 파라미터는 변경할 필요가 없다.
-  2 : GPS 사용. GPS 상태가 아주 좋을 때 유용하고 barometer drift는 문제가 될 수 있다. 예제로 100m 이상의 고도 변화가 있는 장거리 미션을 수행하는 경우에 문제가 된다.

:ref:`EK2_ALT_M_NSE <dev:extended-kalman-filter_ekf_alt_noise>`: "1.0"이 기본값. 낮은 값일 수록 가속도센서에 대한 의존을 줄이고 barometer에 대한 의존을 높인다.

:ref:`EK2_GPS_TYPE <dev:extended-kalman-filter_ekf_gps_type>`:
GPS를 어떻게 사용할지 제어

-  0 : GPS로부터 3D 속도와 2D 위치 사용
-  1 : 2D 속도와 2D 위치에 사용 (GPS 속도는 고도 추정에 사용하지 않음)
-  2 : 2D 위치에 사용
-  3 : GPS 없음 (:ref:`optical flow <copter:common-optical-flow-sensors-landingpage>`가 있다면 optical flow만 사용)

:ref:`EK2_YAW_M_NSE <EK2_YAW_M_NSE>`: heading을 계산할 때 GPS와 컴파스 사이에 weight를 줘서 제어. 기본값은 "0.5"이고 낮은 값일 수록 컴파스에 의존한다.(즉 컴파스에 weight가 높아짐)
   
위에서 언급한 바와 같이 EKF 이론에 대한 상세한 개요와 튜닝 파라미터는 :ref:`Extended Kalman Filter Navigation Overview and Tuning <dev:extended-kalman-filter>`를 참고하자.
