.. _tuning-process-instructions:

===========================
튜닝 과정(Tuning Process Instructions)
===========================

비행체를 튜닝 가능한 상태로 설정하기(Setting the aircraft up ready for tuning)
----------------------------------------

자신의 비행체의 사양에 맞게 아래의 파라미터를 바르게 설정해야만 한다.

배터리 설정(Battery setting)
^^^^^^^^^^^^^^^
모터 thrust 커브를 선형으로 증가시키기 위한 파라미터

- :ref:`MOT_BAT_VOLT_MAX <MOT_BAT_VOLT_MAX>` : 4.2v x No. Cells
- :ref:`MOT_BAT_VOLT_MIN <MOT_BAT_VOLT_MIN>` : 3.3v x No. Cells
- :ref:`MOT_THST_EXPO <MOT_THST_EXPO>` : 5인치 프로펠러인 경우 0.55, 10인치 프로펠러인 경우 0.65, 20인치 프로펠러인 경우 0.75. 이 프라미터는 최적의 결과를 얻기 위해서 thrust 표준 측정에서 얻어야만 한다.(제조사의 데이터를 믿지마라!)

.. image:: ../images/tuning-process-instructions-1.hires.png
    :target: ../_images/tuning-process-instructions-1.hires.png

모터 셋업(Motors setup)
^^^^^^^^^^^^
ESC에 전달되는 출력 범위를 정의하는데 사용하는 파라미터

- :ref:`MOT_PWM_MAX <MOT_PWM_MAX>` : 고정 범위를 위해서 ESC 메뉴얼 확인하거나 2000us
- :ref:`MOT_PWM_MIN <MOT_PWM_MIN>` : 고정 범위를 위해서 ESC 메뉴얼 확인하거나 1000us
- :ref:`MOT_SPIN_ARM <MOT_SPIN_ARM>` : :ref:`motor 테스트 기능 <connect-escs-and-motors_testing_motor_spin_directions>` 사용
- :ref:`MOT_SPIN_MAX <MOT_SPIN_MAX>` : 0.95
- :ref:`MOT_SPIN_MIN <MOT_SPIN_MIN>` : :ref:`motor 테스트 기능 <connect-escs-and-motors_testing_motor_spin_directions>` 사용
- :ref:`MOT_THST_HOVER <MOT_THST_HOVER>` : 0.25 혹은 기대한 hover thrust 퍼센트 아래 (낮은 값이 안전하다)

PID 제어기 초기화 셋업
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- :ref:`INS_ACCEL_FILTER <INS_ACCEL_FILTER>` :  10Hz to 20Hz
- :ref:`INS_GYRO_FILTER <INS_GYRO_FILTER>` : 5인치 프로펠러인 경우 80Hz, 10인치 프로펠러인 경우 40Hz, 20인치 프로펠러인 경우 20Hz
- :ref:`ATC_ACCEL_P_MAX <ATC_ACCEL_P_MAX>` : 10인치 프로펠러인 경우 110000, 20인치 프로펠러인 경우 50000, 30인치 프로펠러인 경우 20000
- :ref:`ATC_ACCEL_R_MAX <ATC_ACCEL_R_MAX>` : 110000 for 10 inch props, 50000 for 20 inch props, 20000 for 30 inch props
- :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>` : 27000 for 10 inch props, 18000 for 20 inch props, 9000 for 30 inch props
- :ref:`ACRO_YAW_P <ACRO_YAW_P>` : 0.5 x :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>` / 4500

Copter-4.0 이후 버전:

- :ref:`ATC_RAT_PIT_FLTD <ATC_RAT_PIT_FLTD__AC_AttitudeControl_Multi>` : :ref:`INS_GYRO_FILTER <INS_GYRO_FILTER>` / 2
- :ref:`ATC_RAT_PIT_FLTT <ATC_RAT_PIT_FLTT__AC_AttitudeControl_Multi>` : :ref:`INS_GYRO_FILTER <INS_GYRO_FILTER>` / 2
- :ref:`ATC_RAT_RLL_FLTD <ATC_RAT_RLL_FLTD__AC_AttitudeControl_Multi>` : :ref:`INS_GYRO_FILTER <INS_GYRO_FILTER>` / 2
- :ref:`ATC_RAT_RLL_FLTT <ATC_RAT_RLL_FLTT__AC_AttitudeControl_Multi>` : :ref:`INS_GYRO_FILTER <INS_GYRO_FILTER>` / 2
- :ref:`ATC_RAT_YAW_FLTE <ATC_RAT_YAW_FLTE__AC_AttitudeControl_Multi>` : 2
- :ref:`ATC_RAT_YAW_FLTT <ATC_RAT_YAW_FLTT__AC_AttitudeControl_Multi>` : :ref:`INS_GYRO_FILTER <INS_GYRO_FILTER>` / 2

Copter-3.6 이전 버전:

- ``ATC_RAT_PIT_FILT`` : :ref:`INS_GYRO_FILTER <INS_GYRO_FILTER>` / 2
- ``ATC_RAT_RLL_FILT`` : :ref:`INS_GYRO_FILTER <INS_GYRO_FILTER>` / 2
- ``ATC_RAT_YAW_FILT`` : 2

.. image:: ../images/tuning-process-instructions-2.hires.png
    :target: ../_images/tuning-process-instructions-2.hires.png

.. image:: ../images/tuning-process-instructions-3.hires.png
    :target: ../_images/tuning-process-instructions-3.hires.png

.. image:: ../images/tuning-process-instructions-4.hires.png
    :target: ../_images/tuning-process-instructions-4.hires.png

비행체의 초기 튜닝은 비행체에서 가장 간편하게 설정이 완료되어야만 한다. 이것이 일반적으로 의미하는 바는 완충된 배터리를 가지고 최소 이륙 무게인 상태를 말한다.

처음 비행을 위해 조정사가 준비할 것들(Pilot's preparation for first flight)
------------------------------------

튜닝을 하지 않은 비행체를 처음으로 이륙시킬 때가 가장 위험한 시기다. 비행체에 갑자기 전원 공급이 늘어나면 비행체는 매우 불안전한 상태가 되어 갑자기 공중으로 튀어 오르게 될 수 있다. 그리고 제대로 튜닝이 안된 상태라면 비행체 제어하기가 어려운 상태이다. 조정사는 비행체 손상이나 사람이 다칠 수 있는 상황이 발생하지 않도록 튜닝 비행을 하는 동안은 아주 충실히 임해야만 한다.

조종사가 초기 튜닝 단계에서 위험을 최소화시키기 위해서 할 수 있는 몇 가지 일에 대해서 소개한다. :

1. 조종사는 모터 번호 확인 및 방향 검사를 해야만 한다.(참고 :ref:`미션 프래너 모터 테스트를 이용한 모터 번호 확인 <connect-escs-and-motors_testing_motor_spin_directions>`). Care should be taken to ensure that the correct frame type is selected. Incorrect frame type can result in a very fast yaw rotation or complete loss of control. Take note of the output percentage required to spin the propellers and ensure that:

- :ref:`MOT_SPIN_ARM <MOT_SPIN_ARM>` is set high enough to spin the motors cleanly.
- :ref:`MOT_SPIN_MIN <MOT_SPIN_MIN>` is set high enough to spin the motors win a minimal level of thrust.

2. 중요 튜닝 파리미터를 변경한 후에는 비행을 하는 경우 항상 스테빌라이저 모드로 비행해야만 한다. 스테빌라이저 모드는 자세 제어가 불안정한 경우가 발생하면 조종사가 비행체에 대한 제어할 수 있도록 허용하기 떄문이다.
3. 조종사는 고도 제어기를 테스트하기 전까지 AltHold 모드에서 비행체를 이륙시키지 말아야 한다. 테스트는 스테빌라이저 모드로 이륙시켜서 Alt Hold로 전환하는 방법으로 테스트한다. 매우 낮은 hover 쓰로틀이 아닌 경우라면 Alt Hold는 거의 문제가 되지 않는다.
4. 초기 비행 동안에 조종사는 아래 파라미터들이 설정되었는지 확인해야만 한다. :

- :ref:`ATC_THR_MIX_MAN <ATC_THR_MIX_MAN>` 가 0.1로 설정
- :ref:`MOT_THST_HOVER <MOT_THST_HOVER>` 가 0.25로 설정 (혹은 예상하는 hover 쓰로틀보다 낮게)

5. 조정기를 사용하는 경우 조정기를 제대로 칼리브레이션이 되었는지 확인 (참고 :ref:`common-radio-control-calibration`).
6. 모터 긴급 정지 스위치 설정 및 테스트 하기 ( 참고 :ref:`Auxiliary Functions <common-auxiliary-functions>`).
7. 바람이 불지 않는 보통 날씨 환경에서 튜닝 비행을 시도한다. (맑은 날 온도는 15°C/59°F ~ 25°C/77°F).
8. 시뮬레이터로 스테빌라이저 비행을 연습하거나 저가 드론으로 먼저 연습한다. 튜닝이 안된 비행체로 이착륙을 할 수 있는 자신감이 생길떄까지 연습한다.


첫 비행
------------

첫 이륙 비행이 가장 위험하다. 따라서 비행을 시작하고 처음 몇 초 만에 비행체가 부서지지 않도록 주의하고 주변에 사람들이 다치지 않도록 주의한다.

- **주변에 구경하는 사람들은 안정 거리를 확보해야 한다.**.
- **조종사는 안전거리와 위치를 확보해야 한다.**.
- 조종사는 기체를 모터 interlock으로 disarm 시키는 방법에 대해서 알고 Arm/Disarm이 나은 방법인지 판단할 수 있어야 한다.

이번 비행은 "튜닝을 위해서 비행 가능한" 상태에서 비행체를 셋업하는 방법이다.

1. 비행체는 스테빌라이저 비행 모드인지 확인한다.
2. 비행체를 arming한다.
3. disarm 절차가 제대로 알고 있는지 확인하기 위해서 즉시 비행체를 disarm 시켜본다.
4. 비행체를 arming한다.
5. 비행체에 진동이 발생하는지 찾아보기 위해서 천천히 쓰로틀을 증가시킨다. (랜딩 기어가 길고 유연한 소재인 경우 랜딩 기어 진동을 발생시킬 수 있다. 이륙을 하고 나서야 랜딩 기어 진동 영향을 받지 않게 된다.)
6. 비행체가 지상으로 이륙하자마자 즉시 천천히 착륙 시킨다.
7. 비행체를 disarm 시킨다.
8. 관찰한 결과로 파라미터 튜닝이 필요한지 아니면 다시 이륙시켜도 괜찮은지 판단한다.
9. arming하고 쓰로틀을 올려서 이륙시킨다.
10. 대략 1m 고도에서 호버링을 시킨 상태로 조정기의 roll/pitch로 약 5도 정도만 약간씩 움직여본다.
11. 만약 비행체 진동이 발생하면 바로 착륙 시킨다.

다음 섹션에서는 비행체 진동을 제거하는 방법에 대해서 설명한다.

초기 비행체 튜닝
---------------------

비행체를 튜닝할때 가장 높은 우선순위는 비행체가 안정적 비행하도록 튜닝하고 진동이 없어야 한다. 이를 위해서 다음과 같은 테스트를 수행한다.

1. 스테빌라이즈 모드에서 arming하기
2. 비행체가 땅위에서 뜨도록 천천히 쓰로틀을 증가시킨다.
3. 이때 비행체가 바로 진행이 감지된다면 이륙시키지 말고 바로 착륙시킨다.
4. 아래 모든 파라미터 값을 50% 감소시킨다.

a. :ref:`ATC_RAT_PIT_P <ATC_RAT_PIT_P__AC_AttitudeControl_Multi>`
b. :ref:`ATC_RAT_PIT_I <ATC_RAT_PIT_I__AC_AttitudeControl_Multi>`
c. :ref:`ATC_RAT_PIT_D <ATC_RAT_PIT_D__AC_AttitudeControl_Multi>`
d. :ref:`ATC_RAT_RLL_P <ATC_RAT_RLL_P__AC_AttitudeControl_Multi>`
e. :ref:`ATC_RAT_RLL_I <ATC_RAT_RLL_I__AC_AttitudeControl_Multi>`
f. :ref:`ATC_RAT_RLL_D <ATC_RAT_RLL_D__AC_AttitudeControl_Multi>`

호버링 하는 동안 비행체가 진동하지 않는 것처럼 보일때까지 이 과정을 반복한다.

만약 비행체가 아주 길고 유연한 landing gear를 가지고 있다면 ground resonance가 멈추기 전에 ...
If the aircraft has very long or flexible landing gear then you may need to leave the ground before ground resonance stops.

Be aware that in this state the aircraft may be very slow to respond to large control inputs and disturbances. The pilot should be extremely careful to put minimal stick inputs into the aircraft to avoid the possibility of a crash.

Test AltHold
-------------

This test will allow to test the altitude controller and ensure the stability of your aircraft.

1. Check :ref:`MOT_HOVER_LEARN <MOT_HOVER_LEARN>` is set to 2. This will allow the controller to learn by itself the correct hover value when flying.

2. Take off in STABILIZE and increase altitude to 5m. Switch to AltHold and be ready to switch back to STABILIZE. If the aircraft is hovering at a very low hover throttle value you may hear a reasonably fast oscillation in the motors. Ensure the aircraft has spent at least 30 seconds in hover to let the hover throttle parameter converge to the correct value. Land and disarm the aircraft.

3. Set these parameters on ground and preferably disarm  (A confident pilot could set them in flight with GCS or CH6 tuning knob):

  - :ref:`PSC_ACCZ_I <PSC_ACCZ_I>` to 2 x :ref:`MOT_THST_HOVER <MOT_THST_HOVER>`
  - :ref:`PSC_ACCZ_P <PSC_ACCZ_P>` to :ref:`MOT_THST_HOVER <MOT_THST_HOVER>`

AltHold starts to move up and down the position and velocity controllers may need to be reduced by 50%. These values are: :ref:`PSC_POSZ_P <PSC_POSZ_P>` and :ref:`PSC_VELZ_P <PSC_VELZ_P>`.

.. _evaluating-the-aircraft-tune:

Evaluating the aircraft tune
----------------------------

Most pilots will look to move to Autotune as quickly as possible once their aircraft can hover safely in AltHold. Before Autotune is run the pilot should ensure that the current tune is good enough to recover from the repeated tests run by Autotune. To test the current state of tune:

1. Take off in AltHold or STABILIZE
2. Apply small roll and pitch inputs. Start with 5 degree inputs and releasing the stick to centre, pitch, left, right, roll forward back, then all 4 points on the diagonal
3. Increase inputs gradually to full stick deflection
4. Go to full stick deflection and letting the sticks spring back to centre

If the aircraft begins to overshoot significantly or oscillate after the stick input, halt the tests before the situation begins to endanger the aircraft. The aircraft may require manual tuning (:ref:`see next section <ac_rollpitchtuning>`) before autotune can be run.

To test the stabilization loops independent of the input shaping, set the parameter: :ref:`ATC_RATE_FF_ENAB <ATC_RATE_FF_ENAB>` to 0.

1. Take off in AltHold or STABILIZE
2. Hold a roll or pitch input
3. Release the stick and observe the overshoot as the aircraft levels itself
4. Gradually increase the stick deflection to 100%

Halt the tests if the aircraft overshoots level significantly or if the aircraft oscillates, the aircraft may require manual tuning (:ref:`see next section <ac_rollpitchtuning>`) before autotune can be run.

Set :ref:`ATC_RATE_FF_ENAB <ATC_RATE_FF_ENAB>` to 1 after the tests are complete.

수동으로 Roll과 Pitch 튜닝하기
-------------------------------

Autotune을 실행하기 전에 수동 튜닝으로 어느 정도 안정적인 비행이 가능한 상태를 제공해야 한다. 혹은 Autotune이 제대로 동작지 않는 경우에 필요하다. 아래 절차는 대칭인 비행체가 주어진 경우 빠르게 수동으로 튜닝을 위해서 roll과 pitch를 동시에 튜닝할 수 있다. 만약 비행체가 대칭이 아니라면 roll과 pitch 각각에 대해서 튜닝 절차를 수행해야만 한다.

조정사는 수동 튜닝을 시작하기 전에 특별히 :ref:`ATC_THR_MIX_MAN <ATC_THR_MIX_MAN>` 와 :ref:`MOT_THST_HOVER <MOT_THST_HOVER>`가 제대로 설정되어 있는지 확인해야만 한다.

비행체 진동이 시작될때, 조정기로 큰 입력이나 갑잡스러운 조정을 하지 않도록 한다. 비행체의 위치를 제어하기 위해서 roll과 pitch 조정 스틱을 천천히 그리고 조금씩만 움직여서 비행체를 천천히 착륙시킨다.

1. 비행체 진동 발생이 관측될때까지 D term을 50%씩 증가시킨다.
2. 비행체 진동 발생이 관측되지 않을때까지 D term을 10%씩 줄인다.
3. D term을 25% 더 줄인다.
4. 비행체 진동 발생이 관측될때까지 50%씩 P term을 증가시킨다.
5. 비행체 진동 발생이 관측되지 않을때까지 P term을 10%씩 줄인다.
6. P term을 25% 더 줄인다.

P term을 변경시킬때마다 I term은 P term과 동일하게 설정한다. 이 파라미터들은 지상에서 disarm한 상태로 변경하는 것이 좋다. 경험이 많은 조정사라면 비행 중에 미션 플래너나 조정기를 이용해서 설정이 가능하다.(참고 :ref:`Transmitter based tuning<common-transmitter-tuning>` section).

:ref:`조정기로 튜닝<common-transmitter-tuning>`을 사용하는 경우에, 튜닝 범위의 최소 값을 현재 안전 값에서 대략 최대 4배 정도의 값으로 설정한다. 설정한 값으로 복구되기 위해서 화면이 갱신될때까지 슬라이더를 움직이지 않도록 주의하자. 파라미터 값을 설정하기 전에 조정기 튜닝은 꺼야한다. 그렇지 않으면 튜닌ㅇ이 즉시 덮어쓰기가 된다.

오토튠(Autotune)
--------

비행체가 오토튠을 할만큼 충분히 안정적이라면 오토튠 페이지에 있는 지시대로 따라해보자.

오토튠이 제대로 동작하지 않는데는 다양한 문제점들이 있을 수 있다. 오토튠 시에 실패하는 이유들은 :

- 자이로 센서의 노이즈가 큰 경우
- :ref:`MOT_THST_EXPO <MOT_THST_EXPO>` 파라미터 값이 제대로 설정되지 않은 경우
- 비행체 프레임 혹은 장착된 물체가 단단하지 않은 경우
- 진동 차단을 위한 마운트가 과하게 낭창거릴때.
- 비선형 ESC 응답
- :ref:`MOT_SPIN_MIN <MOT_SPIN_MIN>`의 설정값이 너무 낮게 설정한 경우
- 프로펠러와 모터에 부하가 심한 경우

만약에 오토튠이 실패하면 수동으로 튜닝을 해야한다.

오토튠을 성공적으로 수행한 경우 :

- :ref:`ATC_ANG_PIT_P <ATC_ANG_PIT_P>` 와 :ref:`ATC_ANG_RLL_P <ATC_ANG_RLL_P>` 값의 증가
- :ref:`ATC_RAT_PIT_D <ATC_RAT_PIT_D__AC_AttitudeControl_Multi>` 와 :ref:`ATC_RAT_RLL_D <ATC_RAT_RLL_D__AC_AttitudeControl_Multi>`는 :ref:`AUTOTUNE_MIN_D <AUTOTUNE_MIN_D>` 보다 크다.

오토튠을 수행하면 비행체는 각 축에 대해서 비행체가 견딜 수 있을 만큼으로 튜닝을 시도한다. 일부 비행체에서는 이러한 작업이 불필요하게 응답성을 보일 수 있다. 대부분 비행체에 대해서는 다음과 같다 :

- :ref:`ATC_ANG_PIT_P <ATC_ANG_PIT_P>` 는 10에서 6으로 줄여야한다.
- :ref:`ATC_ANG_RLL_P <ATC_ANG_RLL_P>` 는 10에서 6으로 줄여야한다.
- :ref:`ATC_ANG_YAW_P <ATC_ANG_YAW_P>` 는 10에서 6으로 줄여야한다.
- :ref:`ATC_RAT_YAW_P <ATC_RAT_YAW_P__AC_AttitudeControl_Multi>` 는 1에서 0.5으로 줄여야한다.
- :ref:`ATC_RAT_YAW_I <ATC_RAT_YAW_I__AC_AttitudeControl_Multi>` : :ref:`ATC_RAT_YAW_P <ATC_RAT_YAW_P__AC_AttitudeControl_Multi>` x 0.1

만약 오토튠에서 더 높은 값이 설정된다면 그 값들만 변경해야만 한다. 작은 곡예용 비행체에서는 가능한한 큰 값을 그대로 유지하는게 더 나을 수도 있다.

input shaping 파라미터 설정하기 (Setting the input shaping parameters)
------------------------------------

Arducopter has a set of parameters that define the way the aircraft feels to fly. This allows the aircraft to be set up with a very aggressive tune but still feel like a very docile and friendly aircraft to fly.

이 파라미터들 중에서 가장 중요한 것들은:

- :ref:`ACRO_YAW_P <ACRO_YAW_P>` : yaw rate x 45 degrees/s
- :ref:`ANGLE_MAX <ANGLE_MAX>` :  maximum lean angle
- :ref:`ATC_ACCEL_P_MAX <ATC_ACCEL_P_MAX>` : Pitch rate acceleration
- :ref:`ATC_ACCEL_R_MAX <ATC_ACCEL_R_MAX>` : Roll rate acceleration
- :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>` : Yaw rate acceleration
- :ref:`ATC_ANG_LIM_TC <ATC_ANG_LIM_TC>` : Aircraft smoothing time

Autotune will set the :ref:`ATC_ACCEL_P_MAX <ATC_ACCEL_P_MAX>`, :ref:`ATC_ACCEL_R_MAX <ATC_ACCEL_R_MAX>` and :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>` parameters to their maximum based on measurements done during the Autotune tests. These values should not be increased beyond what Autotune suggests without careful testing. In most cases pilots will want to reduce these values significantly.

For aircraft designed to carry large directly mounted payloads, the maximum values of :ref:`ATC_ACCEL_P_MAX <ATC_ACCEL_P_MAX>`, :ref:`ATC_ACCEL_R_MAX <ATC_ACCEL_R_MAX>` and :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>` should be reduced based on the minimum and maximum takeoff weight (TOW):

- :ref:`ATC_ACCEL_P_MAX <ATC_ACCEL_P_MAX>`  x (min_TOW / max_TOW)
- :ref:`ATC_ACCEL_R_MAX <ATC_ACCEL_R_MAX>`  x (min_TOW / max_TOW)
- :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>`  x (min_TOW / max_TOW)

:ref:`ACRO_YAW_P <ACRO_YAW_P>` should be set to be approximately 0.5 x :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>` / 4500 to ensure that the aircraft can achieve full yaw rate in approximately half a second.

:ref:`ATC_ANG_LIM_TC <ATC_ANG_LIM_TC>` may be increased to provide a very smooth feeling on the sticks at the expense of a slower reaction time.

Aerobatic aircraft should keep the :ref:`ATC_ACCEL_P_MAX <ATC_ACCEL_P_MAX>`, :ref:`ATC_ACCEL_R_MAX <ATC_ACCEL_R_MAX>` and :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>` provided by autotune and reduce :ref:`ATC_ANG_LIM_TC <ATC_ANG_LIM_TC>` to achieve the stick feel desired by the pilot. For pilots wanting to fly ACRO the following input shaping parameters can be used to tune the feel of ACRO:

- :ref:`ACRO_BAL_PITCH <ACRO_BAL_PITCH>`
- :ref:`ACRO_BAL_ROLL <ACRO_BAL_ROLL>`
- :ref:`ACRO_RP_EXPO <ACRO_RP_EXPO>`
- :ref:`ACRO_RP_P <ACRO_RP_P>`
- :ref:`ACRO_THR_MID <ACRO_THR_MID>`
- :ref:`ACRO_TRAINER <ACRO_TRAINER>`
- :ref:`ACRO_Y_EXPO <ACRO_Y_EXPO>`
- :ref:`ACRO_YAW_P <ACRO_YAW_P>`

The full list of input shaping parameters are:

- :ref:`ACRO_BAL_PITCH <ACRO_BAL_PITCH>`
- :ref:`ACRO_BAL_ROLL <ACRO_BAL_ROLL>`
- :ref:`ACRO_RP_EXPO <ACRO_RP_EXPO>`
- :ref:`ACRO_RP_P <ACRO_RP_P>`
- :ref:`ACRO_THR_MID <ACRO_THR_MID>`
- :ref:`ACRO_TRAINER <ACRO_TRAINER>`
- :ref:`ACRO_Y_EXPO <ACRO_Y_EXPO>`
- :ref:`ACRO_YAW_P <ACRO_YAW_P>`
- :ref:`ANGLE_MAX <ANGLE_MAX>`
- :ref:`ATC_ACCEL_P_MAX <ATC_ACCEL_P_MAX>`
- :ref:`ATC_ACCEL_R_MAX <ATC_ACCEL_R_MAX>`
- :ref:`ATC_ACCEL_Y_MAX <ATC_ACCEL_Y_MAX>`
- :ref:`ATC_ANG_LIM_TC <ATC_ANG_LIM_TC>`
- :ref:`ATC_RATE_P_MAX <ATC_RATE_P_MAX>`
- :ref:`ATC_RATE_R_MAX <ATC_RATE_R_MAX>`
- :ref:`ATC_RATE_Y_MAX <ATC_RATE_Y_MAX>`
- :ref:`ATC_SLEW_YAW <ATC_SLEW_YAW>`
- :ref:`PILOT_ACCEL_Z <PILOT_ACCEL_Z>`
- :ref:`PILOT_SPEED_DN <PILOT_SPEED_DN>`
- :ref:`PILOT_SPEED_UP <PILOT_SPEED_UP>`
- :ref:`PILOT_THR_BHV <PILOT_THR_BHV>`
- :ref:`PILOT_THR_FILT <PILOT_THR_FILT>`
- :ref:`PILOT_TKOFF_ALT <PILOT_TKOFF_ALT>`
- :ref:`LOIT_ACC_MAX <LOIT_ACC_MAX>`
- :ref:`LOIT_ANG_MAX <LOIT_ANG_MAX>`
- :ref:`LOIT_BRK_ACCEL <LOIT_BRK_ACCEL>`
- :ref:`LOIT_BRK_DELAY <LOIT_BRK_DELAY>`
- :ref:`LOIT_BRK_JERK <LOIT_BRK_JERK>`
- :ref:`LOIT_SPEED <LOIT_SPEED>`

고급 튜닝(Advanced Tuning)
---------------

100g ~ 500kg까지의 비행에 대해서 훌륭한 성능을 보여주는 매우 유연한 제어기로 설계되었다. 좀더 깊이 이해해야 하는 많은 어려운 제어 문제가 있다. 다음과 같은 이슈가 여기에 포함된다 :

- High gyro noise levels
- Flexible airframes
- Soft vibration dampers
- Large payloads on flexible or loose mounts
- Rate limited actuators
- Non-Linear actuators
- Extremely aggressive or dynamic flight
