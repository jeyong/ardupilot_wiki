.. _sprayer:

============
Crop Sprayer
============

Copter에는 crop sprayer 지원 기능이 들어가 있다. 이 기능은 Pixhawk에 PWM으로 동작하는 펌프와 스피너(옵션)가 연결되어 있고 비행체의 속도 기반으로 액체 분무 속도를 제어할 수 있다.

..  youtube:: O8ZnxkXMv6A
    :width: 100%

스프레이 기능을 사용하여 드론을 보여주는데 좀 오래되긴 했다. (2:25로 가면 sprayer의 동작을 볼 수 있다.)

.. note:: 
flash 메모리 용량이 1MB인 Pixhawk는 이 기능을 사용할 수 없다. 용량에 관련된 내용은 :ref:`Firmware Limitations <common-limited_firmware>` 를 참고하자.

필요한 HW
=================

   .. image:: ../images/sprayer_EnRoute_AC940D.jpg
       :target: https://enroute.co.jp/products/ac940d/

`EnRoute AC 940-D <https://enroute.co.jp/products/ac940d/>`_ 멀티콥터는 PWM으로 펌프를 제어하고 옵션으로 스피닝을 PWM으로 제어한다. (EnRoute 비행체는 이런 이차 스피너 제어가 필요없다.)

펌프로 분무되는 속도를 제어한다.

옵션으로 장착한 스피너는 스프레이 노즐의 끝에 장착하고 농약이나 비료를 더 넓게 뿌릴 수 있게 된다.

Sprayer 활성화
====================

-  Mission Planner에 Pixhawk를 연결한다.
-  :ref:`SPRAY_ENABLE <SPRAY_ENABLE>` 파라미터를 1로 서렂앟고 파리미터를 refresh한다. ( sprayer는 일반적으로 사용하는 기능이 아니라 초기에 드러나지 않는다.)
-  펌프를 Pixhawk의 추가 PWM 출력 중에 하나로 연결하고 적절한 SERVO*_FUNCTION 혹은 RC*_FUNCTION을 22로 설정한다. ("*"는 RC 출력 숫자로 펌프가 Pixhawk의 AUX1으로 연결되면 :ref:`SERVO9_FUNCTION <SERVO9_FUNCTION>` 를 2로 설정한다.)
-  옵션 spinner를 다른 aux 출력에 연결하고 SERVO*_FUNCTION 혹은 RC*_FUNCTION 를 23으로 설정한다. (Pixhawk의 AUX2를 사용한다면 옵션 spinner를 다른 aux 출력 포트에 연결하고 SERVO*_FUNCTION 혹은 RC*_FUNCTION를 23으로 설정한다.(만약 Pixhawk AUX2를 사용한다면  :ref:`SERVO10_FUNCTION <SERVO10_FUNCTION>` 를 23으로 설정한다.)
-  조종사가 sprayer를 켜고/끄기를 허용하려면 sprayer는 aux 스위치를 설정한다. (예제로 Copter-4.0 이전 버전에서는 CH7_OPT 혹은 Copter-4.0 이상인 경우 ``RCx_OPTIONS`` 를 "15"로 설정한다.

.. note::

   Copter 3.5 이상에서 RCx_FUNCTION 파라미터는 SERVOx_FUNCTION으로 rename하는데 이는 RC 입력과 의미를 분리시키는게 더 낫기 때문이다.

펌프 설정
====================
-  펌프와 스피너를 제어하는데 사용하는 PWM 범위는 펌프와 스피너에 연결된 pwm 출력 채널에 대해서 SERVO*_MIN/RC*_MIN, SERVO*_MAX/RC*_MAX 파라미터를 설정함으로써 설정할 수 있다.
-  :ref:`SPRAY_PUMP_MIN <SPRAY_PUMP_MIN>` 는 최소 펌프 속도(퍼센트로 표현)를 제어한다. 기본적으로 0%가 의미하는 것은 비행체가 멈추면 펌프가 완전히 멈추게 된다.
-  :ref:`SPRAY_PUMP_RATE <SPRAY_PUMP_RATE>` 는 비행체가 1m/s로 비행할때의 펌프 속도(퍼센트로 표현)를 제어한다. 기본 값은 10%이다. 펌프 속도는 펌프가 10m/s에서 100%도달할때까지 비행체 속도와 linear하게 증가한다.
-  :ref:`SPRAY_SPINNER <SPRAY_SPINNER>` 펌프가 켜져있을때 스피너로 보내는 펌프 값을 설정한다.
-  :ref:`SPRAY_SPEED_MIN <SPRAY_SPEED_MIN>` 펌프가 동작할때 최소 비행체 속도를 설정한다. 기본값은 100이고 이 의미는 비행체가 1m/s로 비행할때 펌프가 구동된다는 의미다.

