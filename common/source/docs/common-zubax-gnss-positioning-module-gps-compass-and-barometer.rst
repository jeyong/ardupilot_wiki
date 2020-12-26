.. _common-zubax-gnss-positioning-module-gps-compass-and-barometer:

==========================================================
Zubax GNSS 포지셔닝 모듈 — GPS, Compass, Barometer
==========================================================

`Zubax GNSS 2 <https://zubax.com/products/gnss_2>`__ 이중화 `UAVCAN <https://uavcan.org>`__ 버스 인터페이스를 지원하며 야외 환경에서 사용에 적합한 고성능 포지셔닝 모듈이다. GPS 수신기, 정밀 바로미터, 3축 compass을 포함하고 있다.

.. figure:: ../../../images/ZubaxGNSS2.jpg
   :target: ../_images/ZubaxGNSS2.jpg

   Zubax GNSS 2: GPS, Compass와 Barometer

다음 파라미터를 설정하고 시스템에 적용하기 위해서 리부팅한다.:

- :ref:`CAN_P1_DRIVER <CAN_P1_DRIVER>` = 1 (첫번째 CAN 포트를 활성화 시키기 위해서)
- :ref:`GPS_TYPE <GPS_TYPE>` = 9 (UAVCAN)

장치가 제대로 동작하지 않는다면 :ref:`common-canbus-setup-advanced`에서 CANBUS를 활성화 시키고 다음 단계로 :ref:`common-uavcan-setup-advanced` 수행하고 :ref:`GPS_TYPE <GPS_TYPE>` 혹은 :ref:`GPS_TYPE2 <GPS_TYPE2>` 파라미터를 9로 설정한다.

`제조사의 제품 관련 소개는 여기 <https://zubax.com/products/gnss_2>`__에서 찾을 수 있다.