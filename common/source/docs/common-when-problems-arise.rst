.. _common-when-problems-arise:

===================
문제가 발생했을때
===================

Ardupilot은 아주 다양한 기능과 유연성을 제공한다. 하지만 높은 성능과 유연성을 제대로 사용하기 위해서는 다양한 설정, 파라미터 및 복잡성도 동반된다.

이 문서는 최대한 설정, 파라미터, 운영 모드 등에 대한 정보를 최대한 상세히 제공해서 설정과 운영에 드는 시간과 노력을 줄이는데 도움을 주고자한다. 지속적으로 새로운 버전을 내면서 더 설명이 필요한 부분에 대해서 업데이트를 진행해 나갈 예정이다. 수정 및 보완은 :ref:`common_wiki_editing_guide`을 참고한다.

이슈가 발생했을때 무엇을 해야할까?
===============================

1. 먼저 "처음 셋업하기"와 "처음 비행하기" 섹션을 따라하면서 관련된 자료를 꼼꼼히 읽도록 한다. 만약 고급 설정과 하드웨어 옵션을 처리해야하는 경우에는 관련 문서를 꼼꼼히 읽고 진행해야 한다.

2. 여기서 제공되는 자료가 이슈를 해결하는데 도움이 되지 않았다면 `Discuss Forum <https://discuss.ardupilot.org/>`__ 섹션에 도움을 요청하라. 

3. :Ref:`dataflash log <common-diagnosing-problems-using-logs>`에서 도움을 얻을 수 있다. 이 정보를 가지고 이슈를 분석해서 다른 사람으로부터 도움을 받을 수 있다.

.. note:: WatchDog resets ("WDG:")은 `이 페이지 <https://github.com/ArduPilot/ardupilot/issues/15915>`_ 로 Internal Errors ("Internal Error:")는 `여기 <https://github.com/ArduPilot/ardupilot/issues/15916>`_ 에 리포팅하면 된다.


[site wiki="copter"]


Copter 일반적인 문제들
======================

-  새로 설정한 비행체가 이륙시 뒤집어 지는 문제에 대한 원인들 :
   모터 순서나 회전 방향이 제대로 설치했는지 확인
   혹은 프로펠러의 ccw/cw 맞게 설치했는지 확인
   Pixhawk와 RC수신치 연결 확인  
-  roll 혹은 pitch 축으로 흔들리는 현상. 보통은 Rate P 값이 제대로 설정되지 않은 경우다. :ref:`common-tuning` 섹션을 보고 이 gain 값들을 조정하도록 한다.
-  빠르게 하강할때 비행체가 흔들리는 현상. prop wash로 생기는 문제일 확률이 높다. 이 경우 Roll/Pitch P 값을 올리더라도 튜닝하기가 거의 불가능하다.
-  이륙시에 오른쪽 혹은 왼쪽으로 yaw가 15도 정도 생기는 현상. 모터가 기울어져 있거나 :ref:`ESC가 제대로 칼리브레이션 되어 있지 않아서 <esc-calibration>` 발생할 수 있다.
-  바람이 없는 상황에서도 비행체가 항상 특정 방향으로 기울어져 날라가는 경우. 비행체의 수평 비행을 위해서 `SaveTrim 혹은 AutoTrim <autotrim>`을 수행해 본다.
-  조정기 쓰로틀 스틱을 내리더라도 비행체가 빠르게 올라가는 현상. 비행체 자체 진동에 의해서 발행할 수 있으며 https://ardupilot.org/copter/docs/common-vibration-damping.html 를 참고한다.
-  roll 혹은 pitch 축으로 가끔씩 경련을 일으키는 원인은
   Pixhawk에 연결된 조정 수신게에 간섭이 발생하는 경우(FPV 장치들을 수신기 주변에 설치하지 않는다.) 혹은 ESC 문제일 수도 있는데 이 경우 :ref:`calibrating them <esc-calibration>`를 참고한다.
-  비행 중에 갑자기 뒤집히는 경우의 원인. 이 경우 거의 대부분 모터나 ESC의 :ref:`기계적 이상 <common-diagnosing-problems-using-logs_mechanical_failures>`이 원인이다.
[/site]
