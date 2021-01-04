.. _guide-tilt-rotor:

=================
Tilt Rotor Planes
=================

ArduPilot에서 titl-rotor는 QuadPlane의 일종으로 취급된다. titl-rotor 문서를 읽기 전에 먼저 :ref:`QuadPlane
documentation <quadplane-support>` 읽도록 하자.

ArduPilot에서 명명법은 tilt-rotor은 VTOL 비행체의 일종이고 호버링과 전진 비행 사이의 전환은 1개 이상의 rotor를 틸팅하여 수행할 수 있다. 따라서 윗방향 thrust가 아니라 전진 thrust가 된다.

.. note:: :ref:`tailsitters <guide-tailsitter>` 와 구별되는 점은 비행제어기와 메인 기체가 호버링과 전진 비행시에 방향이 전환되느냐 전환되지 않느냐는 것이다. 아래 설정을 Tailsitter용으로 사용하지 말자. 일부 파라미터는 공유되지만 tailsitter 관련해서는 :ref:`guide-tailsitter` 를 참고하자. 


Tilt-Rotors의 타입(Types of Tilt-Rotors)
====================

ArduPilot은 다양한 tilt-rotor 설정을 지원한다. 공통 설정은 다음과 같은 내용을 포함하고 있다 :

- tilt-quadplanes : 전방 2개 모터 틸팅
- tilt-quadplanes : 4개 모든 모터 틸팅
- tilt-tricopters : 전방 2개 모터 틸팅 및 yaw를 위해서 rear tilt
- tilt-tricopters : 전방 2개 모터 틸팅 및 vectored yaw
- tilt-hexacopters : 전방 4개 모터 틸팅
- tilt-wings : main 날개가 틸팅되어 2개 모터도 따라서 틸팅이 된다.
- binary-tiltrotors : 틸트 매커니즘이 발생하는 지점은 2개 position 중에 하나가 될 수 있다.
- continuous-tiltrotors : 틸트 매커니즘은 직선 up방향부터 직선 전방까지 어떤 각도로든 제어될 수 있다. 
- vectored tilt-rotors : 왼쪽에 rotor의 tilt는 오른쪽 motor의 tilt로부터 독립적으로 제어될 수 있다. 

이런 변종들은 고정익 비행을 위해서 ailerons, elevons, vtail, 다른 control surface를 사용한다. 여기에는 엄청난 수의 조합이 가능하고 VTOL 설계로 실험이 일반적이다. ArduPilot은 적은 수의 파라미터로 여러 tilt-rotor 설정이 가능하게 하는 것을 목표로하고 있다.

Tilt-Rotor 셋업하기(Setting Up A Tilt-Rotor)
=======================

가장 먼저 해야할 것은 QuadPlane 지원을 활성화 시키기 위해서 :ref:`Q_ENABLE<Q_ENABLE>` 을 1로 설정한다. 다음으로 quadplane frame class와 frame type을 선택한다.

:ref:`Q_FRAME_CLASS<Q_FRAME_CLASS>` 에서 quadplane frame class를 설정한다. frame class는 만드는 호버링하는 동안 비행체의 rotor 설정을 기반으로 선택한다. 현재 지원하는 tilt-rotor frame class는 다음과 같다 :

.. raw:: html

   <table border="1" class="docutils">
   <tr><th>Frame Class</th><th>Q_FRAME_CLASS</th></th></tr>
   <tr><td>Quadcopter</td><td>1</td></tr>
   <tr><td>Hexacopter</td><td>2</td></tr>
   <tr><td>Octacopter</td><td>3</td></tr>
   <tr><td>Octaquad</td><td>4</td></tr>
   <tr><td>Y6</td><td>5</td></tr>
   <tr><td>Tricopter</td><td>7</td></tr>
   </table>

.. note: BiCopter tilt-rotor QuadPlane는 특별한 케이스다. :ref:`Q_FRAME_CLASS<Q_FRAME_CLASS>` 이 10으로 설정되어야 한다. Tailsitter 설정을 위해 고정되어 있다. 아래 :ref:`BiCopter <bicopter>` 를 참고하자.

일단 frame class를 선택하면 :ref:`Q_FRAME_TYPE<Q_FRAME_TYPE>` 를 설정해야 한다. :ref:`Q_FRAME_TYPE<Q_FRAME_TYPE>` 은 frame의 sub-type이다. 예제로 Quadcopter에 대해서 frame type이 1이면 "X" frame이고 frame type이 3이면 "H" frame이다. Tri와 Y6에 대해서 이 파라미터는 무시된다.

멀티콥터의 frame type을 선택에 대해서 좀더 상세한 정보는 ArduCopter 셋업 가이드를 참고하자. 

:ref:`Q_ENABLE<Q_ENABLE>`, :ref:`Q_FRAME_CLASS<Q_FRAME_CLASS>`, :ref:`Q_FRAME_TYPE<Q_FRAME_TYPE>` 셋업한 후에는 reboot해야 한다.

The Tilt Mask
=============

:ref:`Q_TILT_MASK<Q_TILT_MASK>` 에서 tilt-rotor를 위한 가장 중요한 파라미터는 tilt-mask이다.

:ref:`Q_TILT_MASK<Q_TILT_MASK>` 는 비행체에 있는 어떤 motor가 tilt되는지에 대한 bitmask이다. 선택한 frame class와 frame type을 위한 표준 ArduCopter motor map의 motor 순서에 대응하는 bit를 활성화시킬 수 있다.

예제로 전면에 2개 motor tilt인 tilt-tricopter가 있다면 :ref:`Q_TILT_MASK<Q_TILT_MASK>` 을 3으로 설정한다. 2+1 이기 때문이다.

모두 4개 motor tilt인 tilt-quadplane을 가지고 있다면  :ref:`Q_TILT_MASK<Q_TILT_MASK>` 는 15로 설정해야 한다. 8+4+2+1 이기 때문이다.

Tilt 타입(The Tilt Type)
=============

대부분의 tilt-rotors는 rotor를 틸팅하기 위해서 일반 servo를 사용한다. 이렇게 되면 Pixhawk는 직선 up방향에서 직선 전방 범위내에서 연속으로 tilt의 각도를 제어 가능하다.

일부 tilt-rotors는 대신에 바이너리 매커니즘을 가지고 있다. 일반적으로 retract servo를 사용해서 Pixhawk는 해당 servo에 완전 up 혹은 완전 전방 중에 하나의 명령을 줄 수 있다. 하지만 틸트가 이 사이에 어떤 각도로 멈춰있게 할 수는 없다.

마지막으로 일부 tilt-rotors는 yaw의 vectored control을 가진다. 이렇게 하면 right rotor의 독립적으로 left rotor를 틸팅해서 yaw를 제어할 수 있다.

:ref:`Q_TILT_TYPE<Q_TILT_TYPE>` 파라미터를 사용해서 titl 타입을 설정해야한다.:

.. raw:: html

   <table border="1" class="docutils">
   <tr><th>Tilt Type</th><th>Q_TILT_TYPE</th></tr>
   <tr><td>Continuous</td><td>0</td></tr>
   <tr><td>Binary</td><td>1</td></tr>
   <tr><td>Vectored</td><td>2</td></tr>
   <tr><td>BiCopter</td><td>3</td></tr>
   </table>


Tilt Servos
===========

어떤 servo 출력으로 틸트가능한 rotor의 틸팅을 설정이 필요하다.

3개의 servo 기능 값으로 제어할 수 있다.

.. raw:: html

   <table border="1" class="docutils">
   <tr><th>Tilt Control</th><th>SERVOn_FUNCTION</th></tr>
   <tr><td>Motor tilt</td><td>41</td></tr>
   <tr><td>Left Motor tilt</td><td>75</td></tr>
   <tr><td>Right Motor tilt</td><td>76</td></tr>
   </table>

vectored yaw aircraft를 설정하지 않으면 일반 ``Motor tilt`` 을 선택해야만 하고 :ref:`Q_TILT_TYPE<Q_TILT_TYPE>` 는 2로 설정한다.

예제로 단일 servo로 servo output 11에 연결된 rotors를 틸팅하고자 한다면 :ref:`SERVO11_FUNCTION<SERVO11_FUNCTION>` 를 41로 설정해야만 한다.

Tilt 연전환 및 범위(Tilt Reversal and Range)
=======================

servo의 방향에 따라서 tilt servo에 ``SERVOn_REVERSED`` 파라미터를 설정이 필요할 수 있다. MANUAL 모드에서 rotor가 앞으로 tilt되고 QSTABILIZE 모드에서 up 방향으로 tilt되게 하기 위해서 조정을 해야만 한다.

움직임 범위를 조정과 각 servo의 전방 비행및 호버링을 위해서 정확한 각도를 조정하기 위해서 SERVOn_MIN 와 SERVOn_MAX 조정이 필요할 수 있다. 

Tilt Angle
==========

:ref:`Q_TILT_MAX<Q_TILT_MAX>` 파라미터는 연속 틸팅을 위해서 전이되는 동안 tilt angle을 제어한다. transition airspeed에 도달하기 위해서 기다리는 동안 rotor가 이동하는 각도가 바로 이 각도가 된다.

:ref:`Q_TILT_MAX<Q_TILT_MAX>` 를 위한 올바른 값은 날개가 양력을 받기 위해서 충분한 airspeed 달성을 위한 틸팅 정도에 따라 달라진다. 대부분의 tilt-rotors에 대해서 기본값은 45도면 괜찮다.

Tilt Rate
=========

tilt rotors를 위한 중요한 파라미터는 호버링과 전진 비행 사이의 전이 동안 얼마나 빠르게 tilt servo를 움직이느냐이다.

tilt rate를 제어하는 2개 파라미터는:

- :ref:`Q_TILT_RATE_UP<Q_TILT_RATE_UP>` 는 초당 upward 방향으로 tilt rate 각도이다.
- :ref:`Q_TILT_RATE_DN<Q_TILT_RATE_DN>` 는 초당 downward 방향으로 tilt rate 각도이다.

:ref:`Q_TILT_RATE_DN<Q_TILT_RATE_DN>` 가 0이면 :ref:`Q_TILT_RATE_UP<Q_TILT_RATE_UP>` 는 양방향에 사용된다.

얼마나 빠르게 tilt servo를 움직여야 하는지는 factor의 수에 의존한다. 특별히 멀티로터 비행에서는 얼마나 잘 비행체가 튜닝되었는지가 중요하다. 일반적으로 초기 테스팅에서 천천히 전이 되는 쪽이 error이며 다음으로 천천히 필요에 따라서 속도를 높여준다.

up과 down에 대해서 일반적인 값은 초당 15도이다. 

주의 : ArduPilot tilt-rotor code에서 tilt rate에 자동 예외처리가 있다.:

- MANUAL 모드로 변경될때 tilt rate은 초당 90도이다. 이렇게 하면 MANUAL 모드가 필요한 경우에 빠른 전면 비행 제어가 가능해진다.

- 일단 전면 전이가 완료되면 다음으로 motor는 남은 각도에 대해서 초당 90도 속도로 움직인다.

.. note:: Binary 타입 tilt servo에 대해서 이런 rate는 servo의 실ㅈ 측정 rate로 설정해야한다. 왜냐하면 이런 동작은 ArduPilot 제어와는 관련이 없기 때문이다. 즉 servo 자체가 그렇게 동작하는 것이기 때문이다.

Vectored Yaw
============

vectored yaw 비행체는 호버링에서 yaw를 제어하기 위해서 왼쪽과 오른쪽 rotor를 각각 틸트시킨다. 이렇게 하면 yaw 제어에 필요한 rear motor에 대한 tilt servo를 위한 필요성이 없어지므로 tilt-tricopters에서 기계적인 복잡도가 줄어든다.

vectored yaw 비행체를 셋업하기 위해서 :ref:`Q_TILT_TYPE<Q_TILT_TYPE>` =2 로 설정하고 :ref:`Q_TILT_YAW_ANGLE<Q_TILT_YAW_ANGLE>` 는 tilt motor가 90도 up할 수 있는 각도로 설정한다.

예제에서 vectored yaw의 tilt-tricopter가 있다면 motor는 전진 비행으로부터 전체 110 도 tilt할 수 있다. :ref:`Q_TILT_YAW_ANGLE<Q_TILT_YAW_ANGLE>` 는 20이 되고 ilt 매커니즘이 진행된 90도에서 벗어난 각도가 된다. 

left tilt을 위해서 ``SERVOn_FUNCTION`` =75, right tilt를 위해서 ``SERVOn_FUNCTION`` =76로 2개 tilt servo를 셋업하자.

Non-Vectored Yaw
================

Non-Vectored yaw 비행체 (:ref:`Q_TILT_TYPE<Q_TILT_TYPE>` = 0 or 1)는 yaw 제어를 위해서 tilt servo가 필요하다.

만약 frame이 Tricopter라면 전면 tilt servo를 ``SERVOn_FUNCTION=41``로 설정하고 ``SERVOn_FUNCTION=39``로 yaw 제어를 설정한다. :ref:`Q_M_YAW_SV_ANGLE<Q_M_YAW_SV_ANGLE>` 로 yaw servo의 최대 lean 각도를 셋업해야만 한다. 이 lean angle은 ``SERVOn_MIN`` 과 ``SERVOn_MAX`` 는 +/- 90도를 나타내고 ``SERVOn_TRIM`` 는 0 도 lean을 뜻한다.

Note:
``SERVO_FUNCTION=39`` 는 일반적으로 motor 7에 대한 출력 함수이지만 non-vectored yaw tilt-rotor Tricopter에서 yaw servo는 ``SERVOn_FUNCTION`` = 39로 제어된다.

BLEHeli esc 텔레메트리를 셋업하고자 한다면, :ref:`Q_M_PWM_TYPE<Q_M_PWM_TYPE>` 를 4(DShot 150)으로 설정하고 텔레메트리 신호를 SERIAL port에 연결하고 ``SERIALn_PROTOCOL`` 를 23으로 설정한다.

만약 BLHeli passthru 셋업 혹은 non-vecotred yaw Tricopter에서 텔레메트리를 사용하고자 한다면 :ref:`SERVO_BLH_AUTO<SERVO_BLH_AUTO>` 을 1로 설정하지 말아야 한다. 대신에 :ref:`SERVO_BLH_MASK<SERVO_BLH_MASK>` 을 실제로 BLHELI-ESCs에 연결된 servo-channel의 output-bitmask로 설정한다.

예제로 motor가 servo 9, 10, 11에 연결된다면(Pixhawk1의 처음 3개 aux-outputs), :ref:`SERVO_BLH_MASK<SERVO_BLH_MASK>` 를 1792로 설정한다.

.. _bicopter:

BiCopter Tilt-Rotor
===================

tilt-rotor QuadPlane의 특별한 케이스이다. 셋업은 약간 다르지만 설정은 실제 일반 QuadPlane이며 QuadPlane 전이를 수행한다. 이 비행체를 셋업하기 위한 설정:

- :ref:`Q_FRAME_CLASS<Q_FRAME_CLASS>` = 10 (Tailsitter, 비록 tailsitter는 아니지만!)
- :ref:`Q_TILT_TYPE<Q_TILT_TYPE>` = 3 (BiCopter)

모터와 Tilt 셋업(Motor and Tilt Setup)
--------------------

모터와 tilt servo에 대해서 왼쪽/오른쪽 모터를 위한 2개 titl servo와 왼쪽/오른쪽 모터 쓰로틀에 대해서 SERVOn_FUNCTION 값을 설정해야만 한다.

tilt servo limits은 다른 Tilt Rotors와는 약간 다르게 셋업한다. 셋업하기 위해서 일반 tilt rotor 범위에 대해서 :ref:`Q_TILT_YAW_ANGLE<Q_TILT_YAW_ANGLE>` 를 설정하고 QSTABLIZE에서의 수직과 MANUAL에서 수평을 얻기 위해서 tilt servo의 MIN/MAX 출력 범위를 설정한다. TRIM 값은 무시된다.

이 frame type으로 tilt servo의 MIN은 수평 position을 설정한다. 이 경우 사용자는 수직ㅇ서 forward yaw의 양에 대해서 :ref:`Q_TILT_YAW_ANGLE<Q_TILT_YAW_ANGLE>` 을 설정해야만 한다. (한 방향으로 비대칭 yaw authority를 방지하기 위해서 rearward angleㅇ르 매치시켜야만 한다.)

.. note:: tilt servo가 적당한 방향을 얻기 위해서 reverse되어야 한다면 MIN과 MAX는 서로 바꿔치기 된다.

반면에 이 frame type은 일반 vectored yaw tilt-rotor, QuadPlane 전이, 파라미터를 확인해야만 한다.

Tilt Rotor 이동 셋업(Tilt Rotor Movement Setup)
=========================

.. toctree::
    :maxdepth: 1

    Tilt Rotor Setup Tips<tilt-rotor-tips>

Also:
    :ref:`Tilt Rotor Servo Setup<tilt-rotor-setup>`



Pre Flight Checks
=================

QuadPlane의 경우 일반 pre-flight 체크에서 추가로 MANUAL과 QSTABLIZE 모드 사이에 변경시켜서 tilt-rotor 전이를 검사해야만 한다. tilt는 부드럽게 움직이고 servo는 올바른 rotor 각도에 대해서 제대로 trim되는지 확인한다.
