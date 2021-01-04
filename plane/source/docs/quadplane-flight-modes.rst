.. _quadplane-flight-modes:

QuadPlane 비행 모드 (QuadPlane Flight modes)
======================

QuadPlane 코드는 Plane master 펌웨어를 기반으로 하지만 6개의 추가 모드가 있다. 대부분은 Copter 모드와 동일하다:

.. toctree::
    :maxdepth: 1

    QSTABILIZE (Mode 17) <qstabilize-mode>
    QHOVER    (Mode 18) <qhover-mode>
    QLOITER   (Mode 19) <qloiter-mode>
    QLAND     (Mode 20) <qland-mode>
    QRTL      (Mode 21) <qrtl-mode>
    QAUTOTUNE (Mode 22) <qautotune-mode>
    QACRO     (Mode 23) <qacro-mode>
    AIRMODE** <airmode>

** 실제로는 flight mode가 아니라 QACRO와 QSTABILIZE의 기능이라고 볼 수도 있다.

.. tip::

   GCS가 이 값을 이해하지 못하는 경우 ``FLTMODE*`` 파라미터를 숫자 값으로 추가 모드를 설정할 수 있다.

동일한 Copter 비행 모드에 익숙하다면 QuadPlane로 비행이 쉽게 느껴질 수 있다.
고정익과 QuadPlane 사이의 가장 큰 차이점은 전이하는 동안에 있다. 이에 대한 설명은 :ref:`여기 <quadplane-flying>` 를 참고하자.

.. tip::

   Copter로부터 쓰로틀 채널 파라미터에도 차이점이 있다.: deadzone 설정을 위한 :ref:`THR_DZ<THR_DZ>` 대신에 QuadPlane은 RCn_DZ을(n은 쓰로틀 입력으로 매핑된 채널이다.) 사용한다. 쓰로틀 채널 deadzone을 위한 기본값은 60 (+/- 6%)이다. 만약 QuadPlane이 QSTABILIZE에서 중간 스틱 위치(+/- 6%)에서 호버링하지 않는 경우 :ref:`Q_M_THST_HOVER<Q_M_THST_HOVER>` 을 이용하여 중간을 맞춰야 한다. 중간을 맞추는 방법은 QSTABILIZE에서 스틱 중간 지점에 고도 변화가 없도록 쓰로틀 퍼센트를 적용한다. 이 값은 :ref:`Q_M_HOVER_LEARN<Q_M_HOVER_LEARN>` 를 활성화해서 자동으로 학습되고 QLOITER와 QHOVER 모드에서 적용된다.

.. tip::

   Since Quadplanes have much higher surface area than most Copters, hovering or loitering in a tail wind can be a challenge for stability. It is generally better to hover or loiter with the nose pointed into the wind, see :ref:`Weathervaning <quadplane-weathervaning>`.

.. note::

   The landing detection logic in QLOITER mode is not as sophisticated as the landing detection logic in Copter, so if you get GPS movement while on the ground in QLOITER mode then the aircraft may try to tip over as it tries to hold position while in contact with the ground. 
   It is suggested that you switch to QHOVER or QSTABILIZE once landed as these are unaffected by GPS movement.

Flight modes to avoid
---------------------

The linked nature of of the vertical lift and fixed wing control in quadplanes means the autopilot always needs to know what the pilot is trying to do in terms of speed and attitude. 
For this reason you should avoid the following flight modes in a quadplane:

-  ACRO
-  STABILIZE
-  TRAINING

these modes are problematic as the stick input from the pilot is not sufficient to tell the autopilot what attitude the aircraft wants or what climb rate is wanted, so the quadplane logic does not engage the quad motors when in these modes. 
These modes also make log analysis difficult. Please use FBWA mode instead of STABILIZE for manual flight.

In the future we may adds ways to use the quad motors in these modes, but for now please avoid them.

The other mode where the quad motors are disabled is MANUAL mode. 
That mode still can be useful for checking aircraft trim in fixed wing flight or for taxiing your aircraft.
