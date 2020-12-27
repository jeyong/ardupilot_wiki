.. _throw-mode:

==========
던지기 모드(Throw Mode)
==========

모터를 구동시키기 위해서 비행체를 공중으로 던지는 조금 위험한 비행 모드이다.
일단 공중에서는 이 모드에서는 조정사의 입력을 받아들이지 않는다. 이 모드는 GPS가 필요하다.

.. warning::

   주의해서 사용한다. 비행체를 던져야 하므로 arming된 비행체 근처에 있으면 위험하다. 가능하다면 던지기 모드를 사용하는 대신에 일반적인 takeoff를 사용하는 것을 추천한다.

.. note::

   던지기 비행 모드는 AC3.4에서 지원되었다.

..  youtube:: JIPMpDJqdJ8
    :width: 100%

사용하는 방법
==========

#. Disarm copter
#. Switch to throw mode
#. Check GPS light is green
#. Arm copter and listen for ready tune (if vehicle has a buzzer).  The motors will not spin by default.
#. Pick up the vehicle and throw it up and away from you (it must climb by 50cm/s and reach a total speed of 5m/s)
#. Once the vehicle has stopped, switch the flight mode to Loiter (or other mode) to retake manual control

The motors should start when the vehicle reaches the apex of it's trajectory.
After the motors start this flight mode will first try to control it's attitude (return to level and stop rotating), then stop descending and finally it will attempt to stop moving horizontally.

Settings
========
- :ref:`THROW_TYPE <THROW_TYPE>` : set to 0 if throwing the vehicle up, 1 if dropping the vehicle.  If dropping, drop from a height of at least 10m.
- :ref:`THROW_MOT_START <THROW_MOT_START>` : controls whether the motors will spin slowly or not at all while waiting for the throw (0 = stopped, 1 = spinning slowly).  The default is 0 (will not spin after arming).
- :ref:`THROW_NEXTMODE <THROW_NEXTMODE>` : the vehicle will switch into this flight mode after stopping (Auto, Guided, RTL, Land and Brake are support).  Set to "Throw" (the default) to simply remain in Throw mode and wait for the pilot to switch modes manually

..  youtube:: ZnEFcJx1qko
    :width: 100%

Log Analysis
============
During the throw, THRO messages are written to the :ref:`dataflash log <common-downloading-and-analyzing-data-logs-in-mission-planner>`.  These can be useful in diagnosing problems in case the motors failed to start as part of a throw.  The graph below shows a successful throw in which the overall velocity climbs above 5m/s and the vertical velocity is over 0.5m/s.

   .. image:: ../images/throw_log.png
       :target: ../_images/throw_log.png
       :width: 500px
