.. _quadplane-esc-calibration:

ESC calibration
===============

PWM 기반의 대부분 ESC들은 동일한 입력에 대해서 동일한 속도로 반응하도록 칼리브레이션이 필요하다.
ESC를 칼리브레이션하기 위해서 초기 전원을 줄때 최대 PWM 입력을 받고 다음으로 최소 PWM 입력을 받는다. beep음이 최대값이 등록되었다는 시점에 최소 PWM을 받는다.

ESC를 칼리브레이션하는 방법은 ArduPilot의 버전에 따라 달라진다. 예전 방식은 3.6.0 이전 버전이고 3.6.0 이후가 새로운 방식이다.

.. warning::
   ESC 칼리브레이션을 시작하기 전에 비행체의 모든 프로펠러를 제거한다. 프로펠러가 장착된 상태로 칼리브레이션하는 것은 위험한다.

ESC 칼리브레이션 절차 (ESC Calibration Procedure (3.6.0 and later))
-------------------------------------------

:ref:`Q_ESC_CAL <Q_ESC_CAL>` 파라미터를 이용해서 QSTABILIZE 모드에서 ESC 칼리브레이션을 활성화시킨다. 가능한 2가지 모드가 있다.:

#. Q\_ESC_CAL=1 : 모터에 대한 출력은 비행체가 arming되었을때 QSTABILIZE 모드에서 쓰로틀 스틱으로부터 직접 오게 한다.
#. Q\_ESC_CAL=2 : 모터에 대한 출력은 모터가 arming되었을때 full 쓰로틀이 된다.

Q\_ESC_CAL=1을 사용하는 절차는

#. remove your propellers for safety
#. power up just the flight board and not your motors. If you don't have
   the ability to isolate power to the ESCs when on battery power then
   power up your flight board on USB power
#. set the Q\_ESC_CAL parameter to 1
#. change to QSTABILIZE mode
#. set the safety switch off to activate the outputs
#. arm your aircraft. The PWM output on all quad motors will now be
   controlled by your throttle stick
#. move the throttle stick to maximum
#. add power to your ESCs by connecting the battery
#. wait for the ESCs to beep to indicate they have registered the
   maximum PWM
#. lower the throttle stick to zero and disarm your aircraft
#. you should hear a beep from your ESCs to indicate they have
   registered the throttle range

The process when using Q\_ESC\_CAL=2 is

#. remove your propellers for safety
#. power up just the flight board and not your motors. If you don't have
   the ability to isolate power to the ESCs when on battery power then
   power up your flight board on USB power
#. set the Q\_ESC_CAL parameter to 2
#. change to QSTABILIZE mode
#. set the safety switch off to activate the outputs
#. arm your aircraft. The PWM output on all quad motors will now be at maximum
#. add power to your ESCs by connecting the battery
#. wait for the ESCs to beep to indicate they have registered the
   maximum PWM
#. disarm your aircraft
#. you should hear a beep from your ESCs to indicate they have
   registered the throttle range

Note that using Q\_ESC_CAL=1 can be useful for testing your motors
response. This is the only mode when you are able to directly control
the throttle level on all your motors at once. While in this mode you
can use a laser tachometer to test your motor speeds at different
throttle levels if you have one.

.. warning:: Be sure to set :ref:`Q_ESC_CAL<Q_ESC_CAL>` back to zero after calibrating for normal operation


Old ESC Calibration Procedure (3.5.3 and earlier)
-------------------------------------------------

#. remove your propellers for safety
#. power up just the flight board and not your motors. If you don't have
   the ability to isolate power to the ESCs when on battery power then
   power up your flight board on USB power
#. set both the parameters :ref:`Q_M_SPIN_ARM<Q_M_SPIN_ARM>` and Q_THR_MID to 1000.
   This sets the PWM output when armed at zero throttle to full power
#. set the safety switch off to activate the outputs
#. arm your aircraft. The PWM output on all quad motors will now climb
   to maximum.
#. add power to your ESCs by connecting the battery
#. wait for the ESCs to beep to indicate they have registered the
   maximum PWM
#. disarm your aircraft. The ESCs should beep again indicating they have
   registered minimum PWM

Now set the :ref:`Q_M_SPIN_ARM<Q_M_SPIN_ARM>` and Q_THR_MID parameters back to the
correct values. A value of 50 for :ref:`Q_M_SPIN_ARM<Q_M_SPIN_ARM>` is a reasonable
starting point. For Q_THR_MID a value of between 500 and 600 is good
depending on the power of your motors

