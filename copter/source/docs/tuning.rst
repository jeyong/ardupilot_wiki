.. _tuning:

===============
고급 튜닝(Advanced Tuning)
===============

파라미터 튜닝 방법에 대해서 알아보자.

개요
========

기본 `PID gains <https://en.wikipedia.org/wiki/PID_controller>`__ 은 3DR IRIS 에 맞춰져 있다. 하지만 비슷한 형태의 다양한 프레임에서도 잘 동작한다. 최적의 성능을 얻기 위해서는 가지고 있는 기체에 맞게 파라미터 조정이 필요하다. *미션 플래너*의 **Config/Tuning \| Copter Pids** 화면에서 조정할 수 있다. 아래 화면은 Roll/Pitch(노란색)과 Yaw(오렌지색), :ref:`Altitude hold <altholdmode>` (녹색),
:ref:`Loiter <loiter-mode>` (핑크색) and
:ref:`Waypoint navigation <auto-mode>`
(파랑색)에 대한 가장 중요한 파라미터를 보여준다.

.. image:: ../images/Tuning_CommonThingsToChange.png
    :target: ../_images/Tuning_CommonThingsToChange.png

가장 중요한 파라미터는 Rate Roll P이다.(Rate Pitch P는 기본적으로 동일한 값으로 설정되어 있다.) 관련된 논의는 :ref:`here <ac_rollpitchtuning>` 를 참고하자.

일반적으로 스테빌라이즈 모드에서 Rate Roll/Pitch P를 튜닝하는 것이 가장 좋다. 다음으로 AltHold 모드에서 altitude를 튜닝하고 다음으로 Loiter (튜닝 하지 않는 경우가 많음)그리고 마지막으로 Auto 모드에서 waypoint 네비게이션 성능을 튜닝합니다. 


.. note::

   `Dave C's AC2.8.1 튜닝 가이드 <https://diydrones.com/forum/topics/arducopter-tuning-guide>`__ 에 rate roll과 pitch 튜닝에 대해서 참고하기 좋다. 하지만 altitude hold, Loiter, 네비게이션은 AC2.8.1 이후로 아주 많이 변경되어서 추천하지 않는다.

Roll/Pitch tuning
=================

스테빌라이즈 Roll/Pitch와 Rate Roll/Pitch 파라미터는 위 이미지에서 노란색으로 표시되어 있으며 roll pitch 반응성을 제어한다.

desired rotation rate를 모터 출력으로 변환하는 Rate 파라미터가 가장 중요하다. :ref:`Rate Roll와 Pitch P 튜닝 페이지 <ac_rollpitchtuning>` 가 가장 중요한 정보이다.

스테빌라이즈 Roll/Pitch P가 desired angle을 desired rotation rate로 변환되며 이 값이 Rate Controller로 입력된다.

-  값이 커질 수록 roll/pitch 입력에 대한 반응성이 높아지고 값이 작을 수록 느리게 반응한다.
-  너무 높게 설정하면 비행체는 roll이나 pitch 축이 진동하게 될 수도 있다.
-  너누 낮게 설정하면 비행체는 조정에 대해서 느리게 반응할 수 있다.

roll과 pitch에 대한 튜닝에 관련된 정보는 :ref:`Stabilize mode page's Tuning section <stabilize-mode_tuning>` 를 참고하자.

전반적인 Roll과 Pitch 성능의 관점에서는 the :ref:`dataflash log's <common-downloading-and-analyzing-data-logs-in-mission-planner>` ATT 메시지의 Roll-In vs Roll 그리고 Pitch-in vs Pitch에 대한 그래프로 확인할 수 있다. 스테빌라이즈나 AltHold 모드에서 "Roll" (실제 roll 값)은 "Roll-In"와 거의 비슷하게 나와야 한다. Pitch는 Pitch-In과 유사하게 비슷하게 나와야 한다.

다른 대안으로는 :ref:`AutoTune feature <autotune>` 를 사용해서 rate와 stabilize(예로 각도) 파라미터 모두를 튜닝되기를 원할수도 있다.

Yaw tuning
==========

위 화면에서 오렌지색으로 표시된 스테빌라이즈 Yaw와 Rate Yaw 파라미터는 yaw의 반응속도를 제어한다. yaw를 많이 튜닝하는 것은 드물다.

roll이나 pitch와 유사하게 스테빌라이저 Yaw P나 Rate Yaw P는 너무 높으면 비행체의 heading이 진동한다. 만약 이 값이 너무 낮으면 비행체는 heading을 유지하지 못할 수도 있다.

:ref:`Stabilize mode's tuning section <stabilize-mode_tuning>` 에서 언급한 바와 같이 ACRO_YAW_P 파라미터는 조정사의 yaw 스틱 입력에 대해서 얼마나 빨리 비행체가 회전하는지를 제어한다. 기본값인 4.5는 yaw 스틱을 완전히 왼쪽이나 오른쪽으로 움직이는 경우 200deg/sec rate로 회전하는 명령이다. 더 높은 값을 주면 더 빠르게 비행체가 회전하게 된다.

Altitude Tuning
===============

위에 화면에서 녹색으로 표시된 부분이 고도(Altitude)와 관련된 튜닝 파라미터들이다. 

Altitude Hold P는 altitude error를(desired altitude와 실제 altitude의 차이) desired climb나 descent rate로 변환하는데 사용된다. 더 높은 rate인 경우 altitude를 유지하기 하는데 좀더 적극적이지만 너무 높으면 쓰로틀 입력에 대해서 떨리는 현상이 발생할 수 있다.

Throttle Rate(일반적으로 튜닝이 필요없음)는 desired climb나 descent rate를 desired acceleration 위 혹은 아래로 변환한다.

Throttle Accel PID gains은 acceleration error(desired acceleration과 실제 accelration의 차이)를 모터 입력으로 변환한다. 이 파라미터들을 수정한다면 P와 I의 비율이 1:2 (I가 P의 2배)를 유지해야만 한다. 이 값들은 절대로 증가되면 안된다. 하지만 매우 힘이 좋은 비행체에 대해서는 반응성을 향상시키기 위해서 50%까지 줄일 수 있다.(P에 0.5, I 1.0)

:ref:`Altitude Hold 비행 모드 페이지 <altholdmode>` 에서 자세한 내용은 확인하자.

Loiter Tuning
=============

일반적으로 Roll과 Pitch가 제대로 튜닝되어 있다면 :ref:`GPS <common-diagnosing-problems-using-logs_gps_glitches>` 와 :ref:`compass <common-diagnosing-problems-using-logs_compass_interference>` 는 셋업하고 성능이 좋고 :ref:`vibration levels <common-diagnosing-problems-using-logs_vibrations>` 가 괜찮은 수준이 된다. Loiter는 튜닝이 많이 필요하지는 않지만 수평 속도를 포함한 튜닝 가능한 파라미터에 관련된 내용은 :ref:`Loiter Mode <loiter-mode_tuning>`을 참조하자.

비행 중 튜닝(In-flight tuning)
================

자세한 내용은 :ref:`Transmitter based tuning<common-transmitter-tuning>` 을 참고하자.

필터링 튜닝(Filter tuning)
=============

비행체는 진동에 영향을 자주 받으므로 사용 가능한 여러 SW 필터를 튜닝하는 것이 전반적으로 튜닝 성능을 달성하는데 아주 중요하다. 사용 가능한 notch filters를 튜닝하는 방법에 대한 가이드는 :ref:`Notch Filtering wiki page <common-imu-notch-filtering>` 에서 참조할 수 있다.

PID에 대한 소개 비디오
==========================

PIDs (Proportional - Integral - Derivative) 는 연속적으로 비행체를 안정화 시키기 위해서 펌웨어로 사용하는 방법

-  Proportional = 즉각 보정: error에 비례해서 보정
-  Integral = 긴 시간동안 꾸준한 상태 보정 : 시간이 지나도 안잡히는 경우 보정
-  Derivative = 쉬운 보정 : overshoot이 되지 않도록 댐핑 역할

..  youtube:: l03SioQ9ySg
    :width: 100%

..  youtube:: sDd4VOpOnnA
    :width: 100%

-----

.. image:: ../../../images/banner-freespace.png
   :target: https://freespace.solutions/
