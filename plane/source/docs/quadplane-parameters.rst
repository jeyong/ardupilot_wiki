.. _quadplane-parameters:

QuadPlane 파라미터 셋업(QuadPlane Parameter setup)
=========================

모든 QuadPlane 관련 파라미터는 "Q\_" 로 시작한다. 파라미터들은 Copter 파라미터와 아주 유사해서 경험이 있다면 쉬울 것이다.

핵심 파라미터들 :

-  QuadPlane 기능을 활성화 시키기 위해서 :ref:`Q_ENABLE<Q_ENABLE>` 파라미터를 1로 설정하고 파라미터 목록을 refresh한다.
-  :ref:`Q_THR_MIN_PWM<Q_THR_MIN_PWM>` 와 :ref:`Q_THR_MAX_PWM<Q_THR_MAX_PWM>` 파라미터는 quad motor의 PWM 범위를 설정하는데 사용한다. (이렇게 하면 전면 motor를 위해서 해당 범위와 다른 설정할 수 있다.) 비행체의 ESC가 기대하는 범위로 설정할 필요가 있다.
-  가장 중요한 튜닝 파리미터는 :ref:`Q_A_RAT_RLL_P<Q_A_RAT_RLL_P>` 와
   :ref:`Q_A_RAT_PIT_P<Q_A_RAT_PIT_P>` 이다. 이것들의 기본값은 0.25이지만 QuadPlane에는 더 높은 값일 수 있다.
-  :ref:`Q_M_SPIN_ARM<Q_M_SPIN_ARM>` 파라미터는 quad 모드로 arming되는 때에 모터 출력의 올바른 레벨을 얻기 위해서 중요하다.
-  :ref:`ARMING_RUDDER<ARMING_RUDDER>` 을 2로 설정하는 것을 추천한다. 이렇게 하면 rudder의 disarm을 허용한다. 대안으로 유효한 비행 모드 중에 하나로 :ref:`MANUAL <manual-mode>` 를 가질 수 있다. (quad motor를 shut down 가능) 비행하는 동안 모터를 disarm 시키는 hard left rudder와 zero throttle하지 않도록 주의하자.

추가로 QuadPlane의 동작은 :ref:`Q_OPTIONS<Q_OPTIONS>` bitmask 파라미터의 설정으로 수정할 수 있다. (기본적으로 어떤 bit도 설정되어 있지 않다.):

- bit 0 : 설정한 경우 날개 레벨을 유지하기 위해서 VTOL에서 Plane 모드로 강제 전이시키고, 전이가 일어나는 동안 VTOL 모터에서 climbing 하지 않게 한다. (미션에서 VTOL takeoff 후에 더 높은 waypoint로 이동하는 것처럼)
- bit 1 : 설정한 경우 VTOL takeoff 대신에 고정익 takeoff을 사용한다. 별도의 VTOL_TAKEOFF 미션 명령 대신에 TAKEOFF만 보낼 수 있다. 반면에 QuadPlane은 TAKEOFF 미션 명령을 위해서 VTOL takeoff를 사용할 것이다.
-  bit 2 : 설정한 경우 VTOL 착륙 대신에 고정익 착륙을 사용한다. 별도의 VTOL_LAND 미션 명령 대신에 LAND만 보낼 수 있다. 반면에 QuadPlane은 LAND 미션 명령에 대해서 VTOL_LAND 를 사용한다.
-  bit 3 : 설정한 경우 미션 VTOL_TAKEOFF의 takeoff 고도를 Mission Planner에서 설정한 대로 해석한다.(ie Relative to Home/Absolute {ASL}/Terrain {AGL}) 그렇지 않으면 takeoff 지점의 고도와 관련된다. (AGL)
-  bit 4 : 설정한 경우 "고정익 접근법 사용"에 대해서 VTOL_LAND 미션 명령 동안 VTOL 비행으로 전이되는 대신에 VTOL 착륙을 수행하며 plane 모드로 남아서 착륙 위치로 가서 VTOL_LAND waypoint에 설정한 고도로 상승/하강을 한다. 착륙 지점의 :ref:`Q_FW_LND_APR_RAD<Q_FW_LND_APR_RAD>` 내에 도착하면 waypoint에서 설정한 해당 고도로 상승/하강을 마치기 위해서 LOITER_TO_ALT을 수행한다. 바람으로 VTOL 모드로 전이되고 착륙지점으로 가서 착륙한다. 그렇지 않으면 표준 VTOL_LAND가 실행된다. 추가 정보는 :ref:`quadplane-auto-mode` 를 참고하자.

동작은 :ref:`Q_RTL_MODE<Q_RTL_MODE>` 나 :ref:`Q_GUIDED_MODE<Q_GUIDED_MODE>` 파라미터로 수정할 수 있다.

.. note::

   QuadPlane code는 제대로 동작하기 위해서는 GPS lock이 필요하다. 이는 plane code의 속성을 그대로 따르는 것이고 GPS lock이 되지 않은 경우에는 자세와 위치의 inertial estimation이 비활성화된다. QuadPlane을 실내에서 날리지 말자. 잘 안난다.

