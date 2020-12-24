.. _ac_throttlemid:

======================
호버링 쓰로틀 설정(Setting Hover Throttle)
======================

Copter에는 호버링 쓰로틀을 자동으로 학습하는 기능이 포함되어 있다. (기존에는 "mid throttle"라는 용어를 사용)
비행체가 수동 비행 모드(스테빌라이져 및 아크로 모드)가 아닌 비행 모드 상태에서 안정적인 호버링을 유지할때마다 :ref:`MOT_THST_HOVER <MOT_THST_HOVER>` 값은 천천히 평균 모터 출력으로 이동하게 된다. 

:ref:`MOT_THST_HOVER <MOT_THST_HOVER>` 값을 수동으로 설정하기를 원하는 경우라면 log를 다운받아서 CTUN.ThO 필드에 있는 값으로 설정하면 된다. 이 값의 범위는 반드시 0.2 ~ 0.8 사이여야 한다.

만약에 이런 학습 기능을 비활성화 하고자 한다면 :ref:`MOT_HOVER_LEARN <MOT_HOVER_LEARN>` 파라미터를 0으로 설정하면 된다.

.. image:: ../images/throttle_mid_learning.png
    :target: ../_images/throttle_mid_learning.png
