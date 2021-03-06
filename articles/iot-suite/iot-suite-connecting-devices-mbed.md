---
title: "mbed에서 C를 사용하여 장치 연결 | Microsoft Docs"
description: "mbed 장치에서 실행되는 C로 작성된 응용 프로그램을 사용하여 미리 구성된 Azure IoT Suite 원격 모니터링 솔루션에 장치를 연결하는 방법을 설명합니다."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 9551075e-dcf9-488f-943e-d0eb0e6260be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 9ded95283b52f0fc21ca5b99df8e72e1e152fe1c
ms.openlocfilehash: a70eb51e7ebbc79e1aab4176d154dbef754368c1


---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>미리 구성된 원격 모니터링 솔루션에 장치 연결(mbed)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>C 샘플 솔루션 빌드 및 실행
다음 지침은 [mbed 사용 가능 Freescale FRDM-K64F][lnk-mbed-home] 장치를 원격 모니터링 솔루션에 연결하기 위한 단계를 설명합니다.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>네트워크 및 데스크톱 컴퓨터에 mbed 장치 연결
1. 이더넷 케이블을 사용하여 mbed 장치를 네트워크에 연결합니다. 이 단계는 샘플 응용 프로그램에서 인터넷에 액세스해야 하기 때문에 필요합니다.
2. [mbed 시작][lnk-mbed-getstarted]을 참조하여 mbed 장치를 데스크톱 PC에 연결합니다.
3. 데스크톱 PC에서 Windows를 실행 중인 경우 [PC 구성][lnk-mbed-pcconnect]을 참조하여 mbed 장치에 대한 직렬 포트 액세스를 구성합니다.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>mbed 프로젝트 만들기 및 샘플 코드 가져오기
1. 웹 브라우저에서 mbed.org [개발자 사이트](https://developer.mbed.org/)로 이동합니다. 아직 등록하지 않은 경우 새 계정(무료)을 만들 수 있는 옵션이 표시됩니다. 그렇지 않은 경우 계정 자격 증명으로 로그인합니다. 그런 다음 페이지의 오른쪽 위 모서리에서 **컴파일러** 를 클릭합니다. 그러면 *작업 영역* 인터페이스로 이동합니다.
2. 사용 중인 하드웨어 플랫폼이 창의 오른쪽 위 모서리에 표시되는지 확인하거나 오른쪽 모서리에 있는 아이콘을 클릭하여 하드웨어 플랫폼을 선택합니다.
3. 주 메뉴에서 **가져오기** 를 클릭합니다. 그런 다음 **여기를 클릭**을 클릭하여 mbed 구형 로고 옆에 있는 URL 링크에서 가져옵니다.
   
    ![][6]
4. 팝업 창에서 샘플 코드에 대한 링크인 https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/을 입력한 다음 **가져오기**를 클릭합니다.
   
    ![][7]
5. mbed 컴파일러 창에서 이 프로젝트를 가져오면 다양한 라이브러리도 가져온다는 사실을 확인할 수 있습니다. 일부는 Azure IoT 팀([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/))에서 제공 및 유지 관리되며 일부는 mbed 라이브러리 카탈로그에서 사용할 수 있는 타사 라이브러리입니다.
   
    ![][8]
6. remote_monitoring\remote_monitoring.c 파일을 열고 파일에서 다음 코드를 찾습니다.
   
    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```
7. 샘플 프로그램에서 IoT Hub에 연결할 수 있도록 [Device Id] 및 [Device Key]를 장치 데이터로 바꿉니다. IoT Hub 호스트 이름을 사용하여 [IoTHub 이름] 및 [IoTHub 접미사, 즉 azure-devices.net] 자리 표시자를 바꿉니다. 예를 들어 IoT Hub 호스트 이름이 **contoso.azure-devices.net**인 경우 **contoso**가 **hubName**이고 나머지는 **hubSuffix**입니다.
   
    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```
   
    ![][9]

### <a name="walk-through-the-code"></a>코드 안내
프로그램 작동 방식에 관심이 있는 경우 이 섹션에 설명된 샘플 코드의 주요 부분을 참조하세요. 코드를 실행하기만 하려면 [프로그램 빌드 및 실행](#buildandrun)으로 건너뛰세요.

#### <a name="defining-the-model"></a>모델 정의
이 샘플에서는 [직렬 변환기][lnk-serializer] 라이브러리를 사용하여 장치와 IoT Hub 간에 보내고 받을 수 있는 메시지를 지정하는 모델을 정의합니다. 이 샘플에서 **Contoso** 네임스페이스는 다음을 지정하는 **자동 온도 조절기**를 정의합니다.

- **온도**, **ExternalTemperature** 및 **습도** 원격 분석 데이터.
- 장치 ID, 장치 속성 등의 메타데이터.
- 장치에서 응답하는 명령은 다음과 같습니다.

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

장치에서 응답하는 **SetTemperature** 및 **SetHumidity** 명령에 대한 정의는 모델 정의와 관련됩니다.

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>라이브러리에 모델 연결
**sendMessage** 및 **IoTHubMessage** 함수는 장치에서 원격 분석을 보내고 IoT Hub의 메시지를 명령 처리기에 연결하는 상용구 코드입니다.

#### <a name="the-remotemonitoringrun-function"></a>remote_monitoring_run 함수
프로그램의 **주** 함수는 응용 프로그램이 IoT Hub 장치 클라이언트로 장치의 동작을 실행하기 시작할 때 **remote_monitoring_run** 함수를 호출합니다. 이 **remote_monitoring_run** 함수는 대부분 중첩된 함수 쌍으로 구성됩니다.

* **platform\_init** 및 **platform\_deinit**는 플랫폼별 초기화 및 종료 작업을 수행합니다.
* **serializer\_init** 및 **serializer\_deinit**는 직렬 변환기 라이브러리를 초기화하고 초기화를 해제합니다.
* **IoTHubClient\_Create** 및 **IoTHubClient\_Destroy**는 장치 자격 증명을 사용하여 IoT Hub에 연결하는 클라이언트 핸들 **iotHubClientHandle**을 만듭니다.

**remote_monitoring_run** 함수의 주 섹션에서 프로그램은 **iotHubClientHandle** 처리를 사용하여 다음 작업을 수행합니다.

* Contoso 자동 온도 조절기 모델의 인스턴스를 만들고 두 가지 명령에 대한 메시지 콜백을 설정합니다.
* 직렬 변환기 라이브러리를 사용하여 장치에서 지원하는 명령을 비롯해 장치 자체에 대한 정보를 IoT Hub로 보냅니다. 허브에서는 이 메시지를 받은 경우 대시보드의 장치 상태를 **보류 중**에서 **실행 중**으로 변경합니다.
* 온도, 외부 온도 및 습도 값을 매초마다 IoT Hub로 보내는 **while** 루프를 시작합니다.

참고로, 시작 시 IoT Hub로 전송되는 샘플 **장치 정보** 메시지는 다음과 같습니다.

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

참고로, IoT Hub로 전송되는 샘플 **원격 분석** 메시지는 다음과 같습니다.

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

참고로, IoT Hub에서 수신되는 샘플 **명령** 은 다음과 같습니다.

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>

### <a name="build-and-run-the-program"></a>프로그램 빌드 및 실행
1. **컴파일** 을 클릭하여 프로그램을 빌드합니다. 안전하게 모든 경고를 무시할 수 있지만 빌드가 오류를 생성하는 경우 계속하기 전에 해결하십시오.
2. 빌드가 성공한 경우 mbed 컴파일러 웹 사이트에서는 프로젝트 이름이 지정된 .bin 파일을 생성하여 로컬 컴퓨터에 다운로드합니다. .bin 파일을 장치에 복사합니다. .bin 파일을 장치에 저장하면 장치가 다시 시작하고 .bin 파일에 포함된 프로그램이 실행됩니다. 언제든지 mbed 장치의 리셋 단추를 눌러 프로그램을 수동으로 다시 시작할 수 있습니다.
3. PuTTY와 같은 SSH 클라이언트 응용 프로그램을 사용하여 장치에 연결합니다. Windows 장치 관리자를 확인하여 장치가 사용하는 직렬 포트를 확인할 수 있습니다.
   
    ![][11]
4. PuTTY에서 **직렬** 연결 형식을 클릭합니다. 장치는 일반적으로 9600 보드로 연결되므로 **속도** 상자에 9600을 입력합니다. 그런 다음 **열기**를 클릭합니다.
5. 프로그램이 실행을 시작합니다. 연결할 때 프로그램이 자동으로 시작되지 않는 경우 보드를 다시 설정해야 할 수 있습니다(CTRL + Break를 누르거나 보드의 다시 설정 단추를 누름).
   
    ![][10]

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer



<!--HONumber=Jan17_HO1-->


