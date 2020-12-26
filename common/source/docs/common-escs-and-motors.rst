.. _common-escs-and-motors:

===============
ESCs 와 모터
===============

.. image:: ../../../images/motors-and-escs-topimage.png

ArduPilot은 다양한 ESC와 모터를 지원한다. 아래는 가장 일반적인 타입을 위한 셋업 방법을 제공한다. 

.. toctree::
    :maxdepth: 1

    브러쉬리스 ESC와 관련된 설정 <common-dshot>
[site wiki="copter,rover"]
    브러쉬 모터 <common-brushed-motors>
[/site]
[site wiki="copter"]
    Booster motor <booster-motor>
[/site]
    Brushless ESCs <common-brushless-escs>
    ICE (Internal Combustion Engines) <common-ice>
    KDE CAN ESCs <common-kde-can-escs>
[site wiki="rover"]
    Thrusters (for boats) <thrusters>
[/site]
    Toshiba CAN ESCs <common-toshiba-can-escs>
[site wiki="rover"]
    Trolling motors <trolling-motor>
[/site]
    UAVCAN ESCs <common-uavcan-escs>
