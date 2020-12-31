.. _sitl-with-realflight:

==========================
RealFlight와 STIL 사용하기(Using SITL with RealFlight)
==========================

.. youtube:: 5XqQ52n_U8M
    :width: 100%

`RealFlight <http://www.realflight.com/>`__ 는 상업용 3D 비행 시뮬레이션으로 커스텀 비행체를 설계하고 테스트할 수 있다.


이 시뮬레이터는 Windows에서만 동작하고 RealFlight 8이나 9가 필요하다. RealFlight-X는 ArduPilot에서 동작하지 않는다. 만약 처음 설치하는 경우라면 RealFlight 9를 추천한다.

가장 쉽고 빠르게 사용하는 방법은 `Steam <https://store.steampowered.com/>`__ 에서 `RealFlight 9 <https://store.steampowered.com/app/1070820/RealFlight_9/>`__ 를 구매해서 설치하는 것이다.

사전에 Flight Control box method를 설정하였다는 가정과 Interlink Controller나 조이스탁 혹은 OpenTX 조정기를 조이스틱 모드로 사용하고 있다고 가정한다.

.. note:: RealFlight는 조이스틱 장치가 없으면 실행되지 않는다. AUTO 미션만 시뮬레이션하고 싶거나 조이스틱/조정기 입력을 많이 사용하지 않는 경우라면 `vJoy <http://vjoystick.sourceforge.net/site/index.php/download-a-install/download>`_ 가상 조이스틱을 이용할 수 있다. 이렇게 하면 이 조정기로 RealFlight로 시작할 수 있고 비행체는 GCS나 가상 화면 조이스틱("vJoy Feeder")프로그램로 조정할 수 있다. (가상 장치 드라이버와 동시 설치됨)

만약 OpenTX 조정기를 사용하는 경우라면 맨 마지막 섹션을 참고하자. RealFlight를 설치 후에 조장에 익숙해지도록 한다.

RealFlight Link 기능 활성화(Enabling RealFlight Link Feature)
================================

RealFlight 9에서 Settings->Physics 로 가서 FlightAxis 옵션을 활성화시키고 RealFlight를 재시작한다.

RealFlight 설정(Configure RealFlight)
====================

  - RealFlight를 실행한다.(일반적인 RealFlight와 같음)
  - `ArduPilot/SITL_Models/RealFlight/Released_Models/MultiRotors/QuadCopterX/QuadcopterX-flightaxis_AV.RFX <https://github.com/ArduPilot/SITL_Models/blob/master/RealFlight/Released_Models/Multicopters/QuadCopterX/QuadcopterX-flightaxis_AV.RFX>`__ 에서 QuadcopterX를 다운로드한다.
  - `parameter file for this model <https://github.com/ArduPilot/SITL_Models/blob/master/RealFlight/Released_Models/Multicopters/QuadCopterX/QuadCopterX.param>`__ 를 다운로드한다. text 포맷으로 저장되었는지 확인한다. 나중에 사용된다.
  - Simulation -> Import -> RealFlight -> RealFlight Archive(RFX, G3X)를 선택하고 위에서 다운받은 QuadcopterX를 선택한다. "..was successfully imported" 메시지가 나타나야 한다.
  - Aircraft -> Select Aircraft -> "Custom Aircraft" 열기 -> "QuadcopterX-flightaxis" 선택한다. 현재 상태에서 RC 입력은 조정기 스틱에서 받으며 날수 있는 상태는 아니다.

  .. image:: ../images/realflight-select-aircraft.png
    :target: ../_images/realflight-select-aircraft.png

RealFlight 내부에서 성능 개선을 위해서 그래픽 옵션을 줄인다.:

  - Simulation, Settings, Graphics
  - "Quality" 아래에 있는 모든 설정을 "No" 혹은 "Low"로 한다.(예제 "Clouds"는 No, "Water Quality"는 Low)
  - "Hardware" 아래에 "Resolution"을 "800 x600 Medium(16 bit)"로 설정하고 "Full Screen" 모드를 선택한다.
  - physics settings 아래에 "Pause Sim When in Background"를 No로 설정하고 "Automatic Reset Delay(sec)"를 2.0으로 설정한다.
   
  .. image:: ../images/realflight-settings-graphics.png
    :target: ../_images/realflight-settings-graphics.png
   
미션 플래너의 STIL로 연결하기(Connecting to Mission Planner's SITL)
------------------------------------

.. note:: 사양이 낮은 PC의 경우 RealFlight와 미션 플래너 SITL를 동시에 실행하기가 쉽지 않을 수 있다. 이 경우 2대 PC로 셋업하는 방법을 참고한다. Linux PC를 가지고 잇다면 처리 부하를 나눌 수 있다.

- Config/Tuning에서 플래너는 Layout 드롭다운을 "Advanced"로 설정한다.
- 상단 메뉴바에서 Simulation 선택
- "Model" 드롭다운에서 "flightaxis"를 선택하고 멀티로터 아이콘을 누른다.
- 전체 파라미터 목록이나 트리 화면에서 오른쪽에서 realflight-quad를 선택하고 load parameter를 누른다.

  .. image:: ../images/realflight-mp-sitl.jpg
    :target: ../_images/realflight-mp-sitl.jpg

real-flight 조정기에서 붉은색의 "reset" 버튼을 누르거나 PC의 스페이스 바를 누른다. 이렇게 하면 비행체의 자세나 위치를 리셋하여 SITL과의 연결을 초기화한다.

- "FlightAxis Controller Device has been activated." 메시지가 나타나야 하고 모터는 조용한 상태가 되어야 한다.

비행체의 위치가 리셋되지 않는 경우라면 RealFlight 내에서 다음을 시도한다. : 

  - Aircraft, Aircraft 선택
  - Custom Aircraft, QuadcopterX-flightaxis
  - OK 누르기 
  - 비행체의 위치가 리셋된 후에 조정기의 "Reset" 버튼을 누르거나 PC의 스페이스 바를 다시 누른다.

이 시점에 미션 플래너를 통해서 "QuadCopterX-flightaxis" 모델에 대해서 파라미터 파일을 로드한다. 이제 arm 및 비행을 할 준비가 되었다.

별도의 PC에서 실행되는 SITL에 연결하기(
Connecting to SITL running on a separate (or Virtual) machine) :
--------------------------------------------------------------

이 기술은 2개 PC에게 각각 처리할 일을 분리시킨다.:
RealFlight와 물리/비행 그래픽을 실행하는 윈도우 PC와 SITL 모델을 실행하는 Linux VM이 있다. 이렇게 하면 ArduPilot의 master branch가 아니라 직접 생성한 코드를 미션 플래너 STIL로 테스트도 가능하다.

.. image:: ../images/dualPC-realflight.png


- 최선의 성능을 얻기 위해서는 2개 PC 사이를 기가비트 이더넷으로 직접 연결하는 것이다.
- 콘솔을 열고 "ipconfig"를 입력해서 RealFlight가 실행되는 윈도우 PC의 IP 주소를 결정한다. 윈도우 PC에서 :ref:`cygwin <building-setup-windows-cygwin>` 혹은 :ref:`WSL <building-setup-windows10>` 를 사용해서 sitl을 실행한다면 192.168.x.x 혹은 127.0.0.1 와 같은 결과를 얻을 수 있다. 2대 PC를 직접 연결한다면 10.26.0.2 와 비슷한 결과를 보게 될 것이다.

.. note:: 2대 PC 사이에 통신을 방해하는 방화벽이 없어야 한다. 

- SITL이 실행되는 별도의 PC에서 SITL sim_vehicle.py를 "-f flightaxis:192.168.x.x" 옵션으로 실행하거나 만약 전통 헬기를 사용한다면 "-f heli - -model flightaxis:192.168.x.x" 옵션을 사용해서 실행한다.

     - cd ArduCopter (아래 Note 참고)
     - sim_vehicle.py -f flightaxis:192.168.x.x - -map - -console
- RealFlight로 돌아와서 조정기의 붉은색 "RESET" 버튼을 누르거나 PC의 스페이스 바를 누른다.
- 대략 몇 분 뒤에 비행체가 SITL map에 나타나야한다.
- SITL 타입 내부 ``param load <filename>`` 을 입력해서 동일한 디렉토리에 있는 목록을 모델로 load한다. 어쩌면 ``param fetch`` 입력하고 나서 load를 다시 해야만 할 수도 있다. 이는 파라미터 집합을 나타나기 전에 활성화가 필요한 파라미터를 load하기 위해서이다. 그리고 일부 경우에서는 새로운 파라미터가 적용되기 위해서 SITL을 재시작해야 할 수도 있다.
- 연결의 성능은 "ArduCopter" 창을(SITL이 실행되고 있는 PC) 열어서 검사할 수 있다.  "FPS" (Frames Per Second) 카운트는 150 이상이여야 비행체가 잘 날 수 있다. (평균은 이 값보다 낮을 수 있다.)

.. note:: 위 내용은 Copter에 해당된다. ArduPlane이나 ArduRover로 변경해서 비행체 타입을 선택하기 위해서 sim_vehicle.py를 시작하거나 시작할때 -v <vehicletype>를 직접 입력할 수 있다.

제공되는 모델들 사용하기(Using ready-made models)
-----------------------

위에서 언급한 바와 같이 RealFlight는 커스텀 비행체를 설계할 수 있다. 크기, 무게, 외관, 모터, control surface 위치 등.

ArduPilot 개발자들이 많은 커스텀 모델들을 만들고 있고 이를 `ArduPilot/SITL_Models repository <https://github.com/ArduPilot/SITL_Models>`__ 저장소에 저장해놨다.
``git clone https://github.com/ArduPilot/SITL_Models.git`` 를 사용해서 이 repo를 :ref:`clone <git-clone>` 할 수 있다. 다음으로 RealFlight로 이 모델들을 load한다.
각 모델에 대한 디렉토리에서 .parm 파일이 있는데 해당 모델에 사용가능한 적절한 튜닝 파라미터 집합으로 SITL로 load시킬 수 있다.

SITL_Models 폴더는 모델들을 위한 현재 작업 중인 서브 디렉토리를 RealFlight 디렉토리를 가지고 있다. 그리고 Released_Models 디렉토리는 InterLink 제어기와 테스트 작업을 완료한 모델이며 셋업과 기능에 대해서는 README.md 파일에서 확인할 수 있다.

이 모델들 중에 하나를 import하기 :

  - RealFlight에서 Simulation >> Import >> RealFlight Archive (RFX, G3X) 선택하고 원하는 모델을 선택한다.
  - Aircraft >> Select Aircraft 를 선택하고 위 단계에서 import시킨 모델을 선택한다.

  .. image:: ../images/realflight-import-model.png
    :width: 70%
    :target: ../_images/realflight-import-model.png

  - SITL 내부에서 해당 모델과 동일한 디렉토리에 있는 파리미터를 load하려면 ``param load <filename>`` 을 입력한다. ``param fetch`` 를 입력한 후에는 다시 이를 load해야만 한다. 그렇게 해야 파라미터 집합이 나타나기 전에 활성화가 필요한 파라미터를 load할 수 있다.

  .. image:: ../images/realflight-import-parms.png
    :width: 70%
    :target: ../_images/realflight-import-parms.png

RealFlight와 SITL와 OpenTX 사용하기
-----------------------------------

3가지 접근법이 있다. Minimal: AETR flight control axes만 셋업, Maximal : SITL 모듈에 대해서 적어도 7개 채널 가지기, TX를 사용해서 비행체를 날리는 것을 사용하는 방법에 대해서 에뮬레이팅. Interlink DX 제어기 에뮬레이션.

Minimal: TX 전원켜고 wizard로 시뮬레이션에서 사용할 새로운 plane 모델을 프로그램하기 위해서 USB로 연결하고 joystick을 선택한다.(최신 OpenTX 버전은 main radio setup페이지에 영구 선택을 허용한다.) RealFlight에서 Simulation-> Select controller를 선택한다. Taranis를 선택하고 roll, pitch, yaw, throttle을 설정하고 칼리브레이션한다. 이제 모드를 변경하거나 스위치를 설정하기 위해서 MAVProxy나 미션 플래너 명령을 사용할 수도 있다.

Maximal: 위 내용에 추가로 채널 5, 6, 7, 8에 대해서 TX 모델을 스위치나 슬라이더/pots로 설정한다. 다음으로 RealFlight controller 셋업에서 이 채널에 대한 기능을 추가한다. function 이름에 신경쓸 필요는 없다. 단지 SITL 모델로 전달되는지만 확인하면 된다. 이제 ``RCx_OPTION`` 기능을 model 파라미터에 있는 채너들로 할당할 수 있다.


InterLink DX/Elite controller emulation: Interlink 조정기와 거의 비슷하게 동작한다. 일반 조정기를 사용하여 RealFlight 시뮬레이션과 SITL에서 사용할 수 있게 해준다. 상세 설정은 :ref:`interlink-emulation` 를 참고한다.

6개 position mode 스위치를 셋업하기 위해서 :ref:`here<common-rc-transmitter-flight-mode-configuration>` 를 통해서 OpenTX 조정기 설정하는 것을 알아보았다. 하지만 먼저 RealFlight controller를 칼리브레이션해야한다. 이때 mode 채널에 있는 dual position 스위치를 사용한다. 다음으로 조정기로 돌아와서 6개 PWM 레벨을 제공하도록 변경한다. 이 작업은 RealFlight가 칼리브레이션 값을 바탕으로 오토 스케일링을 하기 때문에 필요하다. 만약 6개 PWM 레빌이 인식하는 범위의 가운데 있다면 채널의 PWM 양끝값은 칼리브레이션에서 사용되지 않고 PWM 레벨은 SITL로 전달되기 전에 RealFlight로 변경된다. 

.. toctree::
    :hidden:

    Interlink Emulation <interlink-emulation>
    Understanding SITL using RealFlight <flightaxis>

