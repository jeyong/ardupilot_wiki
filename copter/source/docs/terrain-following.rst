.. _terrain-following:

========================================
지형 순응(Terrain Following (in Auto, Guided, etc))
========================================

Copter 3.4 이상에서 :ref:`AUTO <auto-mode>`, :ref:`Guided <ac2_guidedmode>`, :ref:`RTL <rtl-mode>` and :ref:`Land <land-mode>`와 같이 자동 비행 모드를 포함한 거의 모든 비행 모드에서 "terrain following"을 지원한다. 이 기능은 비행체가 지형 위로 지정한 높이를 유지하기 위해서 비행 중에 위/아래로 이동하게 된다. 이때 사용하는 지형 정보는 :ref:`비행체 하방으로 장착된 Lidar나 Sonar 센서<common-rangefinder-landingpage>`와 지상국 SW의 구글 지도 서비스가 제공하는 `SRTM <https://en.wikipedia.org/wiki/Shuttle_Radar_Topography_Mission>`__ 데이터(terrain altitude data)를 이용한다. SRTM 데이터에 대한 내용은 :ref:`plane terrain following page <plane:common-terrain-following>`를 참조한다.

..  youtube:: mT67QOAxuG8
    :width: 100%
    
고도에 대한 정의는 :ref:`common-understanding-altitude`를 참고한다.

.. note::

   :ref:`Loiter <loiter-mode>`, :ref:`PosHold <poshold-mode>`와 :ref:`AltHold <altholdmode>` 모드도 낮은 고도 terrain following을 지원한다. 이를 지표면 추적(Surface Tracking)이라 부른다. :ref:`Surface Tracking <terrain-following-manual-modes>` 위키 페이지를 참고하자.

Terrain 데이터를 사용하기 위해서 미션을 설정하기
========================================
-  LIDAR를 사용하는 경우 :ref:`LIDAR 설정은 여기를 참조하자. <common-rangefinder-landingpage>`
-  지형 데이터를 지원하는 미션 플래너를 사용하는 경우 :ref:`TERRAIN_ENABLE <TERRAIN_ENABLE>` 파라미터를 1로 설정한다.
-  미션 플래너의 최신 버전을 사용하는 경우 altitude type을 "Terrain"으로 설정한다. 일단 "Alt" 필드를 포함하는 모든 미션 명령을 설정은 고도 기준 지형(altitudes-above-terrain)으로 해석된다.
-  미션을 비행체로 업로드 시키고 :ref:`AUTO <auto-mode>` 모드로 미션을 수행한다.

   .. image:: ../images/terrain_mission.png
       :target: ../_images/terrain_mission.png
       :width: 500px

.. warning::

    :ref:`EK2_ALT_SOURCE <EK2_ALT_SOURCE>`나 ``EK3_ALT_SOURCE`` 파라미터를 설정하지 않는다. 이 파라미터는 "0"(바로미터)로 그대로 둔다.

    :ref:`EK2_RNG_USE_HGT <EK2_RNG_USE_HGT>`나 :ref:`EK3_RNG_USE_HGT <EK3_RNG_USE_HGT>` 파라미터는 설정하지 않는다. 이 파리미터는 "-1"로 남겨둬야 한다.

지형 데이터의 소스(Sources of Terrain Data)
=======================

The ground station is normally responsible for providing the raw terrain data which is sent to the aircraft via MAVLink. Right now only Mission Planner and MAVProxy support the required TERRAIN_DATA and TERRAIN_REQUEST MAVLink messages needed for terrain following download support. If you are using a different ground station , in order to download terrain data you will need to connect using one of those two ground stations in order to allow ArduPilot to load terrain data onto your board on the ground or in flight.  Once it is loaded, it is saved permanently on the microSD card.

Both MissionPlanner and MAVProxy support the global SRTM database for terrain data. The ArduPilot SRTM server used by MAVProxy and Mission Planner has 100m grid spacing. Unless the ground control station uses a server with closer spacing, setting the :ref:`TERRAIN_SPACING <TERRAIN_SPACING>` parameter lower than 100m provides no better resolution, and only consumes more space on the SD card. 

Terrain Data is downloaded any time you save or connect with a loaded mission with these ground stations, or, if flying, the autopilot will request data if its flying into an area not already downloaded. Assuming the ground station can provide it. Usually an internet connection is required by the ground station.

Alternatively, you can download a set of terrain data tiles for any anticipated flight area using this `web utility <https://terrain.ardupilot.org/>`__.

.. image:: ../../../images/common-terrain-dl-utility.png

It will create tiles for the specified radius around a geographic location. Then you can download them, unzip and write in the APM/TERRAIN folder of the SD card.

You can also download .zip files for entire continents, or individual tiles from `here <https://terrain.ardupilot.org/data/>`__.

.. warning:: A long standing bug in the downloaded terrain data files, which occasionally caused terrain data to be missing, even though supposedly downloaded, was fixed in Plane 4.0.6, Copter 4.0.4, and Rover 4.1. It will automatically be re-downloaded when connected to a compatible GCS. However, if you are relying on SD terrain data for an area and don't plan on being connected to a GCS when flying over it, or its not part of a mission, you should download the area data using the utility above, or from the linked tiles data repository and place on your SD card in the Terrain directory.

RTL과 착륙 시에 지형 고도 사용하기 (Using Terrain Altitude during RTL and Land)
==========================================
Set the :ref:`TERRAIN_FOLLOW <TERRAIN_FOLLOW>` parameter to 1 to enable using terrain data in :ref:`RTL <rtl-mode>` and :ref:`Land <land-mode>` flight modes.  If set the vehicle will interpret the :ref:`RTL_ALT <RTL_ALT>` as an altitude-above-terrain meaning it will generally climb over hills on it's return path to home.  Similarly Land will slow to the :ref:`LAND_SPEED <LAND_SPEED>` (normally 50cm/s) when it is 10m above the terrain (instead of 10m above home).
Currently setting this parameter is not recommended because of the edge case mentioned below involving the somewhat unlikely situation in which the vehicle is unable to retrieve terrain data during the :ref:`RTL <rtl-mode>`.  In these cases the :ref:`RTL_ALT <RTL_ALT>` will be interpreted as an alt-above home. 

지형 데이터가 없는 경우 Failsafe(Failsafe in case of no Terrain data)
===================================
If the vehicle is executing a mission command that requires terrain data but it is unable to retrieve terrain data for two seconds (normally because the range finder fails, goes out of range or the Ground Station is unable to provide terrain data) the vehicle will switch to RTL mode (if it is flying) or disarm (if it is landed).

Note that because it does not immediately have access to terrain data in this situation it will perform a normal RTL interpreting the :ref:`RTL_ALT <RTL_ALT>` as an altitude-above-home regardless of whether :ref:`TERRAIN_FOLLOW <TERRAIN_FOLLOW>` has been set to "1" or not.

One common problem reported by users is the vehicle immediately disarms when the user switches to AUTO mode to start a mission while the vehicle is on the ground.  The cause is the altitude reported by the range finder (which can be checked from the MP's Flight Data screen's Status tab's sonar_range field) is shorter than the RNGFNDx_MIN_CM (for example :ref:`RNGFND1_MIN_CM <RNGFND1_MIN_CM>`)parameter which means the range finder reports "unhealthy" when on the ground.  The solution is to reduce the RNGFNDx_MIN_CM value (to perhaps "5").

Terrain Spacing and Accuracy
============================

The :ref:`TERRAIN_SPACING <TERRAIN_SPACING>` parameter controls the size of the grid used when requesting terrain altitude from the Ground Station (it is not used if using a Lidar). This is 100m by default but reducing to 30 may provide better accuracy at the expense of more telemetry traffic between the GCS and autopilot, and 9x more file storage space on the SD card, but only if the ground station uses a server with that resolution. MavProxy and Mission Planner currently do not. Also, if the vehicle is moving very fast, the autopilot may not be able to retrieve and cache the data quickly enough for the increased resolution to be actually used.  It is therefore recommended that you use a :ref:`TERRAIN_SPACING <TERRAIN_SPACING>` of 100 meters.

If the ground station does not have terrain data available at the resolution requested by the aircraft then the ground station will interpolate as necessary to provide the requested grid size.

지형 정확도(Terrain Accuracy)
================

The accuracy of the SRTM database varies over the surface of the earth.  Typical accuracy is around 10m but one developer noticed an inaccuracy of 35m at the peak of a ski hill.  This makes terrain following suitable for aircraft that are flying at altitudes of 60 meters or more.  For very accurate terrain following at lower altitudes it is recommended to use a :ref:`downward facing Lidar or Sonar <common-rangefinder-landingpage>`.

Warning
=======

When planning missions containing commands with different altitudes-above-terrain keep in mind that the vehicle's altitude-above-terrain will gradually change between the waypoints.  I.e. it will not immediately climb or descend to the new target altitude-above-terrain as it starts towards the next waypoint.

In practice it is best to set the initial take-off command's altitude high enough to clear obstacles.

   .. image:: ../images/terrain-warning-diagram.png
       :target: ../_images/terrain-warning-diagram.png
       :width: 500px

Example mission at 2m using Lidar
---------------------------------

..  youtube:: r4RBP0_LQ5Y
    :width: 100%
