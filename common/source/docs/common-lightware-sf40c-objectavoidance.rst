.. _common-lightware-sf40c-objectavoidance:

==========================
LightWare SF40/C 360 Lidar
==========================

`Lightware SF40/C 360degree lidar <https://lightware.co.za/collections/lidar-rangefinders/products/sf40-c-100-m>`__ 로 Copter-3.4 이상 버전의 Loiter 모드로  장애물 회피를 할 수 있다.

.. 경고::

   이 기능은 다양한 경우에 대해서 테스트하지 못하였으므로 사용시 주의해야만 한다.

..  youtube:: BDBSpR1Dw_8
    :width: 100%

SF40c 장착하기
------------------

   .. image:: ../../../images/lightware-sf40c.png
       :target: ../_images/lightware-sf40c.png
       :width: 300px

SF40c는 비행체의 위나 아래 부분에 장착해야만 한다. 그래야 회전하는 부분이 수평으로 스캔이 가능하며 상부의 GPS나 하부의 비행체 다리에 가려지지 않도록 해야한다. 둥근 금과 검은 lightware 로고가 앞을 향하도록 해야만 한다.
    
Pixhawk에 연결하기
---------------------------

   .. image:: ../../../images/lightware-sf40c-pixhawk.png
       :target: ../_images/lightware-sf40c-pixhawk.png

위 그림에서 SF40c를 Pixhawk의 시리얼 input에 연결하였다. 위에서는 Serial 4에 연결하였지만 남은 시리얼 포트에 연결도 가능하다.

미션 플래너를 이용한 설정
----------------------------------------

- :ref:`SERIAL4_PROTOCOL <SERIAL4_PROTOCOL>` = "11" ("Lidar360") if using Serial4.
- :ref:`SERIAL4_BAUD <SERIAL4_BAUD>` =  "921" if using Serial4.
- :ref:`PRX_TYPE <PRX_TYPE>` = "7" (LightwareSF40c) or "1" (LightwareSF40C-legacy) if using a very old version of the sensor
- :ref:`PRX_ORIENT <PRX_ORIENT>` = "0" if mounted on the top of the vehicle, "1" if mounted upside-down on the bottom of the vehicle.
- :ref:`PRX_YAW_CORR <PRX_YAW_CORR>` allows adjusting the forward direction of the SF40c.  One way to determine this angle is to use the Mission Planner's Setup >> Advanced, Proximity viewer and then walk around the vehicle and ensure that the sector distances shorten appropriately.
- :ref:`PRX_IGN_ANG1 <PRX_IGN_ANG1>` and :ref:`PRX_IGN_WID1 <PRX_IGN_WID1>` parameters allow defining zones around the vehicle that should be ignored.  For example to avoid a 20deg area behind the vehicle, set :ref:`PRX_IGN_ANG1 <PRX_IGN_ANG1>` to 180 and :ref:`PRX_IGN_WID1 <PRX_IGN_WID1>` to 20.

이 센서를 이용한 장애물 회피 정보는 :ref:`here <common-object-avoidance-landing-page>` 를 참고하자.
