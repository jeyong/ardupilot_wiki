.. _flight-modes:

============
비행 모드(Flight Modes)
============

멀티콥터에서 사용할 수 있는 비행 모드에 대한 소개 및 관련 링크를 제공합니다.

개요
========

멀티콥터에는 23가지의 비행모드를 지원합니다. 이 중에 10개가 일반적으로 사용됩니다. 비행 안정
Copter has 23 flight built-in flight modes, 10 of which are regularly
used. There are modes to support different levels/types of flight
stabilization, a sophisticated autopilot, a follow-me system etc.

비행 모드는 라디오 조정기(:ref:`transmitter switch <common-rc-transmitter-flight-mode-configuration>`), 미션 명령, GCS나 컴퍼니온 컴퓨터를 통해서 변경할 수 있습니다.

.. raw:: html

   <table border="1" class="docutils">
   <tr><th>모드(Mode)</th><th>알티튜드 제어(Alt Ctrl)</th><th>포지션 제어(Pos Ctrl)</th><th>GPS</th><th>요약</th></tr>
   <tr><td>아크로(Acro)</td><td>-</td><td>-</td><td></td><td>자세 유지(Holds attitude), 스스로 수평 유지는 되지 않음(no self-level)</td></tr>
   <tr><td>에어모드(Airmode)</td><td>-</td><td>-/+</td><td></td><td>실제로는 비행모드는 아니라 기능. 아래 참조</td></tr>
   <tr><td>알티튜드 홀드(Alt Hold)</td><td>s</td><td>+</td><td></td><td>고도를 유지하고 롤과 피치를 통해 수평 유지</td></tr>
   <tr><td>오토(Auto)</td><td>A</td><td>A</td><td>Y</td><td>미리 정의한 미션을 수행</td></tr>
   <tr><td>오토튠(AutoTune)</td><td>s</td><td>A</td><td>Y</td><td>제어 루프를 개선하기 위해서 pitch와 뱅크를 자동 수행</td></tr>
   <tr><td>브레이크(Brake)</td><td>s</td><td>A</td><td>Y</td><td>비행체를 즉시 멈추기</td></tr>
   <tr><td>써클(Circle)</td><td>s</td><td>A</td><td>Y</td><td>비행체 앞에 특정 지점을 중심으로 원형으로 자동 비행</td></tr>
   <tr><td>드리프트(Drift)</td><td>-</td><td>+</td><td>Y</td><td>Like stabilize, but coordinates yaw with roll like a plane</td></tr>
   <tr><td>플립(Flip)</td><td>A</td><td>A</td><td></td><td>자동으로 플립 수행</td></tr>
   <tr><td>FlowHold</td><td>s</td><td>A</td><td></td><td>옵티컬 플로를 사용하여 포지션 제어</td></tr>
   <tr><td>팔로우(Follow)</td><td>s</td><td>A</td><td>Y</td><td>다른 비행체를 따라가기</td></tr>
   <tr><td>가이드(Guided)</td><td>A</td><td>A</td><td>Y</td><td>Navigates to single points commanded by GCS</td></tr>
    <tr><td>Heli_Autorotate</td><td>A</td><td>A</td><td>Y</td><td>Used for emergencies in traditional helicopters. Helicopter only.  Currently SITL only.</td></tr>
   <tr><td>착륙(Land)</td><td>A</td><td>s</td><td>(Y)</td><td>Reduces altitude to ground level, attempts to go straight down</td></tr>
   <tr><td>로이터(Loiter)</td><td>s</td><td>s</td><td>Y</td><td>Holds altitude and position, uses GPS for movements</td></tr>
   <tr><td>PosHold</td><td>s</td><td>+</td><td>Y</td><td>Like loiter, but manual roll and pitch when sticks not centered</td></tr>
   <tr><td>RTL</td><td>A</td><td>A</td><td>Y</td><td>Retruns above takeoff location, may aslo include landing</td></tr>
   <tr><td>단순/매우 단순</td><td></td><td></td><td>Y</td><td>An add-on to flight modes to use pilot's view instead of yaw orientation</td></tr>
   <tr><td>스마트RTL(SmartRTL)</td><td>A</td><td>A</td><td>Y</td><td>RTL, but traces path to get home</td></tr>
   <tr><td>스포츠(Sport)</td><td>s</td><td>s</td><td></td><td>Alt-hold, but holds pitch & roll when sticks centered</td></tr>
   <tr><td>스테빌라이즈(Stabilize)</td><td>-</td><td>+</td><td></td><td>Self-levels the roll and pitch axis</td></tr>
   <tr><td>시스템ID(SysID)</td><td>-</td><td>+</td><td></td><td>Special diagnostic/modeling mode</td></tr>
   <tr><td>던지기(Throw)</td><td>A</td><td>A</td><td>Y</td><td>Holds position after a throwing takeoff</td></tr>
   <tr><td>지그재그(ZigZag)</td><td>A</td><td>A</td><td>Y</td><td>Useful for crop spraying</td></tr>
   </table>


.. raw:: html

   <table border="1" class="docutils">
   <tr><th>기호</th><th>정의</th></tr>
   <tr><td>-</td><td>수동 제어(Manual control)</td><tr>
   <tr><td>+</td><td>수동 제어(Manual control with limits & self-level)</td><tr>
   <tr><td>s</td><td>Pilot controls climb rate</td></tr>
   <tr><td>A</td><td>자동 제어(Automatic control)</td></tr>
   </table>


추천 비행 모드(Recommended Flight Modes)
========================

일반적으로 처음 멀티콥터를 사용하는 경우 아래 순서대로 비행 모드를 경험해 보는 것을 추천합니다. 다음 비행 모드로 넘어가기 전에 각각의 비행모드에 대해서 명확히 이해해야 합니다.:

-  :ref:`스테빌라이즈(Stabilize) <stabilize-mode>`
-  :ref:`알티튜드 홀드(Alt Hold) <altholdmode>`
-  :ref:`로이터(Loiter) <loiter-mode>`
-  :ref:`RTL (Return-to-Launch) <rtl-mode>`
-  :ref:`오토(Auto) <auto-mode>`

추가 비행 모드:

-  :ref:`아크로(Acro) <acro-mode>`
-  :ref:`에어모드(AirMode) <airmode>`
-  :ref:`Heli_Autorotate <traditional-helicopter-autorotation-mode>` for traditional helicopters only.
-  :ref:`오토튠(AutoTune) <autotune>`
-  :ref:`브레이크(Brake) <brake-mode>`
-  :ref:`써클(Circle) <circle-mode>`
-  :ref:`드리프트(Drift) <drift-mode>`
-  :ref:`플립(Flip) <flip-mode>`
-  :ref:`FlowHold <flowhold-mode>`
-  :ref:`팔로우(Follow) <follow-mode>`
-  :ref:`가이드(Guided) <ac2_guidedmode>` (and :ref:`Guided_NoGPS <guided_nogps>`)
-  :ref:`착륙(Land) <land-mode>`
-  :ref:`포지션홀드(PosHold) <poshold-mode>`
-  :ref:`스포츠(Sport) <sport-mode>`
-  :ref:`Throw <throw-mode>`
-  :ref:`Follow Me <ac2_followme>`
-  :ref:`Simple and Super Simple <simpleandsuper-simple-modes>`
-  :ref:`Smart RTL (Return-to-Launch) <smartrtl-mode>`
-  :ref:`SysID (System Identificaton) <systemid-mode>`
-  :ref:`ZigZag <zigzag-mode>`
-  :ref:`Avoid_ADSB <common-ads-b-receiver>` for ADS-B based avoidance of manned aircraft.  Should not be set-up as a pilot selectable flight mode.

대부분의 조정기에 있는 스위치 중에 3개 값을 가지는 스위치를 제공하고 있다. 비행 모드 6개를 지정하기 위한 방법 :ref:`here for setting up a 6-position flight mode switch <common-rc-transmitter-flight-mode-configuration>`.

GPS 의존
==============

Flight modes that use GPS-positioning data require an active GPS lock
prior to takeoff. To see if your autopilot has acquired GPS lock,
connect to a ground station or consult your autopilot's hardware
overview page to see the LED indication for GPS lock. Below is a summary
of GPS dependency for Copter flight modes.

Requires GPS lock prior to takeoff:

-  :ref:`Auto <auto-mode>`
-  :ref:`Heli_Autorotate <traditional-helicopter-autorotation-mode>`
-  :ref:`Circle <circle-mode>`
-  :ref:`Drift <drift-mode>`
-  :ref:`Follow <follow-mode>`
-  :ref:`Follow Me <ac2_followme>`
-  :ref:`Guided <ac2_guidedmode>`
-  :ref:`Loiter <loiter-mode>`
-  :ref:`PosHold <poshold-mode>`
-  :ref:`RTL (Return-to-Launch) <rtl-mode>`
-  :ref:`Smart RTL (Return-to-Launch) <smartrtl-mode>`
-  :ref:`Throw <throw-mode>`
-  :ref:`ZigZag <zigzag-mode>`

GPS lock이 필요없는 경우:

-  :ref:`Acro <acro-mode>`
-  :ref:`AirMode<airmode>`
-  :ref:`Alt Hold <altholdmode>`
-  :ref:`Stabilize <stabilize-mode>`
-  :ref:`Sport <sport-mode>`
-  :ref:`SysID <systemid-mode>`
-  :ref:`Land <land-mode>`

비행 모드의 전체 목록
=========================

.. toctree::
    :maxdepth: 1

    Acro <acro-mode>
    Altitude Hold <altholdmode>
    AirMode<airmode>
    Auto <auto-mode>
    Brake <brake-mode>
    Circle <circle-mode>
    Drift <drift-mode>
    Flip <flip-mode>
    FlowHold <flowhold-mode>
    Follow <follow-mode>
    Follow Me (GCS Enabled) <ac2_followme>
    Guided <ac2_guidedmode>
    Heli_Autorotate <traditional-helicopter-autorotation-mode>
    Land <land-mode>
    Loiter <loiter-mode>
    PosHold <poshold-mode>
    Position <ac2_positionmode>
    RTL <rtl-mode>
    Simple and Super Simple <simpleandsuper-simple-modes>
    Smart RTL (Return-to-Launch) <smartrtl-mode>
    Sport <sport-mode>
    Stabilize <stabilize-mode>
    System Identification <systemid-mode>
    Throw <throw-mode>
    ZigZag <zigzag-mode>
