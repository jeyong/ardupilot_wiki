.. _precision-landing-with-irlock:

=========================================
IR-Lock을 이용한 정밀 착륙 및 로이터(Precision Landing and Loiter with IR-LOCK)
=========================================

개요
========

Copter 3.4(이상 버전)에서 `IR-LOCK sensor <https://irlock.com/collections/frontpage/products/ir-lock-sensor-precision-landing-kit>`__ 와 :ref:`sonar or lidar <common-rangefinder-landingpage>` 를 이용해서 정밀 착륙 기능을 지원한다.
이 시스템을 사용해서 비행체가 착륙(LAND) 모드(GPS lock이 된 경우)로 진입할 때 IR beacon이 1m/s이하의 속도로 이동하더라도 30cm 범위 내로 착륙이 가능하다.

Copter 3.5 (이상 버전)은 추가 기능으로 비행체가 Loiter 모드 중에 타겟 위의 위치를 유지하면서 정밀 Loiter를 지원한다.
조정사가 조정기의 :ref:`auxiliary 기능 스위치 <common-auxiliary-functions>` 중에 하나를 사용해서 이 기능을 활성화 시킬 수 있다. (Copter-4.0 이전 버전에서, CH7_OPT 나 CH_8_OPT를 사용한다.)

..  youtube:: rGFO73ZxADY
    :width: 100%

구매하는 방법
===============

`IR-LOCK sensor <https://irlock.com/collections/frontpage/products/ir-lock-sensor-precision-landing-kit>`__ 는 `irlock.com <https://irlock.com/>`__ 에서 구매할 수 있다.
IR-LOCK 센서는 `Pixy camera <https://pixycam.com/pixy-cmucam5/>`__ 를 수정하여 IR beacon detector로 동작하도록 사전에 설정되어 나온 제품입니다.
이 센서와 호환되는 여러 가지 IR beacon들이 있습니다. `MarkOne Beacon <https://irlock.com/collections/markone>`__ 는 **15 미터* 범위에서 모든 빛의 환경에서 잘 동작한다.
`Beacon (V1.1) <https://irlock.com/collections/shop/products/beacon>`__ 은 다양한 환경에서 동작하는 적당한 가격의 제품이다.

.. figure:: ../images/sensorandMarkers01.jpg
   :target: ../_images/sensorandMarkers01.jpg

   IR-LOCK Sensor and IR Beacons

Pixhawk에 연결하기
=====================

IR-Lock 센서는 `I2C cable <https://irlock.com/collections/shop/products/pixhawk-cable>`__ 을 통해서 Pixhawk에 바로 연결할 수 있다. 만약 여러 개의 I2C 센서를 사용하고 있다면  \ `I2C splitter <http://store.jdrones.com/Pixhawk_I2C_splitter_p/dstpx4i2c01.htm>`__ 가 필요하다.
더 자세한 내용은 `irlock.com 문서 <https://irlock.readme.io/docs>`__ 를 참조하자.

.. figure:: ../images/precision_landing_connect_irlock_to_pixhawk.jpg
   :target: ../_images/precision_landing_connect_irlock_to_pixhawk.jpg

   IRLock sensor/Pixhawk Wiring

비행체에 장착하기
=====================

IRLOCK 센서는 비행체 하단부에 카메라 렌즈가 땅을 보도록 장착해야만 한다. IRIS용 장착 브락켓은 `여기 <https://irlock.com/collections/frontpage/products/sensor-bracket-for-iris>`__ 서 판매하고 있다.
센서 보드는 방향성을 가지므로 보드에 있는 흰색 버튼이 비행체의 앞을 향하도록 해야한다.(혹은 다른 방법은 카메라 렌즈에 가장 가까운 면이 비행체의 정면을 향하도록 해야만 한다.)

아래 이미지는 3DR IRIS+의 바닥에 카메라를 장착하는 것을 보여주고 있다.
센서를 장착하는 가장 추천하는 방법은 최대한 Pixhawk에 가까운 위치에 장착하는 것이다. 하지만 테스트 결과 다른 위치에서도 정상적인 성능을 발휘하고 있다.

.. figure:: ../images/IRISbracket03.jpg
   :target: ../_images/IRISbracket03.jpg

   IR-LOCK 센서는 Iris+의 바닥에 장착

..  youtube:: I8QF313F3bs
    :width: 100%

미션 플래너를 통한 설정
=============================

정밀 착륙 기능을 활성화 시키기 위해서 미션 플래너를 이용하여 다음 파라미터들을 설정하고 비행제어기를 리부팅시킨다.

-  :ref:`PLND_ENABLED <PLND_ENABLED>` = 1
-  :ref:`PLND_TYPE <PLND_TYPE>` = 2

정밀 Loiter 기능을 활성화 시키기 위해서 :ref:`Auxiliary Function Switch <common-auxiliary-functions>`를  39로 설정하여야 한다. 39는 "Precision Loiter" 를 의미한다.

-  Copter-4.0 이전 버전에서 이 기능을 활성화하기 위해서 CHx_OPT 파라미터 미션 플래너를 통해서 39로 설정할 수 있다.

비행 및 테스트
==================

LAND 모드에서 동작한다.
특정 비행 모드 상태에서 LAND 모드로 설정해야 한다.(정말 착륙 기능은 LAND 모드에서만 동작한다.)

지상에 IR beacon 장치를 두고 비행체를 타겟 위 대략 10m 위치로 이륙시킨다.
비행체의 비행 모드를 LAND 모드로 변경한다. 만약 모든 것이 잘 동작한다면 비행체가 IR beacon 방향으로 이동해야만 한다. 성공한 데모는 아래에서 확인할 수 있다.(예전 펌웨어 사용) 

.. tip::

   갑자기 이상 동작이 발생하면 제어권을 다시 가져올 수 있도록 대비해야한다. (비행 모드를 스테빌라이져, 알티튜드 홀드나 Loiter 모드로 변경)

만약 비행체가 제대로 동작하지 않는다면, 로그를 다운받아서 PL 메시지를 검토한다.

-  만약 "Heal"(health) 필드가 "1" 이 아닌 경우라면 Pixhawk <-> IR-Lock 센서 사이에 통신 문제가 있었을 수 있다.
- "TAcq"(Target Acquired) 필드가 "1"이 아닌 경우라면 센서가 타겟을 보지 못하는 경우이다.
- pX, pY 값은 비행체로부터 타겟까지의 수평 거리를 뜻한다.
- vX, vY 값은 타겟이 비행체에 대한 추정 상대 속도를 뜻한다.

..  youtube:: IRfo5GcHniU
    :width: 100%

정밀 Loiter 데모:

..  youtube:: KoLZpSZDfII
    :width: 100%
