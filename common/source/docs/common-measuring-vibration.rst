.. _common-measuring-vibration:

===================
진동 측정(Measuring Vibration)
===================

[copywiki destination="copter,plane,rover,dev,planner"]

비행제어기는 가속도 센서가 장착되어 있는데 이 센서는 진동에 민감하다. 이 가속도 센서 값은 barometer와 GPS 데이터를 합쳐서 위치를 추정하는데 사용한다. 과도한 진동이 발생하는 경우 추정이 어려워지고 정확한 위치를 기반으로 하는 비행 모드에서는 성능이 떨어지게 된다. : (비행 모드들 : AltHold, Loiter, RTL, Guided, Position and Auto flight modes)

여기서는 진동 정도를 측정하는 방법에 대해서 설명한다. 만약 임계 진동 이상이 발생하는 경우에는 :ref:`Vibration Damping <common-vibration-damping>` 를 참고하자.

실시간 관측 (미션 플래너)
--------------------------------

미션 플래너는 진동과 clippng을 실시간으로 볼 수 있다. 미션 플래너에서 HUD에서 "Vibe"을 클릭하면 현재 진동 수준을 표시한다.

   .. image:: ../../../images/vibration-realtime-mp.png
       :target: ../_images/vibration-realtime-mp.png
       :width: 450px

진동 수준이 30m/s/s 이하라면 정상이다. 30m/s/s를 넘으면 문제가 발생할 수 있으며 60m/s/s 를 넘으면 position이나 altitude hold에서 거의 항상 문제가 발생한다.

Vibe Dataflash Log message
--------------------------

기록된 Vibe 수준이 대부분 30m/s/s 이하연지 확인한다.

-  적어도 몇 분간 일반적인 비행을 수행하고 :ref:`dataflash logs를 다운받자 <common-downloading-and-analyzing-data-logs-in-mission-planner_downloading_logs_via_mavlink>`
-  미션 플래너를 사용해서 VibeX, VibeY와 VibeZ를 그래프를 확인한다. 이 값들은 m/s/s 단위로 가속도 센서의 출력의 표준편차를 보여준다. 아래 이미지는 3DR IRIS에서 수집한 데이터로 15m/s/s 이하로 일반적인 수준이다. 하지만 때때로 30m/s/s까지 올라가기도 한다. 용인되는 최대 값은 30m/s/s 아래로 보인다. (아래 2번째 사진)
-  Clip0, Clip1, Clip2 값을 그래프로 확인한다. 매번 가속도 센서 중에 하나가 최대 제한값에 도달한다.(16G) 이상적으로는  이러한 것들은 전체 비행에 대해서 0여야 한다. 하지만 착륙하는 동안에 낮은 숫자는(<100) 특별히 별 문제가 없다. 해당 log에서 정기적으로 값이 증가되는 것은 심각한 진동이 있다는 것을 뜻이므로 이 문제를. :ref:`해결해야 한다 <common-vibration-damping>`

   .. image:: ../../../images/mp_vibe_dataflash_msg.png
       :target: ../_images/mp_vibe_dataflash_msg.png
       :width: 450px

비행체에 진동이 높아서 위치 추정에 문제가 있다는 것을 보여주는 예제이다.

.. image:: ../../../images/mp_measuring_vibration_bad_vibes.png
    :target: ../_images/mp_measuring_vibration_bad_vibes.png
    :width: 450px

진동 수준을 계산하기 위한 알고리즘은 `AP_InertialSensor.cpp's calc_vibration_and_clipping() <https://github.com/ArduPilot/ardupilot/blob/master/libraries/AP_InertialSensor/AP_InertialSensor.cpp#L1609>`__ 에서 볼 수 있다. 하지만 간단히 말하자면 다음과 같이 가속도 센서에서 읽은 값의 표준편차와 관련이 있다.:

-  IMU 센서로부터 가속도 raw x, y, z을 살펴본다.
-  비행체의 움직임을 제거할려면 5hz 속도로 raw 값을 high-pass 필터를 적용하고 x, y, z축에 대해서는 "accel_vibe_floor"를 생성한다.
-  가장 최근의 가속도 값과 accel_vibe_floor 사이 차이값을 계산한다.
-  위에서 구한 차이값을 제곱한다. 2hz로 필터링하고 난 후에 제곱근을(x, y, z에 대해서) 구한다. 이 마지막 3개 값은 VIBE msg의 VibeX, Y, Z 필드에 나타나게 된다.

Looking for "The Leans"
-----------------------

**The Leans**은 비행체의 자세 추정이 제대로 되지 않은 때에 발생한다. 이런 일이 발생하는 이유는 조종사가 수평 비행 명령을 주고 있더라도 심각하게 기울어진 상태가 되기 때문이다. 이 문제의 원인은 가끔은 가속도 센서의 노이즈로 각 추정 시스템(각 AHRSs 혹은 EFKs)으로부터 Roll과 Pitch 자세 추정을 비교해서 확인할 수 있다. 자세 추정값은 서로 몇 도 이내 오차만 허용한다.

- dataflash log를 다운받고 log viwer를 통해서 열어보자.
- AHRS2.Roll, NKF1[0].Roll과 NKF1[1].Roll을 비교한다.

아래 이미지는 전형적인 log로 자세값이 매칭이 잘되어 있는 경우다.

.. image:: ../../../images/vibration-measuring-leans.png
    :target: ../_images/vibration-measuring-leans.png
    :width: 450px

FFT로 고급 분석하기 (Advanced Analysis with FFT)
--------------------------

많은 IMU 데이터를 수집하는 방법과 진동의 주파수를 파악하기 위해 FFT 분석을 수행하는 방법에 대해서 :ref:`IMU 배치 샘플러를 이용한 진동 측정 <common-imu-batchsampling>` 를 참조하자.

IMU Dataflash Log message
-------------------------

ArduPilot의 아주 예전 버전에는 Vibe 메시지가 포함되어 있지 않아서 IMU 값을 직접 확인해야 했다.

-  :ref:`LOG_BITMASK <LOG_BITMASK>` 파라미터는 IMU 데이터가 포함되도록 설정해야만 가속도 값이 dataflash logs에 저장된다.
-  스테빌라이저 모드에서 비행하고 수평 호버링을 유지하도록 한다.(여기서 완전히 안정적이거나 수평이 될 필요는 없다.)
-  :ref:`dataflash logs 다운받기 <common-downloading-and-analyzing-data-logs-in-mission-planner_downloading_logs_via_mavlink>` 은 후에 미션 플래너의 "Review a Log" 버튼을 사용하여 log 디펙토리에 있는 최신 파일을 열도록 한다. (마지막 숫자가 Log number로 Log #1인 파일은 마지막 파일명이 1.log가 된다.)
-  Log 브라우저가 나타나면 IMU 메시지를 찾을때까지 스크롤을 내린다. row의 AccX를 클릭하고 **Graph this data** 왼쪽 버튼을 클릭한다. AccY와 AccZ 칼럼에 대해서 아래와 같이 그래프를 생성하기 위해서 되풀이 한다.

   |DiagnosingWithLogs_Vibes|

-  왼쪽에 해당 스케일을 확인하고 AccX와 AccY에 대한 진동 수준이 -3 ~ +3 사이 값인지 확인한다. AccZ의 수용 가능 범위는 -15 ~ -5이다. 여기에 매우 가까운 값이거나 제한을 넘어가는 경우에는 :ref:`Vibration Damping <common-vibration-damping>` 를 참고해서 가능한 해결책을 찾아야한다.
-  위에 과정 모두가 완료되면 미션 플래너의 표준 파라미터 페이지로 이동한다. (**Connect** 버튼을 다리 눌러야 진입될 수도 있다.) 그리고 Log Bitmask을 다시 "Default"로 설정한다. APM 로깅은 CPU 자원을 많이 사용하기 때문에 더이상 필요없는 경우에는 기본값으로 돌려서 낭비를 줄인다.

.. |DiagnosingWithLogs_Vibes| image:: ../../../images/DiagnosingWithLogs_Vibes.png
    :target: ../_images/DiagnosingWithLogs_Vibes.png

