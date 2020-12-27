.. _tuning-process-instructions:

===========================
튜닝 과정(Tuning Process Instructions)
===========================

비행체를 튜닝 가능한 상태로 설정하기(Setting the aircraft up ready for tuning)
----------------------------------------

자신의 비행체의 사양에 맞게 아래의 파라미터를 제대로 설정해야만 한다.

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

2. All flights after a significant tuning change should be done in Stabilize. Stabilize provides the pilot with significantly more control over the aircraft in the event that the attitude controllers are unstable.
3. The pilot should not take off in AltHold until the altitude controller has been tested in flight. This should be done by taking off in Stabilize and switching to Alt Hold. While Alt Hold is rarely a problem unless the aircraft has a very low hover throttle.
4. For the initial flights the pilot should ensure that these parameters are set:

- :ref:`ATC_THR_MIX_MAN <ATC_THR_MIX_MAN>` to 0.1
- :ref:`MOT_THST_HOVER <MOT_THST_HOVER>` to 0.25 (or lower than the expected hover throttle)

5. Use a radio and calibrate the radio correctly (see :ref:`common-radio-control-calibration`).
6. Configure an Emergency Stop Motors switch and test it (see :ref:`Auxiliary Functions <common-auxiliary-functions>`).
7. Do tuning flights in low-wind condition and normal weather (no rain and between 15°C/59°F and 25°C/77°F).
8. Practice STABILIZE flight in simulator or on a low-end drone first, you should be confident to be able to takeoff and land with your untuned aircraft.


First Flight
------------

The first take off is the most dangerous time for any multirotor. Care must be taken to ensure the aircraft is not destroyed in the first seconds of flight and nobody is injured.

- **Ensure that all spectators are at a safe distance**.
- **Ensure the pilot is at a safe distance and position**.
- The pilot should refresh themselves on the method used to disarm the aircraft (using :ref:`Auxiliary Functions <common-auxiliary-functions>` for Motor Interlock or Arm/Disarm may be beneficial).

This flight will allow to setup your aircraft in a "flyable for tuning" state.

1. Ensure the aircraft is in STABILIZE mode
2. Arm the aircraft
3. Immediately disarm the aircraft to ensure your disarm procedure is correct
4. Arm the aircraft
5. Slowly increase the throttle looking for signs of oscillation. (long or flexible landing gear may cause some landing gear oscillation that will only go away after the aircraft leaves the ground)
6. As soon as the aircraft lifts off the ground immediately put the aircraft back down as gently as possible
7. Disarm the aircraft
8. Evaluate what you observed to decide if you need to make adjustments to the tuning parameters or if it is safe to take off again
9. Arm and increase the throttle to initiate a takeoff
10. Hover at approximately 1m altitude and apply small (5 degrees) control inputs into roll and pitch
11. Immediately land if any oscillation is observed

Next section will explain how to remove the oscillations.

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

Manual tuning of Roll and Pitch
-------------------------------

Manual tuning may be required to provide a stable tune before Autotune is run, or if Autotune does not produce an acceptable tune. The process below can be done on roll and pitch at the same time for a quick manual tune provided the aircraft is symmetrical. If the aircraft is not symmetrical then the process should be repeated for both roll and pitch individually.

The pilot should be especially careful to ensure that :ref:`ATC_THR_MIX_MAN <ATC_THR_MIX_MAN>` and :ref:`MOT_THST_HOVER <MOT_THST_HOVER>` are set correctly before manual tuning is started.

When oscillations start do not make large or sudden stick inputs. Reduce the throttle smoothly to land the aircraft while using very slow and small roll and pitch inputs to control the aircraft position.

1. Increase the D term in steps of 50% until oscillation is observed
2. Reduce the D term in steps of 10% until the oscillation disappears
3. Reduce the D term by a further 25%
4. Increase the P term in steps of 50% until oscillation is observed
5. Reduce the P term in steps of 10% until the oscillation disappears
6. Reduce the P term by a further 25%

Each time the P term is changed set the I term equal to the P term. Those parameters can be changed on ground and preferably disarmed. A confident pilot could set them in flight with GCS or transmitter tuning knob (see :ref:`Transmitter based tuning<common-transmitter-tuning>` section).

If using :ref:`Transmitter based tuning<common-transmitter-tuning>` , set the minimum value of the tuning range to the current safe value and the upper range to approximately 4 times the current value. Be careful not to move the slider before the parameter list is refreshed to recover the set value. Ensure the transmitter tuning is switched off before setting the parameter value or the tuning may immediately overwrite it.

Autotune
--------

If the aircraft appears stable enough to attempt autotune follow the instructions in the autotune page.

There a number of problems that can prevent Autotune from providing a good tune. Some of the reasons Autotune can fail are:

- High levels of gyro noise.
- Incorrect value of :ref:`MOT_THST_EXPO <MOT_THST_EXPO>`.
- Flexible frame or payload mount.
- Overly flexible vibration isolation mount.
- Non-linear ESC response.
- Very low setting for :ref:`MOT_SPIN_MIN <MOT_SPIN_MIN>`.
- Overloaded propellers or motors.

If Autotune has failed you will need to do a manual tune.

Some signs that Autotune has been successful are:

- An increase in the values of :ref:`ATC_ANG_PIT_P <ATC_ANG_PIT_P>` and :ref:`ATC_ANG_RLL_P <ATC_ANG_RLL_P>`.
- :ref:`ATC_RAT_PIT_D <ATC_RAT_PIT_D__AC_AttitudeControl_Multi>` and :ref:`ATC_RAT_RLL_D <ATC_RAT_RLL_D__AC_AttitudeControl_Multi>` are larger than :ref:`AUTOTUNE_MIN_D <AUTOTUNE_MIN_D>`.

Autotune will attempt to tune each axis as tight as the aircraft can tolerate. In some aircraft this can be unnecessarily responsive. A guide for most aircraft:

- :ref:`ATC_ANG_PIT_P <ATC_ANG_PIT_P>` should be reduced from 10 to 6
- :ref:`ATC_ANG_RLL_P <ATC_ANG_RLL_P>` should be reduced from 10 to 6
- :ref:`ATC_ANG_YAW_P <ATC_ANG_YAW_P>` should be reduced from 10 to 6
- :ref:`ATC_RAT_YAW_P <ATC_RAT_YAW_P__AC_AttitudeControl_Multi>` should be reduced from 1 to 0.5
- :ref:`ATC_RAT_YAW_I <ATC_RAT_YAW_I__AC_AttitudeControl_Multi>` : :ref:`ATC_RAT_YAW_P <ATC_RAT_YAW_P__AC_AttitudeControl_Multi>` x 0.1

These values should only be changed if Autotune produces higher values. Small aerobatic aircraft may prefer to keep these values as high as possible.

Setting the input shaping parameters
------------------------------------

Arducopter has a set of parameters that define the way the aircraft feels to fly. This allows the aircraft to be set up with a very aggressive tune but still feel like a very docile and friendly aircraft to fly.

The most important of these parameters is:

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
