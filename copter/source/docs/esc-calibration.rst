.. _esc-calibration:

=============================================
변속기(Electronic Speed Controller (ESC)) 칼리브레이션
=============================================

ESC는 비행제어기가 요청한 속도로 모터를 회전시키는 역할을 수행합니다. 대부분의 ESC에 대해서 칼리브레이션이 필요한데 제어기가 보내는 PWM의 최대/최소값을 알기 위해서입니다.
여기서는 ESC를 칼리브레이션하는 절차에 대해서 설명합니다.

.. note::

   ESC 칼리브레이션을 수행하기 전에 :ref:`라디오 칼리브레이션 <common-radio-control-calibration>` 을 완료해야 합니다.

ESC 칼리브레이션에 대해서
=====================

ESC 칼리브레이션은 사용하는 ESC 제조사에 따라 달라립니다. 따라서 사용하는 ESC의 제조사가 제공하는 문서를 참고하세요.(ex) 소리 정보). 대부분 경우 "한꺼번에" 칼리브레이션이 되므로 먼저 수행해 보고 만약 제대로 되지 않는다면 하나씩 수동으로 칼리브레이션을 수행합니다.

-  BLHeli나 DShot ESC를 사용하는 경우 :ref:`DShot and BLHeli ESC Support <common-dshot>` 를 참고하세요.
-  Some ESCs like the DJI Opto ESCs do not require and do not support calibration, so skip this page completely
-  Some brands of ESC do not allow calibration and will not arm unless you adjust your radio's throttle end-points so that the minimum throttle is around 1000 PWM and maximum is around 2000.  Note that if you change the end-points on your TX you must re-do the :ref:`Radio Calibration <common-radio-control-calibration>`.  Alternatively with Copter-3.4 (and higher) you may manually set the :ref:`MOT_PWM_MIN <MOT_PWM_MIN>` to 1000 and :ref:`MOT_PWM_MAX <MOT_PWM_MAX>` to 2000.
-  If using OneShot ESCs set the :ref:`MOT_PWM_TYPE <MOT_PWM_TYPE>` to 1 (for regular OneShot) or 2 (for OneShot125).  Note only supported in Copter-3.4 (and higher).
-  Begin this procedure only after you have completed the :ref:`radio control calibration <common-radio-control-calibration>` and :ref:`Connect ESCs and motors <connect-escs-and-motors>` part of the :ref:`Autopilot System Assembly Instructions <autopilot-assembly-instructions>`.  Next follow these steps:

.. 경고::

   **안전 점검!**

   ESC를 칼리브레이션하기 전에 프로펠러를 제거합니다. 그리고 USB로 연결 제거 및 Lipo 배터리 연결을 해제합니다.

   .. image:: ../images/copter_disconnect_props_banner.png
       :target: ../_images/copter_disconnect_props_banner.png
       :width: 400px

   .. image:: ../images/MicroUSB1__57056_zoom.jpg
       :target: ../_images/MicroUSB1__57056_zoom.jpg
       :width: 400px

한꺼번에 칼리브레이션하기
=======================

#. Turn on your transmitter and put the throttle stick at maximum.

   .. image:: ../images/transmitter-throttle-max.jpg
       :target: ../_images/transmitter-throttle-max.jpg
       :width: 400px
    
#. Connect the Lipo battery.  The autopilot's red, blue and yellow LEDs
   will light up in a cyclical pattern. This means the it's ready to go
   into ESC calibration mode the next time you plug it in.

   .. image:: ../images/Connect-Battery.jpg
       :target: ../_images/Connect-Battery.jpg
       :width: 400px
   
#. With the transmitter throttle stick still high, disconnect and
   reconnect the battery.

   .. image:: ../images/Disconnect-Battery.jpg
       :target: ../_images/Disconnect-Battery.jpg
       :width: 400px

   .. image:: ../images/Connect-Battery.jpg
    :target: ../_images/Connect-Battery.jpg
    :width: 400px
    
#. For Autopilots with a safety switch, push it until the LED displays solid red
#. The autopilot is now in ESC calibration mode. (On an APM you may
   notice the red and blue LEDs blinking alternatively on and off like a
   police car).
#. Wait for your ESCs to emit the musical tone, the regular number of
   beeps indicating your battery's cell count (i.e. 3 for 3S, 4 for 4S)
   and then an additional two beeps to indicate that the maximum
   throttle has been captured.
#. Pull the transmitter's throttle stick down to its minimum position.

   .. image:: ../images/transmitter-throttle-min.jpg
       :target: ../_images/transmitter-throttle-min.jpg
       :width: 400px
    
#. The ESCs should then emit a long tone indicating that the minimum
   throttle has been captured and the calibration is complete.
#. If the long tone indicating successful calibration was heard, the
   ESCs are "live" now and if you raise the throttle a bit they should
   spin. Test that the motors spin by raising the throttle a bit and
   then lowering it again.
#. Set the throttle to minimum and disconnect the battery to exit
   ESC-calibration mode.

**Here is a video demonstrating the process:**

..  youtube:: gYoknRObfOg
    :width: 100%

Manual ESC-by-ESC Calibration
=============================

#. Plug one of your ESC three-wire cables into the throttle channel of
   the RC receiver. (This is usually channel 3.)
#. Turn on the transmitter and set throttle stick to maximum (full up).
#. Connect the LiPo battery
#. You will hear a musical tone then two beeps.
#. After the two beeps, lower the throttle stick to full down.
#. You will then hear a number of beeps (one for each battery cell
   you're using) and finally a single long beep indicating the end
   points have been set and the ESC is calibrated.
#. Disconnect battery. Repeat these steps for all ESCs.
#. If it appears that the ESC’s did not calibrate then the throttle
   channel on the transmitter might need to be reversed.
#. If you are still having trouble after trying these methods (for
   example, ESCs still beep continuously) try lowering your throttle
   trim 50%.
#. You can also try powering your APM board via the USB first to boot it
   up before plugging in the LiPo.

Semi Automatic ESC-by-ESC Calibration
=====================================

#. Connect to the autopilot from a ground station such as the Mission Planner and set the :ref:`ESC_CALIBRATION <ESC_CALIBRATION>` parameter to 3
#. Disconnect the battery and USB cable so the autopilot powers down
#. Connect the battery
#. The arming tone will be played (if the vehicle has a buzzer attached)
#. If using a autopilot with a safety button (like the Pixhawk) press it until it displays solid red
#. You will hear a musical tone then two beeps
#. A few seconds later you should hear a number of beeps (one for each battery cell you're using) and finally a single long beep indicating the end points have been set and the ESC is calibrated
#. Disconnect the battery and power up again normally and test as described below

Testing
=======

Once you have calibrated your ESCs, you can test them by plugging in
your LiPo.  Remember: no propellers!

-  Ensure your transmitter's flight mode switch is set to “Stabilize
   Mode”.
-  :ref:`Arm your copter <arming_the_motors>`
-  Give a small amount of throttle.  All motors should spin at about
   same speed and they should start at the same time. If the motors do
   not all start at the same time and spin at the same speed, the ESC’s
   are still not properly calibrated.
-  Disarm your copter

Notes / Troubleshooting
=======================

The All-at-once ESC calibration mode simply causes the APM to pass
through the pilot's throttle directly through to the ESCs. If you power
up the APM while in this mode you’ll send the same PWM signal to all the
ESCs. That's all it does.  Many ESCs use full throttle at startup to
enter programming mode, full throttle postition is then saved as the
upper end point and when you pull the throttle down to zero, that
position is saved as the lower end point.

If after calibration your motors do NOT spin same speed nor start at the
same time, repeat the calibration process. If you tried the auto
calibration above and it didn’t work or the ESCs do not drive the motors
identically, try the manual calibration method described above. That
should work almost every time. (Rarely after a full manual calibration
you will also need to do an additional final automatic calibration).

Finally, there are a huge number of brands and types of ESCs available
and some of them do not adhere to the normal programming conventions
(sometimes even though they claim to) and they may simply not work with
the APM the way it is now. This is an unfortunately necessary but true
disclaimer.

Recommended ESC settings as follows:

#. Brake: OFF
#. Battery Type: Ni-xx(NiMH or NiCd)  (even if you're using Li-po
   batteries this setting reduces the likelihood that the ESC's low
   voltage detection will turn off the motors)
#. CutOff Mode: Soft-Cut (Default)
#. CutOff Threshold: Low
#. Start Mode: Normal (Default)
#. Timing: MEDIUM
