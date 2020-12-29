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

가장 중요한 파라미터는 Rate Roll P이다.(Rate Pitch P는 기본적으로 동일한 값으로 설정되어 있다.) 관련된 논의는 :ref:`here <ac_rollpitchtuning>`를 참고하자.

일반적으로 스테빌라이즈 모드에서 Rate Roll/Pitch P를 튜닝하는 것이 가장 좋다. 다음으로 AltHold 모드에서 altitude를 튜닝하고 다음으로 Loiter (튜닝 하지 않는 경우가 많음)그리고 마지막으로 Auto 모드에서 waypoint 네비게이션 성능을 튜닝합니다. 


.. note::

   `Dave C's AC2.8.1 튜닝 가이드 <https://diydrones.com/forum/topics/arducopter-tuning-guide>`__ 에 rate roll과 pitch 튜닝에 대해서 참고하기 좋다. 하지만 altitude hold, Loiter, 네비게이션은 AC2.8.1 이후로 아주 많이 변경되어서 추천하지 않는다.

Roll/Pitch tuning
=================

스테빌라이즈 Roll/Pitch와 Rate Roll/Pitch 파라미터는 위 이미지에서 노란색으로 표시되어 있으며 roll pitch 반응성을 제어한다.

desired rotation rate를 모터 출력으로 변환하는 Rate 파라미터가 가장 중요하다. :ref:`Rate Roll와 Pitch P 튜닝 페이지 <ac_rollpitchtuning>`가 가장 중요한 정보이다.

The Stabilize Roll/Pitch P converts the desired angle into a desired
rotation rate which is then fed to the Rate controller.

-  A higher value will make the copter more responsive to roll/pitch
   inputs, a lower value will make it smoother
-  If set too high, the copter will oscillate on the roll and/or pitch
   axis
-  If set too low the copter will become sluggish to inputs

More information on tuning the roll and pitch can be found on the
:ref:`Stabilize mode page's Tuning section <stabilize-mode_tuning>`.

An objective view of the overall Roll and Pitch performance can be seen
by graphing the :ref:`dataflash log's <common-downloading-and-analyzing-data-logs-in-mission-planner>`
ATT message's Roll-In vs Roll and Pitch-In vs Pitch. The "Roll" (i.e.
actual roll) should closely follow the "Roll-In" while in Stabilize or
AltHold modes. Pitch should similarly closely follow Pitch-In.

Alternatively you may wish to try tuning both the rate and stabilize
(i.e. angular) parameters using the :ref:`AutoTune feature <autotune>`.

Yaw tuning
==========

The Stabilize Yaw and Rate Yaw parameters, highlighted in orange in the
screen shot above control the yaw response. It's rare that the yaw
requires much tuning.

Similar to roll and pitch if either Stabilize Yaw P or Rate Yaw P is too
high the copter's heading will oscillate. If they are too low the copter
may be unable to maintain its heading.

As mentioned on the :ref:`Stabilize mode's tuning section <stabilize-mode_tuning>`,
the ACRO_YAW_P parameter controls how quickly copter rotates based on
a pilot’s yaw input.  The default of 4.5 commands a 200 deg/sec rate of
rotation when the yaw stick is held fully left or right.  Higher values
will make it rotate more quickly.

Altitude Tuning
===============

The Altitude hold related tuning parameters are highlighted in green in
the screen shot above.

The Altitude Hold P is used to convert the altitude error (the
difference between the desired altitude and the actual altitude) to a
desired climb or descent rate.  A higher rate will make it more
aggressively attempt to maintain its altitude but if set too high leads
to a jerky throttle response.

The Throttle Rate (which normally requires no tuning) converts the
desired climb or descent rate into a desired acceleration up or down.

The Throttle Accel PID gains convert the acceleration error (i.e the
difference between the desired acceleration and the actual acceleration)
into a motor output.  The 1:2 ratio of P to I (i.e. I is twice the size
of P) should be maintained if you modify these parameters.  These values
should never be increased but for very powerful copters you may get
better response by reducing both by 50% (i.e P to 0.5, I to 1.0).

See the :ref:`Altitude Hold flight mode page <altholdmode>` for more information.

Loiter Tuning
=============

Generally if Roll and Pitch are tuned correctly,  the
:ref:`GPS <common-diagnosing-problems-using-logs_gps_glitches>`
and :ref:`compass <common-diagnosing-problems-using-logs_compass_interference>`
are set-up and performing well and :ref:`vibration levels <common-diagnosing-problems-using-logs_vibrations>`
are acceptable, Loiter does not require much tuning but please see the
:ref:`Loiter Mode <loiter-mode_tuning>` page for more details on tunable
parameters including the horizontal speed.

In-flight tuning
================

See the :ref:`Transmitter based tuning<common-transmitter-tuning>` page for details.

Filter tuning
=============

Copters are often affected by vibration and tuning the various software filters available is critical to achieving an overall tune.
A guide on tuning the various notch filters available can be found on the :ref:`Notch Filtering wiki page <common-imu-notch-filtering>`.

Video introduction to PIDs
==========================

PIDs (Proportional - Integral - Derivative) are the method used by our
firmware to continuously stabilize the vehicle

-  Proportional = Immediate Correction: The further off you are the
   bigger the correction you make.
-  Integral = Over time or steady state correction: If we are failing to
   make progress add additional correction.
-  Derivative = Take it Easy correction: Is the correction going to
   fast? if it is slow it down (dampen) it a bit to avoid overshoot.

..  youtube:: l03SioQ9ySg
    :width: 100%

..  youtube:: sDd4VOpOnnA
    :width: 100%

-----

.. image:: ../../../images/banner-freespace.png
   :target: https://freespace.solutions/
