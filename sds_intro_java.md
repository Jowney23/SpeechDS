-   简介
    -   概述
    -   <span id="services">服务说明</span>
    -   <span id="parameter">参数说明</span>
    -   服务调用流程概述
        -   基本流程：
        -   <span id="init-sds">1、初始化SDS</span>
            -   SDS\_INIT\_SPEECH\_SDS包含以下参数：
        -   2、获取service
        -   3、执行service
        -   <span id="release-service">4、释放service</span>
        -   <span id="release-sds">5、释放SDS</span>
-   <span id="sds-hotword">热词唤醒</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   命令意图和参数
            -   SDS\_FEED\_SPEECH包含以下参数：
    -   服务调用流程
        -   1、获取热词唤醒服务
        -   2、执行热词唤醒
            -   <span id="sds-set-buf">设置监听语音的Buf</span>
            -   设置回调监听
        -   3、释放热词唤醒
-   <span id="sds-asr-online">在线语音识别</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   <span id="reg-online-intent-param">命令意图和参数</span>
            -   SDS\_ASR\_START包含以下参数：
            -   SDS\_FEED\_SPEECH包含以下参数：
    -   服务调用流程
        -   1、获取在线语音识别服务
        -   2、执行在线语音识别
            -   <span id="online-optional">设置可选内容</span>
            -   <span id="set-cb">设置回调监听</span>
            -   <span id="set-feed-speech">设置监听语音的Buf</span>
        -   3、释放在线语音识别服务
-   <span id="sds-asr-offline">离线语音识别</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   <span id="reg-offline-intent-param">命令意图和参数</span>
            -   SDS\_ASR\_START包含以下参数：
            -   SDS\_FEED\_SPEECH包含以下参数：
    -   服务调用流程
        -   1、获取离线语音识别服务
        -   2、执行离线语音识别
            -   <span id="offline-optional">设置可选内容</span>
            -   设置回调监听
            -   设置监听语音的Buf
        -   3、释放离线语音识别服务
-   <span id="sds-asr-mix">混合式语音识别</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   命令意图和参数
        -   1、获取混合式语音识别服务
        -   2、执行混合式语音识别
            -   设置可选内容
            -   设置回调监听
            -   设置监听语音的Buf
        -   3、释放离线语音识别服务
-   <span id="sds-asr-keywords">多关键词识别</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   命令意图和参数
            -   SDS\_ASR\_START包含以下参数：
            -   SDS\_FEED\_SPEECH包含以下参数：
    -   服务调用流程
        -   1、获取多关键词识别服务
        -   2、执行多关键词识别
            -   多关键词内容设置
            -   设置可选内容
            -   设置回调监听
            -   设置监听语音的Buf
        -   3、释放多关键词识别服务
-   <span id="sds-asr-contact">联系人识别</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   命令意图和参数
            -   SDS\_ASR\_START包含以下参数：
            -   SDS\_FEED\_SPEECH包含以下参数：
    -   服务调用流程
        -   1、获取联系人识别服务
        -   2、执行联系人识别
            -   设置联系人内容
            -   设置可选内容
            -   设置回调监听
            -   设置监听语音的Buf
        -   3、释放联系人识别服务
-   <span id="sds-asr-psm">页面相关文字识别</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   命令意图和参数
            -   SDS\_ASR\_START包含以下参数：
            -   SDS\_FEED\_SPEECH包含以下参数：
    -   服务调用流程
        -   1、获取页面相关文字识别服务
        -   2、执行页面相关文字识别
            -   设置页面相关文字
            -   设置可选内容
            -   设置回调监听
            -   设置监听语音的Buf
        -   3、释放页面相关文字识别服务
-   <span id="sds-tts">语音合成</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   命令意图和参数
            -   SDS\_TTS\_START包含以下参数：
            -   SDS\_TTS\_READ包含以下参数：
    -   服务调用流程
        -   1、获取语音合成服务
        -   2、执行语音合成
            -   设置语音合成内容
        -   3、释放语音合成服务
-   <span id="sds-asr-always-on">Always On</span>
    -   服务描述
    -   参数描述
        -   服务类型
        -   命令意图和参数
            -   SDS\_ASR\_START包含以下参数：
            -   SDS\_FEED\_SPEECH包含以下参数：
    -   服务调用流程
        -   1、获取Always On服务
        -   2、执行Always On
            -   设置超时时间
            -   设置可选内容
            -   设置回调监听
            -   设置监听语音的Buf
        -   3、释放Always On服务
-   <span id="sds-error">错误说明</span>

# 简介

## 概述

本文档是由[出门问问](https://ai.chumenwenwen.com)提供的AI开放平台Speech Development Service (SDS) 的用户指南，包括热词唤醒、语音识别、语义理解、垂直搜索和语音合成等组成部分。

**注意：本文档中所有Service，Intent，Param都定义在类MobvoiSDSConstants中。**

## <span id="services">服务说明</span>

SDS通过service的方式来提供语音相关功能。

每个service提供一种特定的功能，SDS目前支持的service如下：

|Service|描述|
|-------|----|
|[SDS\_HOTWORD\_DETECTION](#sds-hotword)|热词唤醒|
|[SDS\_ASR\_ONLINE\_ASR](#sds-asr-online)|在线语音识别|
|[SDS\_ASR\_ONLINE\_SEMANTIC](#sds-asr-online)|在线语音识别和语义理解|
|[SDS\_ASR\_ONLINE\_ONEBOX](#sds-asr-online)|在线语音识别，语义理解和垂直搜索|
|[SDS\_ASR\_OFFLINE](#sds-asr-offline)|离线语音识别|
|[SDS\_ASR\_MIX](#sds-asr-mix)|离在线混合语音识别|
|[SDS\_ASR\_MULTI\_KEYWORDS](#sds-asr-keywords)|多关键词识别|
|[SDS\_ASR\_CONTACT\_ONLY](#sds-asr-contact)|联系人识别|
|[SDS\_ASR\_PSM](#sds-asr-psm)|页面相关文字识别|
|[SDS\_TTS](#sds-tts)|语音合成|
|[SDS\_ASR\_ALWAYS\_ON](#sds-asr-always-on)|Always On|

## <span id="parameter">参数说明</span>

**Parameter**是一个参数类，用来传递[服务](#services)意图，配置参数，结果信息等数据。

|方法|描述|
|----|----|
|setIntent|设置命令意图|
|getIntent|获取命令意图|
|setBoolParam|设置bool类型数据|
|getBoolParam|获取bool类型数据|
|setLongParam|设置long类型数据|
|getLongParam|获取long类型数据|
|setFloatParam|设置float类型数据|
|getFloatParam|获取float类型数据|
|setStrParam|设置字符串类型数据|
|getStrParam|获取字符串类型数据|
|setCbParam|设置[CallBackBase类型](#callback)数据|
|getCbParam|获取[CallBackBase类型](#callback)数据|
|setBufParam|设置[Buf类型](#buf)数据|
|getBufParam|获取[Buf类型](#buf)数据|

<span id="callback">CallBackBase类型</span>

~~~~ {.java}
public class CallBackBase {
  public void callback(Parameter parameter) {}
}
~~~~

Param| 类型 | 描述 ---- | --- | --- SDS\_CB\_TYPE | String |回调返回类型 SDS\_CB\_INFO | String |回调返回内容 用户需要实现自己的回调类，根据回调返回的类型，获取返回内容。

比如语音识别的结果和状态是通过回调的方式返回的。

*例子，回调获取语音识别返回结果:*

~~~~ {.java}
class MyCallback extends CallBackBase {
    @Override
    public void callback(Parameter parameter) {
      String type = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_TYPE);
      if (type.equals(MobvoiSDSConstants.SDS_CB_ON_ERROR)) {
        String error = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO);
    }
}
~~~~

|SDS\_CB\_TYPE|描述|
|-------------|----|
|SDS\_CB\_ON\_ERROR|执行错误时会回调|
|SDS\_CB\_ON\_RESULT|语音搜索结果返回, 为JSON格式字符串|
|SDS\_CB\_ON\_VOLUME|输入语音数据实时的音量回调。<br>根据语音的能量算出，单位为db, 范围为[0, 100]|
|SDS\_CB\_ON\_PARTIAL\_TRANSCRIPT|语音识别部分结果返回，比如“今天天气怎么样”，会按顺序返回“今天”，“今天天气”|
|SDS\_CB\_ON\_FINAL\_TRANSCRIPT|语音识别最终结果返回，比如“今天天气怎么样”，会按顺序返回“今天”，“今天天气”，“今天天气怎么样”，最后一个就是Final Transcription|
|SDS\_CB\_ON\_LOCAL\_SILENCE\_DETECTED|在检测到本地语音之后，又检测到本地静音时回调|
|SDS\_CB\_ON\_REMOTE\_SILENCE\_DETECTED|服务器端检测到静音（说话人停止说话）后回调|
|SDS\_CB\_ON\_HOTWORD\_DETECTED|返回热词检测结果|
|SDS\_CB\_ON\_TIMEOUT|Always On 超时|

<span id="buf">Buf类型</span>

~~~~ {.java}
class Buf {
  public Buf(ByteBuffer addr, long size);
};
~~~~

**注意：当使用Buf时，必须使用allocateDirect(int capacity)去构造ByteBuffer，具体使用方法可参考下面[例子](#sds-set-buf)。**

## 服务调用流程概述

### 基本流程：

-   初始化SDS。
-   通过[Parameter](#parameter)传递环境参数给SDS。
-   获取service。
-   根据需要的功能获取对应的[service](#services)。
-   执行service。
-   通过[Parameter](#parameter)传递参数给service。
-   通过[Parameter](#parameter)传递结果给用户。
-   释放service (不可以直接delete service)。
-   释放SDS。

### <span id="init-sds">1、初始化SDS</span>

|Intent|描述|
|------|----|
|SDS\_INIT\_SPEECH\_SDS|初始化SDS|

#### SDS\_INIT\_SPEECH\_SDS包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_BASE\_DIR|String|设置SDS的mobvoi配置目录所在的路径，即mobvoi的父目录|是|
|SDS\_LOG\_FILE|String|设置日志保存文件。<br>默认输出屏幕|否|
|SDS\_LOG\_LEVEL|long|设置日志级别，取值范围[0, 4]。<br>默认值是1，数值越大，日志越详尽|否|

~~~~ {.java}
Parameter params(MobvoiSDSConstants.SDS_INIT_SPEECH_SDS);
params.setStrParam(MobvoiSDSConstants.SDS_BASE_DIR, <base_dir>);
SpeechSDS.getInstance().init(params);
~~~~

### 2、获取service

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(<service type>);
~~~~

### 3、执行service

~~~~ {.java}
Parameter parameter = new Parameter(<service intent>);
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### <span id="release-service">4、释放service</span>

~~~~ {.java}
SpeechSDS.getInstance().releaseService(service);
~~~~

### <span id="release-sds">5、释放SDS</span>

~~~~ {.java}
SpeechSDS.getInstance().cleanUp();
~~~~

* * * * *

# <span id="sds-hotword">热词唤醒</span>

## 服务描述

支持用户通过说特定的“唤醒词”的方式打开提供语音服务的开关。

用户可以自定义唤醒所使用的关键词，默认使用的唤醒关键词是：“你好问问”。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

## 参数描述

### 服务类型

|Service|描述|
|-------|----|
|SDS\_HOTWORD\_DETECTION|热词唤醒|

### 命令意图和参数

|Intent|描述|
|------|----|
|SDS\_FEED\_SPEECH|发送语音数据给语音识别服务|

#### SDS\_FEED\_SPEECH包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_AUDIO\_BUF|Buf|设置语音Buf|是|
|SDS\_ASYNC\_CB|CallBackBase|设置结果回调函数|是|
|SDS\_HOTWORD\_DETECTION\_MODEL|String|设置唤醒关键词模型，缺省唤醒关键词：“你好问问”。<br>唤醒关键词模型存放在SDS的mobvoi配置目录下hotword文件夹中，如果需要更换可以联系[出门问问](https://ai.chumenwenwen.com)|否|

## 服务调用流程

### 1、获取热词唤醒服务

~~~~ {.java}
Service service = SpeechSDS.getInstance.getService(MobvoiSDSConstants.SDS_HOTWORD_DETECTION);
~~~~

### 2、执行热词唤醒

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_FEED_SPEECH);
~~~~

#### <span id="sds-set-buf">设置监听语音的Buf</span>

~~~~ {.java}
// Must use allocateDirect(int capacity) to construct a ByteBuffer when using Buf.
// bufferSize：传入的语音长度，建议长度320 （10ms语音数据）
ByteBuffer buffer = ByteBuffer.allocateDirect(bufferSize);
Buf buf = new Buf(buffer, bufferSize);

parameter.setBufParam(MobvoiSDSConstants.SDS_AUDIO_BUF, buf);
~~~~

#### 设置回调监听

需要实现输出事件回调类(CallBackBase),该类要处理热词识别结果。

*例子,显示热词检测结果*

~~~~ {.java}
class HotwordCallback extends CallBackBase {
    public void callback(Parameter parameter) {
      String type = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_TYPE);
      if (type.equals(MobvoiSDSConstants.SDS_CB_ON_HOTWORD_DETECTED)) {
        String hotword = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO);
        Log.d("SDS: ", "Hotword detected: " + hotword);
      }
    }
}

HotwordCallback callback = new HotwordCallback();
parameter.setCbParam(MobvoiSDSConstants.SDS_ASYNC_CB, callback);
~~~~

**用户需要多次执行传入语音数据检测，建议每10ms语音数据执行一次唤醒服务。**

~~~~ {.java}
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放热词唤醒

请参考简介部分[释放服务](#release-service)。

* * * * *

# <span id="sds-asr-online">在线语音识别</span>

## 服务描述

可对输入语音进行在线识别,可对识别的语音进行语义理解，垂直搜索。

语音理解和垂直搜索详细说明请参考[搜索结果输出介绍](https://ai.chumenwenwen.com/pages/document/search-output-intro)。

输入音频流的音频格式为16k采样,16bit的小端(Little-Endian),单声道PCM数据.

## 参数描述

### 服务类型

支持三种<span id="online-reg-type">在线识别服务</span>

|Service|描述|
|-------|----|
|SDS\_ASR\_ONLINE\_ASR|返回在线语音识别结果|
|SDS\_ASR\_ONLINE\_SEMANTIC|返回在线语音识别结果,并对识别的语音进行语义理解|
|SDS\_ASR\_ONLINE\_ONEBOX|返回在线语音识别结果,并对识别的语音进行语义理解和垂直搜索|

### <span id="reg-online-intent-param">命令意图和参数</span>

|Intent|描述|
|------|----|
|SDS\_ASR\_START|执行语音识别服务|
|SDS\_FEED\_SPEECH|发送语音数据给语音识别服务|
|SDS\_ASR\_STOP|停止语音识别服务|

#### SDS\_ASR\_START包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_ASYNC\_CB|CallBackBase|设置语音识别结果回调函数|是|
|SDS\_ASR\_LANGUAGE|String|设置识别语音语言，取值："cn"（中文）， "en"（英文），缺省值："cn"|否|
|SDS\_ASR\_LOCATION|String|设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568"|否|
|SDS\_ASR\_NAVI\_COORD\_SYSTEM|String|用户设置给SDS的当前位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系），缺省值："bd09"|否|
|SDS\_ASR\_ADDR\_COORD\_SYSTEM|String|SDS返回给用户位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系），缺省值："bd09"|否|
|SDS\_ASR\_ENABLE\_ON\_VOLUME|Bool|是否输出语音片断的声压（SPL）值，缺省值：false|否|

#### SDS\_FEED\_SPEECH包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_AUDIO\_BUF|Buf|设置语音Buf|是|

## 服务调用流程

### 1、获取在线语音识别服务

选择一种[在线识别服务](#online-reg-type)类型:

~~~~ {.java}
// 根据不同需求，online_recognition_service可以取值：
// MobvoiSDSConstants.SDS_ASR_ONLINE_ASR
// MobvoiSDSConstants.SDS_ASR_ONLINE_SEMANTIC
// MobvoiSDSConstants.SDS_ASR_ONLINE_ONEBOX
Service* service = SpeechSDS.getInstance().getService(<online_recognition_service>);
~~~~

### 2、执行在线语音识别

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_ASR_START);
~~~~

#### <span id="online-optional">设置可选内容</span>

~~~~ {.java}
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LANGUAGE, "cn");
final String location = "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LOCATION, location);
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_NAVI_COORD_SYSTEM, "bd09");
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_ADDR_COORD_SYSTEM, "bd09");
~~~~

#### <span id="set-cb">设置回调监听</span>

需要实现输出事件回调类(CallBackBase)，该类要处理识别事件。

*例子，回调显示识别错误和结果:*

~~~~ {.java}
private class ASRCallBack extends CallBackBase {
  public void callback(Parameter parameter) {
    String type = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_TYPE);
    if (type.equals(MobvoiSDSConstants.SDS_CB_ON_ERROR)) {
      String error = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO)
      Log.e("SDS: ", error);
    } else if (type.equals(MobvoiSDSConstants.SDS_CB_ON_RESULT)) {
      String result = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO);
      Log.i("SDS: ", result);
    }
  }
}

ASRCallBack callback = new ASRCallBack();
parameter.setCbParam(MobvoiSDSConstants.SDS_ASYNC_CB, callback);
~~~~

#### <span id="set-feed-speech">设置监听语音的Buf</span>

~~~~ {.java}
// Must use allocateDirect(int capacity) to construct a ByteBuffer when using Buf.
// bufferSize：传入的语音长度，建议长度320 （10ms语音数据）
ByteBuffer buffer = ByteBuffer.allocateDirect(bufferSize);
Buf buf = new Buf(buffer, bufferSize);

parameter.setBufParam(MobvoiSDSConstants.SDS_AUDIO_BUF, buf);
~~~~

**用户需要多次执行传入语音数据检测，建议每10ms语音数据执行一次唤醒服务。**

~~~~ {.java}
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放在线语音识别服务

请参考简介部分[释放服务](#release-service).

* * * * *

# <span id="sds-asr-offline">离线语音识别</span>

## 服务描述

在没有网络的情况下，可对输入语音进行离线识别。

输入音频流的音频格式为16k采样,16bit的小端(Little-Endian),单声道PCM数据.

## 参数描述

### 服务类型

|Service|描述|
|-------|----|
|SDS\_ASR\_OFFLINE|离线语音识别|

### <span id="reg-offline-intent-param">命令意图和参数</span>

|Intent|描述|
|------|----|
|SDS\_ASR\_START|执行语音识别服务|
|SDS\_FEED\_SPEECH|发送语音数据给语音识别服务|
|SDS\_ASR\_STOP|停止语音识别服务|

#### SDS\_ASR\_START包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_ASYNC\_CB|CallBackBase|设置语音识别结果回调函数|是|
|SDS\_ASR\_LANGUAGE|String|设置识别语音语言，取值："cn"（中文）， "en"（英文），缺省值："cn"|否|
|SDS\_ASR\_LOCATION|String|设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568"|否|
|SDS\_ASR\_NAVI\_COORD\_SYSTEM|String|用户设置给SDS的当前位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）|否|
|SDS\_ASR\_ADDR\_COORD\_SYSTEM|String|SDS返回给用户位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系），缺省值："bd09"|否|
|SDS\_ASR\_ENABLE\_ON\_VOLUME|Bool|是否输出语音片断的声压（SPL）值，缺省值：false|否|
|SDS\_ASR\_OFFLINE\_DATA\_APPS|String|设置某种类型对应的字符串列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_CONTACTS|String|设置联系人列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_ARTISTS|String|设置艺术家列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_SONGS|String|设置歌曲列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_ALBUMS|String|设置专辑列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_VIDEOS|String|设置视频列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|

#### SDS\_FEED\_SPEECH包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_AUDIO\_BUF|Buf|设置语音Buf|是|

## 服务调用流程

### 1、获取离线语音识别服务

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(MobvoiSDSConstants.SDS_ASR_OFFLINE);
~~~~

### 2、执行离线语音识别

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_ASR_START);
~~~~

#### <span id="offline-optional">设置可选内容</span>

~~~~ {.java}
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LANGUAGE, "cn");
final String location = "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LOCATION, location);
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_NAVI_COORD_SYSTEM, "bd09");
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_ADDR_COORD_SYSTEM, "bd09");

final String apps = "微信" + MobvoiSDSConstants.SDS_IFS + "支付宝";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_OFFLINE_DATA_APPS, apps);
final String artists = "周杰伦" + MobvoiSDSConstants.SDS_IFS + "王力宏";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_OFFLINE_DATA_ARTISTS, artists);
final String songs = "黑色幽默 " + MobvoiSDSConstants.SDS_IFS + "唯一";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_OFFLINE_DATA_SONGS, songs);
final String albums = "范特西" + MobvoiSDSConstants.SDS_IFS + "唯一";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_OFFLINE_DATA_ALBUMS, albums);
final String videos = "让子弹飞" + MobvoiSDSConstants.SDS_IFS + "星球大战";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_OFFLINE_DATA_VIDEOS, videos);
~~~~

#### 设置回调监听

请参考在线语音识别部分中[设置回调监听](#set-cb)

#### 设置监听语音的Buf

请参考在线语音识别部分中[设置监听语音的Buf](#set-feed-speech)。

**用户需要多次执行传入语音数据检测，建议每10ms语音数据执行一次唤醒服务。**

~~~~ {.java}
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放离线语音识别服务

请参考简介部分[释放服务](#release-service)。

* * * * *

# <span id="sds-asr-mix">混合式语音识别</span>

## 服务描述

通过混合策略技术，充分发挥离线和在线识别的各自优势，实现对离在线识别结果的优化筛选，从而给用户提供更快更准确的识别反馈。

输入音频流的音频格式为16k采样,16bit的小端(Little-Endian),单声道PCM数据。

## 参数描述

### 服务类型

|Service|描述|
|-------|----|
|SDS\_ASR\_MIX|混合式语音识别|

|Intent|描述|
|------|----|
|SDS\_ASR\_START|执行语音识别服务|
|SDS\_FEED\_SPEECH|发送语音数据给语音识别服务|
|SDS\_ASR\_STOP|停止语音识别服务|

### 命令意图和参数

请参考[在线识别](#reg-online-intent-param)部分和[离线识别](#reg-offline-intent-param)部分。

支持所有在线识别和离线识别的意图，参数。

### 1、获取混合式语音识别服务

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(MobvoiSDSConstants.SDS_ASR_MIX);
~~~~

### 2、执行混合式语音识别

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_ASR_START);
~~~~

#### 设置可选内容

请参考在线语音识别[设置可选内容](#online-optional)和离线语音识别部分[设置可选内容](#offline-optional)。

#### 设置回调监听

请参考在线语音识别部分中[设置回调监听](#set-cb)。

#### 设置监听语音的Buf

请参考在线语音识别部分中[设置监听语音的Buf](#set-feed-speech)。

**用户需要多次执行传入语音数据检测，建议每10ms语音数据执行一次唤醒服务。**

~~~~ {.java}
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放离线语音识别服务

请参考简介部分[释放服务](#release-service)。

* * * * *

# <span id="sds-asr-keywords">多关键词识别</span>

## 服务描述

多关键词识别，功能类似于热词唤醒，可以不开启在线语音识别的情况下，同时对多个关键词进行监听。

例如，在听音乐的时候，直接说出“上一首”、“下一首”等指令。

多关键词识别的使用方法，类似于离线语音识别，也应先进行语音识别的相关配置。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

## 参数描述

### 服务类型

|Service|描述|
|-------|----|
|SDS\_ASR\_MULTI\_KEYWORDS|多关键词识别|

### 命令意图和参数

|Intent|描述|
|------|----|
|SDS\_ASR\_START|执行语音识别服务|
|SDS\_FEED\_SPEECH|发送语音数据给语音识别服务|
|SDS\_ASR\_STOP|停止语音识别服务|

#### SDS\_ASR\_START包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_ASR\_OFFLINE\_DATA\_KEYWORDS|String|设置关键词列表。<br>列表中每项内容使用分隔符：SDS\_IFS|是|
|SDS\_ASYNC\_CB|CallBackBase|设置关键字识别结果回调函数|是|
|SDS\_ASR\_LANGUAGE|String|设置识别语音语言，取值："cn"（中文）， "en"（英文），缺省值："cn"|否|
|SDS\_ASR\_LOCATION|String|设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568"|否|
|SDS\_ASR\_NAVI\_COORD\_SYSTEM|String|用户设置给SDS的当前位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）|否|
|SDS\_ASR\_ADDR\_COORD\_SYSTEM|String|SDS返回给用户位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系），缺省值："bd09"|否|
|SDS\_ASR\_ENABLE\_ON\_VOLUME|Bool|是否输出语音片断的声压（SPL）值，缺省值：false|否|

#### SDS\_FEED\_SPEECH包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_AUDIO\_BUF|Buf|设置语音Buf|是|

## 服务调用流程

### 1、获取多关键词识别服务

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(MobvoiSDSConstants.SDS_ASR_OFFLINE_DATA_KEYWORDS);
~~~~

### 2、执行多关键词识别

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_ASR_START);
~~~~

#### 多关键词内容设置

~~~~ {.java}
final String keywords = "下一页" + MobvoiSDSConstants.SDS_IFS + "上一页" + MobvoiSDSConstants.SDS_IFS + "下一首" + MobvoiSDSConstants.SDS_IFS + "上一首";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_KEYWORDS_DATA, keywords);
~~~~

#### 设置可选内容

~~~~ {.java}
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LANGUAGE, "cn");
final String location = "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LOCATION, location);
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_NAVI_COORD_SYSTEM, "bd09");
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_ADDR_COORD_SYSTEM, "bd09");
~~~~

#### 设置回调监听

~~~~ {.java}
// 多关键词的回调结果类型是：SDS_CB_ON_PARTIAL_TRANSCRIPT
private class MultiKeywordsCallBack extends CallBackBase {
  public void callback(Parameter parameter) {
    String type = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_TYPE);
    if (type.equals(MobvoiSDSConstants.SDS_CB_ON_ERROR)) {
      String error = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO)
      Log.e("SDS: ", error);
    } else if (type.equals(MobvoiSDSConstants.SDS_CB_ON_PARTIAL_TRANSCRIPT)) {
      String result = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO);
      Log.i("SDS: ", result);
    }
  }
}

MultiKeywordsCallBack callback = new MultiKeywordsCallBack();
parameter.setCbParam(MobvoiSDSConstants.SDS_ASYNC_CB, callback);
~~~~

#### 设置监听语音的Buf

请参考在线语音识别部分中[设置监听语音的Buf](#set-feed-speech)。

**用户需要多次执行传入语音数据检测，建议每10ms语音数据执行一次唤醒服务。**

~~~~ {.java}
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放多关键词识别服务

请参考简介部分[释放服务](#release-service)。

* * * * *

# <span id="sds-asr-contact">联系人识别</span>

## 服务描述

生成联系人模型，使得识别联系人达到更高的准确率。

比如语音输入“打电话给张三”，其中联系人“张三”通过生成的联系人模型识别，提高识别“张三”的准确率。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

## 参数描述

### 服务类型

|Service|描述|
|-------|----|
|SDS\_ASR\_CONTACT\_ONLY|联系人识别|

### 命令意图和参数

|Intent|描述|
|------|----|
|SDS\_ASR\_START|执行语音识别服务|
|SDS\_FEED\_SPEECH|发送语音数据给语音识别服务|
|SDS\_ASR\_STOP|停止语音识别服务|

#### SDS\_ASR\_START包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_ASR\_OFFLINE\_DATA\_CONTACTS|String|设置联系人列表。<br>列表中每项内容使用分隔符：SDS\_IFS|是|
|SDS\_ASYNC\_CB|CallBackBase|设置联系人识别结果回调函数|是|
|SDS\_ASR\_LANGUAGE|String|设置识别语音语言，取值："cn"（中文）， "en"（英文），缺省值："cn"|否|
|SDS\_ASR\_LOCATION|String|设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568"|否|
|SDS\_ASR\_NAVI\_COORD\_SYSTEM|String|用户设置给SDS的当前位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）|否|
|SDS\_ASR\_ADDR\_COORD\_SYSTEM|String|SDS返回给用户位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系），缺省值："bd09"|否|
|SDS\_ASR\_ENABLE\_ON\_VOLUME|Bool|是否输出语音片断的声压（SPL）值，缺省值：false|否|

#### SDS\_FEED\_SPEECH包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_AUDIO\_BUF|Buf|设置语音Buf|是|

## 服务调用流程

### 1、获取联系人识别服务

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(MobvoiSDSConstants.SDS_ASR_CONTACT_ONLY);
~~~~

### 2、执行联系人识别

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_ASR_START);
~~~~

#### 设置联系人内容

~~~~ {.java}
final String contacts = "张三" + MobvoiSDSConstants.SDS_IFS + "小问" + MobvoiSDSConstants.SDS_IFS + "小明";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_OFFLINE_DATA_CONTACTS, contacts);
~~~~

#### 设置可选内容

~~~~ {.java}
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LANGUAGE, "cn");
final String location = "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LOCATION, location);
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_NAVI_COORD_SYSTEM, "bd09");
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_ADDR_COORD_SYSTEM, "bd09");
~~~~

#### 设置回调监听

请参考在线语音识别部分中[设置回调监听](#set-cb)。

#### 设置监听语音的Buf

请参考在线语音识别部分中[设置监听语音的Buf](#set-feed-speech)。

**用户需要多次执行传入语音数据检测，建议每10ms语音数据执行一次唤醒服务。**

~~~~ {.java}
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放联系人识别服务

请参考简介部分[释放服务](#release-service)。

* * * * *

# <span id="sds-asr-psm">页面相关文字识别</span>

## 服务描述

对生成的或已有的页面表项，根据表项的部分文字的语音，完成表项的选择和表项内容的返回。

例如:

查询附近的餐厅,页面返回结果是:

-   "拿渡麻辣香锅"
-   "必胜客中关村店"
-   "全聚德烤鸭店"

可以说"必胜客"会"匹配到"必胜客中关村店"，返回“第２个”（“必胜客中关村店”在列表中位置是第２个）。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

## 参数描述

### 服务类型

|Service|描述|
|-------|----|
|SDS\_ASR\_PSM|页面相关文字识别|

### 命令意图和参数

|Intent|描述|
|------|----|
|SDS\_ASR\_START|执行语音识别服务|
|SDS\_FEED\_SPEECH|发送语音数据给语音识别服务|
|SDS\_ASR\_STOP|停止语音识别服务|

#### SDS\_ASR\_START包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_ASR\_PSM\_OPTIONS|String|设置当前页面文字选项列表。<br>列表中每项内容使用分隔符：SDS\_IFS|是|
|SDS\_ASR\_PSM\_KEYWORDS|String|设置当前页面文字列表。<br>列表中每项内容使用分隔符：SDS\_IFS|是|
|SDS\_ASYNC\_CB|CallBackBase|设置联系人识别结果回调函数|是|
|SDS\_ASR\_LANGUAGE|String|设置识别语音语言，取值："cn"（中文）， "en"（英文），缺省值："cn"|否|
|SDS\_ASR\_LOCATION|String|设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568"|否|
|SDS\_ASR\_NAVI\_COORD\_SYSTEM|String|用户设置给SDS的当前位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）|否|
|SDS\_ASR\_ADDR\_COORD\_SYSTEM|String|SDS返回给用户位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系），缺省值："bd09"|否|
|SDS\_ASR\_ENABLE\_ON\_VOLUME|Bool|是否输出语音片断的声压（SPL）值，缺省值：false|否|

#### SDS\_FEED\_SPEECH包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_AUDIO\_BUF|Buf|设置语音Buf|是|

## 服务调用流程

### 1、获取页面相关文字识别服务

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(MobvoiSDSConstants.SDS_ASR_CONTACT_ONLY);
~~~~

### 2、执行页面相关文字识别

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_ASR_START);
~~~~

#### 设置页面相关文字

~~~~ {.java}
final String keywords = "拿渡麻辣香锅" + SDS_IFS + "必胜客中关村店" + SDS_IFS + "全聚德烤鸭店";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_PSM_KEYWORDS, keywords);
final String options = "第一个" + SDS_IFS + "第二个" + SDS_IFS + "第三个";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_PSM_OPTIONS, options);
~~~~

#### 设置可选内容

~~~~ {.java}
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LANGUAGE, "cn");
final String location = "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568";
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_LOCATION, location);
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_NAVI_COORD_SYSTEM, "bd09");
parameter.setStrParam(MobvoiSDSConstants.SDS_ASR_ADDR_COORD_SYSTEM, "bd09");
~~~~

#### 设置回调监听

~~~~ {.java}
// 页面相关文字识别的回调结果类型是：SDS_CB_ON_PARTIAL_TRANSCRIPT
private class PsmCallBack extends CallBackBase {
  @Override
  public void callback(Parameter parameter) {
    String type = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_TYPE);
    if (type.equals(MobvoiSDSConstants.SDS_CB_ON_ERROR)) {
      String error = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO)
      Log.e("SDS: ", error);
    } else if (type.equals(MobvoiSDSConstants.SDS_CB_ON_PARTIAL_TRANSCRIPT)) {
      String result = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO);
      Log.i("SDS: ", result);
    }
  }
}

PsmCallBack callback = new PsmCallBack();
parameter.setCbParam(MobvoiSDSConstants.SDS_ASYNC_CB, callback);
~~~~

#### 设置监听语音的Buf

请参考在线语音识别部分中[设置监听语音的Buf](#set-feed-speech)。

**用户需要多次执行传入语音数据检测，建议每10ms语音数据执行一次唤醒服务。**

~~~~ {.java}
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放页面相关文字识别服务

请参考简介部分[释放服务](#release-service)。

* * * * *

# <span id="sds-tts">语音合成</span>

## 服务描述

可对输入的文本进行语音合成返回合成音频。

合成文本长度不能超过500个汉字。

合成的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

## 参数描述

### 服务类型

|Service|描述|
|-------|----|
|SDS\_TTS|语音合成|

### 命令意图和参数

|Intent|描述|
|------|----|
|SDS\_TTS\_START|开始语音合成|
|SDS\_TTS\_READ|读取合成语音数据|

#### SDS\_TTS\_START包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_TTS\_TEXT|String|输入需要语音合成的文字，支持中文和英文|是|
|SDS\_TTS\_LANGUAGE|String|设置合成语音语言，取值："Mandarin"， "English"，缺省值："Mandarin"|否|
|SDS\_TTS\_SPEED|String|设置合成语音语速，取值："0.5"(slow) - "2.0"(fast)|否|

#### SDS\_TTS\_READ包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_AUDIO\_BUF|Buf|设置存放合成语音数据Buf|是|
|SDS\_TTS\_READ\_SIZE|long|返回实际合成语音长度|否|　|

## 服务调用流程

### 1、获取语音合成服务

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(MobvoiSDSConstants.SDS_TTS);
~~~~

### 2、执行语音合成

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_TTS_START);
~~~~

#### 设置语音合成内容

~~~~ {.java}
final String text = "语音合成测试。"
parameter.setStrParam(MobvoiSDSConstants.SDS_TTS_TEXT, text);
parameter.setStrParam(MobvoiSDSConstants.SDS_TTS_SPEED, "1.2");
parameter.setStrParam(MobvoiSDSConstants.SDS_TTS_LANGUAGE, "Mandarin");
~~~~

~~~~ {.java}
service.invoke(parameter);
~~~~

获取合成语音数据

~~~~ {.java}
// Must use direct buffer.
ByteBuffer buf = ByteBuffer.allocateDirect(BUFFER_SIZE);
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_TTS_READ);
Buf audioBuffer = new Buf(buf, BUFFER_SIZE);
parameter.setBufParam(MobvoiSDSConstants.SDS_AUDIO_BUF, audioBuffer);
Parameter ret = service.invoke(parameter);

// getLongParam(SDS_TTS_READ_SIZE):返回合成语音数据实际长度
// 如果返回值为-1,表示合成语音结束.
ret.getLongParam(MobvoiSDSConstants.SDS_TTS_READ_SIZE);
// 返回合成语音数据
buf.array()；
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放语音合成服务

请参考简介部分[释放服务](#release-service)。

* * * * *

# <span id="sds-asr-always-on">Always On</span>

## 服务描述

在一次唤醒后的一段特定时间内，不需要进行再一次的热词唤醒，期间可以一直提供预设的语音服务。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

## 参数描述

### 服务类型

|Service|描述|
|-------|----|
|SDS\_ASR\_ALWAYS\_ON|Always On|

### 命令意图和参数

|Intent|描述|
|------|----|
|SDS\_ASR\_START|执行语音识别服务|
|SDS\_FEED\_SPEECH|发送语音数据给语音识别服务|
|SDS\_ASR\_STOP|停止语音识别服务|

#### SDS\_ASR\_START包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_ASYNC\_CB|CallBackBase|设置语音识别结果回调函数|是|
|SDS\_ASR\_AOR\_TIMEOUT|long|设置always on超时时间，单位：秒，缺省值：15|否|
|SDS\_ASR\_LANGUAGE|String|设置识别语音语言，取值："cn"（中文）， "en"（英文），缺省值："cn"|否|
|SDS\_ASR\_LOCATION|String|设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568"|否|
|SDS\_ASR\_NAVI\_COORD\_SYSTEM|String|用户设置给SDS的当前位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）|否|
|SDS\_ASR\_ADDR\_COORD\_SYSTEM|String|SDS返回给用户位置信息的经纬度所采用的坐标系统，取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系），缺省值："bd09"|否|
|SDS\_ASR\_ENABLE\_ON\_VOLUME|Bool|是否输出语音片断的声压（SPL）值，缺省值：false|否|
|SDS\_ASR\_OFFLINE\_DATA\_APPS|String|设置某种类型对应的字符串列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_CONTACTS|String|设置联系人列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_ARTISTS|String|设置艺术家列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_SONGS|String|设置歌曲列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_ALBUMS|String|设置专辑列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|
|SDS\_ASR\_OFFLINE\_DATA\_VIDEOS|String|设置视频列表。<br>列表中每项内容使用分隔符：SDS\_IFS|否|

#### SDS\_FEED\_SPEECH包含以下参数：

|Param|类型|描述|必选|
|-----|----|----|----|
|SDS\_AUDIO\_BUF|Buf|设置语音Buf|是|

## 服务调用流程

### 1、获取Always On服务

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(MobvoiSDSConstants.SDS_ASR_ALWAYS_ON);
~~~~

### 2、执行Always On

~~~~ {.java}
Parameter parameter = new Parameter(MobvoiSDSConstants.SDS_ASR_START);
~~~~

#### 设置超时时间

~~~~ {.java}
// 设置超时时间为60，在超时前不需要热词唤醒。
parameter.setLongParam(MobvoiSDSConstants.SDS_ASR_AOR_TIMEOUT, 60);
~~~~

#### 设置可选内容

离线语音识别部分[设置可选内容](#offline-optional)。

#### 设置回调监听

*例子，回调显示识别错误和结果:*

~~~~ {.java}
class AlwaysOnCallBack extends CallBackBase {
  @Override
  public void callback(Parameter parameter) {
    String type = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_TYPE);
    Log.i("SDS: ", "Recognizer callback type: " + type);
    if (type.equals(MobvoiSDSConstants.SDS_CB_ON_ERROR)) {
      String error = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO));
      Log.e("SDS: ", error);
    } else if (type.equals(MobvoiSDSConstants.SDS_CB_ON_RESULT)) {
      String result = parameter.getStrParam(MobvoiSDSConstants.SDS_CB_INFO);
      Log.i("SDS: ", result);
    } else if (type.equals(MobvoiSDSConstants.SDS_CB_ON_TIMEOUT)) {
      Log.i("SDS: ", "Time out, always on is stopped.");
    }
  }
}

AlwaysOnCallBack callback = new AlwaysOnCallBack();
parameter.setCbParam(MobvoiSDSConstants.SDS_ASYNC_CB, callback);
~~~~

#### 设置监听语音的Buf

请参考在线语音识别部分中[设置监听语音的Buf](#set-feed-speech)。

**用户需要多次执行传入语音数据检测，建议每10ms语音数据执行一次唤醒服务。**

~~~~ {.java}
service.invoke(parameter);
~~~~

执行结果返回值及错误说明请参考[错误说明](#sds-error)。

### 3、释放Always On服务

请参考简介部分[释放服务](#release-service)。

* * * * *

# <span id="sds-error">错误说明</span>

|Param|类型|描述|
|-----|----|----|
|SDS\_INVOKE\_RESULT|Bool|执行service结果。<br>取值：true（成功），false（失败）|
|SDS\_ERROR\_CODE|String|获取执行service失败的原因|

*例子，执行服务失败后，查看失败原因*

~~~~ {.java}
Service service = SpeechSDS.getInstance().getService(<service_type>)
Parameter result = service.invoke(params);
if (!result.getBoolParam(MobvoiSDSConstants.SDS_INVOKE_RESULT)) {
  Log.i("SDS: ", result.getStrParam(MobvoiSDSConstants.SDS_ERROR_CODE));
}
~~~~

|SDS\_ERROR\_CODE|描述|
|----------------|----|
|SDS\_ERR\_INVALID\_PARAM|传入的Param有误|
|SDS\_ERR\_UNKNOWN\_INTENT|不支持的intent|
|SDS\_ERR\_INTERNAL\_ERROR|SDS内部错误|
|SDS\_ERR\_BAD\_STATE|非预期的SDS状态，比如用户未初始化便使用某个service|
|SDS\_ERR\_BUF\_FULL|缓存已满|
|SDS\_ERR\_ASR\_SERVER\_ERROR|服务器错误|
|SDS\_ERR\_ASR\_NETWORK\_ERROR|网络错误|
|SDS\_ERR\_ASR\_NO\_SPEECH|识别语音内容为空，即认为没有人说话|
|SDS\_ERR\_ASR\_GARBAGE|无法识别当前语音内容|
|SDS\_ERR\_ASR\_INTERNAL\_ERROR|内部错误|
|SDS\_ERR\_ASR\_INIT\_ERROR|SDS初始化错误|
|SDS\_ERR\_ASR\_UNKNOWN\_ERROR|未知错误|
