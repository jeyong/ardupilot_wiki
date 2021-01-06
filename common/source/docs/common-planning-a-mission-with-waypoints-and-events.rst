.. _common-planning-a-mission-with-waypoints-and-events:

============================================
Waypoint와 Event로 미션 계획하기(Planning a Mission with Waypoints and Events)
============================================

여기서는 모든 종류의 비행체에 대해서 일반적인 waypoint를 설정하는 방법에 대해서 설명한다.

[site wiki="rover"]
.. note::

   Rover 사용자: 더 간단한 Rover 관련 waypoint 셋업 방법은 :ref:`Learning a Mission <rover:learning-a-mission>` 를 참고하자.
[/site]

.. _common-planning-a-mission-with-waypoints-and-events_setting_the_home_position:

Home Position 셋팅(Setting the Home Position)
=========================

**Copter**, **Plane**, **Rover**를 위해서 home position은 비행체가 arming된 location으로 설정된다. 이 말은 만약 RTL을 실행한다면 arming된 위치로 돌아온다는 뜻이다. 따라서 비행체를 arming한 위치로 비행체를 돌아오게 하고 싶거나 리턴 지점을 셋업하기 위해서 rally point를 사용할 수 있다.

Video: 다중 waypoint Mission 생성 및 저장
================================================

..  youtube:: HAjkuJdjZw4
    :width: 100%

Video: 이미 저장된 다중 waypoint Mission을 load하기
===================================================

..  youtube:: nBq8YHShkVU
    :width: 100%

Instructions
============

아래 이미지에서 Copter 미션은 자동 takeoff부터 시작하여 20미터 고도로 올라간다. 다음으로 WP 2로 가서 100미터 고도로 올라간다. 다음으로 10초 기다리고 WP3을(50미터 고도로 하강한다.) 진행하고 이륙했던 지점으로 돌아온다. 이륙 지점에 도착하면 비행체는 착륙한다. 미션은 이륙 지점이 home position으로 설정되어 있다는 것을 가정한다.

.. figure:: ../../../images/mp_mission_planning.jpg
   :target: ../_images/mp_mission_planning.jpg

   Copter: Mission Planning 예제

waypoint나 다른 명령을 시작할 수 있다.( :ref:`Mission commands <common-planning-a-mission-with-waypoints-and-events_mission_commands>` 참고 )
드롭다운 메뉴에 각 행에서 원하는 명령을 선택한다. 컬럼 헤더에는 해당 명령에서 필요한 데이터가 무엇인지를 보여준다. Lat와 Lon은 지도에서 클릭해서 들어갈 수 있다. 고도는 이륙한 고도/home position에 상대적이다. 따라서 만약에 100m로 설정하면 머리위 100m까지 올라가게 된다.

**Default Alt** 는 새로운 waypoint를 시작할때 기본 고도이다. 고도 정의에 대해서는 :ref:`common-understanding-altitude` 를 참고하자.

**Verify height** 는 Mission Planner는 구글 Earth 위상 데이터를 사용한다. 해당 지상의 높이를 반역해서 각 waypoint에서의 원하는 고도를 조정한다. 따라서 만약 waypoint가 언덕인데 이 옵션이 선택되어 있다면 *Mission Planner* 는 hill의 높이만큼 ALT 설정을 증가시킬 것이다. 이렇게 해야 산에 충돌하는 것을 막을 수 있다.

일단 mission을 설정을 완료하고 **Write** 를 선택하면 APM에 보내서 EEPROM에 저장하게 된다. **Read** 를 선택해서 확인할 수 있다.

여러 mission 파일을 **Save WP File** 를 선택해서 PC 드라이브에 저장할 수 있다. 혹은 오른쪽 클릭 메뉴에서 **Load WP File** 로 파일을 읽을 수 있다.:

.. image:: ../../../images/mp_save_waypoints.jpg
    :target: ../_images/mp_save_waypoints.jpg

Tips
====

-  Prefetch: 지도 데이터를 캐쉬에 저장할 수 있다. 이렇게 하면 야외에 나갔을 때 인터넷에 접속하지 않아도 된다. **Prefetch** 버튼을 클릭하고, 선택한 영역을 다운받기 위한 박스를 그리기 위해서 **Alt** 누르고 있는다.
-  Grid: 폴리곤을 (오른쪽 클릭) 그리고 자동으로 선택한 영역에 대해서 waypoint를 생성한다. "island detection"을 하지 않는다. 이말은 만약 큰 폴리곤이 있고 그 내부에 작은 폴리곤이 있는 경우 작은 폴리곤은 큰 폴리곤으로부터 배제되지 않는다. (`this <http://wiki.openstreetmap.org/wiki/Relation:multipolygon>`__ 참고) 또 U자와 같이 부분적으로 xxxxx한 경우에 중간에 있는 오픈 영역은 flyover의(고가도로) 부분으로 포함될 수 있다.
-  현재 위치에 대한 home location 셋팅은 쉽다. 원하는 위치에서 그냥 **Home Location** 을 클릭하면 된다. 이렇게 하면 현재 좌표를 home location으로 설정하게 된다.
-  waypoint들 사이에서 거리를 측정할려면 한쪽을 오른쪽 클릭하고 Measure Distance를 선택한다. 다음으로 다른 끝쪽에서 오른쪽 클릭하고 **Measure Distance** 를 다시 선택한다. 다이얼로그 박스가 열리면서 2지점 사이의 거리를 보여준다.

Auto grid
=========

*Mission Planner*로 원하는 mission을 생성할 수 있다. 이렇게 하면 매핑 미션과 같은 기능에 유용하다. 이렇게 하면 비행체는 "lawnmower" 패턴으로 해당 영역 위를 앞으로 움직이면서 사진 자료를 수집할 수 있다.

이렇게 하기 위해서 오른쪽 클릭 메뉴에서 폴리곤을 선택하고 매핑할려는 해당 영역 주변에 box를 그린다. 다음으로 Auto WP, Grid를 선택한다. 다음 다이얼로그 박스는 고도와 여백을 선택하는 절차이다. *Mission Planner* 는 다음과 같은 mission을 생성한다.:

.. figure:: ../../../images/mp_auto_mission_grid.jpg
   :target: ../_images/mp_auto_mission_grid.jpg

   Mission Planner auto-generated grid

   
.. _common-planning-a-mission-with-waypoints-and-events_mission_commands:

Mission commands
================

*Mission Planner* 는 현재 비행체 타입에 적절한 명령 목록을 제공한다. 그리고 사용자가 입력값이 필요한 해당 파라미터에 대한 컬럼 헤더를 추가한다.
여기에는 waypoint 지점으로 이동하라는 명령과 근처에서 loiter하라는 navigation 명령이 포함되어 있다. DO 명령은 특정 action을 수행하고 condition 명령은 DO 명령이 실행 가능할때 제어를 할 수 있다.

.. figure:: ../../../images/MissionList_LoiterTurns.png
   :target: ../_images/MissionList_LoiterTurns.png

   예제: LOITER_TURNS command withheadings for number of turns, direction, and location to loiteraround.

모든 ArduPilot 플랫폼에서 지원하는 미션 명령의 전체 집합은 :ref:`MAVLink Mission Command Messages (MAV_CMD) <common-mavlink-mission-command-messages-mav_cmd>` 에서 목록을 확인할 수 있다. 여기에는 각 명령의 전체 이름, 어떤 파라미터를 지원하는지에 대한 정보, 관련 *Mission Planner* 컬럼 헤딩이 포함되어 있다.

.. note::

   Mission Planner는 전체 명령 이름의 cut-down 버전을 사용한다. 예제로 MAV_CMD_NAV_WAYPOINT,
   MAV_CMD_CONDITION_DISTANCE, MAV_CMD_DO_SET_SERVO 와 같은 명령은 MP에서는 각각 WAYPOINT, CONDITION_DISTANCE and DO_SET_SERVO로 나타난다.

[site wiki="copter"]
Copter 관련된 내용은 :ref:`Copter Mission Command List <copter:mission-command-list>` 를 참고하자.
[/site]

저장된 Mission Map을 prefetch하는 방법
====================================

..  youtube:: 1s8gsXTdPY8
    :width: 100%

가끔 큰 숫자가 나타나기도 한다.
=================================================

..  youtube:: J5ClTnggZKk
    :width: 100%

[copywiki destination="copter,plane,rover,planner"]
