.. _apmcopter-programming-libraries:

===================
ArduPilot 라이브러리
===================

\ `libraries <https://github.com/ArduPilot/ardupilot/tree/master/libraries>`__ 는 Copter, Plane, Rover가 공유한다. 아래는 라이브러리와 그 기능에 대한 목록이다.

**핵심 libraries:**

-  `AP_AHRS <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_AHRS>`__ -
   DCM이나 EKF를 이용한 자세 추정
-  `AP_Common <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Common>`__ -
   모든 sketches와 라이브러리에서 필요한 것을 포함
-  `AP_Math <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Math>`__ -
   vector 처리에 특히 유용한 여러 math 함수
-  `AC_PID <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AC_PID>`__ -
   PID(Proportional-Integral-Derivative) controller library
-  `AP_InertialNav <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_InertialNav>`__ -
   가속도 입력을 gps와 baro와 함께 섞어서 inertial navigation 라이브러리
-  `AC_AttitudeControl <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AC_AttitudeControl>`__ -
   ArduCopter 제어 라이브러리로 PID 제어를 기반으로 자세, 위치, 제어의 여러 기능을 포함하고 있다.
   
-  `AC_WPNav <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AC_WPNav>`__
   - waypoint navigation library
-  `AP_Motors <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Motors>`__
   - 멀티콥터와 전통 헬리콥터 모터 믹싱
-  `RC_Channel <https://github.com/ArduPilot/ardupilot/tree/master/libraries/RC_Channel>`__ -
   pwm 입력/출력을 APM_RC로부터 내부 unit으로(예로 각도) 변환하는 라이브러리
-  `AP_HAL <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_HAL>`__,
   `AP_HAL_ChibiOS <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_HAL_ChibiOS>`__,
   `AP_HAL_Linux <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_HAL_Linux>`__
   - HAL을 구현하는 라이브러리. 다른 보드들에 쉽게 포팅이 되도록 한다.

**Sensor libraries:**

-  `AP_InertialSensor <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_InertialSensor>`__ -
   gyro와 가속도 데이터를 읽어서 calibration을 수행하고 메인 코드와 다른 라이브러리들에게 표준 단위로(deg/s, m/s) data를 제공한다.
-  `AP_RangeFinder <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_RangeFinder>`__ -
   sonar and ir distance sensor interfaced library
-  `AP_Baro <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Baro>`__ -
   barometer interface library
-  `AP_GPS <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_GPS>`__ -
   gps interface library
-  `AP_Compass <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Compass>`__ -
   3-axis compass interface library
-  `AP_OpticalFlow <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_OpticalFlow>`__ -
   optical flow sensor interface library

**Other libraries:**

-  `AP_Mount <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Mount>`__, \ `AP_Camera <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Camera>`__, \ `AP_Relay <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Relay>`__ -
   camera mount control library, camera shutter control libraries
-  `AP_Mission <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Mission>`__
   - stores/retrieves mission commands from eeprom
-  `AP_Buffer <https://github.com/ArduPilot/ardupilot/tree/master/libraries/AP_Buffer>`__ -
   a simple FIFO buffer for use with inertial navigation
