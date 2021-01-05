.. _common-connect-mission-planner-autopilot:

====================================
Mission Planner와 Pixhawk 연결하기
====================================

여기서는 텔레메트리로 데이터 송수신 및 비행체 제어를 위해서 *Mission Planner* 를 Pixhawk에 연결하는 방법을 설명한다.

.. note::

   기존 Pixhawk 펌웨어 설치를 위해서 Pixhawk이 이미 ArduPilot 펌웨어가 설치된 상태에서 :ref:`Load Firmware <common-loading-firmware-onto-pixhawk>` 와 :ref:`기존 Ardupilot이 없는 상태에서 펌웨어 load <common-loading-firmware-onto-chibios-only-boards>` 가 있다.


연결 셋업하기
=========================

연결하기 위해서 먼저 사용하고자 하는 통신 방법/채널을 선택해야만 한다.  다음으로 물리적인 HW와 Windows 장치 드라이버를 셋업한다. USB 케이블, :ref:`Telemetry Radios <copter:common-telemetry-landingpage>`, :ref:`Bluetooth <common-mission-planner-bluetooth-connectivity_detailed_connecting_with_mission_planner>` , IP 연결 등의 방법을 사용하여 PC와 Pixhawk를 연결할 수 있다.

.. note::

   Windows에서 연결 HW를 위한 드라이버는 COM 포트와 기본 data rate 설정은 *Mission Planner*에서 가능하다.

.. figure:: ../../../images/pixhawk_usb_connection.jpg
   :target: ../_images/pixhawk_usb_connection.jpg
   :width: 450px

   Pixhawk USB 연결

.. figure:: ../../../images/new-radio-laptop.jpg
   :target: ../_images/new-radio-laptop.jpg
   :width: 450px

   SiK Radio를 사용해서 연결

*Mission Planner*에서 연결 및 data rate는 화면 우상단에 있는 드롭다운 메뉴를 이용해서 셋업한다.

.. image:: ../../../images/MisionPlanner_ConnectButton.png
    :target: ../_images/MisionPlanner_ConnectButton.png

일단 USB나 텔레메트리에 연결되면 Windows는 자동으로 Pixhawk에 COM 포트 숫자가 부여한다. 이렇게 부여한 숫자는 드롭다운 메뉴에서 확인할 수 있다.(숫자 자체가 중요한 것은 아니다.) 연결에 대한 적절한 data rate도 설정된다. (일반적으로 USB 연결 data rate는 115200이고 라디오 연결 rate는 57600이다.)

포트와 data rate를 선택하고 **Connect** 버튼을 눌러서 Pixhawk와 연결한다. 연결하고 난 후에 **Mission Planner** 는 Pixhawk로부터 파라미터를 다운로드 받는다. 그리고 버튼은 **Disconnect** 로 변경된다.:

.. image:: ../../../images/MisionPlanner_DisconnectButton.png
    :target: ../_images/MisionPlanner_DisconnectButton.png

.. tip::

   "select port" 드롭다운은 TCP나 UDP 포트 옵션도 포함되어 있다. 네트워크로 비행제어기에 연결하는 경우 사용할 수 있다.

포트 선택 박스 아래에 "Stats..." 핫링크를 클릭하면 연결과 관련된 정보를 제공한다. :ref:`Signing security<common-MAVLink2-signing>` 가 활성화된 경우처럼. 가끔 이 윈도우는 현재 화면 아래에 팝업으로 나타날 수 있고 전면에 보여줘야 한다.

.. image:: ../../../images/MP-stats.png
   :target: ../_images/MP-stats.png


Troubleshooting
===============

Mission Planner가 연결되지 않으면:

-  올바른 baud rate로 설정하였는지 확인하기(USB는 115200이고 텔레메트리는 57600이다.)
-  USB로 연결하면 전원이 들어오고 몇 초 후에 연결된다. 만약 bootloader 초기화 시간 동안 연결을 시도하면 Windows는 제대로된 USB 정보를 가져오지 못한다.
-  Windows에서 COM 포트를 사용하면 연결 COM 포트가 장치관리자의 시리얼 포트 목록에 존재한다.
-  비행제어기가 F7 혹은 H7 프로세서면서 CAN 포트를 가지는 경우라면 아래 섹션 :ref:`Troubleshooting Composite Connections <troubleshooting-composite-connections>` 를 참고하자.
-  USB 포트를 사용하면 다른 물리적인 USB 포트에 시도하자.
-  UDP나 TCP 연결을 사용하면 방화벽이 IP 트래픽을 블록키하고 있는지 확인한다.

Pixhawk에 ArduPilot 펌웨어가 설치되어 있고 제대로 부팅되는지 확인해야 한다. (Pixhawk에는 비행제어장치의 상태를 알려주는 :ref:`LEDs <common-leds-pixhawk>` 와
:ref:`Sounds <common-sounds-pixhawkpx4>` 가 있다.)

리모트 링크(USB 아니고)를 사용해서 Mission Planner와 연결하지만 모드 변경과 같은 명령이나  파라미터를 다운받지 못한다면 아마 비행제어기의 Signing이 켜져있어서일 수도 있다. :ref:`common-MAVLink2-signing` 를 참고하자.

.. _troubleshooting-composite-connections:

Troubleshooting Composite Connections
=====================================

Autopilots with F7 or H7 processors and having CAN interfaces use firmware that presents two USB interfaces: One for the normal MAVLink connection, and one for SLCAN serial connections to the CAN interface for configuration and firmware updates.This is called a composite USB device.

By default, the MAVLink USB interface is SERIAL0 and the SLCAN USB interface is the highest SERIALx port the board presents. The Windows driver currently installed with Mission Planner may select to use either one, and since both are set by default in ArduPilot firmware for MAVLINK protocol, it will work fine, whichever one it chooses as the COM port. 

However, there is a situation where the user will find that it will not connect to the obvious COM port in the Mission Planner dropdown box.This occurs when the user accidentally changes the protocol of whichever SERIALx port the Windows driver is using as the MAVLink COM port to something other than MAVLink. This can easily happen if the user takes an existing parameter file from a vehicle configuration used with a different autopilot that has the protocol changed. For example, the user has a plane with non F7/H7 CAN capable autopilot and upgrades it to one that is, then loads his existing parameter file while setting up the plane with the new autopilot. As soon as the parameter file is loaded and the autopilot is rebooted, communication is lost and cannot be re-established. 

What has occurred, is that the protocol for the SERIALx port that Windows was using has been changed. Almost always, this is the highest numbered SERIALx port since that is commonly set to -1 on non-CAN capable autopilots, and the Windows COM port driver has selected this interface as the COM port instead of SERIAL0.

The procedure to recover is as follows:

.. _loading-composite-USB:

- Go to Windows Device Manager and find the COM port being used by the autopilot in the Ports listings. It will have the COM Port # you used to connect initially to Mission Planner. Right click and it will present "Update driver software" as one of the options. Click it.

.. image:: ../../../images/devicemanager.png

- Click the "Browse my computer......" option and then click the "Choose from a list..." option and you will see this screen:

.. image:: ../../../images/composite-driver.png

- Scroll down the top list until "Composite USB" option appears and click it.

- Now reconnect your autopilot to the PC and two COM ports will be presented. One will connect (the remaining one with MAVLink Protocol) and the other will not. If you do not connect to one, try the other. But DO NOT disconnect the autopilot from the PC or the composite driver will unload and you will have to start over.

- Now that you are connected to Mission Planner, change back the protocol of the Serialx port protocol to 2 (MAVLink2). You can now disconnect and reconnect the autopilot and it will present only one COM port and you should be able to connect from now on. Do not change this protocol from now on unless trying to utilize the SLCAN interface. It may be a bit unfamiliar since the Mission Planner SERIALx port being used is no longer the normal SERIAL0 but rather,the highest port, but this does not affect anything in the autopilot's configuration and operation.


Related topics
==============

:ref:`Mission Planner Bluetooth Connectivity <common-mission-planner-bluetooth-connectivity_detailed_connecting_with_mission_planner>`

[copywiki destination="plane,copter,rover,planner"]
