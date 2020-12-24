.. _ac_rollpitchtuning:

============================
수동 roll 및 pitch 튜닝 (Manual Roll and Pitch Tuning)
============================

비행체가 최적의 성능을 내기 위해서 튜닝할 수 있는 다양한 gain들이 있다.
가장 중요한 것은 Rate Roll과 Pitch P 값이다. 이 값들로 원하는 회전 rate가 모터 출력으로 변환된다.
적어도 Stabilize 모드에서 이 값만 가지고도 꽤 비행이 잘되도록 할 수 있다.

미션 플래너를 이용하여 설정을 진행하도록 한다.

이 파라미터 튜닝과 관련된 일반적인 조언 :

-  이 파라미터 값이 너무 높은 값이면 비행체가 roll과 pitch로 빠르게 진동(oscillation)이 발생한다.
-  이 파라미터 값이 너무 낮은 값이면 비행체가 느리게 반응한다.
-  높은 전원을 사용하는 비행체는 낮은 gain을 사용해야만 하고 낮은 전원을 사용하는 비행체는 더 높은 gain을 사용해야만 한다.

.. _ac_rollpitchtuning_in-flight_tuning:

조정기를 이용한 튜닝
~~~~~~~~~~~~~~~~~~~~~~~~

:ref:`Transmitter based tuning <common-transmitter-tuning>` 를 참고하면 비행 중에 이 파라미터와 다른 파라미터도 튜닝하는 방법에 대한 정보를 얻을 수 있다.

비행 중 튜닝 비디오
~~~~~~~~~~~~~~~~~~~~~~~~~

..  youtube:: NOQPrTdrQJM#t=145
    :width: 100%

로그를 이용한 성능 검증
=========================================

스테빌라이져 모드 선응을 보기 위한 최선의 방법은 비행 로그를 받아서 확인할 수 있다.
log를 미션 플래너를 통해서 열고 ATT 메시지의 Roll-In 이나 DesRoll 그래프와 Roll (actual roll)과 Pitch-In 혹은 DesPitch(desired pitch angle)과 Pitch(실제 pitch angle) 그래프를 확인할 수 있다.
이렇게 2개는 아래와 같이 잘 추적할 수 있다.

.. image:: ../images/Tuning_StabilizeCheck.png
    :target: ../_images/Tuning_StabilizeCheck.png

