.. _common-loading-firmware-onto-pixhawk:

================
Firmware 로딩하기
================

여기서는 Mission Planner를 이용해서 최신 펌웨어를 Pixhawk로 다운받는 방법을 보여준다. :ref:`common-loading-firmware-onto-chibios-only-boards` 를 참고하자.

[copywiki destination="copter,plane,rover,planner"]

컴퓨터에 Pixhawk 연결하기
=============================

일단 :ref:`ground station <common-install-gcs>` 를 설치했다면 Pixhawk를 PC를 USB 케이블로 연결하자. USB 허브를 사용하지 말고 가급적 직접 연결하자.

.. figure:: ../../../images/pixhawk_usb_connection.jpg
   :target: ../_images/pixhawk_usb_connection.jpg
   :width: 450px

   Pixhawk USB 연결

Windows는 자동으로 드라이버 SW를 찾아서 설치한다.

COM 포트 선택
===================

*Mission Planner* 사용하는 경우 화면의 우상단에 COM 포트를 선택할 수 있다.(**Connect** 버튼 근처) **AUTO** 혹은 보드에 해당하는 특정 포트를 선택한다.
Baud rate는 **115200**로 설정한다. 아직 **Connect** 를 누르지 않는다.

.. image:: ../../../images/Pixhawk_ConnectWithMP.png
    :target: ../_images/Pixhawk_ConnectWithMP.png

firmware 설치하기
================

Mission Planner에서 **SETUP \| Install Firmware** 화면에서 원하는 사용하는 비행체의 프레임(Quad or Hexa 등)과 일치하는 아이콘을 선택한다. "Are you sure?" 라는 메시지가 뜨면 **Yes** 로 답하자.

.. figure:: ../../../images/Pixhawk_InstallFirmware.jpg
   :target: ../_images/Pixhawk_InstallFirmware.jpg

   Mission Planner: Install FirmwareScreen

다음으로 사용하는 보드를 검출하고 Pixhawk의 USB를 빼라는 요청이 뜨면 OK를 누르고 다시 연결한다.

.. figure:: ../../../images/Pixhawk_InstallFirmware2.png
   :target: ../_images/Pixhawk_InstallFirmware2.png

   Mission Planner: Install FirmwarePrompt


제대로 동작하는 경우라면 오른쪽 맨아래 부분에 "erase...", "program...", "verify..", "Upload 문구가 나온다. firmware가 성공적으로 보드에 업로드되게 된다.

보통 bootloader가 exit되고 업로드되고 다시 전원이 켜진 후에 정상동작까지 수 초가 걸린다. 이제 CONNECT를 누른다.

Testing
=======

*Mission Planner Flight Data* 화면으로 전환하고 **Connect** 버튼을 눌러서 firmware가 기본적으로 동작하는지 테스트할 수 있다. HUD는 연결한 Pixhawk를 기울여봐서 값이 바뀌는지 확인해야한다.

:ref:`Mission Planner를 Pixhawk에 연결하기 <common-connect-mission-planner-autopilot>` 에서 Mission Planner에 연결 관련된 더 많은 정보를 찾을 수 있다.