.. _quadplane-building:

QuadPlane 만들기
====================

QuadPlane를 위한 일반적인 가이드와 설계 원칙에 대해서 알아보자. 만드는 경우 참고하면 도움이 될 것이다.

.. image:: ../images/quadplane-porter-octaquad.jpg
    :target: ../_images/quadplane-porter-octaquad.jpg

일반 원칙(General Rules)
-------------

다양한 고정익 비행체들은 VTOL 기능을 가지도록 변환할 수 있다. ArduPilot에서는 이런 비행체에 대해서 QuadPlane이라는 이름을 사용하지만 반드시 QuadCopter 모터 형태를 가져야만 하는 것은 아니다.
거의 대부분의 멀티콥터 모터 구조로 QuadPlane과 사용할 수 있다. 즉 quad, hex, tricopter, octa, octaquad가 여기에 포함된다. 추가로 VTOL과 고정익 비행에서 사용할 수 있는 tilt motor도 유효하다.
마지막으로 VTOL 운영을 위해서 수직 stance(nose vertical)를 사용하는 버전은 유효하다.

성공을 위한 핵심 요소 몇 가지 :

- 고정익 프레임은 추가된 모터와 배터리의 무게와 필요한 payload를 싣을 수 있어야 한다.

- 전체 비행체 무게뿐만 아니라 추가한 모터에 충분한 전원을 공급할 수 있어야하고 수평 VTOL stance를 사용하는 경우 날개의 downforce에서 생기는 load도 고려해야 한다.

- lifting 모터의 전체 disk 영역 위/아래로 완전히 비어있어야 한다.

- 모터는 항상 수직 thrust를 제공하기 위해 최소 날개, 프레임, 마운트 twist와 flex 고려.

- lifting 모터를 위해서 충분히 단단히 장착된 시스템으로 구성해야만 한다.

- lifting 모터와 프레임으로부터 에어로다이나믹 드래그를 최소화하기

  .. image:: ../images/quadplane-quadstar.jpg
    :target: ../_images/quadplane-quadstar.jpg

QuadPlane을 설계할때 `eCalc <http://ecalc.ch/>`__ 을 사용하여 모터, ESC, 배터리, 프로펠러 및 다른 부품을 선택하는 것을 추천한다. brushless 모터는 파워에 따른 무게 비율에 따라 많이 변한다.
충분한 lifting power를 공급하는 동안 weight를 유지하는 모터를 선택하는 것이 중요하다.

 .. Tip:: 무게와 표면 면적이 커지면 대부분 QuadPlanes은 작은 quadcopter에 비해서 robust YAW authority를 가지지 못한다. (만약에 tilt rotor가 vectored thrust를 사용하는 경우가 아니라면) 모터 위치가 아주 잘 맞춰지는게 가장 중요하다. 몇 도의 오차만 있더라도 가끔은 한 방향이나 양방향으로 yaw가 제대로 동작하지 않게 된다. 사실 이런 효과는 yaw authority를 증가시키기 위해서 사용한다. 1 혹은 2도 토크 방향으로 인접 회전 모터의 하나 혹은 2개 쌍을 고의로 tilt 시키는 방식이다. (H 프레임은 안쪽으로 X나 + 프레임은 바깥쪽)

QuadPlane Range
---------------

Perhaps surprisingly, it is sometimes possible to increase the
potential range of an aircraft using a QuadPlane conversion. This may
seem counter intuitive as a QuadPlane conversion will both add weight
and increase aerodynamic drag to an airframe.

The reason why range can be increased is the extra carrying capacity
of a QuadPlane. Many fixed wing aircraft are limited in the amount of
battery they can carry due to the requirements for reliable
launch. During launch, and especially when using a flying launch such
as a catapult or bungee, the aircraft needs to rapidly accelerate to
an airspeed above its stall speed. If it fails to reach that speed
suffiently quickly then it will crash. A QuadPlane avoids this problem
by taking off vertically, and can spend longer on the acceleration
needed to sufficient speed for forward flight.

This means it is often possible to pack a lot more battery into a
QuadPlane than is possible in the same airframe without VTOL
motors. The extra battery capacity can more than make up for the
increased weight and drag of the VTOL motors.

To make the most of this advantage you need to do very rapid VTOL
takeoffs and landings to minimise the battery consumption in VTOL
flight. The video below demonstrates just how rapid a takeoff can be
achieved with a properly setup quadplane.

..  youtube:: 4oVlSQaplZc
    :width: 100%
            
A second factor that can help with QuadPlane range is the flexibility
available in choosing the propeller and power train for the forward
motor. As conventional takeoff is not needed the forward motor does
not need to be optimised for the high level of thrust needed for
takeoff. This can allow larger propellers and geared motors to be used
that are highly efficient for forward cruise flight.

마지막으로 실제 장거리 QuadPlane의 경우 전진 모터에 대해서 내부 combustion 엔진을 사용할 수 있다. 가스 엔진은 동일 무게 연료에 대해서 전기 모터보다 훨씬 오래 실행된다. 

Build Logs
----------

몇 가지 QuadPlane를 만들었던 자료들이 있다. 이를 참고하면 도움이 될 것이다.

-  Porter OctaQuadPlane build:
   https://diydrones.com/profiles/blogs/building-flying-and-not-crashing-a-large-octaquadplane
-  Porter QuadPlane build:
   https://diydrones.com/profiles/blogs/building-flying-and-crashing-a-large-quadplane
-  The PerthUAV Mozzie build: http://mozzie.readthedocs.io/
-  Convergence conversion to ArduPlane Tilt Rotor Tricopter: https://discuss.ardupilot.org/t/e-flite-convergence-using-matek-f405-wing
-  C1 Chaser conversion to TVBS (Twin motor Vectored Belly Sitter, a Tailsitter that can takeoff and land from a plane stance): https://discuss.ardupilot.org/t/c1-chaser-tvbs-conversion-instructions
-  Foamboard Copter Tailsitter: https://discuss.ardupilot.org/t/box-kite-copter-tailsitter-build-instructions
