.. _quadplane-frame-setup:

=====================
QuadPlane 프레임 설정
=====================

QuadPlane 코드는 다양한 quadcopter, hexa, octa, octaquad 멀티콥터 프레임을 지원한다. 설정은 VTOL 모터가 transition이나 제어하는 동안 tilt되는지 여부, 수평 VTOL이나 수직 VTOL이냐에 설정.


Tailsitters
===========

Tailsitters 관련 프레임 설정은 :ref:`guide-tailsitter` 를 참조한다. 일단 프레임 구성이 완료되면 다른 :ref:`quadplane-setup` 를 진행해 나간다. 

Plan VTOL 자세(Plane VTOL Stance)
=================

이 설정은 멀티로터 스타일 lift 모터를 기존 고정익 설정에 추가한다. 이 모터들은 고정익에서 사용하는 tilt 모터로 구성할 수도 있다.

frame 타입과 분류(Frame Types and Classes)
-----------------------

다른 frame 타입을 사용하기 위해서 :ref:`Q_FRAME_CLASS<Q_FRAME_CLASS>` 와 :ref:`Q_FRAME_TYPE<Q_FRAME_TYPE>` 를 설정할 수 있다. :ref:`Q_FRAME_CLASS<Q_FRAME_CLASS>` 은 다음과 같이 설정할 수 있다.:

-  1 for quad
-  2 for hexa
-  3 for octa
-  4 for octaquad
-  5 for Y6
-  7 for Tri
-  10 for Tailsitter

이 frame 분류의 각 내부에서 :ref:`Q_FRAME_TYPE<Q_FRAME_TYPE>` 은 motor 레이아웃을 선택한다. Tri와 Y6을 위해서 이 파라미터는 무시된다.

-  0 for plus frame
-  1 for X frame
-  2 for V frame
-  3 for H frame
-  11 for FireFly6Y6 (for Y6 only)

모터 순서(Motor Ordering)
--------------

모터 순서와 출력 채널은 기본 출력 채널 번호가 1대신 5부터 시작한다는 것을 제외하고는 copter와 동일하다. (:ref:`Copter motor layout <copter:connect-escs-and-motors>` 참고)

.. note:: :ref:`guide-tailsitter` 설정은 특별한 경우이다. Tailsitter는 아래를 참고하자.

예제로 기본 Quad-X 프레임 내부에서 모터 출력은 5에서 8이다.  다음과 같은 순서를 따른다 :

-  **Output 5:** Front right motor, counter-clockwise
-  **Output 6:** Rear left motor, counter-clockwise
-  **Output 7:** Front left motor, clockwise
-  **Output 8:** Rear right motor, clockwise

H 설정을 제외하고 "motor는 기체 방향으로 회전한다"는 CW/CCW 법칙을 명심하자. 모든 방향은 invert된다.

다른 공통 셋업은 octa-quad이다. 다름 순서를 따라 사용한다.

-  **Output 5:** Front right top motor, counter-clockwise
-  **Output 6:** Front left top motor, clockwise
-  **Output 7:** Rear left top motor, counter-clockwise
-  **Output 8:** Rear right top motor, clockwise
-  **Output 9:** Front left bottom motor, counter-clockwise
-  **Output 10:** Front right bottom motor, clockwise
-  **Output 11:** Rear right bottom motor, counter-clockwise
-  **Output 12:** Rear left bottom motor, clockwise

octa-quad에 대해서는 "top motor는 기체 방향으로 회전하고 bottom motor는 기체로부터 먼 방향으로 회전한다"는 CW/CCW 법칙을 명심하자. 

일반 plane 출력은 1 ~ 4를 사용한다. 오직 수직 lift 출력만(quad 설정시 5 ~ 8 사용) high PWM rate(400Hz)로 실행된다. 일반 Plan 코드에서 원한다면 quad 설정에서 9 ~ 14 출력도 사용할 수 있다.

아래 섹션에서 소개하는 절차를 이용하면 quad motor를 4 이상의 다른 채널로 옮기는 것도 가능하다. 

Tricopter
---------

Frame Type 7은 Tricopter이다. Vectored이냐 Non-Vectored yaw 제어냐에 따라서 :ref:`Tiltrotor<guide-tilt-rotor>` 설정과 비 Tiltrotor로 설정할 수 있다.
non-Tiltrotor 혹은 Non-Vectored Yaw Tilt-rotor를 사용한다면 yaw 제어 출력은 Motor 7(``SERVOn_FUNCTION`` = 39)로 설정되고 yaw 모터에 대한 tilt 매커니즘은 Motor 4로 설정된다. yaw servo의 최대 lean angle을 :ref:`Q_M_YAW_SV_ANGLE<Q_M_YAW_SV_ANGLE>` 각도로 설정해야만 한다. 이 lean angle은 ``SERVOn_MIN`` 와 ``SERVOn_MAX``는 +/- 90도를 ``SERVOn_TRIM``는 0 도 lean을 표현한다.


Tilt-Rotors
===========

참고 :ref:`guide-tilt-rotor`

다른 채널 매핑 사용하기(Using different channel mappings)
================================

lifting motors의 output 채널은 SERVOn_FUNCTION에 대한 값을 설정해서 remap한다. 이것은 :ref:`other output functions <common-rcoutput-mapping>` 과 동일한 접근법을 따른다.

.. note::
   비표준 motor 순서, vectored thrust, Tailsitter인 경우에 SERVOn_FUNCTION 값 설정이 필요하다. motor 표준 순서를 따르는 것을 추천하며 SERVOn_FUNCTION 파라미터를 설정하지 말고 0으로 남겨둔다. 부팅시에 frame을 위한 올바른 값을 자동으로 설정할 것이다.

output function numbers :

-  33: motor1
-  34: motor2
-  35: motor3
-  36: motor4
-  37: motor5
-  38: motor6
-  39: motor7
-  40: motor8

quad motor를 outputs 9 ~ 12로(Pixhawk에 auxillary channels) 설정하려면 advanced parameter list에서 아래 값들을 설정해야한다. :

-  :ref:`SERVO9_FUNCTION<SERVO9_FUNCTION>` = 33
-  :ref:`SERVO10_FUNCTION<SERVO10_FUNCTION>` = 34
-  :ref:`SERVO11_FUNCTION<SERVO11_FUNCTION>` = 35
-  :ref:`SERVO12_FUNCTION<SERVO12_FUNCTION>` = 36ㅌㅍ1

