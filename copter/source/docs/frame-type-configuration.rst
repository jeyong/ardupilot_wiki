.. _frame-type-configuration:

==================================
Frame 종류와 타입 설정
==================================

:ref:`FRAME_CLASS <copter:FRAME_CLASS>`와 :ref:`FRAME_TYPE <copter:FRAME_TYPE>` 파라미터는 사용하는 물리적 프레임에 따라서 설정해야만 한다. 지원하는 멀티콥터 프레임의 목록에 대해서 :ref:`ESCs 와 Motors 연결 <connect-escs-and-motors>` 페이지를 참고하자.

미션 플래너를 사용하고 있다면 초기 셋업(Initial Setup)에서 **Mandatory Hardware \| Frame Type** 를 선택한다. 다른 지상국 SW를 사용하는 경우에는 파라미터 업데이트 화면을 통해서 :ref:`FRAME_CLASS <copter:FRAME_CLASS>` 와 :ref:`FRAME_TYPE <copter:FRAME_TYPE>` 파라미터를 설정할 수 있다.

.. figure:: ../images/MissionPlanner_Select_Frame-Type.jpg
   :target: ../_images/MissionPlanner_Select_Frame-Type.jpg

Copter-3.5 이상 버전을 사용하는 경우 "Frame Class" 섹션이 반드시 보여야 하며 여기서 비행체(Quad, Hexa, Octa 등)의 "Class"를 선택할 수 있다.

.. note::

   For Traditional Helicopters, "Heli" should already be selected and it should not be changed.
   For :ref:`Single Copter and Coax Copter <singlecopter-and-coaxcopter>` the :ref:`FRAME_CLASS <copter:FRAME_CLASS>` parameter should be set directly from the Full Parameter List until `this issue <https://github.com/ArduPilot/MissionPlanner/issues/1552>`__ is resolved.

다음으로 비행체에 맞는 프레임 "Type"을 선택한다. 기본 타입은 **X**이다.

트라이콥터인 Y6, 전통적인 헬기, 바이콥터, 싱글콥터와 콕스콥터 인 경우에는 프레임 타입을 무시한다.

모터 순서 다이어그램
====================

지원하는 멀티콥터 프레임 목록에 대해서는 :ref:`ESCs와 Motors 연결하기<connect-escs-and-motors>` 참고하라.