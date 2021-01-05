.. _mission-planner-overview:

========================
Mission Planner 개요
========================

Mission Planner는 ArduPilot 오픈소스 프로젝트로 모든 기능을 갖춘 지상국 어플리케이션이다. 여기서는 Mission Planner에 대한 배경 지식과 이 사이트의 구조에 대해서 소개한다.

Mission Planner란?
=======================

.. image:: ../../../images/mission_planner_flight_data.jpg
    :target: ../_images/mission_planner_flight_data.jpg

Mission Planner는 Plane, Copter, Rover를 위한 지상 제어 스테이션이다. Windows 에서만 동작한다. Mission Planner는 설정 도구로 사용할 수 있고 자동 비행을 위한 동적인 제어로 사용할 수 있다. Mission Planner로 할 수 있는 몇 가지만 소개한다:

-  비행체를 제어하는 비행제어 장치인 Pixhawk에 :ref:`firmware <common-glossary>` 소프트웨어를 load한다.
-  셋업, 설정, 최적 성능을 위한 비행체 튜닝
-  자동 미션을 계획, 저장 및 Pixhawk에 load한다. 이때 구글이나 다른 지도 상에서 waypoint를 간단히 클릭해서 작업 가능하다.
-  Pixhawk에서 생성한 mission log를 다운받고 분석한다.
-  PC 비행 시뮬레이터와 인터페이스 역할로 HITL UAV 시뮬레이터를 생성
-  텔레메트리 장치로 다음과 같은 일을 수행 :

   -  비행하는 동안 비행체의 상태 모니터링
   -  텔레메트리 log를 기록
   -  텔레메트리 log 보기 및 분석
   -  FPV로 비행체 운영

위에서 언급한 것과 더 다양한 기능에 대해서 알아보자.

역사
=======

Mission Planner is a free, open-source, community-supported application
developed by Michael Oborne for the open-source APM autopilot project.
If you would like to donate to the ongoing development of Mission
Planner, please select the Donate button on the Mission Planner
interface.

.. image:: ../images/mp_donate_button.jpg
    :target: ../_images/mp_donate_button.jpg

.. _mission-planner-overview_support:

Support
=======

**The Help Screen:**

Clicking the Help icon at the top of the Mission Planner interface will
open a screen with general information about help with Mission Planner.
The "Check for Updates" button will check for available updates to
Mission Planner manually. Mission Planner automatically checks for
updates upon start up and notifies you if an update is available. Please
always run the most current version of Mission Planner, although it is
not necessary to check for updates more often than upon start up. 

The "Check for BETA Updates" button will install the current development version of Mission Planner. This contains all the latest features and updates, but also might have bugs, since it does not have extensive community testing.

At the bottom of the HELP screen is the check box "Show Console Window (restart)" , which enables the console window during Mission Planner operation.  That window shows Mission Planner
activity and is primarily for diagnostic purposes.  It sometimes shows
some interesting information. A restart of Mission Planner is required
for the option to take effect.  (TBD need a link to console page TBD)

**Getting Help:**

The support for Mission Planner comes from the community of users like
you.  All of the documentation is created by users who volunteer their
time. If you have questions, first look through the table of contents
(upper left of every page) for a topic that may address your question.
Next, try a search of the website.  If you still need help, then the
community forums are the place to go. There you will find may friendly
users, developers and often, even Michael will chime in.  There are two
primary forums. The diydrones.com forum
`here <https://diydrones.com/forum>`__ and the Arduilot forum
`here <https://discuss.ardupilot.org/>`__. The diydrones forum has existed
for years and has a very large community and numerous general topics.
The ArduPilot forum is newer and is more specific to ArduPilot and
its vehicles.

**Reporting Issues:**

Usually, you can either resolve a question on a forum. Sometimes you
will discover a bug and can confirm the problem on a forum. If you have
discovered a bug, use the forums to request it be logged as an official
issue. One of the developers will normally be glad to do so.

If you see a need to change or enhance the documentation, please let us
know - again using the forums.  We welcome your suggestions and there
are dozens of qualified editors who can implement your suggestions.

Navigating the Documentation
============================

Use the table of contents at the top of each page to navigate the
Mission Planner Manual - and the contents of the vehicle specific areas.

This section of our website contains information on how to use Mission
Planner as a "general" application. However, some of the pages will also
have some vehicle specific information. Those pages will also be
contained in the specific vehicle's section of the website. Information
that is primarily specific to a particular vehicle will only be located
in that vehicles section so, if you cannot find information here, try
the section of the website for the vehicle you are using.
