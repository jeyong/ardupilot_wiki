.. _common-autopilots:

==========================
Autopilot 하드웨어 옵션
==========================

선택가능한 ArduPilot 비행제어기 하드웨어에 대해서 알아보자. 아래 목록은 제조사와 제품 이름을 기준으로 정렬하였다.

오픈 하드웨어
-------------

.. toctree::
    :maxdepth: 1

    BBBMini* (Linux) <https://github.com/mirkix/BBBMINI>
    Beagle Bone Blue (Linux) <common-beagle-bone-blue>
    CUAV V5 Plus <common-cuav-v5plus-overview>
    CUAV V5 Nano <common-cuav-v5nano-overview>
    CUAV Nora <common-cuav-nora-overview>
    CUAV X7/ X7 Pro <common-cuav-x7-overview>
    Drotek Pixhawk3 <https://drotek.gitbook.io/pixhawk-3-pro/>
    F4BY <common-f4by>
    Hex/ProfiCNC Cube Black <common-thecube-overview>
    Hex/ProfiCNC Cube Orange <common-thecubeorange-overview>
    Hex/ProfiCNC Cube Purple <common-thecubepurple-overview>
    Hex/ProfiCNC Cube Yellow <common-thecubeyellow-overview>
    Hex/ProfiCNC Cube Green <https://docs.cubepilot.org/user-guides/autopilot/the-cube-module-overview>
    Holybro Pix32 v5 <common-holybro-pix32v5>
    mRo Pixhawk <common-pixhawk-overview>
    mRo Pixracer <common-pixracer-overview>
    mRo X2.1 <https://store.mrobotics.io/mRo-X2-1-Rev-2-p/mro-x2.1rv2-mr.htm>
    mRo X2.1-777 <https://store.mrobotics.io/mRo-X2-1-777-p/mro-x2.1-777-mr.htm>
    OpenPilot Revolution <common-openpilot-revo-mini>
    PocketPilot* (Linux) <https://github.com/PocketPilot/PocketPilot>
    TauLabs Sparky2** <common-taulabs-sparky2>

\* 이 장치들은 Beagle Bone 마이크로 컴퓨터에 센서를 추가한 보드이다. 상세한 내용은 보드 링크에서 확인하자. 

\** flash 메모리의 제약으로 이 보드들은 ArduPilot의 모든 기능을 담을 수 없다. 상세 내용은 :ref:`펌웨어 제약사항 <common-limited_firmware>`을 참고하자.

Closed 하드웨어
---------------

.. toctree::
    :maxdepth: 1

    Aerotenna Ocpoc-Zynq <https://aerotenna.com/shop/ocpoc-zynq-mini/>
    Emlid NAVIO2 (Linux) <common-navio2-overview>
    Furious FPV F-35 Lightning and Wing FC-10 <common-furiousfpv-f35>
    Holybro Durandal H7 <common-durandal-overview>
    Holybro Kakute F4 <common-holybro-kakutef4>
    Holybro Kakute F7 AIO* <common-holybro-kakutef7aio>
    Holybro Kakute F7 Mini* (only V1 and V2 are compatible) <common-holybro-kakutef7mini>
    Holybro Pixhawk 4 <https://github.com/ArduPilot/ardupilot/blob/master/libraries/AP_HAL_ChibiOS/hwdef/Pixhawk4/README.md>
    Holybro Pixhawk 4 Mini <https://github.com/ArduPilot/ardupilot/blob/master/libraries/AP_HAL_ChibiOS/hwdef/PH4-mini/README.md>
    Mateksys F405-SE <common-matekf405-se>
    Mateksys F405-STD and variants* <common-matekf405>
    Mateksys F405-Wing* <common-matekf405-wing>
    Mateksys F765-Wing <common-matekf765-wing>
    Mateksys H743-Wing <common-matekh743-wing>
    mRo ControlZero F7 <common-mro-control-zero-F7>
    mRo Pixracer Pro (H7) <common-pixracer-pro>
    mRo Nexus <common-mro-nexus>
    Omnibus F4 AIO/Pro* <common-omnibusf4pro>
    OmnibusNanoV6 <common-omnibusnanov6>
    Omnibus F7V2* <common-omnibusf7>
[site wiki="copter"]
    Parrot Bebop Autopilot <parrot-bebop-autopilot>
[/site]
    Parrot C.H.U.C.K <common-CHUCK-overview>
    RadioLink MiniPix <common-radiolink-minipix>
    QioTek Zealot F427 <common-qiotek-zealot>
    SpeedyBee F4 (this board currently is non-verified) <common-speedybeef4>
    VR Brain 5 <http://www.virtualrobotix.it/index.php/en/shop/autopilot/vrbrain5-detail>
    VR uBrain 5.1 <http://www.virtualrobotix.it/index.php/en/shop/autopilot/vrbrainmicro51-detail>

\* flash 메모리의 제약으로 이 보드들은 ArduPilot의 모든 기능을 담을 수 없다. 상세 내용은 :ref:`펌웨어 제약사항 <common-limited_firmware>`을 참고하자.

.. note:: Linux 보드를 기반으로 ArduPilot을 사용하는 경우 :ref:`building-the-code` 를 참고하자.

펌웨어 제약사항
--------------------

일부 보드는 해당 보드의 메모리 용량에 맞게 기능을 줄여서 펌웨어를 만든다. 아래 내용을 참조 :

.. toctree::
    :maxdepth: 1

    Firmware Limitations <common-limited-firmware>
    

더이상 생산되지 않는 보드
-------------------
아래 보드는 더 이상 생산되지 않는다. 하지만 문서는 제공되며 최근 빌드도 동작한다.
아래 보드는 새로운 프로젝트에서 사용은 추천하지 않는다.

.. toctree::
    :maxdepth: 1

    CUAV V5 <common-pixhackV5-overview>
    Emlid Edge <common-emlid-edge>
    Erle PXFmini RPi Zero Shield <common-pxfmini>
    Erle ErleBrain <common-erle-brain-linux-autopilot>
    Intel Aero <common-intel-aero-overview>
    Intel Aero RTF vehicle <common-intel-aero-rtf>

아래 보드는 더이상 지원하지 않는다. 문서는 :ref:`archived<common-archived-topics>`를 참고하자.

   APM 2.x (APM 2.6 and later)는 더이상 Copter, Plane, Rover에서 지원하지 않는다. Copter 3.2.1과 Plane 3.4.0, Rover 2.5.1이 최종 지원하는 펌웨어 빌드 버전이다.
   NAVIO+ 
   PX4FMU
   Qualcomm Snapdragon Flight Kit

