.. _quadplane-overview:

==================
QuadPlane 개요
==================

.. image:: ../images/quadplane_PorterOctaQuad.png
    :target: ../_images/quadplane_PorterOctaQuad.png

QuadPlane은 고정익과 멀티콥터가 결합된 형태의 비행체다. 이런 형태의 비행체가 갖는 장점은 수직 이착륙이 가능하고 비행 속도가 빠르고 넓은 범위 비행이 가능하다. 도착지점에서는 호버링이 가능하며 멀티콥터와 같은 형태의 비행이 가능하다.

QuadPlane은 Plane을 기반으로 만들지만 1개 이상의 모터로 안정적인 제어를 하는 콥터와 비슷해진다. 일부 설정에서는 4개 이상의 모터가 추가된다. 추가적인 모드와 명령으로 QuadPlane은 이착륙과 콥터처럼 비행이 가능하다. 자동 및 비행제어 보조 모드에서 Plane과 VTOL 모드 사이에 부드러운 변환이 가능하다. 추가 로터는 일반 Plane 모드에서 양력과 안정성을 제공하는 역할도 한다.

타입과 설정(Types and Configurations)
========================

아래 이미지는 가능한 다양한 여러 설정들을 보여준다. 모터는 tilt 제어를 사용하는 경우도 있다. 모터의 수는 1 ~ 8개 혹은 그 이상으로 다양한다. QuadPlane의 VTOL stance는 수평일 수도 있다. QuadPlane의 VTOL stance는 수평일 수도 있고 수직일 수도 있다. 일반 고정익 비행에서는 수평이고 Tailsitter에서는 수직이다.

.. image:: ../../../images/quadplane.png
  :target: ../_images/quadplane.png


Firmware 설치하기(Installing the Firmware)
=======================

QuadPlane 기능은 Plane 펌웨어 내에서 하나의 옵션이므로 Plane 펌웨어를 설치하는 방법과 동일하다.

plane 펌웨어를 설치할때, 파라미터 목록에서 :ref:`Q_ENABLE<Q_ENABLE>` 파리미터르 볼 수 있다. 기본값은 0으로 되어 있으며 QuadPlane 지원이 비활성화되어 있는 상태다. :ref:`Q_ENABLE<Q_ENABLE>`을 1로 설정하면 QuadPlane 지원이 활성화된다. 다음으로 파라미터 목록을 refresh하면 다른 QuadPlane 옵션을 볼 수 있다. QuadPlane와 관련된 모든 파라미터들은 \Q_ 로 시작한다.
