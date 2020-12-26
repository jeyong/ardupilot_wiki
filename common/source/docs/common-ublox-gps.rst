.. _common-ublox-gps:

=======================
UBlox GPS 설정
=======================

:ref:`3DR uBlox <common-installing-3dr-ublox-gps-compass-module>` 모듈의 설정을 변경하기 위해서 u-center에 연결하는 방법에 대해서 소개한다.
일반 사용자에게는 해당되는 내용은 아니다.

.. image:: ../../../images/3DR-ublox.jpg
    :target: ../_images/3DR-ublox.jpg

연결 옵션 #1 - 미션 플래너와 Pixhawk를 통한 연결
=================================================================

미션 플래너와 Pixhawk는 u-center와 GPS가 다음을 통해서 통신할 수 있다:

-  Pixhawk를 PC에 연결하고 미션 플래너와 연결한다.
-  비행 데이터 화면에서 Ctrl-F를 누르고 "MAVSerial pass"를 선택한다.
-  u-center를 열고 Receiver, TCP Client를 선택한다.
   Connection 윈도우에서 주소를 "localhost"로 Port는 "500"으로 선택하고 OK를 누른다.
-  u-center로 설정을 업로드하는 방법은 아래와 같다.

.. image:: ../../../images/GPS_PassThrough_MP.jpg
    :target: ../_images/GPS_PassThrough_MP.jpg

.. image:: ../../../images/GPS_PassThrough_Ucenter.png
    :target: ../_images/GPS_PassThrough_Ucenter.png

연결 옵션 #2 - FTDI 케이블
=================================

GPS를 컴퓨터로 연결하기 위해서 
`FTDI 케이블 <http://store.jdrones.com/cable_ftdi_6pin_5v_p/cblftdi5v6p.htm>`__ 와 `GPS 어댑터 케이블 <http://store.scoutuav.com/product/cables-connectors/gps-cable-10-cm/>`__ 이 필요하다. FTDI 장치를 컴퓨터에 처음 연결하는 경우라면 `Virtual COM port driver <http://www.ftdichip.com/Drivers/VCP.htm>`__ 를 다운받아서 설치해야 할 수도 있다.

.. image:: ../../../images/ublox_gps_ftdi_connection.jpg
    :target: ../_images/ublox_gps_ftdi_connection.jpg
