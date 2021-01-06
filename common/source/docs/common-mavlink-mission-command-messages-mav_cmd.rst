.. _common-mavlink-mission-command-messages-mav_cmd:

================
Mission Commands
================

여기서는 Copter, Plane, Rover에서 Auto 모드로 스위치될때 지원되는 mission 명령을 설명한다.

.. warning::

   현재 작업 중이며 아직 리뷰가 끝나지 않았다. :ref:`Copter can be found here <copter:mission-command-list>` 가 낫다.

개요
========

MAVLink 프로토콜은 엄청나게 많은 `MAV_CMD <https://github.com/ArduPilot/mavlink/blob/master/message_definitions/v1.0/common.xml#L1008>`__ waypoint 명령 타입을 정의하고있다. ArduPilot은 이런 명령의 부분집합과 각 비행체에 의미있고 관련있는 명령-파라미터에 대한 처리를 구현한다. 지원하지 않는 명령은 처리하지 않고 무시된다.

여기서는 각 비행체 타입이 지원하는 명령과 명령 파라미터를 설명한다. "회색"으로 된 파라미터는 Pixhawk에서 지원하지 않으므로 무시된다. `MAV_CMD protocol <https://github.com/ArduPilot/mavlink/blob/master/message_definitions/v1.0/common.xml#L1008>`__ 에서 지원하는 속성은 해당 비행체에서 구현되지 않는다는 것을 명확히 하기 위해서 문서화해놨다.

일부 명령과 명령-파라미터는 구현하지 않았는데 왜냐하면 특정 비행체 타입에는 관련이 없기 때문이다. (예제로 "MAV_CMD_NAV_TAKEOFF" 명령은 Rover에는 상관이 없음)
메시지 크기에 대한 제한 때문에 처리하고 있지는 않지만 향후 유용한 명령-파라미터도 있다. 다른 파라미터보다 우선 순위가 높다는 결정이 내려진다.

.. note::

   Copter에서 지원하는 명령에 대한 추가 정보는 :ref:`Copter Mission Command List <copter:mission-command-list>` 를 참고하자.

명령의 종류
-----------------

mission 내부에서 사용할 수 있는 여러 종류의 명령이 있다.:

-  Navigation 명령은 비행체의 움직임을 제어하는데 사용한다. 여기에는 takeoff, waypoint 주변 이동, 고도 변경, 착륙이 속한다.
-  DO 명령은 추가 기능으로 비행체의 위치에 영향을 미치지 않는다. 예제로 카메라 트리거 거리, 서보 값 등이 여기에 속한다.
-  Condition 명령은 특정 condition을 만족할때까지 DO 명령을 지연시키는데 사용한다. 예제로 UAV가 특정 고도나 waypoint로부터 특정 거리에 도달하는 경우가 해당된다.

미션동안 기껏해야 1개 “Navigation” 명령과 1개 “Do” 혹은 "Condition" 명령은 한 번에 실행될 수 있다. 일반적인 mission은 waypoint (NAV command)를 설정하고 목적지로부터(:ref:`MAV_CMD_CONDITION_DISTANCE <mav_cmd_condition_distance>`) 특정 거리까지 완료시키지 않는 CONDITION 명령을 추가한다. 그리고 난후에 순차적으로 실행되는 여러 DO 명령을 추가한다. (예제로 특정 조건이 만족되면 :ref:`MAV_CMD_DO_SET_CAM_TRIGG_DIST <mav_cmd_do_set_cam_trigg_dist>` 는 일정한 시간 간격으로 사진을 찍는다)

.. note::

   CONDITION과 DO 명령은 앞단의 NAV 명령과 관련이 있다.: 만약 UAV가 이런 명령들이 실행되기 전에 다음 waypoint에 도달한다면 다음 NAV 명령은 load되고 skip된다.


.. _common-mavlink-mission-command-messages-mav_cmd_navigation_commands_frames:

Frames of reference
-------------------

많은 명령들이 (특히 :ref:`NAV\_ commands <common-mavlink-mission-command-messages-mav_cmd_navigation_commands>`) 위치에 관한 정보를 포함하고 있다. 이 정보는 특정 "frame of reference"에 따라 제공되며 :ref:`common-mavlink-mission-command-messages-mav_cmd_navigation_commands_frames` 필드에 지정한다. Copter와 Rover Mission은 :ref:`MAV_CMD_DO_SET_HOME <mav_cmd_do_set_home>` 을 사용해서 global coordinate frame (MAV_FRAME_GLOBAL)에서의 "home position"을 설정한다. `WGS84 coordinate system <https://en.wikipedia.org/wiki/World_Geodetic_System>`__ 를 참고하자. 여기서 고도는 평균 해수면을 기준으로 한다. 다른 모든 명령들은 MAV_FRAME_GLOBAL_RELATIVE_ALT frame을 사용하고 이는 동일한 lat와 lon을 사용한다. 하지만 고도는 *home position* (home altitude = 0)을 기준으로 한다.

Plane 명령은 추가적으로 MAV_FRAME_GLOBAL_TERRAIN_ALT frame 을 사용할 수 있다. 이것은 다시 동일한 lat/long에 대해서 WGS84 frame을 가진다. 하지만 지상 높이를 기준으로 고도를 지정한다.(지형도 DB에 정의된 것처럼)

.. note::

   MAVLink 프로토콜(`MAV_FRAME <https://github.com/ArduPilot/mavlink/blob/master/message_definitions/v1.0/common.xml#L795>`__ ) 에 정의된 다른 frame 타입은 mission 명령에서 지원하지 않는다.

정보가 얼마나 정확한가?
--------------------------------

만약 명령과 파라미터가 지원으로 설정되어 있다면 지정한 대로 동작할 것이다.(하지만 보장은 못하지만) 만약 명령이나 파라미터가 목록에 없다면 ArduPilot에서 지원안할 확률이 높다.

이런 이유로 해당 정보는 주로 메시지가 해당 명령 처리를 어떻게 할지 추정할 수 있다.:

-  The switch statement in `AP_Mission::mavlink_to_mission_cmd <https://github.com/ArduPilot/ardupilot/blob/master/libraries/AP_Mission/AP_Mission.cpp#L466>`__
   was inspected to determine which commands are handled by *all*
   vehicle platforms, and which parameters from the message are stored.
-  The command handler switch for each vehicle type
   (`Plane <https://github.com/ArduPilot/ardupilot/blob/master/ArduPlane/commands_logic.cpp#L33>`__,
   `Copter <https://github.com/ArduPilot/ardupilot/blob/master/ArduCopter/commands_logic.cpp#L49>`__,
   `Rover <https://github.com/ArduPilot/ardupilot/blob/master/Rover/commands_logic.cpp#L25>`__)
   tells us which commands are likely to be supported in each vehicle
   and which parameters are passed to the handler.

위에 체크는 어떤 명령과 파라미터가 지원되지 않는지를 정확하게 보여준다. 어떤 명령/파라미터가 지원될꺼 같은지 꽤 정확하게 보여준다. 하지만 이런 표시는 정확함을 보장해 주지는 않는다. 왜냐하면 명령 처리자는 모든 정보를 버려버릴 수도 있기 때문이다.(그리고 완벽하게 모든 것을 체크하지는 않았다.)

외에 체크에 추가로 :ref:`Copter Mission Command List <copter:mission-command-list>` 로부터 합쳐진 정보도 가지고 있다.

commands와 parameters 해석하는 방법
----------------------------------------

각 명령에 대한 파라미터는 테이블로 제공된다. "회색"으로 된 파라미터들은 더 이상 지원하지 않는다.
명령 필드 컬럼(param name)은 "볼드"체로 해당 프로토콜에서 정의된 파라미터를 가리킨다.(일반 텍스트는 "empty" 파라미터에 사용된다.)

이렇게 하면 사용자/개발자가 지원하는 것과 어떤 프로토콜 필트가 ArduPilot에서 지원되지 않는지 모두를 제공한다.

GCS에서 이 정보 사용하기
---------------------------------

*Mission Planner* (MP)는 ArduPilot에서 지원하는 명령과 파라미터들의 전체 부분집합을 보여주고 현재 연결된 비행체와 관련된 것들만 필터링해서 보여준다. MP 명령을 이 문서로 매핑하는 것은 쉽다. 왜냐하면 전체 명령 이름을 줄이면 명령이 되기 때문이다.(예제 ``DO_SET_SERVO`` 는  ``MAV_CMD_DO_SET_SERVO`` 의 단축 표현이다.) 추가로 이 문서에서는 각 해당 파라미터 옆에 Mission Planner에서 사용하는 컬럼 라벨을 제공한다.

달느 GCS(APM Planner 2, Tower 등)은 commands/parameters의 부분 집합을 지원하고 대안으로 names/labels 를 사용할 수도 있다. 대부분의 경우 매핑은 명령해야만 한다.

[site wiki="copter"]
Copter에서 지원하는 명령
============================

여기에 이는 명령 목록은 `/ArduCopter/mode_auto.cpp <https://github.com/ArduPilot/ardupilot/blob/master/ArduCopter/mode_auto.cpp#L388>`__ 에 있는 command handler로부터 추론했다.

:ref:`MAV_CMD_NAV_WAYPOINT <mav_cmd_nav_waypoint>`

:ref:`MAV_CMD_NAV_RETURN_TO_LAUNCH <mav_cmd_nav_return_to_launch>`

:ref:`MAV_CMD_NAV_TAKEOFF <mav_cmd_nav_takeoff>`

:ref:`MAV_CMD_NAV_LAND <mav_cmd_nav_land>`

:ref:`MAV_CMD_NAV_LOITER_UNLIM <mav_cmd_nav_loiter_unlim>`

:ref:`MAV_CMD_NAV_LOITER_TURNS <mav_cmd_nav_loiter_turns>`

:ref:`MAV_CMD_NAV_LOITER_TIME <mav_cmd_nav_loiter_time>`

:ref:`MAV_CMD_NAV_SPLINE_WAYPOINT <mav_cmd_nav_spline_waypoint>`

:ref:`MAV_CMD_NAV_GUIDED_ENABLE <mav_cmd_nav_guided_enable>`
(NAV_GUIDED only)

:ref:`MAV_CMD_DO_JUMP <mav_cmd_do_jump>`

:ref:`MAV_CMD_MISSION_START <mav_cmd_mission_start>`

:ref:`MAV_CMD_COMPONENT_ARM_DISARM <mav_cmd_component_arm_disarm>`

:ref:`MAV_CMD_CONDITION_DELAY <mav_cmd_condition_delay>`

:ref:`MAV_CMD_CONDITION_DISTANCE <mav_cmd_condition_distance>`

:ref:`MAV_CMD_CONDITION_YAW <mav_cmd_condition_yaw>`

:ref:`MAV_CMD_DO_CHANGE_SPEED <mav_cmd_do_change_speed>`

:ref:`MAV_CMD_DO_SET_HOME <mav_cmd_do_set_home>`

:ref:`MAV_CMD_DO_SET_SERVO <mav_cmd_do_set_servo>`

:ref:`MAV_CMD_DO_SET_RELAY <mav_cmd_do_set_relay>`

:ref:`MAV_CMD_DO_REPEAT_SERVO <mav_cmd_do_repeat_servo>`

:ref:`MAV_CMD_DO_REPEAT_RELAY <mav_cmd_do_repeat_relay>`

:ref:`MAV_CMD_DO_DIGICAM_CONFIGURE <mav_cmd_do_digicam_configure>`
(Camera enabled only)

:ref:`MAV_CMD_DO_DIGICAM_CONTROL <mav_cmd_do_digicam_control>` (Camera
enabled only)

:ref:`MAV_CMD_DO_SET_CAM_TRIGG_DIST <mav_cmd_do_set_cam_trigg_dist>`
(Camera enabled only)

:ref:`MAV_CMD_DO_SET_ROI <mav_cmd_do_set_roi>`

:ref:`MAV_CMD_DO_SET_MODE <mav_cmd_do_set_mode>`

:ref:`MAV_CMD_DO_MOUNT_CONTROL <mav_cmd_do_mount_control>`

:ref:`MAV_CMD_DO_PARACHUTE <mav_cmd_do_parachute>` (Parachute enabled
only)

:ref:`MAV_CMD_DO_GRIPPER <mav_cmd_do_gripper>` (EPM enabled only)

:ref:`MAV_CMD_DO_GUIDED_LIMITS <mav_cmd_do_guided_limits>`
(NAV_GUIDED only)

:ref:`MAV_CMD_DO_SET_RESUME_DIST <mav_cmd_do_set_resume_dist>`

:ref:`MAV_CMD_DO_FENCE_ENABLE <mav_cmd_do_fence_enable>`

[/site]

[site wiki="plane"]
Plane에서 지원하는 명령
===========================

여기에 이는 명령 목록은 `/ArduPlane/commands_logic.cpp <https://github.com/ArduPilot/ardupilot/blob/master/ArduPlane/commands_logic.cpp#L33>`__ 에 있는 command handler로부터 추론했다.

:ref:`MAV_CMD_NAV_WAYPOINT <mav_cmd_nav_waypoint>`

:ref:`MAV_CMD_NAV_RETURN_TO_LAUNCH <mav_cmd_nav_return_to_launch>`

:ref:`MAV_CMD_NAV_TAKEOFF <mav_cmd_nav_takeoff>`

:ref:`MAV_CMD_NAV_LAND <mav_cmd_nav_land>`

:ref:`MAV_CMD_NAV_LOITER_UNLIM <mav_cmd_nav_loiter_unlim>`

:ref:`MAV_CMD_NAV_LOITER_TURNS <mav_cmd_nav_loiter_turns>`

:ref:`MAV_CMD_NAV_LOITER_TIME <mav_cmd_nav_loiter_time>`

:ref:`MAV_CMD_NAV_ALTITUDE_WAIT <mav_cmd_nav_altitude_wait>`

:ref:`MAV_CMD_NAV_LOITER_TO_ALT <mav_cmd_nav_loiter_to_alt>`

:ref:`MAV_CMD_NAV_CONTINUE_AND_CHANGE_ALT <mav_cmd_nav_continue_and_change_alt>`

:ref:`MAV_CMD_NAV_VTOL_TAKEOFF <mav_cmd_nav_vtol_takeoff>`

:ref:`MAV_CMD_NAV_VTOL_LAND <mav_cmd_nav_vtol_land>`



:ref:`MAV_CMD_CONDITION_DELAY <mav_cmd_condition_delay>`

:ref:`MAV_CMD_CONDITION_DISTANCE <mav_cmd_condition_distance>`


:ref:`MAV_CMD_DO_CHANGE_SPEED <mav_cmd_do_change_speed>`

:ref:`MAV_CMD_DO_ENGINE_CONTROL <mav_cmd_do_engine_control>`

:ref:`MAV_CMD_DO_VTOL_TRANSITION <mav_cmd_do_vtol_transition>`

:ref:`MAV_CMD_DO_SET_HOME <mav_cmd_do_set_home>`

:ref:`MAV_CMD_DO_SET_SERVO <mav_cmd_do_set_servo>`

:ref:`MAV_CMD_DO_SET_RELAY <mav_cmd_do_set_relay>`

:ref:`MAV_CMD_DO_REPEAT_SERVO <mav_cmd_do_repeat_servo>`

:ref:`MAV_CMD_DO_REPEAT_RELAY <mav_cmd_do_repeat_relay>`

:ref:`MAV_CMD_DO_DIGICAM_CONFIGURE <mav_cmd_do_digicam_configure>`
(Camera enabled only)

:ref:`MAV_CMD_DO_DIGICAM_CONTROL <mav_cmd_do_digicam_control>` (Camera
enabled only)

:ref:`MAV_CMD_DO_SET_CAM_TRIGG_DIST <mav_cmd_do_set_cam_trigg_dist>`
(Camera enabled only)

:ref:`MAV_CMD_DO_SET_ROI <mav_cmd_do_set_roi>` (Gimbal/mount enabled
only)

:ref:`MAV_CMD_DO_SET_MODE <mav_cmd_do_set_mode>`

:ref:`MAV_CMD_DO_JUMP <mav_cmd_do_jump>`

:ref:`MAV_CMD_DO_MOUNT_CONTROL <mav_cmd_do_mount_control>`

:ref:`MAV_CMD_DO_INVERTED_FLIGHT <mav_cmd_do_inverted_flight>`

:ref:`MAV_CMD_DO_LAND_START <mav_cmd_do_land_start>`

:ref:`MAV_CMD_DO_FENCE_ENABLE <mav_cmd_do_fence_enable>`

:ref:`MAV_CMD_DO_AUTOTUNE_ENABLE <mav_cmd_do_autotune_enable>`

:ref:`MAV_CMD_DO_SET_RESUME_DIST <mav_cmd_do_set_resume_dist>`

[/site]

[site wiki="rover" heading="off"]
.. _commands_supported_by_rover:

.. _common-mavlink-mission-command-messages-mav_cmd_commands_supported_by_rover:

Rover가 지원하는 Commands
===========================

여기에 이는 명령 목록은 `/Rover/commands_logic.cpp <https://github.com/ArduPilot/ardupilot/blob/master/Rover/commands_logic.cpp#L25>`__ 에 있는 command handler로부터 추론했다.

:ref:`MAV_CMD_NAV_WAYPOINT <mav_cmd_nav_waypoint>`

:ref:`MAV_CMD_NAV_RETURN_TO_LAUNCH <mav_cmd_nav_return_to_launch>`

:ref:`MAV_CMD_DO_JUMP <mav_cmd_do_jump>`

:ref:`MAV_CMD_CONDITION_DELAY <mav_cmd_condition_delay>`

:ref:`MAV_CMD_CONDITION_DISTANCE <mav_cmd_condition_distance>`

:ref:`MAV_CMD_DO_CHANGE_SPEED <mav_cmd_do_change_speed>`

:ref:`MAV_CMD_DO_SET_HOME <mav_cmd_do_set_home>`

:ref:`MAV_CMD_DO_SET_SERVO <mav_cmd_do_set_servo>`

:ref:`MAV_CMD_DO_SET_RELAY <mav_cmd_do_set_relay>`

:ref:`MAV_CMD_DO_REPEAT_SERVO <mav_cmd_do_repeat_servo>`

:ref:`MAV_CMD_DO_REPEAT_RELAY <mav_cmd_do_repeat_relay>`

:ref:`MAV_CMD_DO_DIGICAM_CONFIGURE <mav_cmd_do_digicam_configure>`
(Camera enabled only)

:ref:`MAV_CMD_DO_DIGICAM_CONTROL <mav_cmd_do_digicam_control>` (Camera
enabled only)

:ref:`MAV_CMD_DO_MOUNT_CONTROL <mav_cmd_do_mount_control>`

:ref:`MAV_CMD_DO_SET_CAM_TRIGG_DIST <mav_cmd_do_set_cam_trigg_dist>`
(Camera enabled only)

:ref:`MAV_CMD_DO_SET_ROI <mav_cmd_do_set_roi>` (Gimbal/mount enabled
only)

:ref:`MAV_CMD_DO_SET_MODE <mav_cmd_do_set_mode>`

:ref:`MAV_CMD_DO_SET_RESUME_DIST <mav_cmd_do_set_resume_dist>`

:ref:`MAV_CMD_DO_FENCE_ENABLE <mav_cmd_do_fence_enable>`

[/site]


.. _common-mavlink-mission-command-messages-mav_cmd_navigation_commands:

Navigation commands
===================

Navigation 명령은 비행체의 움직임을 제어하는데 사용한다. 여기에는 이륙, waypoint 주변으로 이동, 착륙이 해당된다.

NAV 명령은 가장 높은 우선순위를 가진다. NAV 명령이 load될때 실행되지 않는 DO\_ 와 CONDITION\_ 명령은 skip된다. (예제로 waypoint가 완료되고 다른 waypoint를 위한 NAV 명령이 load되면 첫번째 waypoint와 관련된 예상치 못했던 DO/CONDITION은 무시된다.

.. _mav_cmd_nav_waypoint:

MAV_CMD_NAV_WAYPOINT
--------------------

지원: Copter, Plane, Rover.

특정 위치로 네비게이션

[site wiki="copter" heading="off"]
Copter
~~~~~~

Copter는 특정 위도, 경도, 높이에 대해서 직선으로 비행한다. 지정된 지연 시간동안 해당 지점에서 대기하고 이후에 다음 waypoint를 진행한다.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Delay</td>
   <td>Hold time at mission waypoint in decimal seconds - MAX 65535 seconds. (Copter/Rover only)
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param2</strong></td>
   <td>
   </td>
   <td>Acceptance radius in meters (when plain inside the sphere of this radius, the waypoint is considered reached) (Plane only).
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td>
   </td>
   <td>0 to pass through the WP, if > 0 radius in meters to pass by WP.
   Positive value for clockwise orbit, negative value for counter-clockwise
   orbit. Allows trajectory control.
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Desired yaw angle at waypoint target.(rotary wing)</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the Copter will hold at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude. If zero, the Copter will hold at the current longitude.</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target altitude. If zero, the Copter will hold at the current altitude.</td>
   </tr>
   </tbody>
   </table>

Parameters 2-4는 Copter에서 지원하지 않는다.:

-  **Hit Rad** - is supposed to hold the distance (in meters) from the
   target point that will qualify the waypoint as complete. This command
   is not supported. Instead the
   :ref:`WPNAV_RADIUS <copter:WPNAV_RADIUS>`
   parameter should be used (see "WP Radius" field in screen shot or
   adjust through the Standard Parameters List).  Even the WPNAV_RADIUS
   is only used when the waypoint has a Delay. With no delay specified
   the waypoint will be considered complete when the virtual point that
   the vehicle is chasing reaches the waypoint. This can be 10m (or
   more) ahead of the vehicle meaning that the vehicle will turn towards
   the following waypoint long before it actually reaches the current
   waypoint
-  **Yaw Ang** - is supposed to hold the resulting yaw angle in degrees
   (0=north, 90 = east). Instead use a
   :ref:`MAV_CMD_CONDITION_YAW <mav_cmd_condition_yaw>` command.

**Mission Planner screenshots**

.. figure:: ../../../images/WayPoint.jpg
   :target: ../_images/WayPoint.jpg

   Copter: Mission Planner Settings forWAYPOINT command
[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

The vehicle will fly to the specified latitude, longitude and altitude.
The waypoint is considered "complete" when Plane is within the specified
radius of the target location, at which point Plane processes the next
command.

The protocol additionally provides for the plane to circle the waypoint
with a specified radius and direction for a specified time (Delay).
These parameters are not support by Copter.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param1</strong></td>
   <td>
   </td>
   <td>Hold time at mission waypoint in decimal seconds (Copter/Rover only).</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td></td>
   <td>Acceptance radius in meters (waypoint is complete when the plane is this close to the waypoint location (Plane only).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>0 to pass through the WP, if > 0 radius in meters to pass by WP.
   Positive value for clockwise orbit, negative value for counter-clockwise
   orbit. Allows trajectory control.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Desired yaw angle at waypoint target (rotary wing).</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target altitude</td>
   </tr>
   </tbody>
   </table>
[/site]

[site wiki="rover" heading="off"]

Rover
~~~~~

Change the target horizontal speed and/or the vehicle's throttle.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Delay</td>
   <td>Hold time at mission waypoint in decimal seconds - MAX 65535 seconds. (Copter/Rover only)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param2</strong></td>
   <td></td>
   <td>Acceptance radius in meters (when plain inside the sphere of this radius, the waypoint is considered reached) (Plane only).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>0 to pass through the WP, if > 0 radius in meters to pass by WP.
   Positive value for clockwise orbit, negative value for counter-clockwise
   orbit. Allows trajectory control.
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Desired yaw angle at waypoint target.(rotary wing)</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the Copter will hold at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude. If zero, the Copter will hold at the current longitude.</td>
   </tr>
   <tr>
   <strong>param7</strong>
   <td>Alt</td>
   <td>Target altitude. If zero, the Copter will hold at the current altitude.</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_nav_takeoff:
[site  wiki="copter,plane" heading="off"]

MAV_CMD_NAV_TAKEOFF
-------------------

Supported by: Copter, Plane (not Rover).

Takeoff (either from ground or by hand-launch). It should be the first
command of nearly all Plane and Copter missions.
[/site]

[site wiki="copter" heading="off"]

Copter
~~~~~~

The vehicle will climb straight up from it’s current location to the
specified altitude. If the mission is begun while the copter is already
flying, the vehicle will climb straight up to the specified altitude, if
the vehicle is already above the altitude the command will be ignored
and the mission will move onto the next command immediately.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param1</strong></td>
   <td>Grade %</td>
   <td>Pitch/climb angle (Plane only).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td>   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td>
   </td>
   <td>Yaw angle (ignored if compass not present).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Longitude</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/TakeOff.jpg
   :target: ../_images/TakeOff.jpg

   Copter: Mission Planner Settings for TAKEOFF command

[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

The plane climbs to the specified altitude (at the specified pitch/climb
angle) before proceeding to the next waypoint.

The plane is *only* attempting to climb at this point and can be pushed
off its heading by wind. The pitch value is the *minimum* climb angle
the when using an airspeed sensor, and *maximum* angle when **not**
using an airspeed sensor.

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Pitch Angle</td>
   <td>Pitch/climb angle in degrees (Plane only).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Yaw angle (ignored if compass not present).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Longitude</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude</td>
   </tr>
   </tbody>
   </table>

.. _mav_cmd_nav_vtol_takeoff:


MAV_CMD_NAV_VTOL_TAKEOFF
------------------------

Supported by:  Plane  (not Copter or Rover). Specifically QuadPlanes.

Takeoff while in VTOL mode.


QuadPlane
~~~~~~~~~

The vehicle will climb straight up at it’s current location to the
specified altitude as a delta above its current altitude. 

However, if :ref:`Q_OPTIONS<Q_OPTIONS>` bit 3 is set (use altitude reference frames for VTOL takeoff), then the altitude value (in the specified reference frame) will be used for the target altitude, instead of a delta above current altitude. If the command is begun while the vehicle is already flying, the vehicle will climb straight up to the specified altitude, if
the vehicle is already above the altitude the command will be ignored
and the mission will move onto the next command immediately.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td>
   </td>
   <td></td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Longitude</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude</td>
   </tr>
   </tbody>
   </table>


[/site]

.. _mav_cmd_nav_loiter_unlim:

MAV_CMD_NAV_LOITER_UNLIM
------------------------

Supported by: Copter, Plane, Rover.

Loiter at the specified location for an unlimited amount of time.

[site wiki="copter" heading="off"]

Copter
~~~~~~

Fly to the specified location and then loiter there indefinitely — where
loiter means "wait in place" (rather than "circle"). If zero is
specified for a latitude/longitude/altitude parameter then the current
location value for the parameter will be used.

The mission will not proceed past this command while in AUTO mode. In
order to break out of this command you need to change the mode (i.e. to
MANUAL). If there are subsequent commands then you can continue the
mission at the next command, if the Copter ``MIS_RESTART`` parameter is
set to resume, by switching back to AUTO mode (otherwise the mission
will restart).

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param3</strong></td>
   <td></td>
   <td>Radius around MISSION, in meters. If positive loiter clockwise, else counter-clockwise</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Desired yaw angle.</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the vehicle will loiter at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude. If zero, the vehicle will loiter at the current longitude.</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target altitude. If zero, the vehicle will loiter at the current altitude.</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_LoiterUnlimited.png
   :target: ../_images/MissionList_LoiterUnlimited.png

   Copter: Mission Planner Settings forLOITER_UNLIM command

[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

Fly to the specified location and then loiter there indefinitely — where
loiter means "circle the waypoint". If zero is specified for a
latitude/longitude/altitude parameter then the current location value
for the parameter will be used. You can also specify the radius and
direction for the loiter.

The mission will not proceed past this command while in AUTO mode. In
order to break out of this command you need to change the mode (i.e. to
MANUAL). If there are subsequent commands then you can continue the
mission at the next command, if the Copter ``MIS_RESTART`` parameter is
set to resume, by switching back to AUTO mode (otherwise the mission
will restart).

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Dir 1=CW</td>
   <td>Radius around waypoint, in meters. Specify as a positive value to loiter clockwise, negative to move counter-clockwise.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Desired yaw angle.</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the vehicle will loiter at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude. If zero, the vehicle will loiter at the current longitude.</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target altitude. If zero, the vehicle will loiter at the current altitude.</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_nav_loiter_turns:

[site wiki="copter,plane"]
MAV_CMD_NAV_LOITER_TURNS
------------------------

Supported by: Copter, Plane (not Rover).

Loiter (circle) the specified location for a specified number of turns.
[/site]
[site wiki="copter" heading="off"]

Copter
~~~~~~

Loiter (circle) the specified location for a specified number of turns,
and then proceed to the next command. If zero is specified for a
latitude/longitude/altitude parameter then the current location value
for the parameter will be used.

The radius of the circle is controlled by the
:ref:`CIRCLE_RADIUS <copter:CIRCLE_RADIUS>`
parameter (i.e. cannot be set as part of the command).

This is the command equivalent of the :ref:`Circle flight mode <copter:circle-mode>`.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Turns</td>
   <td>Number of turns (N x 360°)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param3</strong></td>
   <td>Radius</td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the vehicle will loiter at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude. If zero, the vehicle will loiter at the current longitude.</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target altitude. If zero, the vehicle will loiter at the current altitude.</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_LoiterTurns.png
   :target: ../_images/MissionList_LoiterTurns.png

   Copter: Mission Planner Settings for LOITER_TURNS command

[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

Loiter (circle) the specified location for a specified number of turns
at the given radius, and then proceed to the next command. If zero is
specified for a latitude/longitude/altitude parameter then the current
location value for the parameter will be used.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Turns</td>
   <td>Number of turns (N x 360°)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Radius</td>
   <td>Radius around waypoint, in meters. Specify as a positive value to loiter clockwise, negative to move counter-clockwise.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Desired yaw angle.</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the vehicle will loiter at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude. If zero, the vehicle will loiter at the current longitude.</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target altitude. If zero, the vehicle will loiter at the current altitude.</td>
   </tr>
   </tbody>
   </table>

[/site]


.. _mav_cmd_nav_loiter_time:

MAV_CMD_NAV_LOITER_TIME
-----------------------

Supported by: Copter, Plane, Rover.

Loiter at the specified location for a set time (in seconds).

[site wiki="copter" heading="off"]

Copter
~~~~~~

Fly to the specified location and then loiter there for the specified
number of seconds — where loiter means "wait in place" (rather than
"circle"). The timer starts when the waypoint is reached; when it
expires the waypoint is complete. If zero is specified for a
latitude/longitude/altitude parameter then the current location value
for the parameter will be used.

This is the mission equivalent of the :ref:`Loiter flight mode <copter:loiter-mode>`.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Time s</td>
   <td>Time to loiter at waypoint (seconds - decimal)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param3</strong></td>
   <td>Dir 1=CW</td>
   <td>Radius around waypoint, in meters. Specify as a positive value to loiter clockwise, negative to move counter-clockwise.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Desired yaw angle.</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the vehicle will loiter at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude. If zero, the vehicle will loiter at the current longitude.</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target altitude. If zero, the vehicle will loiter at the current altitude.</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_LoiterTime.png
   :target: ../_images/MissionList_LoiterTime.png

   Copter: Mission Planner Settings forLOITER_TIME command

[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

Fly to the specified location and then loiter there for the specified
number of seconds — where loiter means "circle the waypoint". The timer
starts when the waypoint is reached; when it expires the waypoint is
complete. If zero is specified for a latitude/longitude/altitude
parameter then the current location value for the parameter will be
used. You can also specify the radius and direction for the loiter.

The radius of loiter is set in the ``WP_LOITER_RAD`` parameter.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Time (s)</td>
   <td>Time to loiter at waypoint (seconds - decimal)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Dir 1=CW</td>
   <td>Radius around waypoint, in meters. Specify as a positive value to loiter clockwise, negative to move counter-clockwise.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td>
   </td>
   <td>Desired yaw angle.</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the vehicle will loiter at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target longitude. If zero, the vehicle will loiter at the current longitude.</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target altitude. If zero, the vehicle will loiter at the current altitude.</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_nav_return_to_launch:

MAV_CMD_NAV_RETURN_TO_LAUNCH
----------------------------

Supported by: Copter, Plane, Rover.

Return to the *home location* or the nearest `Rally
Point <common-rally-points>`__, if closer. The home location is where
the vehicle was last armed (or when it first gets GPS lock after arming
if the vehicle configuration allows this).

[site wiki="copter" heading="off"]


Copter
~~~~~~

Return to the *home location* (or the nearest :ref:`Rally Point <common-rally-points>` if closer) and then land. The home
location is where the vehicle was last armed (or when it first gets GPS
lock after arming if the vehicle configuration allows this).

This is the mission equivalent of the :ref:`RTL flight mode <copter:rtl-mode>`.  The vehicle will
first climb to the
:ref:`RTL_ALT <copter:RTL_ALT>`
parameter's specified altitude (default is 15m) before returning home.

This command takes no parameters and generally should be the last
command in the mission.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_RTL.png
   :target: ../_images/MissionList_RTL.png

   Copter: Mission PlannerSettings for RETURN_TO_LAUNCH command

[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

Return to the *home location* (or the nearest :ref:`Rally Point <common-rally-points>` if closer) and then "Loiter" (circle the
point). The home location is where the vehicle was last armed (or when
it first gets GPS lock after arming if the vehicle configuration allows
this).

If the return is to a rally point, the plane will loiter at the position
and altitude set in the rally point. If the return is to the home
location, then the parameter
:ref:`ALT_HOLD_RTL <plane:ALT_HOLD_RTL>`
is used for the loiter height (default 100m). The radius of the loiter
is defined in the parameter
:ref:`WP_LOITER_RAD <plane:WP_LOITER_RAD>`.

This command takes no parameters and generally should be the last
command in the mission.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]

[site wiki="rover" heading="off"]

Rover
~~~~~

Return to the *home location* (Rally Points are not supported on Rover).

This command takes no parameters and generally should be the last
command in the mission.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_nav_land:

[site wiki="copter,plane"]

MAV_CMD_NAV_LAND
----------------

Supported by: Copter, Plane (not Rover).

Land the vehicle at the current or a specified location.
[/site]
[site wiki="copter"]

Copter
~~~~~~

The copter will land at its current location or proceed at current altitude to the lat/lon
coordinates provided (if non-zero) and land.  This is the mission equivalent of
the :ref:`LAND flight mode <copter:land-mode>`.

The motors will not stop on their own: you must exit AUTO mode to cut
the engines.


**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Desired yaw angle.</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the Copter will land at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Longitude</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_Land.png
   :target: ../_images/MissionList_Land.png

   Copter: Mission Planner Settings for LANDcommand

[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

The plane will land at its current location or proceed to the (non-zero)
lat/lon coordinates provided beginning with current altitude.  Information on the parameters used to
control the landing are provided in :ref:`LAND flight mode <plane:land-mode>`.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Abort Alt</td>
   <td>Altitude to climb to if landing is aborted (From Plane 3.4)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Desired yaw angle.</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Long</td>
   <td>Longitude</td>
   </tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude to target for the landing. Unless you are landing at a location different than home, this should be zero</td>
   </tr>
   </tbody>
   </table>

.. _mav_cmd_nav_vtol_land:

MAV_CMD_NAV_VTOL_LAND
---------------------

Supported by: Plane (not Copter or Rover). Specifically Quadplanes.

Land the vehicle at the current or a specified location.

QuadPlane
~~~~~~~~~

If the :ref:`Q_OPTIONS<Q_OPTIONS>` bit 4 is not set (default),the vehicle will land at its current location or proceed at current altitude to the lat/lon
coordinates provided (if non-zero) and land. The ALT parameter is used to determine final landing phase initiation rather than :ref:`Q_LAND_FINAL_ALT<Q_LAND_FINAL_ALT>` . This is the mission equivalent of the :ref:`QLAND flight mode <plane:qland-mode>`.

If the :ref:`Q_OPTIONS<Q_OPTIONS>` bit 4 is set (Use a fixed wind approach), the it will fly in plane mode to the lat/lon coordinates provided (if non-zero), climbing or descending to the altitude set in the NAV_VTOL_LAND waypoint. When it reaches within :ref:`Q_FW_LND_APR_RAD<Q_FW_LND_APR_RAD>` of the landing location, it will perform a LOITER_TO_ALT to finish the climb or descent to that ALT set in the waypoint, then, turning into the wind, transition to VTOL mode and proceed to the landing location and land.

The motors will disarm on their own once landed


**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param4</strong></td>
   <td></td>
   <td>Desired yaw angle.</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target latitude. If zero, the Quadplane will land at the current latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Longitude</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude</td>
   </tr>
   </tbody>
   </table>


[/site]


.. _mav_cmd_nav_continue_and_change_alt:

[site wiki="plane" heading="off"]
MAV_CMD_NAV_CONTINUE_AND_CHANGE_ALT
-----------------------------------

Supported by: Plane (not Copter, Rover).

Continue on the current course and climb/descend to specified altitude.
Move to the next command when the desired altitude is reached.


Plane
~~~~~

Continue on the current course and climb/descend to specified altitude.
Move to the next command when the desired altitude is reached.

.. note::

   In Plane 3.4 (and later) the ``param1`` value sets how close the
   vehicle altitude must be to target altitude for command
   completion.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>TBD</td>
   <td>Climb or Descend (0 = Neutral, command completes when within 5m of this
   command's altitude, 1 = Climbing, command completes when at or above
   this command's altitude, 2 = Descending, command completes when at or
   below this command's altitude. Introduced in Plane 3.4.
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td>param7</td>
   <td>Alt</td>
   <td>Target altitude</td>
   </tr>
   </tbody>
   </table>

[/site]


.. _mav_cmd_nav_spline_waypoint:
[site wiki="copter"]
MAV_CMD_NAV_SPLINE_WAYPOINT
---------------------------

Supported by: Copter (not Plane or Rover).

Navigate to the target location using a spline path.


Copter
~~~~~~

Fly to the target location using a `Spline path <https://en.wikipedia.org/wiki/Spline_%28mathematics%29>`__, then
wait (hover) for specified time before proceeding to the next command.

The Spline commands take all the same arguments are regular waypoints
(lat, lon, alt, delay) but when executed the vehicle will fly smooth
paths (both vertically and horizontally) instead of straight lines. 
Spline waypoints can be mixed with regular straight line waypoints as
shown in the screenshot below.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td>param1</td>
   <td>Delay</td>
   <td>Hold time at target, in decimal seconds.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td>param5</td>
   <td>Lat</td>
   <td>Latitude/X of goal</td>
   </tr>
   <tr>
   <td>param6</td>
   <td>Long</td>
   <td>Longitude/Y of goal</td>
   </tr>
   <tr>
   <td>param7</td>
   <td>Alt</td>
   <td>Altitude/Z of goal</td>
   </tr>
   </tbody>
   </table>

.. figure:: ../../../images/MissionList_SplineWaypoint.jpg
   :target: ../_images/MissionList_SplineWaypoint.jpg

   Copter: Mission Planner Settingsfor SPLINE_WAYPOINT command

The Mission Planner screenshot shows the path the vehicle will take.

-  The 1 second delay at the end of Waypoint #1 causes the vehicle to
   stop so Spline command #2 begins by taking a sharp 90degree turn
-  The direction of travel as the vehicle passes through Spline Waypoint
   #3 is parallel to an imaginary line drawn between waypoints #2 and #4
-  Waypoint #5 is a straight line so the vehicle lines itself up to
   point towards waypoint #5 even before reaching waypoint #4.



.. _mav_cmd_nav_guided_enable:

MAV_CMD_NAV_GUIDED_ENABLE
-------------------------

Supported by: Copter (not Plane or Rover).

Enable ``GUIDED`` mode to hand over control to an external controller/:ref:`common-companion-computers`. The :ref:`common-companion-computers`  would then send MAVLink commands to control the vehicle.

See also :ref:`MAV_CMD_DO_GUIDED_LIMITS <mav_cmd_do_guided_limits>`
for information on how to apply time, altitude and distance limits on
the external control.

Copter
~~~~~~

Enable ``GUIDED`` mode to hand over control to an external controller. See :ref:`Guided Mode <copter:ac2_guidedmode>` for more information.


**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>on=1/off=0</td>
   <td>A value of > 0.5 enables GUIDED mode. Any value <= 0.5f turns it off.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>
[/site]


.. _mav_cmd_nav_altitude_wait:

[site wiki="plane"]
MAV_CMD_NAV_ALTITUDE_WAIT
-------------------------

Supported by: Plane (not Copter or Rover).

Mission command to wait for an altitude or downwards vertical speed.


Plane
~~~~~

Mission command to wait for an altitude or downwards vertical speed.
This is meant for high altitude balloon launches, allowing the aircraft
to be idle until either an altitude is reached or a negative vertical
speed is reached (indicating early balloon burst). The wiggle time is
how often to wiggle the control surfaces to prevent them seizing up.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>?</td>
   <td>Altitude (m)</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>?</td>
   <td>Descent speed (m/s)</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>?</td>
   <td>Wiggle Time (s)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>


.. _mav_cmd_nav_loiter_to_alt:

MAV_CMD_NAV_LOITER_TO_ALT
-------------------------

Supported by: Plane (not Copter or Rover).

Plane
~~~~~

Loiter while climbing/descending to an altitude.

Begin loiter at the specified Latitude and Longitude. If Lat=Lon=0, then
loiter at the current position. Don't consider the navigation command
complete (don't leave loiter) until the altitude has been reached.
Additionally, if the Heading Required parameter is non-zero the aircraft
will not leave the loiter until heading toward the next waypoint.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Heading</td>
   <td>Heading Required (0 = False)</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>Radius</td>
   <td>Radius in meters. If positive loiter clockwise, negative counter-clockwise, 0 means no change to standard loiter.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude.</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Long</td>
   <td>Longitude</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_do_jump:

MAV_CMD_DO_JUMP
---------------

Supported by: Copter, Plane, Rover.

Jump to the specified command in the mission list. The jump command can
be repeated either a specified number of times before continuing the
mission, or it can be repeated indefinitely.

.. tip::

   Despite the name, this command is really a "NAV\_" command rather
   than a "DO\_" command. Conditional commands like CONDITION_DELAY don't
   affect DO_JUMP (it will always perform the jump as soon as it reaches
   the command).

.. note::

   -  There can be a maximum of 15 jump commands in a mission after which new DO_JUMP commands are ignored.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>WP#</td>
   <td>The command index/sequence number of the command to jump to.</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>Repeat#</td>
   <td>Number of times that the DO_JUMP command will execute before moving to
   the next sequential command. If the value is zero the next command will
   execute immediately. A value of -1 will cause the command to repeat
   indefinitely.
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoJump.png
   :target: ../_images/MissionList_DoJump.png

   Copter: Mission Planner Settings for DO_JUMP command

In the example above the vehicle would fly back-and-forth between
waypoints #1 and #2 a total of 3 times before flying on to waypoint #4.

Conditional commands
====================

Conditional commands control the execution of \_DO\_ commands. For
example, a conditional command can prevent DO commands executing based
on a time delay, until the vehicle is at a certain altitude, or at a
specified distance from the next target position.

A conditional command may not complete before reaching the next
waypoint. In this case, any unexecuted \_DO\_ commands associated with
the last waypoint will be skipped.

.. _mav_cmd_condition_delay:

MAV_CMD_CONDITION_DELAY
-----------------------

Supported by: Copter, Plane, Rover.

After reaching a waypoint, delay the execution of the next conditional
"_DO_" command for the specified number of seconds (e.g.
:ref:`MAV_CMD_DO_SET_ROI <mav_cmd_do_set_roi>`).

.. note::

   This command does not stop the vehicle. If the vehicle reaches the
   next waypoint before the delay timer completes, the delayed "_DO_"
   commands will never trigger.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Time (sec)</td>
   <td>Delay in seconds (decimal).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_ConditionDelay.png
   :target: ../_images/MissionList_ConditionDelay.png

   Copter: Mission Planner Settingsfor CONDITION_DELAY command

In the example above, Command #4 (``DO_SET_ROI``) is delayed so that it
starts 5 seconds after the vehicle has passed Waypoint #2.


.. _mav_cmd_condition_distance:

MAV_CMD_CONDITION_DISTANCE
--------------------------

Supported by: Copter, Plane, Rover.

Delays the start of the next "``_DO_``\ " command until the vehicle is
within the specified number of meters of the next waypoint.

.. note::

   This command does not stop the vehicle: it only affects DO
   commands.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Dist (m)</td>
   <td>Distance from the next waypoint before DO commands are executed (meters).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_ConditionDistance.png
   :target: ../_images/MissionList_ConditionDistance.png

   Copter: Mission PlannerSettings for CONDITION_DISTANCE command

In the example above, Command #4 (``DO_SET_ROI``) is delayed so that it
only starts once the vehicle is within 50m of waypoint #5.


.. _mav_cmd_condition_yaw:

[site wiki="copter" heading="off"]

MAV_CMD_CONDITION_YAW
---------------------

Supported by: Copter (not Plane or Rover).

Point (yaw) the nose of the vehicle towards a specified heading.


Copter
~~~~~~

Point (yaw) the nose of the vehicle towards a specified heading.

The parameters allow you to specify whether the target direction is
absolute or relative to the current yaw direction. If the direction is
relative you can also (separately) specify whether the value is added or
subtracted from the current heading (note that the vehicle will always
turn in direction that most quickly gets it to the new target heading
regardless of the ``param3`` value).


**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Deg</td>
   <td>
   If <code>param4=0</code> (absolute): Target heading in degrees [0-360] (0 is North).

   If <code>param4=1</code> (relative): The change in heading (in degrees).
   </td>
   </tr>
   <td><strong>param2</strong></td>
   <td>Sec</td>
   <td>Speed during yaw change:[deg per second].</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Dir 1=CW</td>
   <td>If <code>param4=1</code> (relative) only: [-1 = CCW, +1 = CW]. This denotes
   whether the autopilot should add (CW) or subtract (CCW) the
   degrees (``param1``) from the current heading to calculate the target heading.
   </td>
   </tr>
   <tr>
   <td><strong>param4</strong></td>
   <td>rel/abs</td>
   <td>Specify if <code>param1</code> ("Deg" field) is an absolute direction (0) or a relative to the current yaw direction (1).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_ConditionYaw.png
   :target: ../_images/MissionList_ConditionYaw.png

   Copter: Mission Planner Settingsfor CONDITION_YAW command

[/site]
[site wiki="copter" heading="off"]

Special Commands
================

This section is for commands that may be relevant to missions, but but
which are not mission commands (part of the mission).

.. _mav_cmd_mission_start:

MAV_CMD_MISSION_START
---------------------

Supported by: Copter

Start running the current mission. This allows a GCS/companion computer
to start a mission in AUTO without raising the throttle.

Copter
~~~~~~

This command can be used to start a mission when the Copter is on the
ground in AUTO mode. If the vehicle is already in the air then the
mission will start as soon as you switch into AUTO mode (so this command
is not needed/ignored).

.. note::

   Previously a mission would only start after the pilot engaged the
   throttle. This command makes it possible to start missions without
   directly controlling the throttle (though that approach is still
   available).

This is not a "mission command" (it can't be used as a type of mission
waypoint). It is run from the **Action** menu (see the screenshot
below).

**Command parameters**

The parameters are all ignored.

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   
   <tr style="color: #c0c0c0">
   <td><strong>param1</strong></td>
   <td></td>
   <td>The first mission item to run.</td>
   </tr>
   
   <tr style="color: #c0c0c0">
   <td><strong>param2</strong></td>
   <td></td>
   <td>The last mission item to run (after this item is run, the mission ends).</td>
   </tr>
   
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td></td>
   </tr>
   
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td></td>
   </tr>
   
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td></td>
   </tr>
   
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td></td>
   </tr>
   
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td></td>
   </tr>
   </tbody>
   </table>

**Mission Planner screenshots**

.. figure:: ../../../images/MissionPlanner_MissionStartCommand.jpg
   :target: ../_images/MissionPlanner_MissionStartCommand.jpg

   Mission Planner: MISSION_STARTcommand

.. _mav_cmd_component_arm_disarm:

MAV_CMD_COMPONENT_ARM_DISARM
--------------------------------

Supported by: Copter

Disarm the motors.

Copter
~~~~~~

Disarm the motors.

The command supports disarming on the ground and in flight.

.. note::

   The motors will disarm automatically after landing.

This is not a "mission command" (it can't be used as a type of mission
waypoint).

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td></td>
   <td>1 to arm, 0 to disarm. This only works when the vehicle is on the ground.
   </td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td></td>
   <td>A value of 21196 will disarm the vehicle in flight.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td></td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td></td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td></td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td></td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td></td>
   </tr>
   </tbody>
   </table>
[/site]

DO commands
===========

The "DO" or "Now" commands are executed once to perform some action. All
the DO commands associated with a waypoint are executed immediately.


.. _mav_cmd_do_set_mode:

MAV_CMD_DO_SET_MODE
-------------------

Supported by: Copter, Plane, Rover.

Set system mode (preflight, armed, disarmed etc.)


**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td></td>
   <td>Mode, as defined by `MAV_MODE <https://mavlink.io/en/messages/common.html#MAV_MODE>`__</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param2</strong></td>
   <td></td>
   <td>Custom mode - this is system specific, please refer to the individual autopilot specifications for details.</td>
   </tr>
   <tr>
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

   
.. _mav_cmd_do_change_speed:

MAV_CMD_DO_CHANGE_SPEED
-----------------------

Supported by: Copter, Plane, Rover.

Change the target horizontal speed and/or throttle of the vehicle. In most cases, the
changes will be used until they are explicitly changed again or the
device is rebooted.

[site wiki="copter" heading="off"]

Copter
~~~~~~

Sets the desired maximum speed in meters/second (only). Both the
speed-type and throttle settings are ignored.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param1</strong></td>
   <td>speed m/s</td>
   <td>Speed type (0=Airspeed, 1=Ground Speed).</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>speed m/s</td>
   <td>Target speed (m/s).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Throttle as a percentage (0-100%). A value of -1 indicates no change.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoChangeSpeed.png
   :target: ../_images/MissionList_DoChangeSpeed.png

   Copter: Mission Planner Settingsfor DO_CHANGE_SPEED command

[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

Change the target horizontal speed (airspeed or groundspeed) and/or the
vehicle's throttle. If airspeed, this changes the :ref:`TRIM_ARSPD<TRIM_ARSPD_CM>` parameter during the flight until reboot or mode is changed to CRUISE or FBWB. If groundspeed option is used, then :ref:`MIN_GNDSPD<MIN_GNDSPD_CM>` parameter is changed to this value until rebooted or changed by this command again. If the throttle field is non-zero and equal to or below 100, then the :ref:`TRIM_THROTTLE<TRIM_THROTTLE>` parameter is changed  until reboot or changed by this command again.

.. note:: Speed changes only have effect if an airspeed sensor is present, healthy, and in use. :ref:`TRIM_THROTTLE<TRIM_THROTTLE>` changes impacts only flight with airspeed sensor not in use.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Type (0=as 1=gs)</td>
   <td>Speed type (0=Airspeed, 1=Ground Speed).</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>Speed (m/s)</td>
   <td>Target speed (m/s). A value of -1 indicates no change.</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Throttle(%)</td>
   <td>Throttle as a percentage (0-100%). A value of -1 indicates no change.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]

[site wiki="rover" heading="off"]

Rover
~~~~~

Change the target horizontal speed and/or the vehicle's throttle.

The value of ``param1`` is ignored from v2.50 (earlier versions should
set to 0).

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param1</strong></td>
   <td>Type (0=as 1=gs)</td>
   <td>Speed type (0=Airspeed, 1=Ground Speed). Set to 0 before v2.50, otherwise ignored.</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>Speed (m/s)</td>
   <td>Target speed (m/s). A value of -1 indicates no change.</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Throttle(%)</td>
   <td>Throttle as a percentage (0-100%). A value of -1 indicates no change.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_do_set_home:

MAV_CMD_DO_SET_HOME
-------------------

Supported by: Copter, Plane, Rover.

Sets the home location either as the current location or at the location
specified in the command.For SITL work, altitude input here needs to be with reference to absolute altitude, taking into account SRTM elevation.

.. note::

   -  For Plane and Rover, if a good GPS fix cannot be obtained the
      location specified in the command is used.
   -  For Copter, the command will also try to use the current position if
      all the location parameters are set to 0. The location information in
      the command is only used if it is close to the EKF origin.

[site wiki="copter" heading="off"]

[/site]

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Current</td>
   <td>Set home location:

   1=Set home as current location.

   0=Use location specified in message parameters.
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Target home latitude (if ``param1=2``)</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Lon</td>
   <td>Target home longitude (if ``param1=2``)</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Target home altitude (if ``param1=2``)</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoSetHome.png
   :target: ../_images/MissionList_DoSetHome.png

   Copter: Mission Planner Settings forDO_SET_HOME command

.. _mav_cmd_do_set_relay:

MAV_CMD_DO_SET_RELAY
------------------------

Supported by: Copter, Plane, Rover.

Set a `Relay <common-relay>`__ pin's voltage high (on) or low (off).


**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Relay No</td>
   <td>Relay number.</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>off(0)/on(1)</td>
   <td>Set relay state:

   1: Set relay high/on (3.3V on Pixhawk, 5V on APM).

   0: Set relay low/off (0v)
   any other value toggles the relay
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionPlanner_DO_SET_RELAY.png
   :target: ../_images/MissionPlanner_DO_SET_RELAY.png

   Copter: MissionPlanner Settings for DO_SET_RELAY command

.. _mav_cmd_do_repeat_relay:

MAV_CMD_DO_REPEAT_RELAY
---------------------------

Supported by: Copter, Plane, Rover.

Toggle the :ref:`Relay <common-relay>` pin's voltage/state a specified
number of times with a given period. Toggling the Relay will turn an off
relay on and vice versa

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Relay No</td>
   <td>Relay number.</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>Repeat #</td>
   <td>Cycle count - the number of times the relay should be toggled</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Delay(s)</td>
   <td>Cycle time (seconds, decimal) - time between each toggle.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoRepeatRelay.png
   :target: ../_images/MissionList_DoRepeatRelay.png

   Copter: Mission Planner Settingsfor DO_RELAY_REPEAT command

In the example above, assuming the relay was off to begin with, it would
be set high and then after 3 seconds it would be toggled low again.

.. _mav_cmd_do_set_servo:

MAV_CMD_DO_SET_SERVO
--------------------

Supported by: Copter, Plane, Rover.

Set a given :ref:`servo pin <common-servo>` output to a specific PWM value.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Ser No</td>
   <td>Servo number - target servo output pin/channel number.</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>PWM</td>
   <td>PWM value to output, in microseconds (typically 1000 to 2000).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoSetServo.png
   :target: ../_images/MissionList_DoSetServo.png

   Copter: Mission Planner Settingsfor DO_SET_SERVO command

In the example above, the servo attached to output channel 8 would be
moved to PWM 1700 (servos generally accept PWM values between 1000 and
2000).

.. note:: as of firmware versions 4.0 and later, this command can be used on any output configured by its ``SERVOx_FUNCTION`` command as 0,1, or 51-66  (disabled or RC pass-throughs)

.. _mav_cmd_do_repeat_servo:

MAV_CMD_DO_REPEAT_SERVO
-----------------------

Supported by: Copter, Plane, Rover.

Cycle a :ref:`servo <common-servo>` PWM output pin between its mid-position
value and a specified PWM value, for a given number of cycles and with a
set period.

The mid-position value is specified in the ``RCn_TRIM`` parameter for
the channel (``RC8_TRIM`` in the screenshot below). The default value is
1500..

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Ser No</td>
   <td>Servo number - target servo output pin/channel.</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>PWM</td>
   <td>PWM value to output, in microseconds (typically 1000 to 2000).</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Repeat #</td>
   <td>Cycle count - number of times to move the servo to the specified PWM value</td>
   </tr>
   <tr>
   <td><strong>param4</strong></td>
   <td>Delay (s)</td>
   <td>Cycle time (seconds) - the delay in seconds between each servo movement.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoRepeatServo.png
   :target: ../_images/MissionList_DoRepeatServo.png

   Copter: Mission Planner Settingsfor DO_REPEAT_SERVO command

In the example above, the servo attached to output channel 8 would be
moved to PWM 1700, then after 4 second, back to mid, after another 4
seconds it would be moved to 1700 again, then finally after 4 more
seconds it would be moved back to mid.

.. _mav_cmd_do_land_start:

[site wiki="plane" heading="off"]

MAV_CMD_DO_LAND_START
---------------------

Supported by: Plane (not Copter, Rover).

Mission command to prepare for a landing.

This is used as a marker in a mission to tell the autopilot where a
sequence of mission items that represents a landing starts. It may also
be sent via a ``COMMAND_LONG`` to trigger a landing, in which case the
nearest (geographically) landing sequence in the mission will be used.

If ``RTL_AUTOLAND`` is set to 2, the plane will jump to the nearest 
``DO_LAND_START`` in the mission table when RTL is initialized. 

.. note::

   General information on landing a plane is provided in the topic
   :ref:`Automatic Landing <plane:automatic-landing>`.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param1</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude used to help find the closest landing sequence, or zero if not needed.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param6</strong></td>
   <td>Long</td>
   <td>Longitude used to help find the closest landing sequence, or zero if not needed.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

.. _mav_cmd_do_vtol_transition:

MAV_CMD_DO_VTOL_TRANSITION
--------------------------

Supported by: Plane (not Copter or Rover).Specifically Quadplanes.

QuadPlane
~~~~~~~~~

Mission command to change to/from VTOL and fixed wing mode of flight. The mode is changed based on the first parameter: 3 = change to VTOL flight, 4 = change to fixed wing flight.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Mode</td>
   <td>3=VTOL,4=Fixed-Wing</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td>
   </td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_do_set_roi:

MAV_CMD_DO_SET_ROI
------------------

Supported by: Copter, Plane, Rover.

Sets the region of interest (ROI) for a sensor set or the vehicle
itself. This can then be used by the vehicles control system to control
the vehicle attitude and the attitude of various sensors such as
cameras.

[site wiki="copter" heading="off"]

Copter
~~~~~~

Points the :ref:`camera gimbal <common-cameras-and-gimbals>` at the "region
of interest", and also rotates the nose of the vehicle if the
mount type does not support a yaw feature.

After setting the ROI, the camera/vehicle will continue to follow it
until the end of the mission, unless it is changed or cleared by setting
another ROI. Clearing the ROI is achieved by setting a later
DO_SET_ROI command with all zero for ``param5``-``param7`` (Lat, Lon
and Alt).

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param1</strong></td>
   <td></td>
   <td>Region of interest mode. (see MAV_ROI enum) // 0 = no roi, 1 = next
   waypoint, 2 = waypoint number, 3 = fixed location, 4 = given target (not supported)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param2</strong></td>
   <td></td>
   <td>MISSION index/ target ID. (see MAV_ROI enum)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param3</strong></td>
   <td></td>
   <td>ROI index (allows a vehicle to manage multiple ROI's)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude (x) of the fixed ROI</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Long</td>
   <td>Longitude (y) of the fixed ROI</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude of the fixed ROI</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots/video**

.. figure:: ../../../images/MissionList_DoSetRoi.jpg
   :target: ../_images/MissionList_DoSetRoi.jpg

   Copter: Mission Planner Settings forDO_SET_ROI command

In the example above the nose and camera would be pointed at the red
marker.

..  youtube:: W8NCFHrEjfU
    :width: 100%

[/site]

[site wiki="plane" heading="off"]

Plane
~~~~~

Points the :ref:`camera gimbal <common-cameras-and-gimbals>` at the "region
of interest".

After setting the ROI, the camera will continue to follow it until the
end of the mission, unless it is changed or cleared by setting another
ROI. Clearing the ROI is achieved by setting a later DO_SET_ROI
command with all zero for ``param5``-``param7`` (Lat, Lon and Alt).


**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param1</strong></td>
   <td></td>
   <td>Region of interest mode. (see MAV_ROI enum) // 0 = no roi, 1 = next
   waypoint, 2 = waypoint number, 3 = fixed location, 4 = given target (not supported)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param2</strong></td>
   <td>
   </td>
   <td>MISSION index/ target ID. (see MAV_ROI enum)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param3</strong></td>
   <td></td>
   <td>ROI index (allows a vehicle to manage multiple ROI's)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude (x) of the fixed ROI</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Long</td>
   <td>Longitude (y) of the fixed ROI</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude of the fixed ROI</td>
   </tr>
   </tbody>
   </table>

[/site]

[site wiki="rover" heading="off"]

Rover
~~~~~

Points the :ref:`camera gimbal <common-cameras-and-gimbals>` at the "region
of interest".

After setting the ROI, the camera will continue to follow it until the
end of the mission, unless it is changed or cleared by setting another
ROI. Clearing the ROI is achieved by setting a later DO_SET_ROI
command with all zero for ``param5``-``param7`` (Lat, Lon and Alt).

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param1</strong></td>
   <td></td>
   <td>Region of interest mode. (see MAV_ROI enum) // 0 = no roi, 1 = next
   waypoint, 2 = waypoint number, 3 = fixed location, 4 = given target (not supported)
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param2</strong></td>
   <td></td>
   <td>MISSION index/ target ID. (see MAV_ROI enum)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param3</strong></td>
   <td></td>
   <td>ROI index (allows a vehicle to manage multiple ROI's)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Lat</td>
   <td>Latitude (x) of the fixed ROI</td>
   </tr>
   <tr>
   <td><strong>param6</strong></td>
   <td>Long</td>
   <td>Longitude (y) of the fixed ROI</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Alt</td>
   <td>Altitude of the fixed ROI</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_do_digicam_configure:

MAV_CMD_DO_DIGICAM_CONFIGURE
----------------------------

Supported by: Copter, Plane, Rover.

Configure an on-board camera controller system.

The parameters are forwarded to an on-board camera controller system
(like the `3DR Camera Control Board <common-camera-control-board>`__),
if one is present.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Mode</td>
   <td>Set camera mode:
   1: ProgramAuto

   2: Aperture Priority

   3: Shutter Priority

   4: Manual

   5: IntelligentAuto

   6: SuperiorAuto
   </td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>Shutter Speed</td>
   <td>Shutter speed (seconds divisor). So if the speed is 1/60 seconds, the
   value entered would be 60. Slowest shutter trigger supported is 1 second.
   </td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>Aperture</td>
   <td>Aperture: F stop number</td>
   </tr>
   <tr>
   <td><strong>param4</strong></td>
   <td>ISO</td>
   <td>ISO number e.g. 80, 100, 200, etc.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param5</strong></td>
   <td>ExposureMode</td>
   <td>Exposure type enumerator</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param6</strong></td>
   <td>CommandID</td>
   <td>Command Identity</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td>Engine Cut-Off</td>
   <td>Main engine cut-off time before camera trigger in seconds/10 (0 means no cut-off).</td>
   </tr>
   </tbody>
   </table>

.. _mav_cmd_do_digicam_control:

MAV_CMD_DO_DIGICAM_CONTROL
--------------------------

Supported by: Copter, Plane, Rover.

Trigger the :ref:`camera shutter <common-camera-shutter-with-servo>` once. This command takes no additional arguments.

**Command parameters**

In general if a command field is set to 0 it is ignored.

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>On/Off</td>
   <td>Session control (on/off or show/hide lens):
   
   0: Turn off the camera / hide the lens

   1: Turn on the camera /Show the lens
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param2</strong></td>
   <td>Zoom Position</td>
   <td>Zoom's absolute position. 2x, 3x, 10x, etc.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param3</strong></td>
   <td>Zoom Step</td>
   <td>Zooming step value to offset zoom from the current position</td>
   </tr>
   <tr>
   <td><strong>param4</strong></td>
   <td>Focus Lock</td>
   <td>Focus Locking, Unlocking or Re-locking:
   0: Ignore

   1: Unlock

   2: Lock
   </td>
   </tr>
   <tr>
   <td><strong>param5</strong></td>
   <td>Shutter Cmd</td>
   <td>Shooting Command. Any non-zero value triggers the camera.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td><strong>param6</strong></td>
   <td>CommandID</td>
   <td>Command Identity</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoDigicamControl.png
   :target: ../_images/MissionList_DoDigicamControl.png

   Copter: Mission PlannerSettings for DO_DIGICAM_CONTROL command.

.. _mav_cmd_do_mount_control:

MAV_CMD_DO_MOUNT_CONTROL
----------------------------

Supported by: Copter, Plane, Rover.

Mission command to control a camera or antenna mount.

This command allows you to specify a roll, pitch and yaw angle which
will be sent to the :ref:`camera gimbal <common-cameras-and-gimbals>`. This
can be used to point the camera in specific directions at various times
in the mission.

.. note::

   Supported from AC3.3, Plane 3.4

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td></td>
   <td>Pitch, in degrees.</td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td></td>
   <td>Roll, in degrees.</td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td></td>
   <td>Yaw, in degrees.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>reserved</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>reserved</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>reserved</td>
   </tr>
   <tr>
   <td><strong>param7</strong></td>
   <td></td>
   <td>`MAV_MOUNT_MODE <https://mavlink.io/en/messages/common.html#MAV_MOUNT_MODE>`__ enum value.</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoMountControl.png
   :target: ../_images/MissionList_DoMountControl.png

   Copter: Mission PlannerSettings for DO_MOUNT_CONTROL command
   
   
.. _mav_cmd_do_set_cam_trigg_dist:

MAV_CMD_DO_SET_CAM_TRIGG_DIST
-----------------------------

Supported by: Copter, Plane, Rover.

Trigger the :ref:`camera shutter <common-camera-shutter-with-servo>` at
regular distance intervals. This command is useful in :ref:`camera survey missions <common-camera-control-and-auto-missions-in-mission-planner>`. 
To trigger the camera once, immediately after passing the DO command, set param3 to 1.  Trigger immediately parameter3 is available from ArduPilot 4.1 onwards.

.. note::

   Providing a distance of zero will stop the camera shutter from being triggered.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Dist (m)</td>
   <td>Camera trigger distance interval (meters). Zero to turn off distance triggering.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <td><strong>param3</strong></td>
   <td>?</td>
   <td>Trigger once instantly. One is on, zero is off.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

**Mission planner screenshots**

.. figure:: ../../../images/MissionList_DoSetCamTriggDist.png
   :target: ../_images/MissionList_DoSetCamTriggDist.png

   Copter: Mission PlannerSettings for DO_SET_CAM_TRIGG_DIST command

The above configuration above will cause the camera shutter to trigger
after every 5m that the vehicle travels.

.. _mav_cmd_do_fence_enable:


MAV_CMD_DO_FENCE_ENABLE
-----------------------

Supported by: Plane , Copter, Rover.

Mission commands to enable the Plane :ref:`GeoFence <plane:geofencing>`, Copter/Rover :ref:`common-ac2_simple_geofence` and/or :ref:`common-polygon_fence`.


**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td></td>
   <td>Set GeoFence enable state (0=disable, 1=enable, 2= disable only floor (Plane only)).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>


.. _mav_cmd_do_parachute:
[site wiki="copter" heading="off"]

MAV_CMD_DO_PARACHUTE
--------------------

Supported by: Copter (not Plane or Rover).

Mission command to trigger a parachute (if enabled).

Copter
~~~~~~

Mission command to trigger a parachute.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Enable/Release</td>
   <td>Parachute action (0=disable, 1=enable, 2=release).</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]


.. _mav_cmd_do_inverted_flight:

[site wiki="plane" heading="off"]

MAV_CMD_DO_INVERTED_FLIGHT
--------------------------

Supported by: Plane (not Copter or Rover).

Change to/from inverted flight.


Plane
~~~~~

Change between normal and :ref:`inverted flight <plane:inverted-flight>`.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>0=normal, 1=inverted</td>
   <td>Set flight type:

   0: normal
   1: inverted
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_do_gripper:

[site wiki="copter" heading="off"]

MAV_CMD_DO_GRIPPER
------------------

Supported by: Copter (not Plane or Rover).

Mission command to operate EPM gripper.



Copter
~~~~~~

Mission command to operate EPM gripper.

.. note::

   The :ref:`instructions for integrating Copter with gripper <common-electro-permanent-magnet-gripper>` are out
   of date and use DO_SET_SERVO to activate the gripper (April 2015).

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>Gripper No</td>
   <td>Gripper number (from 1 to maximum number of grippers on the vehicle). </td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>drop(0)/grab(1)</td>
   <td>Gripper action:
   0:Release
   1:Grab
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>


.. _mav_cmd_do_guided_limits:

MAV_CMD_DO_GUIDED_LIMITS
------------------------

Supported by: Copter (not Plane or Rover).

Set limits for external control.


Copter
~~~~~~

This command sets time, altitude, and distance limits for external
control (GUIDED mode). When these limits are exceeded, control will
return from GUIDED mode to the mission. Setting any of these parameters
to zero will remove the associated limit.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>timeout S</td>
   <td>Maximum time (in seconds) that the external controller is allowed to
   control vehicle. Use 0 to remove time limits (unlimited time allowed).
   </td>
   </tr>
   <tr>
   <td><strong>param2</strong></td>
   <td>min alt</td>
   <td>Minimum allowed absolute altitude (in meters, AMSL), below which the
   command will be aborted and the mission will continue. Use 0 to indicate
   that there is no minimum altitude limit.
   </td>
   </tr>
   <tr>
   <td><strong>param3</strong></td>
   <td>max alt</td>
   <td>Maximum allowed absolute altitude (in meters, AMSL), above which the
   command will be aborted and the mission will continue. Use 0 to indicate
   that there is no maximum altitude limit.
   </td>
   </tr>
   <tr>
   <td><strong>param4</strong></td>
   <td>max dist</td>
   <td>Horizontal move limit (in meters, AMSL). If vehicle moves more than this
   distance from its location at the moment the command was executed, the
   command will be aborted and the mission will continue. Use 0 to indicate
   that there is no horizontal limit.
   </td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_do_autotune_enable:

[site wiki="plane" heading="off"]

MAV_CMD_DO_AUTOTUNE_ENABLE
--------------------------

Supported by: Plane (not Copter or Rover).

Enable/disable autotune.
(not included in Mission Planner, use MAV_CMD_DO_SET_MODE)

Plane
~~~~~

This command sets the Plane to
:ref:`AUTOTUNE <plane:autotune-mode>` mode.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>na</td>
   <td>Enable/disable autotune (1: enable, 0:disable)</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

.. _mav_cmd_do_engine_control:

MAV_CMD_DO_ENGINE_CONTROL
-------------------------

Supported by: Plane (not Copter or Rover).

Stop or start internal combustion engine (ICE)


Plane
~~~~~

This command can be used to start or stop the ICE before a NAV_VTOL_LAND or after a NAV_VTOL_TAKEOFF command for a Quadplane to avoid potential prop strikes in the wind. It should be placed before either of those commands.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>?</td>
   <td>Start/Stop ICE (1: start, 0:stop)</td>
   </tr>
   <td><strong>param2</strong></td>
   <td></td>
   <td>Cold Start (1: enables choke, currently not implemented)</td>
   </tr>
   <td><strong>param3</strong></td>
   <td></td>
   <td>Altitude in cm. Altitude at which action is taken.</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[/site]

.. _mav_cmd_do_set_resume_dist:

MAV_CMD_DO_SET_RESUME_DIST
--------------------------

Supported by: Plane, Copter & Rover.

Set the distance that the mission will be rewound when resuming after an interupt (switching modes).  A full explanation of this feature can be found on the :ref:`Mission Rewind on Resume Page <common-mission-rewind>`.  After setting a rewind distance in a mission, setting the distance to zero will switch off the rewind feature from that point on the mission.

**Command parameters**

.. raw:: html

   <table border="1" class="docutils">
   <tbody>
   <tr>
   <th>Command Field</th>
   <th>Mission Planner Field</th>
   <th>Description</th>
   </tr>
   <tr>
   <td><strong>param1</strong></td>
   <td>?</td>
   <td>Rewind distance in meters</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param2</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param3</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param4</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param5</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param6</td>
   <td></td>
   <td>Empty</td>
   </tr>
   <tr style="color: #c0c0c0">
   <td>param7</td>
   <td></td>
   <td>Empty</td>
   </tr>
   </tbody>
   </table>

[copywiki destination="copter,plane,rover,planner,dev"]
