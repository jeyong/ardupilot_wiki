.. _common-imu-batchsampling:

==========================================
IMU Batch Sampler로 진동 측정(Measuring Vibration with IMU Batch Sampler)
==========================================

IMU Batch Sampler는 IMU 센서로부터 고주파 데이터를 dataflash log로 저장할 수 있다. 
이 데이터로 비행 뒤에 진동 이슈를 진당하는데 사용할 수 있다. 해당 데이터의 TTF(Fast Fourier Transform)으로부터 생성된 그래프로 분석이 가능하다.

FFT는 데이터를 time domain으로부터 frequency domain으로 변환한다. 시간에 대한 가속도 데이터는 진동의 주파수를 보여주는 그래프로 변환할 수 있다.
이 그래프들의 주파수 특성은 프로펠러의 "blade passage frequency"에 spike이다. (프로펠러에서 arm으로 전달되는 frequency) 이 주파수가 결국은 비행체 몸체에 가속도(acceleration)를 유발한다. FFT는 다음과 같은 제약이 있다 :

- FFT는 센서의 샘플링 rate 반 이상의 주파수가 나타날 수 없다.
- 나타날 수 있는 가장 작은 주파수는 sample size / sample rate 의 반이다. 

일반적으로 샘플들은 Pixhawk에서 제공하는 gyro update와 동일한 rate를 가진다. 예제로 MPU9250 센서에서 :ref:`INS_FAST_SAMPLE <INS_FAST_SAMPLE>` 를 사용하고 있다면 샘플들은 8KHz를 가지게 된다. 만약 빠른 샘플링을 사용하지 않는다면 일반적으로 1KHz가 샘플링 rate가 된다.

비행전 설정(Pre-Flight Setup)
================

- :ref:`INS_LOG_BAT_MASK <INS_LOG_BAT_MASK>` = 1으로 설정하면 첫번째 IMU로부터 데이터를 수집한다.
- :ref:`LOG_BITMASK <copter:LOG_BITMASK>`의 IMU_RAW bit는 체크된 상태로 두면 안된다. 기본 :ref:`LOG_BITMASK<LOG_BITMASK>` 값은 괜찮다. 만약 체크 상태로 두면 결과가 혼돈을 줄 수 있다.(post-filter나 regular logging을 사용한다면 샘플을 얻지 못하는 것처럼). 하지만 센서 rate logging을 사용한다면 샘플을 얻게 되고 이때 SD 카드가 이를 저장한다.

비행 및 비행 사후 분석(Flight and Post-Flight Analysis)
===============================

- Perform a regular flight (not just a gentle hover) of at least a few minutes and :ref:`download the dataflash logs <common-downloading-and-analyzing-data-logs-in-mission-planner>`
- Open Mission Planner, press Ctrl-F, press the FFT button, press "new DF log" and select the .bin log file downloaded above

.. image:: ../../../images/imu-batchsampling-fft-mp2.png
    :target:  ../_images/imu-batchsampling-fft-mp2.png
    :width: 450px

- 가속도 데이터는 왼쪽 상단 창에 나타나며 수평축은 주파수를 보여주고 수직축은 amplitude를 보여준다. amplitude는 유용한 값으로 스케일링하지 않는다. 그래프의 의미는 진동의 주파수를 결정하는데 유용하지만 해당 level이 높은지 낮은지를 결정하는 것은 아니다. 진동 주파수가 300Hz를 넘으면 attitude나 position 제어 문제를 초래할 수 있다.
- 기본 설정은 필터되기 전의 raw accelerometer와 gyro 데이터를 보여준다. 필터링은 PID 제어 루프에 noise가 반입되지 않도록 하며 결국 모터에 noise가 작용하는 것을 막는데 결정적인 역할을 수행한다. 따라서 필터링된 후에 데이터를 살펴보는 것도 중요하다. 추가로 notch를 이용하여 (:ref:`INS_NOTCH_ENABLE <INS_NOTCH_ENABLE>`) 고급 필터링을 설정하는 경우에 출력을 보지 않고 효과적으로 필터링 고급 설정을 하는 것은 어렵다. 따라서 필터링된 출력을 볼려면 :ref:`INS_LOG_BAT_OPT <INS_LOG_BAT_OPT>` = 2 로 설정한다.
- 수동 비행 모드의 작은 드론에 대해서 대부분 신호가 100 Hz 아래로 되어야 하고 가능한 이를 넘는 것들이 적어야 한다. post-filter output을 통해서 다음을 볼 수 있다.

.. image:: ../../../images/imu-batchsampling-fft-mp.png
    :target:  ../_images/imu-batchsampling-fft-mp.png
    :width: 450px

고급 설정 및 분석 (Advanced Configuration and Analysis)
-----------------------------------

- 센서의 가장 높은 rate로 batch sampling을 활성화시키기 위해서 :ref:`INS_LOG_BAT_OPT <INS_LOG_BAT_OPT>` = 1 로 설정한다. 이렇게 하면 InvenseSense 제조사의 가장 빠른 IMU에 대해서 500Hz 이상으로 분석이 가능하다.
- :ref:`INS_LOG_BAT_MASK <INS_LOG_BAT_MASK>` 는 단일 센서를 샘플링하는데 사용할 수 있다. 단일 센서로부터 추출가능한 셈플의 수를 증가시킬 수 있다. (해당 플랫폼에서 가장 나은 센서) 분석하는데 더 나은 데이터를 제공하는 것일 수도 있다.
- :ref:`INS_LOG_BAT_CNT <INS_LOG_BAT_CNT>`는 수집할 샘플의 총 수를 지정할 수 있다. 이 값을 증가시키면 문제 주파수들을 대표할 수 있는 개념을 얻을 수 있다. sample rate로 나눠지면 검출할 수 있는 가장 낮은 주파수가 주어진다. 따라서 1024 KHz 속도로 1024 샘플을 샘플링하면 0.5Hz 주파수를  얻게 된다.(poorly)
:ref:`INS_LOG_BAT_CNT <INS_LOG_BAT_CNT>` specifies the number of samples which will be collected.  Increasing this will yield a more representative idea of problem frequencies.  When divided by the sample rate will give the lowest frequency which can be detected, so 1024 samples at 1024kHz sampling will (poorly) pick up 0.5Hz frequencies
- :ref:`INS_LOG_BAT_LGIN <INS_LOG_BAT_LGIN>` interval between pushing samples to the dataflash log, in ms.  Increase this to reduce the time taken to flush data to the dataflash log, reducing cycle time.  This will be at the expense of increased system load and possibly choking up the dataflash log for other messages
- :ref:`INS_LOG_BAT_LGCT <INS_LOG_BAT_LGCT>` Number of samples to push to count every :ref:`INS_LOG_BAT_LGIN <INS_LOG_BAT_LGIN>` ms.  Increase this to push more samples each time they are sent to the dataflash log.  Increasing this may cause timing jitter, and possibly choke up the dataflash log for other messages

The following two graphs are from the same flight on a PixRacer autopilot.  Accel[0] on the right is the InvenseSense IMU and shows higher frequencies than the slower IMU on the left

.. image:: ../../../images/imu-batchsampling-fft-sensorrate-pixracer.png
    :target:  ../_images/imu-batchsampling-fft-sensorrate-pixracer.png

Log Message Contents
====================

There are two types of dataflash log messages involved in batch sampling, `ISBH` and `ISBD`.

- `ISBH` is a batch header; it includes a batch number and metadata about the batch.
- `ISBD` messages contain the actual data for the batch, and reference a header by batch number.

Analysis with pymavlink
=======================

**pymavlink** is a developer focussed tool which supports graph FFT'd data

::

   pbarker@bluebottle:~/rc/ardupilot(fastest-sampling)$ ~/rc/pymavlink/tools/mavfft_isb.py /tmp/000003.BIN
   Processing log /tmp/000003.BIN
   .Skipping ISBD outside ISBH (fftnum=0)

   Skipping ISBD outside ISBH (fftnum=0)

   Skipping ISBD outside ISBH (fftnum=0)

   Skipping ISBD outside ISBH (fftnum=0)

   Skipping ISBD outside ISBH (fftnum=0)

   Skipping ISBD outside ISBH (fftnum=0)

   ...............................
   32560s messages  48433 messages/second  1904039 kB/second
   Extracted 10 fft data sets
   Sensor: Gyro[0]
   Sensor: Accel[0]

This output shows `mavfft_isb.py` extracting data from a single-IMU multicopter log.

.. image:: ../../../images/imu-batchsampling-fft-accel.png
    :target:  ../_images/imu-batchsampling-fft-accel.png
    :width: 450px

This multicopter frame clearly shows vibrations in the 80Hz range.

.. image:: ../../../images/imu-batchsampling-fft-gyro.png
    :target:  ../_images/imu-batchsampling-fft-gyro.png
    :width: 450px

This multicopter frame clearly shows rotational vibrations in the 80Hz range.

[copywiki destination="copter,plane,rover,dev,planner"]
