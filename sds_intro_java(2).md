# 概述

[//]: # (01-intro-start)

## <span id = "sds-intro">简介</span>

本文档是 [出门问问](https://ai.chumenwenwen.com) AI 开放平台语音开发工具包 ` Speech Development Service (SDS) ` 的用户指南，涵盖如下各方面功能的开发：

* 语音识别
* 语音合成
* 热词唤醒
* 语义理解
* 垂直搜索

SDS 为各功能的开发提供了一致的接口，用户在熟悉了某一语音功能的开发后，可以很容易使用 SDS 进行其它方面功能的开发。

SDS 支持使用 ` C++ ` 及 ` Java ` 开发。本文档只介绍如何使用 ` Java ` 进行语音应用的开发。

[//]: # (01-intro-end)

---

[//]: # (02-int-guide-start)

## <span id = "sds-integration_guide">集成指南</span>

SDS 软件包命名格式为：` speechsds-{project}-{arch}-{version}.tar.gz `。 现在所支持的平台有：

* ` x86_64/Linux `
* ` armeabi-v7a/Linux `
* ` arm64-v8a/Linux `
* ` x86/Android `
* ` armeabi-v7a/Android `
* ` arm64-v8a/Android `

有关 ` C++ ` 的开发，请参照 ` C++ ` 版本的文档。

可使用如下命令解压软件包：

```shell
tar xvfz <sds-package>
```

软件包解压后，具有如下的目录结构：

```text
speechsds-open_api-android-...
├── demo
│   └── demo.zip
├── doc
│   ├── sds_intro_java.html
│   └── sds_intro_java.md
├── libs
│   ├── arm64-v8a
│   │   ├── libc++_shared.so
│   │   └── libmobvoisds.so
│   ├── armeabi-v7a
│   │   ├── libc++_shared.so
│   │   └── libmobvoisds.so
│   ├── classes.jar
│   └── x86
│       ├── libc++_shared.so
│       └── libmobvoisds.so
└── mobvoi.zip
```

其中，各目录项的作用如下：

文件目录项 | 用途说明 | 备注
---- | ---- | ----
` demo ` | 样例代码 | 用Android Studio可直接运行的demo project
` doc ` | SDS 文档 | 有 ` MarkDown ` 和 ` HTML ` 两种格式
` libs ` | 存放不同架构的SDS共享库以及jar包|`libmobvoisds.so`、`libc++_shared.so`和`classes.jar`
` mobvoi.zip ` | SDS 运行需要的配置压缩包 | 此压缩包必须要解压之后使用

[//]: # (02-int-guide-end)

---

[//]: # (03-sample-code-start)

## <span id = "sds-demo">示例程序</span>

在 SDS 安装包的  ` demo` 目录下，提供了一个示例程序。

### <span id = "sds-demo-run">运行示例</span>

将`demo.zip`解压之后，直接使用`Android Studio`打开 `android_sds_demo`工程


[//]: # (03-sample-code-end)

---

[//]: # (04-usage-overview-start)

# SDS 使用流程概述

## <span id = "sds-entities">SDS 实体概述</span>

SDS 开发将使用到如下 Java 类，这些类均定义在 SDS 工具包中的 ` libs/classes.jar ` 中。

类 | 描述
---- | ----
[SpeechSDS](#speech-sds) | 获取、释放并管理 Service
[Service](#sds-services) | 封装各项语音功能，如语音识别、语音生成
[Parameter](#sds-parameter) | 传递用户代码与 SDS 交互的信息，带有意图及多项参数值
[Value](#sds-value) | Parameter 某一参数的取值，可存储多种类型的信息
[CallBackBase](#sds-callbackbase) | Service 回调所用的基类，用户回调需基于它来派生

## <span id = "sds-general-process">SDS 使用流程</span>

在使用 SDS 功能之前，用户需要先获取 SpeechSDS 实例。之后，通过 SpeechSDS 实例来获取所需的 [Service](#services)。获取 Service 后，通过 invoke() 方法来调用所需的功能。在 Service 使用结束后，应由 SpeechSDS 释放。最后，释放 SpeechSDS 实例。

具体来说：

### 1. 获取 SpeechSDS 实例

* 所有的 Service 均需要由 SpeechSDS 来获取
* 用户也可以同时获取多个 SpeechSDS 实例，并由不同的 SpeechSDS 实例提供不同语言种类的 Service。目前 SpeechSDS 支持以下语种：
    * 中文
    * 英语
    * 粤语
* 用户也可以在释放一个 SpeechSDS 实例后，再获取另一个 SpeechSDS 实例

### 2. 初始化 SpeechSDS

* 用户通过传递 [Parameter](#sds-parameter) 给 SpeechSDS 来完成 SpeechSDS 的初始化

### 3. 获取 Service

* 用户通过 SpeechSDS 来获取需要的 [Service](#services)
* 通常，用户需要获取多个 Service 来完成一个语音功能的开发
* 通过同一个 SpeechSDS 实例获取的 Service 实例支持的，是同一种语言

### 4. 使用 Service

* 所有的 Service 均只提供 invoke() 方法
* 用户传递 [Parameter](#sds-parameter) 参数给 invoke() 方法来触发 Service 提供的功能
* invoke() 也通过 [Parameter](#sds-parameter) 返回结果给用户，用户应对之进行检查
* 对于某些 Service，用户需要处理 Service 产生的回调

### 5. 释放 Service

* 注意：Service 只能由 SpeechSDS 的 releaseService() 方法来释放 native 资源。

### 6. 释放 SpeechSDS

* 调用 SpeechSDS 中 destroyInstance() 方法来释放 native 资源。

## <span id = "sds-namespace">SDS 包名</span>

SDS 的Java 接口类，位于 ` com.mobvoi.speech.sds ` 包名中。

---

## <span id = "speech-sds">SpeechSDS 类</span>

SpeechSDS 类用于获取、释放并管理各个 Service。同一个 SpeechSDS 实例获取的 Service 支持同一种语言。

以下介绍中，会使用 [Parameter](#sds-parameter) 来进行参数的传递。Parameter 将在稍后进行介绍。

### <span id = "sds-speech-sds-decl">SpeechSDS 类声明</span>

SpeechSDS 类声明如下（部分细节已省略）：

```java
class SpeechSDS {

  public static SpeechSDS makeInstance();
  public static void destroyInstance(SpeechSDS sds);

  public boolean init(Parameter params);

  public boolean setParam(Parameter params);

  public Service getService(String service_type);
  public boolean releaseService(Service service);

  public boolean cleanUp();

}
```

### <span id = "sds-make-instance">1 创建 SDS 实例</span>

使用 SpeechSDS.makeInstance() 创建 SDS 实例。

```java
SpeechSDS sds = SpeechSDS.makeInstance();
```

### <span id = "init-sds">2 初始化SDS</span>

使用 SpeechSDS 中 init() 初始化 SDS 实例。

#### MOBVOI_SDS_INIT 支持以下参数：

Param | 类型 | 描述 | 取值范围 | 缺省值 | 是否必选
---- | ---- | ---- | ---- | ---- | ----
MOBVOI_SDS_BASE_DIR | String | 设置 SDS 的 mobvoi 配置目录所在的路径，即 mobvoi 的父目录 | | | 是
MOBVOI_SDS_LANGUAGE | String | 设置 SDS 所支持的语言 | zh_cn <br> zh_hk <br> en_us | zh_cn |  否
MOBVOI_SDS_LAZY_LOADING | boolean | 指定是否对 SDS 所用的模型文件进行延迟加载 | | true |  否
MOBVOI_SDS_LOG_LEVEL | int | 设置日志级别（INFO、WARNING、ERROR 及 FATAL）。<br>数值越小，日志越详尽 | [0, 3] | 0 |  否
MOBVOI_SDS_VLOG_LEVEL | int | 设置详尽日志（INFO类型）级别。<br>数值越大，日志越详尽 | [0, 4] | 0 |  否

下面是一段示例代码：

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_INIT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_BASE_DIR，    "/sdcard");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LANGUAGE,     "zh_cn");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LAZY_LOADING, false);
sds.init(params);
```

### <span id = "sds-get-service">3 获取 service</span>

使用 SpeechSDS 中 getService() 获取 SDS service。

下面是一段示例代码：

```java
Service hotword    = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_HOTWORD);
Service asr        = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_ASR);
Service tts        = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_MIXED_TTS);
```

### <span id = "sds-invoke-service">4 调用 service</span>

使用 Service 中 invoke() 调用 service 的功能。

如下代码片断，演示如何设置热词侦听服务：

```java
// CallBackBase eventHandler;

Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK， eventHandler);

Parameter ret = hotword.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  Log.d(TAG, "Error setting hotword parameter, error code: " +
          ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt());
}
```

如下代码片断，演示如何启动热词侦听服务：

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = hotword.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  Log.d(TAG, "Error starting hotword, error code: " +
          ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt());
}
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

### <span id = "sds-release-service">5 释放 service</span>

使用 SpeechSDS 中 releaseService() 释放 SDS service。

下面是一段示例代码：

```java
sds.releaseService(hotword);
sds.releaseService(asr);
sds.releaseService(tts);
```

**注意：用户必须通过 releaseService() 来释放 service 的 native 资源**

### <span id = "release-sds">6 释放SDS</span>

使用 SpeechSDS 中 destroyInstance() 释放 SDS 实例。

下面是一段示例代码：

```java
sds.cleanUp();
SpeechSDS.destroyInstance(sds);
```

### <span id = "sds-set-param">动态配置 SDS 参数</span>

SpeechSDS 还可以动态进行一些参数的配置。目前所支持的配置项为：

配置项 | 类型 | 描述 | 取值范围 | 缺省值
---- | ---- | ---- | ---- | ----
MOBVOI_SDS_LOG_FILE | String | 是否将日志重定向到文件。<br>如不设置，则默认输出到终端屏幕。<br>指定为"/dev/tty"，也可以禁用日志重定向 | |
MOBVOI_SDS_LOG_LEVEL | int | 设置日志级别（INFO、WARNING、ERROR 及 FATAL）。<br>数值越小，日志越详尽 | [0, 3] | 0
MOBVOI_SDS_VLOG_LEVEL | int | 设置详尽日志（INFO类型）级别。<br>数值越大，日志越详尽 | [0, 4] | 0
MOBVOI_SDS_AUDIO_DUMP_DIR | String | 用于指定 SDS dump 音频数据时应使用的保存路径。<br>可用于调试目的 | |
MOBVOI_SDS_LOCATION | String | 用于更新 SDS 位置信息，位置服务（LBS）相关的信息查询（如查询附近的电影院）时，将使用该信息。<br>在车机等使用场景下，调用者应及时该信息 | 可以是如下的格式：<br> "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568" <br> ",,,,,,39.99855111136873,116.34438004753852" |　
MOBVOI_SDS_IN_COORD_SYS | String | 设置 MOBVOI_SDS_LOCATION 所用的坐标系统。<br>分别对应于世界坐标系、国测局坐标系（火星坐标系）以及百度坐标系 | wgs84 <br> gcj02 <br> bd09 | bd09
MOBVOI_SDS_OUT_COORD_SYS | String | 设置位置服务返回位置信息时所采用的坐标系。<br>分别对应于世界坐标系、国测局坐标系（火星坐标系）以及百度坐标系 | wgs84 <br> gcj02 <br> bd09 | bd09
MOBVOI_SDS_ENABLE_CB_PARTIAL | boolean | 设置是否使能 partial transcript 的回调 | true <br> false | true
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>用户可使用它来绘制语音波形图。<br>取值范围：[0, 1] |  |　

使用 SpeechSDS 中 setParam() 进行上述参数的动态配置。如下是一些代码片断：

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LOG_LEVEL,         1);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LOG_FILE,          "/dev/tty");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_DUMP_DIR,    "/data/local/tmp/audio_dump");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LOCATION,          "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.31656");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_IN_COORD_SYS,      "bd09";
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OUT_COORD_SYS,     "bd09";
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_ENABLE_CB_PARTIAL, true);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_ENABLE_CB_VOLUME,  true);

Parameter ret = sds.setParam(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  Log.d(TAG, "Error setting SDS parameter, error code: " +
          ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt());
}
```

---

## <span id = "sds-services">Service 类</span>

SDS 的语音服务，均以不同的 Service 来提供。本文档后面将有很大的篇幅用于描述 SDS 的[各种 Service](#services)。

所有的 Service 都具有同样的调用形式，因此用户在掌握一种 Service 的使用方法后，可以很容易的对其它的 Service 进行操作。

### <span id = "sds-service-decl">Service 类声明</span>

Service 类声明如下（部分细节已省略）：

```java
package com.mobvoi.speech.sds;

public class Service {

  public Parameter invoke(Parameter var1) { }

}
```

Service 类所支持的各种操作，是由 [Parameter](#sds-parameter) 参数的差异所体现出来的。关于 Parameter，下文会有进一步的介绍。

### <span id = "sds-service-invoke">Service 类操作</span>

所有对于 Service 的操作，均通过 Service 中 invoke() 进行。

用户通过 Parameter 类来向 Service 传递所欲使用的功能及相应的参数，并应检查 Service 中 invoke() 所返回的 Parameter 中所携带的返回值及出错信息来判断相应的调用是否成功。

具体操作示例，可参见 SpeechSDS 类部分的[示例](#sds-invoke-service)。

如下是另外一个例子。该例子演示了如何进行 offline ASR 服务的参数配置：

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);

// CallBackBase eventHandler;
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

StringVector contacts = new StringVector();
contacts.add("小明");
contacts.add("小亮");

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_CONTACTS, contacts);

Parameter ret = asr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  Log.d(TAG, "Error setting parameter, error code: " +
          ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt());
}
```
---

## <span id = "sds-parameter">Parameter 类</span>

Parameter 在 SDS 中被广泛应用。它在 Service 的 [invoke()](#sds-service-invoke) 调用中用来传递请求和获取结果。在 SDS 向用户代码发起 [回调](#sds-callbackbase) 时，也使用 Parameter 来传递信息。

Parameter 可以看作是一个承载信息的容器。它主要带有 intent 和 parameter key/values 两部分信息。其中，intent 用于指定该 Parameter 的用途（或意图），parameter key/values 则是相应的 0 到多组参数键值。

Parameter key/values 中的 value 值，可以是如下类型：

* ` boolean `
* ` short | int | float | double `
* ` String `
* ` StringVector `
* ` Buf `
* ` CallBackBase `
* ` Value `

[Buf](#sds-parameter-buf) 类型，将在本节稍晚些时候介绍。[CallBackBase](#sds-callbackbase) 和 [Value](#sds-value) 将在稍后另外的小节中单独介绍。

### <span id = "sds-parameter-decl">Parameter 类声明</span>

Parameter 类声明如下（部分细节已省略）：

```java
package com.mobvoi.speech.sds;

public class Parameter {

  public Parameter() { }
  public Parameter(String var1) { }

  public void setIntent(String var1) { }
  public String getIntent() { }

  public void setParam(String var1, String var2) { }
  public void setParam(String var1, Buf var2) { }
  public void setParam(String var1, short var2) { }
  public void setParam(String var1, int var2) { }
  public void setParam(String var1, float var2) { }
  public void setParam(String var1, double var2) { }
  public void setParam(String var1, CallBackBase var2) { }
  public void setParam(String var1, boolean var2) { }
  public void setParam(String var1, Value var2) { }
  public void setParam(String var1, StringVector var2) { }

  public Value getParam(String var1) { }

  public boolean hasParam(String var1) { }

  public StringVector getParamKeys() { }

  public boolean dropParam(String var1) { }

  public void ClearParam() { }

}
```

### <span id = "sds-parameter-invoke">Parameter 类操作</span>

#### <span id = "sds-parameter-invoke-intent">Parameter Intent 操作</span>

Parameter intent 用于表示使用该 Parameter 进行操作的用途，如上面例子中所演示的开启热词侦听、设置 offline ASR 配置参数、……等。

用户可以在声明 Parameter 时，指定相应的 intent。也可以在之后使用 Parameter 中 setIntent() 来进行设置。

用户可以使用 Parameter 中 getIntent() 来获取该 Parameter intent。

#### <span id = "sds-parameter-invoke-intent">Parameter Key/Values 操作</span>

Parameter key/values 键值组用于表示进行 Parameter intent 操作时的详细操作数据。

用户可以使用 Parameter 中 setParam() 重载函数来设定相应的键值。使用 Parameter 中 getParam() 重载函数来获取相应的键值。

```java
// CallBackBase eventHandler;

Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);
```

用户可以使用 Parameter 中 getParamKeys() 来获取该 Parameter 存储的所有的 key 值。

用户可以使用 Parameter 中 dropParam() 来删除 Parameter 存储的指定 key/value。

用户可以使用 Parameter 中 clearParam() 来删除 Parameter 存储的所有 key/value。

### <span id = "sds-parameter-mand-opt">Parameter 参数的可选性</span>

对于特定 Service 所支持的特定 intent 来说，其 invoke() 方法所接收的 Parameter 中，有的键值是必选的，而有的键值则是可选的。

### <span id = "sds-parameter-response-format">作为返回值的 Parameter</span>

用作 Service 中 invoke() 方法的返回值时，Parameter 具有一定的格式。具体说，就是每一个 Parameter 都带有一个 int 型的 MOBVOI_SDS_ERROR_CODE 键值对，来表示 invoke() 调用是否成功，以及在失败时的具体错误类型。

如下是检查该 MOBVOI_SDS_ERROR_CODE 信息的一个示例：

```java
if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  Log.d(TAG, "Error setting hotword parameter, error code: " +
          ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt());
}
```

SDS 支持的错误类型，将在下面一小节介绍。

### <span id = "sds-error-code">SDS 错误说明</span>

SDS 目前支持的 error code 值有：

error code | 描述
---- | ----
MOBVOI_SDS_SUCCESS | 成功
MOBVOI_SDS_ERR_INVALID_PARAM | 参数有误，可能是必选参数不存在，或者存在不支持的可选参数
MOBVOI_SDS_ERR_UNKNOWN_INTENT | 不支持的 intent
MOBVOI_SDS_ERR_INTERNAL_ERROR | SDS 内部错误
MOBVOI_SDS_ERR_BAD_STATE | 非预期的SDS状态，比如用户未初始化便使用某个service
MOBVOI_SDS_ERR_NETWORK_ERROR | 网络错误
MOBVOI_SDS_ERR_SERVER_ERROR | 服务器内部错误
MOBVOI_SDS_ERR_NO_SPEECH | 没有检测到语音，可能用户没有说话
MOBVOI_SDS_ERR_GARBAGE | 无法识别当前语音内容
MOBVOI_SDS_ERR_BAD_HOTWORD | 无效的热词
MOBVOI_SDS_ERR_BUF_FULL | SDS 内部某缓冲区已满
MOBVOI_SDS_ERR_LICENSE_DENIED | SDS 服务被拒绝。<br>对应的 Service 未授权，或者 license 已过期，或已超过当日允许的最大使用次数

### <span id = "sds-parameter-buf">Buf 类型</span>

Parameter 存储的 Buf 类型，用于传递音频信息。比如：传递给 Hotword 服务的录音数据，从 OfflineTTS 服务读取的合成的语音数据等。

注意：

* SDS 使用的音频数据，为 16kHz 采样、16bit 位深、小端 (Little-endian) 的单声道 PCM 数据。

#### <span id = "sds-parameter-buf-decl">Buf 类声明</span>

Buf 类型的声明如下（部分细节已省略）：

```java
package com.mobvoi.speech.sds;

public class Buf {

  public Buf() { }
  public Buf(ByteBuffer var1, int var2) { }

  public void setAddr(ByteBuffer var1) { }
  public byte[] getAddr() { }

  public void setSize(int var1) { }
  public int getSize() { }

}
```

注意：

* Buf 类初始化参数使用的ByteBuffer类型，必须通过 `ByteBuffer.allocateDirect()` 申请。

---

## <span id = "sds-value">Value 类</span>

[Parameter](#sds-parameter) 中存储的键值，实际上是存储在 Value 对象中，它作为一个变体类型存在。

* 用户在为 Parameter key 赋值时，通常不必关心 Value 对象的存在
* 但用户在检查 Parameter key 值时（比如说，检查 [MOBVOI_SDS_ERROR_CODE](#sds-error-code) 的值），就会使用到 Value 对象

### <span id = "sds-value-as-methods">as 方法组</span>

用户读取 Value 对象中存储的信息时，要使用 as...() 方法组来将 Value 中存在的信息转换为相应的类型：

as 方法 | 结果类型 | 备注
---- | ---- | ----
` asString() ` | ` String ` |
` asBuf() ` | ` Buf ` |
` asInt() ` | ` int ` |
` asDouble() ` | ` double ` |
` asCallBack() ` | ` CallBackBase `
` asBool() ` | ` boolean ` |
` asStrVec() ` | ` StringVector ` |

如下代码演示如何使用 Value 中 asInt() 方法：

```java
if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  Log.d(TAG, "Error setting hotword parameter, error code: " +
          ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt());
}
```

---

## <span id = "sds-callbackbase">CallBackBase 类</span>

当某些事件发生时，SDS Service 会以回调形式通知用户代码。比如说，SDS 检测到热词，或者已经完成对语音的识别。

用户代码应当使用 `extends` 继承 `CallBackBase` 类，并重写 `callback()` 方法来实现相应的回调。

### <span id = "sds-callbackbase-decl">CallBackBase 类声明</span>

CallBackBase 类声明如下（部分细节已省略）：

```java
package com.mobvoi.speech.sds;

public class CallBackBase {

  public void callback(Parameter var1) { }

}
```

### <span id = "sds-callbackbase-param-format">作为 CallBack 入参的 Parameter</span>

在 CallBackBase 的 ` CallBack() ` 方法中，Parameter 具有如下的格式：

* Intent 取值为：MOBVOI_SDS_CB_INTENT
* Key/values 部分总是含有 MOBVOI_SDS_CB_TYPE 这个键值。它表示该 CallBack 的类别。用户代码应根据该值来调用相应的处理程序
* Key/values 部分大多数时会含有 MOBVOI_SDS_CB_INFO 这个键值。它表示对应于 MOBVOI_SDS_CB_TYPE 事件所关联的数据。比如：当 MOBVOI_SDS_CB_INFO 为 MOBVOI_SDS_CB_ERROR 这个结果时， MOBVOI_SDS_CB_INFO 存储着相应的错误码

下表概括了一些常见的 MOBVOI_SDS_CB_TYPE 类型。在后面各个 Service 的具体介绍中，会详细概述每个 Service 所产生的回调的类型及 Parameter 中所带的其它参数。

Param | 描述
---- | ----
MOBVOI_SDS_CB_SPEECH | 接收到新的录音数据
MOBVOI_SDS_CB_HOTWORD | 返回热词检测结果
MOBVOI_SDS_CB_LOCAL_SILENCE | 在检测到本地语音之后，又检测到本地静音
MOBVOI_SDS_CB_REMOTE_SILENCE | 远端 ASR 检测到了语音后，又检测到静音
MOBVOI_SDS_CB_PARTIAL_TRANSCRIPT | 语音识别部分结果返回。<br>比如“今天天气怎么样”，会按顺序返回“今天”，“今天天气”
MOBVOI_SDS_CB_FINAL_TRANSCRIPT | 语音识别最终结果返回。<br>比如“今天天气怎么样”，会按顺序返回“今天”，“今天天气”，“今天天气怎么样”，最后一个就是 final transcript。<br>注意：该结果仅包括识别结果，并不包括后继的 NLU 等其它部分的结果
MOBVOI_SDS_CB_RESULT | 语音搜索结果返回, 为JSON格式字符串
MOBVOI_SDS_CB_VOLUME | 输入语音数据的实时音量。<br>根据语音的能量算出。<br>范围为 [0, 1]
MOBVOI_SDS_CB_ERROR | 有错误产生

### <span id = "sds-callbackbase-sample">示例代码</span>

以下代码摘自 SDS 安装包中所附带的 demo 程序：

```java
package com.mobvoi.mobvoisdsdemo;

import com.mobvoi.speech.sds.CallBackBase;
import com.mobvoi.speech.sds.MobvoiSDSConstants;
import com.mobvoi.speech.sds.Parameter;
import com.mobvoi.speech.sds.Service;

public class HotwordActivity extends BaseActivity {
    private Service mHotwordService = null;
    private HotwordCallback mHotwordCB = new HotwordCallback();

    @Override
    protected void init() {
        Log.d(TAG, "init hotword");
        mHotwordService = sSpeechSDS.getService(MobvoiSDSConstants.MOBVOI_SDS_HOTWORD);
        Parameter parameter = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
        parameter.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, mHotwordCB);
        parameter.setParam(MobvoiSDSConstants.MOBVOI_SDS_MODEL, "nihaowenwen_wear");
        mHotwordService.invoke(parameter);
    }

    private class HotwordCallback extends CallBackBase {
        @Override
        public void callback(Parameter parameter) {
            String hotword = parameter.getParam(MobvoiSDSConstants.MOBVOI_SDS_CB_INFO).asString();
            Log.d(TAG, "Hotword detected: " + hotword);
        }
    }
}
```

[//]: # (04-usage-overview-end)

---

[//]: # (05-sds-services-start)

# <span id = "services">服务说明</span>

SDS 通过 [Service](#sds-services) 的方式来提供语音相关功能。

每个 Service 提供一种特定的功能，SDS 目前支持的 Service 如下：

Service | 描述 | 备注
---- | ---- | ----
[MOBVOI_SDS_HOTWORD](#sds-service-hotword) | 热词检测 |
[MOBVOI_SDS_OFFLINE_ASR](#sds-service-offline-asr) | 离线语音识别 | 仅离线语音识别，无语义理解和对话管理
[MOBVOI_SDS_OFFLINE_CONTACT](#sds-service-offline-contact) | 离线联系人识别 |
[MOBVOI_SDS_OFFLINE_KEYWORDS](#sds-service-offline-keywords) | 离线关键词识别 |
[MOBVOI_SDS_OFFLINE_PSM](#sds-service-offline-psm) | 关联关键词识别 |
[MOBVOI_SDS_OFFLINE_NLU](#sds-service-offline-nlu) | 离线语义理解 |
[MOBVOI_SDS_OFFLINE_DM](#sds-service-offline-dm) | 离线对话管理 |
[MOBVOI_SDS_OFFLINE_RECOGNIZER](#sds-service-offline-recognizer) | 离线语音识别 | 离线语音识别，语义理解和对话管理
[MOBVOI_SDS_ONLINE_ASR](#sds-service-online-asr) | 在线语音识别 |
[MOBVOI_SDS_ONLINE_SEMANTIC](#sds-service-online-asr) | 在线语音识别和语义理解 |
[MOBVOI_SDS_ONLINE_ONEBOX](#sds-service-online-asr) | 在线语音识别，语义理解和垂直搜索 |
[MOBVOI_SDS_ONLINE_NLU](#sds-service-online-nlu) | 在线语义理解 |
[MOBVOI_SDS_ONLINE_SEARCH](#sds-service-online-search) | 在线垂直搜索 |
[MOBVOI_SDS_ONLINE_CONTROL](#sds-service-online-control) | 在线参数控制 |
[MOBVOI_SDS_MIXED_GENERAL_ASR](#sds-service-mix-asr) | 离在线语音识别 | 仅语音识别，无语义理解及垂直搜索
[MOBVOI_SDS_MIXED_CONTACT_ONLY](#sds-service-mixed-contact-only) | 混合式联系人识别 |
[MOBVOI_SDS_MIXED_RECOGNIZER](#sds-service-mixed-recognizer) | 离在线语音识别 | 语音识别，语义理解和垂直搜索
[MOBVOI_SDS_ALWAYS_ON](#sds-service-always-on) | Always On 服务 |
[MOBVOI_SDS_CONCURRENT_PSM_MIX](#sds-service-concurrent-psm-mix) | 关联关键词及离在线识别 |
[MOBVOI_SDS_OFFLINE_TTS](#sds-service-offline-tts) | 离线语音合成 |
[MOBVOI_SDS_ONLINE_TTS](#sds-service-online-tts) | 在线语音合成 |
[MOBVOI_SDS_MIXED_TTS](#sds-service-mix-tts) | 离在线语音合成 |
[MOBVOI_SDS_VAD](#sds-service-vad)  | 语音检测 |

## <span id = "sds-service-hotword">热词检测</span>

### 服务描述

支持用户通过说特定的“唤醒词”的方式打开提供语音服务的开关。

用户可以自定义唤醒所使用的关键词，默认使用的唤醒关键词是：“你好问问”。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_HOTWORD | 热词唤醒

#### 命令意图和参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置热词唤醒服务
MOBVOI_SDS_START | 开始热词唤醒服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据给语音识别服务
MOBVOI_SDS_STOP | 停止热词唤醒服务

##### MOBVOI_SDS_SET包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置结果回调函数 | 否
MOBVOI_SDS_MODEL | String  | 设置唤醒关键词模型，缺省唤醒关键词：“你好问问”。<br>唤醒关键词模型存放在SDS的mobvoi配置目录下hotword文件夹中。<br>如果需要定制，可以联系[出门问问](https://ai.chumenwenwen.com) | 否
MOBVOI_SDS_KEYWORDS | String | 自定义唤醒关键词 | 否

**注意：**

* 如果同时设置了 MOBVOI_SDS_MODEL 和 MOBVOI_SDS_KEYWORDS，则会使用 MOBVOI_SDS_KEYWORDS 的设置
* MOBVOI_SDS_MODEL 方式具有唤醒准确率更高，响应速度更快，占用系统资源更少等优点，因此建议使用该方式

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_SPEECH包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置语音Buf | 是

##### MOBVOI_SDS_STOP包含以下参数：

无

### 服务调用流程

#### 1 获取热词唤醒服务

```java
// SpeechSDS sds;

Service hotword = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_HOTWORD);
```

#### 2 执行热词唤醒

##### 设置热词唤醒回调监听
```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

Parameter ret = hotword.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中，传递给 eventHandler 的，可能有如下的回调消息：

Param | 类型 | 描述 | 是否必选
---- | ---- | ----| ----
MOBVOI_SDS_CB_TYPE | String | 指示回调的类型，取值为 MOBVOI_SDS_CB_HOTWORD | 是
MOBVOI_SDS_CB_INFO | String | 检测到的唤醒词 | 是

##### 开始热词唤醒
```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = hotword.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送语音数据
```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = hotword.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止热词唤醒
```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = hotword.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

#### 3 释放热词唤醒

```java
// SpeechSDS sds;

sds.releaseService(hotword);
```

---

## <span id = "sds-service-offline-asr">离线语音识别</span>

### 服务描述

对录入语音进行本地识别，没有网络连接方面的要求。

离线语音识别可以对设置的应用、联系人、艺术家、歌曲、专辑、视频、唤醒词进行准确的识别。

输入的音频流的格式为 16kHz 采样、16bit 位深的小端 (Little-Endian) 单声道 PCM 数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_OFFLINE_ASR | 离线语音识别

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置参数
MOBVOI_SDS_BUILD_MODEL | 编译离线模型
MOBVOI_SDS_START  | 执行语音识别服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据给语音识别服务
MOBVOI_SDS_STOP | 停止语音识别服务
MOBVOI_SDS_CANCEL | 取消语音识别服务

##### MOBVOI_SDS_SET_PARAM 参数：

Param | 类型 | 描述 | 必选
---- | ---- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置语音识别结果回调函数 | 是
MOBVOI_SDS_LOCAL_START_SILENCE | int | 设置语音开始的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_LOCAL_END_SILENCE | int | 设置语音结束的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL | boolean | 设置是否使能 Partial transcript 的回调 | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>取值范围： [0, 1] | 否
MOBVOI_SDS_OFFLINE_APPS | StringVector | 设置应用列表 | 否
MOBVOI_SDS_OFFLINE_CONTACTS| StringVector | 设置联系人列表 | 否
MOBVOI_SDS_OFFLINE_ARTISTS| StringVector | 设置艺术家列表 | 否
MOBVOI_SDS_OFFLINE_SONGS| StringVector | 设置歌曲列表 | 否
MOBVOI_SDS_OFFLINE_ALBUMS| StringVector | 设置专辑列表 | 否
MOBVOI_SDS_OFFLINE_VIDEOS| StringVector | 设置视频列表 | 否
MOBVOI_SDS_ASR_HOTWORD| StringVector | 设置唤醒词列表 | 否
MOBVOI_SDS_RETURN_RAW_PARTIAL | boolean | 设置是否返回原始的部分识别结果。<br>默认为false | 否
MOBVOI_SDS_RETURN_RAW_FINAL | boolean | 设置是否返回原始的最终识别结果。<br>默认为false | 否


##### MOBVOI_SDS_BUILD_MODEL 参数：

无。

##### MOBVOI_SDS_START 参数：

无。

##### MOBVOI_SDS_FEED_SPEECH 参数：

Param | 类型 | 描述 | 必选
---- | ---- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置待识别的声音的 Buf | 是

##### MOBVOI_SDS_STOP 参数：

无。

##### MOBVOI_SDS_CANCEL 参数：

无。

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service offlineAsr = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_ASR);
```

#### 2 调用服务

##### 设置参数

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);


Parameter ret = offlineAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中， eventHandler 的各种可能的回调消息为：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_LOCAL_SILENCE | 无 | 无 | 检测到静音
MOBVOI_SDS_CB_PARTIAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别部分结果
MOBVOI_SDS_CB_FINAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别最终结果
MOBVOI_SDS_CB_VOLUME | MOBVOI_SDS_CB_INFO | String | 输入语音数据的实时音量。<br>根据语音的能量算出。<br>范围为 [0, 1]
MOBVOI_SDS_CB_ERROR | MOBVOI_SDS_ERROR_CODE | int | 错误类型

##### 编译模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

Parameter ret = offlineAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 启动服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = offlineAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送音频数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = offlineAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止语音识别

**停止接收音频数据，但依然继续进行解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = offlineAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 取消语音识别

**停止接收音频数据，并停止解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = offlineAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(offlineAsr);
```

---

## <span id = "sds-service-offline-contact">离线联系人识别</span>

### 服务描述

生成联系人模型，使得识别联系人达到更高的准确率。

比如语音输入“打电话给张三”，其中联系人“张三”通过生成的联系人模型识别，提高识别“张三”的准确率。

输入的音频流的格式为 16kHz 采样、16bit 位深的小端 (Little-Endian) 单声道 PCM 数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_OFFLINE_CONTACT | 离线语音识别

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置参数
MOBVOI_SDS_BUILD_MODEL | 编译离线模型
MOBVOI_SDS_START  | 执行语音识别服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据给语音识别服务
MOBVOI_SDS_STOP | 停止语音识别服务
MOBVOI_SDS_CANCEL | 取消语音识别服务

##### MOBVOI_SDS_SET_PARAM 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置语音识别结果回调函数 | 是
MOBVOI_SDS_OFFLINE_CONTACTS| StringVector | 设置联系人列表 | 是
MOBVOI_SDS_LOCAL_START_SILENCE | int | 设置语音开始的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_LOCAL_END_SILENCE | int | 设置语音结束的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL | boolean | 设置是否使能 Partial transcript 的回调 | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>取值范围： [0, 1] | 否

##### MOBVOI_SDS_BUILD_MODEL 参数：

无。

##### MOBVOI_SDS_START 参数：

无。

##### MOBVOI_SDS_FEED_SPEECH 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置待识别的声音的 Buf | 是

##### MOBVOI_SDS_STOP 参数：

无。

##### MOBVOI_SDS_CANCEL 参数：

无。

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service offlineContact = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_CONTACT);
```

#### 2 调用服务

##### 设置参数

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);

StringVector contacts = new StringVector();
contacts.add("张三");
contacts.add("李四");

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_CONTACTS, contacts);

Parameter ret = offlineContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中， eventHandler 的各种可能的回调消息为：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_LOCAL_SILENCE | 无 | 无 | 检测到静音
MOBVOI_SDS_CB_PARTIAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别部分结果
MOBVOI_SDS_CB_FINAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别最终结果。<br>如：“打电话给张三”
MOBVOI_SDS_CB_VOLUME | MOBVOI_SDS_CB_INFO | String | 输入语音数据的实时音量。<br>根据语音的能量算出。<br>范围为 [0, 1]
MOBVOI_SDS_CB_ERROR | MOBVOI_SDS_ERROR_CODE | int | 错误类型

##### 编译模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

Parameter ret = offlineContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 启动服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = offlineContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送音频数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = offlineContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止语音识别

**停止接收音频数据，但依然继续进行解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = offlineContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 取消语音识别

**停止接收音频数据，并停止解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = offlineContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(service);
```

---

## <span id = "sds-service-offline-keywords">离线关键词识别</span>

### 服务描述

离线关键词识别，功能类似于热词唤醒，可以不开启在线语音识别的情况下，同时对多个关键词进行监听。

例如，在听音乐的时候，直接说出“上一首”、“下一首”等指令。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_OFFLINE_KEYWORDS | 多关键词识别

#### 命令意图和参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置离线关键词服务
MOBVOI_SDS_BUILD_MODEL | 编译离线关键词模型
MOBVOI_SDS_START | 开始离线关键词服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据
MOBVOI_SDS_STOP | 停止离线关键词服务

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置关键字识别结果回调函数 | 是

##### MOBVOI_SDS_BUILD_MODEL包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_KEYWORDS | StringVector | 设置关键词列表内容 | 是

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_SPEECH包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置语音Buf | 是

##### MOBVOI_SDS_STOP包含以下参数：

无

### 服务调用流程

#### 1 获取关键词识别服务

```java
// SpeechSDS sds;

Service offlineKeywords = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_KEYWORDS);
```

#### 2 调用多关键词识别

##### 设置回调监听

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

Parameter ret = offlineKeywords.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中，传递给 eventHandler 的，可能有如下的回调消息：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_KEYWORD | MOBVOI_SDS_CB_INFO | String | 识别到的关键字

##### 编译关键词模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

// Set keywords of interest
StringVector keywords = new StringVector();
keywords.add("第一个");
keywords.add("第二个");
keywords.add("上一首");
keywords.add("下一首");
keywords.add("打开微信");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_KEYWORDS, keywords);

Parameter ret = offlineKeywords.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始关键词识别服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = offlineKeywords.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送语音数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = offlineKeywords.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止关键词识别服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = offlineKeywords.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

#### 3 释放关键词识别服务

```java
sds.releaseService(keywords);
```

----

## <span id = "sds-service-offline-psm">关联关键词识别</span>

### 服务描述

也叫做 PSM。对生成的或已有的页面表项，根据表项的部分文字或全部文字的语音匹配，返回识别内容。

部分文字匹配，例如有以下页面选项：

* "拿渡麻辣香锅"
* "必胜客中关村店"
* "全聚德烤鸭店"

可以说"必胜客"会"匹配到"必胜客中关村店"。

全部文字匹配，例如有以下页面选项：

* "第一个"
* "下一首"
* "下一页"

可以说"第一个"会精确匹配到"第一个"。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_OFFLINE_PSM | 页面相关文字识别

#### 命令意图和参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置PSM服务
MOBVOI_SDS_BUILD_MODEL | 编译PSM模型
MOBVOI_SDS_START | 执行PSM服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据
MOBVOI_SDS_STOP | 停止PSM服务

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置识别结果回调函数 | 是

##### MOBVOI_SDS_BUILD_MODEL包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_PARTIAL_MATCH_KEYWORDS | StringVector | 设置页面部分匹配内容列表 | 否
MOBVOI_SDS_PERFECT_MATCH_KEYWORDS | StringVector | 设置页面完全匹配内容列表 | 否

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_SPEECH包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置语音Buf | 是

##### MOBVOI_SDS_STOP包含以下参数：

无

### 服务调用流程

#### 1 获取PSM服务

```java
// SpeechSDS sds;

Service psm = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_PSM);
```

#### 2 调用PSM服务

##### 设置PSM服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

Parameter ret = psm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中，传递给 eventHandler 的，可能有如下的回调消息：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_PSM_KEYWORD | MOBVOI_SDS_CB_INFO | String | 识别到PSM结果

##### 编译PSM模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

// Set list of contents for just partial matching
StringVector partialKeywords = new StringVector();
partialKeywords.add("拿渡麻辣香锅");
partialKeywords.add("必胜客宅急送");
partialKeywords.add("全聚德烤鸭店清华园店中关村");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_PARTIAL_MATCH_KEYWORDS, partialKeywords);

// Set list of keywords for perfect/full matching
StringVector perfectKeywords = new StringVector();
perfectKeywords.add("第一个");
perfectKeywords.add("第二个");
perfectKeywords.add("第三个");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_PERFECT_MATCH_KEYWORDS, perfectKeywords);

Parameter ret = psm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始PSM服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = psm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送语音数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = psm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止PSM服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = psm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

#### 3 释放PSM服务

```java
sds.releaseService(psm);
```

---

## <span id = "sds-service-offline-nlu">离线语义理解</span>

### 服务描述

离线NLU，输入文本，返回离线NLU结果。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_OFFLINE_NLU | 离线NLU

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_BUILD_MODEL  | 编译离线NLU模型
MOBVOI_SDS_FEED_TEXT    | 输入文本，同步返回结果

##### MOBVOI_SDS_BUILD_MODEL包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_OFFLINE_APPS     | StringVector | 应用名列表 | 否
MOBVOI_SDS_OFFLINE_ALBUMS   | StringVector | 专辑名列表 | 否
MOBVOI_SDS_OFFLINE_ARTISTS  | StringVector | 艺术家列表 | 否
MOBVOI_SDS_OFFLINE_CONTACTS | StringVector | 联系人列表 | 否
MOBVOI_SDS_OFFLINE_SONGS    | StringVector | 歌曲名列表 | 否
MOBVOI_SDS_OFFLINE_VIDEOS   | StringVector | 视频名列表 | 否

##### MOBVOI_SDS_FEED_TEXT包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_TEXT | String | 输入的文本 | 是

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service offlineNlu = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_NLU);
```

#### 2 调用服务

##### 编译离线NLU模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

StringVector apps = new StringVector();
apps.add("支付宝");
apps.add("微信");
apps.add("美团");
apps.add("大众点评");

StringVector contacts = new StringVector();
contacts.add("刘德华");
contacts.add("周杰伦");
contacts.add("张学友");

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_APPS, apps);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_CONTACTS, contacts);

Parameter ret = offlineNlu.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 调用离线NLU，获取结果

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_TEXT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT, query)；

Parameter ret = offlineNlu.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
} else {
  String nluResult = ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT).asString();
}
```

#### 3 释放服务

```java
sds.releaseService(offlineNlu);
```

---

## <span id = "sds-service-offline-dm">离线对话管理</span>

### 服务描述

离线对话管理，输入为NLU的结果字符串。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_OFFLINE_DM | 离线对话管理

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_FEED_TEXT  | 输入文本，同步返回结果
MOBVOI_SDS_RESET      | 重置对话状态

##### MOBVOI_SDS_FEED_TEXT包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_TEXT | String | 输入的文本 | 是

##### MOBVOI_SDS_RESET包含以下参数：

无

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service offlineDm = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_DM);
```

#### 2 调用服务

##### 调用离线DM，获取结果

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_TEXT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT, nluResult)；

Parameter ret = offlineDm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
} else {
  String dmResult = ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT).asString();
}
```

##### 重置对话状态

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_RESET);

Parameter ret = offlineDm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(offlineDm);
```

---

## <span id = "sds-service-offline-recognizer">离线语音识别</span>

### 服务描述

对录入语音进行本地识别，没有网络连接方面的要求。

该语音识别提供[MOBVOI_SDS_OFFLINE_ASR](#sds-service-offline-asr)、[MOBVOI_SDS_OFFLINE_NLU](#sds-service-offline_nlu)、
[MOBVOI_SDS_OFFLINE_DM](#sds-service-offline-dm)等功能的组合。

输入的音频流的格式为 16kHz 采样、16bit 位深的小端 (Little-Endian) 单声道 PCM 数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_OFFLINE_RECOGNIZER | 离线语音识别

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置参数
MOBVOI_SDS_BUILD_MODEL | 编译离线模型
MOBVOI_SDS_START  | 执行语音识别服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据给语音识别服务
MOBVOI_SDS_STOP | 停止语音识别服务
MOBVOI_SDS_CANCEL | 取消语音识别服务

##### MOBVOI_SDS_SET_PARAM 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置语音识别结果回调函数 | 是
MOBVOI_SDS_LOCAL_START_SILENCE | int | 设置语音开始的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_LOCAL_END_SILENCE | int | 设置语音结束的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL | boolean | 设置是否使能 Partial transcript 的回调 | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>取值范围： [0, 1] | 否
MOBVOI_SDS_OFFLINE_APPS | StringVector | 设置应用列表 | 否
MOBVOI_SDS_OFFLINE_CONTACTS| StringVector | 设置联系人列表 | 否
MOBVOI_SDS_OFFLINE_ARTISTS| StringVector | 设置艺术家列表 | 否
MOBVOI_SDS_OFFLINE_SONGS| StringVector | 设置歌曲列表 | 否
MOBVOI_SDS_OFFLINE_ALBUMS| StringVector | 设置专辑列表 | 否
MOBVOI_SDS_OFFLINE_VIDEOS| StringVector | 设置视频列表 | 否
MOBVOI_SDS_ASR_HOTWORD| StringVector | 设置唤醒词列表 | 否
MOBVOI_SDS_RETURN_RAW_PARTIAL | boolean | 设置是否返回原始的部分识别结果。<br>默认为false | 否
MOBVOI_SDS_RETURN_RAW_FINAL | boolean | 设置是否返回原始的最终识别结果。<br>默认为false | 否


##### MOBVOI_SDS_BUILD_MODEL 参数：

无。

##### MOBVOI_SDS_START 参数：

无。

##### MOBVOI_SDS_FEED_SPEECH 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置待识别的声音的 Buf | 是

##### MOBVOI_SDS_STOP 参数：

无。

##### MOBVOI_SDS_CANCEL 参数：

无。

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service offlineRecognizer = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_RECOGNIZER);
```

#### 2 调用服务

##### 设置参数

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

Parameter ret = offlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中， eventHandler 的各种可能的回调消息为：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_LOCAL_SILENCE | 无 | 无 | 检测到静音
MOBVOI_SDS_CB_PARTIAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别部分结果
MOBVOI_SDS_CB_FINAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别最终结果
MOBVOI_SDS_CB_NLU | MOBVOI_SDS_CB_INFO | String | 根据语音识别得到的NLU结果，JSON字符串
MOBVOI_SDS_CB_RESULT | MOBVOI_SDS_CB_INFO | String | 根据NLU结果得到的DM结果，为JSON格式字符串
MOBVOI_SDS_CB_VOLUME | MOBVOI_SDS_CB_INFO | String | 输入语音数据的实时音量。<br>根据语音的能量算出。<br>范围为 [0, 1]
MOBVOI_SDS_CB_ERROR | MOBVOI_SDS_ERROR_CODE | int | 错误类型

##### 编译模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

Parameter ret = offlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 启动服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = offlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送音频数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = offlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止语音识别

**停止接收音频数据，但依然继续进行解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = offlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 取消语音识别

**停止接收音频数据，并停止解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = offlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(offlineRecognizer);
```

---

## <span id = "sds-service-online-asr">在线语音识别</span>

### 服务描述

可对输入语音进行在线识别,可对识别的语音进行语义理解，垂直搜索。

语音理解和垂直搜索详细说明请参考[搜索结果输出介绍](https://ai.chumenwenwen.com/pages/document/search-output-intro)。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

支持三种<span id = "online-reg-type">在线识别服务</span>：

Service | 描述
---- | ---
MOBVOI_SDS_ONLINE_ASR       | 返回在线语音识别结果
MOBVOI_SDS_ONLINE_ONEBOX    | 返回在线语音识别结果,并对识别的语音进行语义理解和垂直搜索
MOBVOI_SDS_ONLINE_SEMANTIC  | 返回在线语音识别结果,并对识别的语音进行语义理解

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM    | 设置语音识别参数
MOBVOI_SDS_START        | 开始语音识别服务
MOBVOI_SDS_FEED_SPEECH  | 发送语音数据给语音识别服务
MOBVOI_SDS_STOP         | 停止语音识别服务
MOBVOI_SDS_CANCEL       | 取消语音识别服务

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase| 设置语音识别结果回调函数 | 是
MOBVOI_SDS_LOCATION| String |设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568" | 否
MOBVOI_SDS_IN_COORD_SYS| String |用户设置给SDS的当前位置信息的经纬度所采用的坐标系统。<br>取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）。<br>缺省值："bd09" | 否
MOBVOI_SDS_OUT_COORD_SYS| String |SDS返回给用户位置信息的经纬度所采用的坐标系统。<br>取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）。<br>缺省值："bd09" | 否
MOBVOI_SDS_REMOTE_START_SILENCE| int |设置云端检测开始有人说话的时间阈值。<br>单位是ms，当检测到说话的时间没有超过这个阈值，认为是有人说话 | 否
MOBVOI_SDS_REMOTE_END_SILENCE| int |设置云端静音检测的时间阈值。<br>单位是ms，当静音检测的时间超过这个阈值，认为说话结束 | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>取值范围： [0, 1] | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL| boolean |是否输出语音识别的中间结果。<br>缺省值：true | 否

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_SPEECH包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF|Buf|设置语音Buf | 是

##### MOBVOI_SDS_STOP包含以下参数：

无

##### MOBVOI_SDS_CANCEL包含以下参数：

无

### 服务调用流程

#### 1 获取服务

选择一种[在线识别服务](#online-reg-type)类型:

```java
// SpeechSDS sds;

// <online_recognition_service> might be:
//   MobvoiSDSConstants.MOBVOI_SDS_ONLINE_ASR
//   MobvoiSDSConstants.MOBVOI_SDS_ONLINE_ONEBOX
//   MobvoiSDSConstants.MOBVOI_SDS_ONLINE_SEMANTIC
Service onlineRecognizer = sds.getService(<online_recognition_service>);
```

#### 2 调用服务

##### 设置语音识别参数

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LOCATION], "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_IN_COORD_SYS, "bd09");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OUT_COORD_SYS, "bd09");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_REMOTE_START_SILENCE, 5000);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_REMOTE_END_SILENCE, 1000);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_ENABLE_ON_VOLUME, true);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_ENABLE_CB_PARTIAL,  true);

Parameter ret = onlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始语音识别

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = onlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送语音数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = onlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 停止语音识别

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = onlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 取消语音识别

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = onlineRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(onlineRecognizer);
```

---

## <span id = "sds-service-online-nlu">在线语义理解</span>

### 服务描述

进行在线的自然语言理解(NLU)。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_ONLINE_NLU | 在线 NLU

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置参数
MOBVOI_SDS_FEED_TEXT | 输入文本

##### MOBVOI_SDS_SET_PARAM 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置NLU结果回调函数 | 是

##### MOBVOI_SDS_FEED_TEXT 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_TEXT | String | 输入文本 | 是

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service onlineNlu = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_ONLINE_NLU);
```

#### 2 调用服务

##### 设置参数

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

Parameter ret = onlineNlu.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中， eventHandler 的各种可能的回调消息为：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_NLU | MOBVOI_SDS_CB_INFO | String | NLU结果，JSON字符串
MOBVOI_SDS_CB_ERROR | MOBVOI_SDS_ERROR_CODE | int | 错误类型

##### 发送文本

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_TEXT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT, text);

Parameter ret = onlineNlu.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(onlineNlu);
```

---

## <span id = "sds-service-online-search">在线垂直搜索</span>

### 服务描述

进行在线搜索。如：“天气怎么样？”，返回结果：”海淀区今天晴，12到23摄氏度“。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_ONLINE_SEARCH | 在线 SEARCH

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置参数
MOBVOI_SDS_FEED_TEXT | 输入文本

##### MOBVOI_SDS_SET_PARAM 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置NLU结果回调函数 | 是

##### MOBVOI_SDS_FEED_TEXT 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_TEXT | String | 输入文本 | 是
MOBVOI_SDS_QA_PARAMS | String | JSON字符串 | 否
MOBVOI_SDS_SEARCH_ASYNC | boolean | 是否进行异步搜索, 默认为true | 否

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service onlineSearch = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_ONLINE_SEARCH);
```

#### 2 调用服务

##### 设置参数

**适用于异步搜索**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

Parameter ret = onlineSearch.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中， eventHandler 的各种可能的回调消息为：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_SEARCH_RESULT | MOBVOI_SDS_CB_INFO | String | 搜索结果，JSON字符串
MOBVOI_SDS_CB_ERROR | MOBVOI_SDS_ERROR_CODE | int | 错误类型

##### 发送文本

**异步搜索**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_TEXT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT, text);

Parameter ret = onlineSearch.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**同步搜索**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_TEXT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT, text);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_SEARCH_ASYNC, false);

Parameter ret = onlineSearch.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
} else {
  String result = ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT).asString();
}
```

进行同步搜索时，返回结果参数如下表：

参数 | 类型 | 描述
---- | ----| ----
MOBVOI_SDS_ERROR_CODE | int | 错误类型
MOBVOI_SDS_TEXT | String | 搜索结果，JSON字符串

#### 3 释放服务

```java
sds.releaseService(onlineSearch);
```

---

## <span id = "sds-service-online-control">在线参数控制</span>

### 服务描述

可上传应用和联系人列表到云端，用于在线的语音识别。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_ONLINE_CONTROL | 在线参数控制

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_UPLOAD_USER_INFO | 上传用户信息

##### MOBVOI_SDS_UPLOAD_USER_INFO包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_USER_INFO_ID     | String | 上传唯一ID   | 是
MOBVOI_SDS_ONLINE_APPS      | StringVector | 专辑名列表   | 否
MOBVOI_SDS_ONLINE_CONTACTS  | StringVector | 艺术家列表   | 否

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service onlineControl = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_ONLINE_CONTROL);
```

#### 2 调用服务

##### 上传联系人和应用列表

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_UPLOAD_USER_INFO);

StringVector apps = new StringVector();
apps.add("支付宝");
apps.add("微信");
apps.add("美团");
apps.add("大众点评");

StringVector contacts = new StringVector();
contacts.add("刘德华");
contacts.add("周杰伦");
contacts.add("张学友");

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_USER_INFO_ID, deviceId);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_ONLINE_APPS, apps);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_ONLINE_CONTACTS, contacts);

Parameter ret = onlineControl.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(onlineControl);
```

---

## <span id = "sds-service-mix-asr">离在线语音识别</span>

### 服务描述

同时启动MOBVOI_SDS_ONLINE_ASR和MOBVOI_SDS_OFFLINE_ASR服务。
如果是本地资源就返回离线ASR结果，否则返回在线ASR结果。
只返回ASR结果，不包括NLU和DM。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_MIXED_GENERAL_ASR | 离在线混合ASR服务

#### 命令意图和参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置离在线混合ASR服务
MOBVOI_SDS_BUILD_MODEL | 编译模型
MOBVOI_SDS_START | 执行离在线混合ASR服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据
MOBVOI_SDS_STOP | 停止离在线混合ASR服务
MOBVOI_SDS_CANCEL | 取消离在线混合ASR服务

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param | 类型 | 描述 | 取值范围 | 是否必选
---- | --- | --- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置识别结果回调函数 | | 否
MOBVOI_SDS_LOCATION | String | 设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568" | | 否
MOBVOI_SDS_IN_COORD_SYS | String | 设置 MOBVOI_SDS_LOCATION 所用的坐标系统。<br>分别对应于世界坐标系、国测局坐标系（火星坐标系）以及百度坐标系。<br>默认为百度坐标系 | wgs84 <br> gcj02 <br> bd09 | 否
MOBVOI_SDS_OUT_COORD_SYS | String | 设置位置服务返回的位置信息所采用的坐标系。<br>分别对应于世界坐标系、国测局坐标系（火星坐标系）以及百度坐标系。<br>默认为百度坐标系 | wgs84 <br> gcj02 <br> bd09 | 否
MOBVOI_SDS_LOCAL_START_SILENCE | int | 设置本地静音开始时间，单位:ms | | 否
MOBVOI_SDS_LOCAL_END_SILENCE | int | 设置本地静音结束时间，单位:ms | | 否
MOBVOI_SDS_REMOTE_START_SILENCE | int | 设置服务器端静音开始时间，单位:ms | | 否
MOBVOI_SDS_REMOTE_START_SILENCE | int | 设置服务器端静音结束时间，单位:ms | | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL | boolean | 是否输出partial结果 | | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出 | [0, 1] | 否
MOBVOI_SDS_OFFLINE_APPS | StringVector | 设置应用列表 | | 否
MOBVOI_SDS_OFFLINE_ALBUMS | StringVector | 设置专辑列表 | | 否
MOBVOI_SDS_OFFLINE_ARTISTS | StringVector | 设置艺术家列表 | | 否
MOBVOI_SDS_OFFLINE_CONTACTS | StringVector | 设置联系人列表 | | 否
MOBVOI_SDS_OFFLINE_SONGS | StringVector | 设置歌曲列表 | | 否
MOBVOI_SDS_OFFLINE_VIDEOS | StringVector | 设置视频列表 | | 否

##### MOBVOI_SDS_BUILD_MODEL包含以下参数：

无

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_SPEECH包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置语音Buf | 是

##### MOBVOI_SDS_STOP包含以下参数：

无

##### MOBVOI_SDS_CANCEL包含以下参数：

无

### 服务调用流程

#### 1 获取离在线混合ASR服务

```java
// SpeechSDS sds;

Service mixAsr = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_MIXED_GENERAL_ASR);
```

#### 2 调用离在线混合ASR服务

##### 设置离在线混合ASR服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

Parameter ret = mixAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中，传递给 eventHandler 的，可能有如下的回调消息：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_REMOTE_SILENCE | 无 | 无 | 检测到服务器端静音
MOBVOI_SDS_CB_LOCAL_SILENCE | 无 | 无 | 检测到本地静音
MOBVOI_SDS_CB_PARTIAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别部分结果
MOBVOI_SDS_CB_FINAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别最终结果
MOBVOI_SDS_CB_VOLUME | MOBVOI_SDS_CB_INFO | String | 输入语音数据的实时音量。<br>根据语音的能量算出。<br>范围为 [0, 1]
MOBVOI_SDS_CB_ERROR | OBVOI_SDS_ERROR_CODE | int | 错误类型

##### 编译模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

Parameter ret = mixAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始离在线混合ASR服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = mixAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送语音数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = mixAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止离在线混合ASR服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter result = mixAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 取消离在线混合ASR服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = mixAsr.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

#### 3 释放离在线混合ASR服务

```java
sds.releaseService(mixAsr);
```

---

## <span id = "sds-service-mixed-contact-only">混合式联系人识别</span>

### 服务描述

在使用联系人识别时，同时可以识别其他语音。

比如语音输入“打电话给张三”，其中联系人“张三”通过生成的联系人模型识别，提高识别“张三”的准确率。还可以进行“打开空调”的识别。

输入的音频流的格式为 16kHz 采样、16bit 位深的小端 (Little-Endian) 单声道 PCM 数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_MIXED_CONTACT_ONLY | 混合式联系人识别

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置参数
MOBVOI_SDS_BUILD_MODEL | 编译离线模型
MOBVOI_SDS_START  | 执行语音识别服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据给语音识别服务
MOBVOI_SDS_STOP | 停止语音识别服务
MOBVOI_SDS_CANCEL | 取消语音识别服务

##### MOBVOI_SDS_SET_PARAM 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置语音识别结果回调函数 | 是
MOBVOI_SDS_OFFLINE_CONTACTS| StringVector | 设置联系人列表 | 是
MOBVOI_SDS_LOCAL_START_SILENCE | int | 设置离线语音开始的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_LOCAL_END_SILENCE | int | 设置离线语音结束的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_REMOTE_START_SILENCE | int | 设置在线语音开始的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_REMOTE_END_SILENCE | int | 设置在线语音结束的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL | boolean | 设置是否使能 Partial transcript 的回调 | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>取值范围： [0, 1] | 否

##### MOBVOI_SDS_BUILD_MODEL 参数：

无。

##### MOBVOI_SDS_START 参数：

无。

##### MOBVOI_SDS_FEED_SPEECH 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置待识别语音的 Buf | 是

##### MOBVOI_SDS_STOP 参数：

无。

##### MOBVOI_SDS_CANCEL 参数：

无。

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service mixContact = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_MIXED_CONTACT_ONLY);
```

#### 2 调用服务

##### 设置参数

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

StringVector contacts = new StringVector();
contacts.add("张三");
contacts.add("李四");

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_CONTACTS, contacts);

Parameter ret = mixContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中， eventHandler 的各种可能的回调消息为：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_LOCAL_SILENCE | 无 | 无 | 检测到静音
MOBVOI_SDS_CB_PARTIAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别部分结果
MOBVOI_SDS_CB_FINAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别最终结果
MOBVOI_SDS_CB_VOLUME | MOBVOI_SDS_CB_INFO | String | 输入语音数据的实时音量。<br>根据语音的能量算出。<br>范围为 [0, 1]
MOBVOI_SDS_CB_NLU | MOBVOI_SDS_CB_INFO | String | NLU的结果，为JSON格式字符串
MOBVOI_SDS_CB_RESULT | MOBVOI_SDS_CB_INFO | String | 语音搜索结果返回，为JSON格式字符串
MOBVOI_SDS_CB_ERROR | MOBVOI_SDS_ERROR_CODE | int | 错误类型

##### 编译模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

Parameter ret = mixContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 启动服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = mixContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送音频数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = mixContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止语音识别

**停止接收音频数据，但依然继续进行解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = mixContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 取消语音识别

**停止接收音频数据，并停止解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = mixContact.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(mixContact);
```

---

## <span id = "sds-service-mixed-recognizer">离在线语音识别</span>

### 服务描述

对输入的语音同时进行离、在线语音识别、语义理解、垂直搜索和对话管理，并根据指定的策略返回相应的结果。

一般来说，在线识别通常具有更准确、更详尽的结果，而离线识别则总可以保证响应的时间，对于某些领域（如联系人识别），离线识别可能具有更优的结果。该服务根据指定的策略综合选择离线或者在线的结果，可以保证语音识别的质量和响应时间。

输入的音频流的格式为 16kHz 采样、16bit 位深的小端 (Little-Endian) 单声道 PCM 数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ----
MOBVOI_SDS_MIXED_RECOGNIZER | 离在线语音识别

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
--- | ---
MOBVOI_SDS_SET_PARAM | 设置参数
MOBVOI_SDS_BUILD_MODEL | 编译离线模型
MOBVOI_SDS_START  | 执行语音识别服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据
MOBVOI_SDS_STOP | 停止语音识别服务
MOBVOI_SDS_CANCEL | 取消语音识别服务

##### MOBVOI_SDS_SET_PARAM 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置语音识别结果回调函数 | 是
MOBVOI_SDS_LOCAL_START_SILENCE | int | 设置离线语音开始的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_LOCAL_END_SILENCE | int | 设置离线语音结束的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_REMOTE_START_SILENCE | int | 设置在线语音开始的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_REMOTE_END_SILENCE | int | 设置在线语音结束的静音检测的时间，单位毫秒 | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL | boolean | 设置是否使能 Partial transcript 的回调 | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>取值范围： [0, 1] | 否
MOBVOI_SDS_OFFLINE_APPS | StringVector | 设置应用名列表 | 否
MOBVOI_SDS_OFFLINE_ALBUMS | StringVector | 设置专辑名列表 | 否
MOBVOI_SDS_OFFLINE_ARTISTS | StringVector | 设置艺术家列表 | 否
MOBVOI_SDS_OFFLINE_CONTACTS | StringVector | 设置联系人列表 | 否
MOBVOI_SDS_OFFLINE_SONGS | StringVector | 设置歌曲名列表 | 否
MOBVOI_SDS_OFFLINE_VIDEOS | StringVector | 设置视频名列表 | 否
MOBVOI_SDS_ASR_HOTWORD| StringVector | 设置唤醒词列表 | 否
MOBVOI_SDS_RETURN_RAW_PARTIAL | boolean | 设置是否返回原始的部分识别结果。<br>默认为false | 否

##### MOBVOI_SDS_BUILD_MODEL 参数：

这里所设置的参数，将会由语义理解部分所使用。而上面 MOBVOI_SDS_SET_PARAM 中所设置的相同参数，由语音识别部分使用。

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_OFFLINE_APPS | StringVector | 设置应用名列表 | 否
MOBVOI_SDS_OFFLINE_ALBUMS | StringVector | 设置专辑名列表 | 否
MOBVOI_SDS_OFFLINE_ARTISTS | StringVector | 设置艺术家列表 | 否
MOBVOI_SDS_OFFLINE_CONTACTS | StringVector | 设置联系人列表 | 否
MOBVOI_SDS_OFFLINE_SONGS | StringVector | 设置歌曲名列表 | 否
MOBVOI_SDS_OFFLINE_VIDEOS | StringVector | 设置视频名列表 | 否

##### MOBVOI_SDS_START 参数：

无。

##### MOBVOI_SDS_FEED_SPEECH 参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置待识别语音的 Buf | 是

##### MOBVOI_SDS_STOP 参数：

无。

##### MOBVOI_SDS_CANCEL 参数：

无。

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service mixRecognizer = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_MIXED_RECOGNIZER);
```

#### 2 调用服务

##### 设置参数

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

StringVector contacts = new StringVector();
contacts.add("张三");
contacts.add("李四");

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_CONTACTS, contacts);

Parameter ret = mixRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中， eventHandler 的各种可能的回调消息为：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ----| ----
MOBVOI_SDS_CB_LOCAL_SILENCE | 无 | 无 | 检测到静音
MOBVOI_SDS_CB_PARTIAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别部分结果
MOBVOI_SDS_CB_FINAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别最终结果
MOBVOI_SDS_CB_VOLUME | MOBVOI_SDS_CB_INFO | String | 输入语音数据的实时音量。<br>根据语音的能量算出。<br>范围为 [0, 1]
MOBVOI_SDS_CB_NLU | MOBVOI_SDS_CB_INFO | String | NLU的结果，为JSON格式字符串
MOBVOI_SDS_CB_RESULT | MOBVOI_SDS_CB_INFO | String | 语音搜索结果返回，为JSON格式字符串
MOBVOI_SDS_CB_ERROR | MOBVOI_SDS_ERROR_CODE | int | 错误类型

##### 编译模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

Parameter ret = mixRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 启动服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = mixRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送音频数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = mixRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 停止语音识别

**停止接收音频数据，但依然继续进行解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = mixRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 取消语音识别

**停止接收音频数据，并停止解码。**

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = mixRecognizer.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(mixRecognizer);
```

---

## <span id = "sds-service-always-on">Always On服务</span>

### 服务描述

由离线 PSM、VAD 和 Mixed Recognizer组成的 pipeline 服务，同时支持PSM和Mixed Recognizer，对于需要同时使用PSM 和 Mixed Recognizer 的场景中，只使 Always On 就可完成。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_ALWAYS_ON | PSM&VAD&Mixed Recognier组合pipeline

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM    | 设置语音识别参数
MOBVOI_SDS_BUILD_MODEL  | 编译离线模型
MOBVOI_SDS_START        | 开始语音识别服务
MOBVOI_SDS_FEED_SPEECH  | 发送语音数据给语音识别服务
MOBVOI_SDS_STOP         | 停止语音识别服务
MOBVOI_SDS_CANCEL       | 取消语音识别服务

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase| 设置语音识别结果回调函数 | 是
MOBVOI_SDS_LOCATION|String|设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568" | 否
MOBVOI_SDS_IN_COORD_SYS|String|用户设置给SDS的当前位置信息的经纬度所采用的坐标系统。<br>取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）。<br>缺省值："bd09" | 否
MOBVOI_SDS_OUT_COORD_SYS|String|SDS返回给用户位置信息的经纬度所采用的坐标系统。<br>取值："wgs84"（世界坐标系）， "gcj02"（国测局坐标系），"bd09"（百度坐标系）。<br>缺省值："bd09" | 否
MOBVOI_SDS_REMOTE_START_SILENCE|int|设置云端检测开始有人说话的时间阈值。<br>单位是ms。<br>当检测到说话的时间没有超过这个阈值，认为是有人说话 | 否
MOBVOI_SDS_REMOTE_END_SILENCE|int|设置云端静音检测的时间阈值。<br>单位是ms。<br>当静音检测的时间超过这个阈值，认为说话结束 | 否
MOBVOI_SDS_LOCAL_START_SILENCE|int|设置本地检测开始有人说话的时间阈值。<br>单位是ms。<br>当检测到说话的时间超过这个阈值，认为是有人说话 | 否
MOBVOI_SDS_LOCAL_END_SILENCE|int|设置本地静音检测的时间阈值。<br>单位是ms。<br>当静音检测的时间超过这个阈值，认为说话结束 | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>取值范围： [0, 1] | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL|boolean|是否输出语音识别的中间结果。<br>缺省值：true | 否

##### MOBVOI_SDS_BUILD_MODEL包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_PARTIAL_MATCH_KEYWORDS     | StringVector | PSM部分匹配列表      | 否
MOBVOI_SDS_PERFECT_MATCH_KEYWORDS     | StringVector | PSM全匹配列表列表     | 否
MOBVOI_SDS_OFFLINE_APPS               | StringVector | 应用名列表           | 否
MOBVOI_SDS_OFFLINE_ALBUMS             | StringVector | 专辑名列表           | 否
MOBVOI_SDS_OFFLINE_ARTISTS            | StringVector | 艺术家列表           | 否
MOBVOI_SDS_OFFLINE_CONTACTS           | StringVector | 联系人列表           | 否
MOBVOI_SDS_OFFLINE_SONGS              | StringVector | 歌曲名列表           | 否
MOBVOI_SDS_OFFLINE_VIDEOS             | StringVector | 视频名列表           | 否

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_SPEECH包含以下参数：

Param | 类型 | 描述 | 是否必选
---- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置语音Buf | 是

##### MOBVOI_SDS_STOP包含以下参数：

无

##### MOBVOI_SDS_CANCEL包含以下参数：

无

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service alwaysOn = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_ALWAYS_ON);
```

#### 2 调用服务

##### 设置语音识别参数

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LOCATION, "中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_IN_COORD_SYS, "bd09");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OUT_COORD_SYS, "bd09");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_REMOTE_START_SILENCE, 5000);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_REMOTE_END_SILENCE, 1000);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LOCAL_START_SILENCE, 60);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LOCAL_END_SILENCE, 600);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_ENABLE_ON_VOLUME, true);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_ENABLE_CB_PARTIAL, true);

Parameter ret = alwaysOn.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 编译离线模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

StringVector apps = new StringVector();
apps.add("支付宝");
apps.add("微信");
apps.add("美团");
apps.add("大众点评");

StringVector contacts = new StringVector();
contacts.add("刘德华");
contacts.add("周杰伦");
contacts.add("张学友");

StringVector partialKeywords = new StringVector();
partialKeywords.add("清真回回熬记肉饼店");
partialKeywords.add("拿渡麻辣香锅");
partialKeywords.add("必胜客宅急送");
partialKeywords.add("全聚德烤鸭店清华园店中关村");

StringVector perfectKeywords = new StringVector();
perfectKeywords.add("第一个");
perfectKeywords.add("第二个");
perfectKeywords.add("第三个");
perfectKeywords.add("第四个");
perfectKeywords.add("上一首");
perfectKeywords.add("下一首");
perfectKeywords.add("播放音乐");

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_APPS], apps);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_CONTACTS, contacts);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_PARTIAL_MATCH_KEYWORDS, partialKeywords);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_PERFECT_MATCH_KEYWORDS, perfectKeywords);

Parameter ret = alwaysOn.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始语音识别

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = alwaysOn.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送语音数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = alwaysOn.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 停止语音识别

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = alwaysOn.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 取消语音识别

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = alwaysOn.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(alwaysOn);
```
---

## <span id = "sds-service-concurrent-psm-mix">关联关键词及离在线识别</span>

### 服务描述

同时启动MOBVOI_SDS_OFFLINE_PSM和MOBVOI_SDS_MIXED_RECOGNIZER服务。
如果语音在PSM设置中，则返回PSM结果，否则返回MIXED RECOGNIZER识别结果。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_CONCURRENT_PSM_MIX | 并行PSM和混合语音识别

#### 命令意图和参数

Intent | 描述
---|---
MOBVOI_SDS_SET_PARAM | 设置服务
MOBVOI_SDS_BUILD_MODEL | 编译模型
MOBVOI_SDS_START | 执行并行PSM混合服务
MOBVOI_SDS_FEED_SPEECH | 发送语音数据
MOBVOI_SDS_CANCEL | 取消并行PSM混合服务

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_CALLBACK | CallBackBase | 设置识别结果回调函数 | 否
MOBVOI_SDS_LOCATION | String | 设置地理位置。<br>例如："中国,北京市,北京市,海淀区,苏州街,3号,39.989602,116.316568" | 否
MOBVOI_SDS_LOCAL_START_SILENCE | int | 设置本地静音开始时间，单位:ms | 否
MOBVOI_SDS_LOCAL_END_SILENCE | int | 设置本地静音结束时间，单位:ms | 否
MOBVOI_SDS_REMOTE_START_SILENCE | int | 设置服务器端静音开始时间，单位:ms | 否
MOBVOI_SDS_REMOTE_START_SILENCE | int | 设置服务器端静音结束时间，单位:ms | 否
MOBVOI_SDS_ENABLE_CB_PARTIAL | boolean | 是否输出partial结果 | 否
MOBVOI_SDS_ENABLE_CB_VOLUME | boolean | 设置是否使能有关输入语音数据的实时音量回调。<br>根据语音的能量算出。<br>取值范围： [0, 1] | 否
MOBVOI_SDS_OFFLINE_APPS | StringVector | 设置应用列表 | 否
MOBVOI_SDS_OFFLINE_ALBUMS | StringVector | 设置专辑列表 | 否
MOBVOI_SDS_OFFLINE_ARTISTS | StringVector | 设置艺术家列表 | 否
MOBVOI_SDS_OFFLINE_CONTACTS | StringVector | 设置联系人列表 | 否
MOBVOI_SDS_OFFLINE_SONGS | StringVector | 设置歌曲列表 | 否
MOBVOI_SDS_OFFLINE_VIDEOS | StringVector | 设置视频列表 | 否

##### MOBVOI_SDS_BUILD_MODEL包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_PARTIAL_MATCH_KEYWORDS | StringVector | 设置页面部分匹配内容列表 | 否
MOBVOI_SDS_PERFECT_MATCH_KEYWORDS | StringVector | 设置页面完全匹配内容列表 | 否

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_SPEECH包含以下参数：

Param | 类型 | 描述 | 必选
--- | --- | --- | ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置语音Buf | 是

##### MOBVOI_SDS_CANCEL包含以下参数：

无

### 服务调用流程

#### 1 获取并行PSM混合服务

```java
// SpeechSDS sds;

Service mixPsm = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_CONCURRENT_PSM_MIX);
```

#### 2 调用并行PSM混合服务

##### 设置并行PSM混合服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);

Parameter ret = mixPsm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中，传递给 eventHandler 的，可能有如下的回调消息：

MOBVOI_SDS_CB_TYPE 取值 | 额外参数 | 类型 | 描述
---- | ---- | ---- | ----
MOBVOI_SDS_CB_SPEECH | 无 | 无 | 接收到语音数据
MOBVOI_SDS_CB_LOCAL_SILENCE | 无 | 无 | 检测到本地静音
MOBVOI_SDS_CB_REMOTE_SILENCE | 无 | 无 | 检测到服务器端静音
MOBVOI_SDS_CB_PARTIAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别部分结果
MOBVOI_SDS_CB_FINAL_TRANSCRIPT | MOBVOI_SDS_CB_INFO | String | 语音识别最终结果
MOBVOI_SDS_CB_NLU | MOBVOI_SDS_CB_INFO | String | NLU的结果，为JSON格式字符串
MOBVOI_SDS_CB_RESULT | MOBVOI_SDS_CB_INFO | String | 语音搜索结果返回，为JSON格式字符串
MOBVOI_SDS_CB_VOLUME | MOBVOI_SDS_CB_INFO | String | 输入语音数据的实时音量。<br>根据语音的能量算出。<br>范围为 [0, 1]
MOBVOI_SDS_CB_ERROR | MOBVOI_SDS_CB_INFO | int | 错误类型

##### 编译模型

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_BUILD_MODEL);

// Set list of contents for just partial matching
StringVector partialKeywords = new StringVector();
partialKeywords.add("拿渡麻辣香锅");
partialKeywords.add("必胜客宅急送");
partialKeywords.add("全聚德烤鸭店清华园店中关村");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_PARTIAL_MATCH_KEYWORDS, partialKeywords);

// Set list of keywords for perfect/full matching
StringVector perfectKeywords = new StringVector();
perfectKeywords.add("第一个");
perfectKeywords.add("第二个");
perfectKeywords.add("第三个");
perfectKeywords.add("上一首");
perfectKeywords.add("下一首");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_PERFECT_MATCH_KEYWORDS, perfectKeywords);

Parameter ret = mixPsm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始并行PSM混合服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = mixPsm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送语音数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = mixPsm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

**用户需要多次发送音频数据，建议每10ms发送一次音频数据，每次音频数据长度为320字节。**

##### 取消并行PSM混合服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_CANCEL);

Parameter ret = mixPsm.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

#### 3 释放并行PSM混合服务

```java
sds.releaseService(mixPsm);
```


## <span id = "sds-service-offline-tts">离线语音合成</span>

### 服务描述

对输入的文本进行语音合成返回合成音频。

合成文本长度不能超过500个汉字。

合成的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_OFFLINE_TTS | 离线语音合成

#### 命令意图和参数

Intent | 描述
---- | ---
MOBVOI_SDS_SET_PARAM | 设置语音合成
MOBVOI_SDS_START | 开始语音合成
MOBVOI_SDS_FEED_TEXT | 传入语音合成文本
MOBVOI_SDS_READ | 读取合成语音数据
MOBVOI_SDS_STOP | 停止语音合成

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_LANGUAGE | String | 设置合成语言类型。<br>取值："Mandarin"， "English"，"Cantonese"。<br>缺省值："Mandarin" | 否
MOBVOI_SDS_SPEED | String | 设置合成语速。<br>取值："0.5"(slow) - "2.0"(fast) | 否
MOBVOI_SDS_SPEAKER | String | 设置发音人。<br>每个发音人支持不同的语言。<br>详细请参考[说明](#lang-speaker) | 否

<span id = "lang-speaker">**说明：**</span>

LANGUAGE | SPEAKER
--- | ---
Mandarin | lucy, tina, cissy, cissy_gru, billy, billy_gru
English | angela
Cantonese | dora

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_TEXT包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_TEXT | String | 输入需要语音合成的文字，支持中文和英文 | 是

##### MOBVOI_SDS_READ包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置存放合成语音数据Buf | 是

MOBVOI_SDS_READ的返回值可以通过MOBVOI_SDS_TTS_READ_SIZE获取，表示实际读取的合成语音数据长度。

返回-1表示读取合成语音数据结束，具体方法请参考[获取合成语音数据](#get-synthesis-audio)。

##### MOBVOI_SDS_STOP包含以下参数：

无

### 服务调用流程

#### 1 获取语音合成服务

```java
// SpeechSDS sds;

Service offlineTts = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_OFFLINE_TTS);
```

#### 2 执行语音合成

##### 设置语音合成

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LANGUAGE, "Mandarin");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_SPEED, "1.2");

Parameter ret = offlineTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始语音合成服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = offlineTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 设置语音合成内容

```java
String text = "语音合成测试。"

Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_TEXT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT, text);

Parameter ret = offlineTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### <span id = "get-synthesis-audio">获取合成语音数据</span>

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_READ);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 3200 (100ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = offlineTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}

// Loop to read, until readBytes == -1.
int readBytes = ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_TTS_READ_SIZE).asInt();
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

#### 3 释放语音合成服务

```java
sds.releaseService(offlineTts);
```

---

## <span id = "sds-service-online-tts">在线语音合成</span>

### 服务描述

在线对输入的文本进行语音合成返回合成音频。

合成文本长度不能超过500个汉字。

合成的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_ONLINE_TTS | 在线语音合成

#### 命令意图和参数

Intent | 描述
---- | ---
MOBVOI_SDS_SET_PARAM | 设置语音合成
MOBVOI_SDS_START | 开始语音合成
MOBVOI_SDS_FEED_TEXT | 传入语音合成文本
MOBVOI_SDS_READ | 读取合成语音数据
MOBVOI_SDS_STOP | 停止语音合成

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_LANGUAGE | String | 设置合成语言类型。<br>取值："Mandarin"， "English"，"Cantonese"。<br>缺省值："Mandarin" | 否
MOBVOI_SDS_SPEED | String | 设置合成语速。<br>取值："0.5"(slow) - "2.0"(fast) | 否
MOBVOI_SDS_SPEAKER | String | 设置发音人。<br>每个发音人支持不同的语言。<br>详细请参考[说明](#lang-speaker) | 否

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_TEXT包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_TEXT | String | 输入需要语音合成的文字，支持中文和英文 | 是

##### MOBVOI_SDS_READ包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置存放合成语音数据Buf | 是

MOBVOI_SDS_READ的返回值可以通过MOBVOI_SDS_TTS_READ_SIZE获取，表示实际读取的合成语音数据长度。

返回-1表示读取合成语音数据结束，具体方法请参考[获取合成语音数据](#get-synthesis-audio)。

##### MOBVOI_SDS_STOP包含以下参数：

无

### 服务调用流程

#### 1 获取语音合成服务

```java
// SpeechSDS sds;

Service onlineTts = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_ONLINE_TTS);
```

#### 2 执行语音合成

##### 设置语音合成

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LANGUAGE, "Mandarin");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_SPEED, "1.2");

Parameter ret = onlineTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始语音合成服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = onlineTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 设置语音合成内容

```java
String text = "语音合成测试。"
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_TEXT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT, text);

Parameter ret = onlineTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 获取合成语音数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_READ);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 3200 (100ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = onlineTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}

// Loop to read, until readBytes == -1.
int readBytes = ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_TTS_READ_SIZE).asInt();
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

#### 3 释放语音合成服务

```java
sds.releaseService(onlineTts);
```

---

## <span id = "sds-service-mix-tts">离在线语音合成</span>

### 服务描述

对输入的文本进行语音合成返回合成音频。
一般来说，在线合成通常具有更佳的音质，而离线合成则总可以保证响应的时间。该服务根据指定的策略综合选择离线或在线合成的结果，可以保证语音合成的质量和速度。

合成文本长度不能超过500个汉字。

合成的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

### 参数描述

#### 服务类型

Service | 描述
---- | ---
MOBVOI_SDS_MIXED_TTS | 离在线混合式语音合成

#### 命令意图和参数

Intent | 描述
---- | ---
MOBVOI_SDS_SET_PARAM | 设置语音合成
MOBVOI_SDS_START | 开始语音合成
MOBVOI_SDS_FEED_TEXT | 传入语音合成文本
MOBVOI_SDS_READ | 读取合成语音数据
MOBVOI_SDS_STOP | 停止语音合成

##### MOBVOI_SDS_SET_PARAM包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_LANGUAGE | String | 设置合成语言类型。<br>取值："Mandarin"， "English"，"Cantonese"。<br>缺省值："Mandarin" | 否
MOBVOI_SDS_SPEED | String | 设置合成语速。<br>取值："0.5"(slow) - "2.0"(fast) | 否
MOBVOI_SDS_SPEAKER | String | 设置发音人。<br>每个发音人支持不同的语言。<br>详细请参考[说明](#lang-speaker) | 否
MOBVOI_SDS_TTS_MIX_STRATEGY | String | 离在线混合合成策略。<br>详细请参考下面[说明](#tts-mix-strategy) | 否

<span id = "tts-mix-strategy">**说明：**</span>

MOBVOI_SDS_TTS_MIX_STRATEGY 支持以下两种策略：

* FAST_PREFERRED: 默认策略。同时启动在线语音合成服务和离线语音合成服务，使用返回合成语音快的服务。
* ONLINE_PREFERRED: 在线合成优先。同时启动在线语音合成服务和离线语音合成服务，由于在线合成效果比离线好，在超时(300ms)范围内优先使用在线合成服务。

##### MOBVOI_SDS_START包含以下参数：

无

##### MOBVOI_SDS_FEED_TEXT包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_TEXT | String | 输入需要语音合成的文字，支持中文和英文 | 是

##### MOBVOI_SDS_READ包含以下参数：

Param |类型| 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_AUDIO_BUF | Buf | 设置存放合成语音数据Buf | 是

MOBVOI_SDS_READ的返回值可以通过MOBVOI_SDS_TTS_READ_SIZE获取，表示实际读取的合成语音数据长度。

返回-1表示读取合成语音数据结束，具体方法请参考[获取合成语音数据](#get-synthesis-audio)。

##### MOBVOI_SDS_STOP包含以下参数：

无

### 服务调用流程

#### 1 获取语音合成服务

```java
// SpeechSDS sds;

Service mixTts = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_MIXED_TTS);
```

#### 2 执行语音合成

##### 设置语音合成

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_LANGUAGE, "Mandarin");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_SPEED, "1.2");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TTS_MIX_STRATEGY, "FAST_PREFERRED");

Parameter ret = mixTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 开始语音合成服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = mixTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 设置语音合成内容

```java
String text = "语音合成测试。"
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_TEXT);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_TEXT, text);

Parameter ret = mixTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 获取合成语音数据

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_READ);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 3200 (100ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = mixTts.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}

// Loop to read, until readBytes == -1.
int readBytes = ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_TTS_READ_SIZE).asInt();
```

执行结果返回值及错误说明请参考[错误说明](#sds-error-code)。

#### 3 释放语音合成服务

```java
sds.releaseService(mixTts);
```

---

## <span id = "sds-service-vad">语音检测</span>

### 服务描述

对输入的音频流进行活动性检测，比如是否有人说话，是否静音。

输入音频流的音频格式为16k采样，16bit的小端(Little-Endian)，单声道PCM数据。

#### 术语表
关键词 | 描述
---- | ----
start silence | 从开始输入语音如果一直没有人在说话，则叫做start silence
speech detected| 从开始输入语音如果检测到有人说话，则叫做speech detected
end silence| 从检测到有人说话开始后的一段时间内，如果无人再说话，则叫做end silence

### 参数描述

#### 服务类型

Service | 描述
---- | ----
MOBVOI_SDS_VAD | 语音检测

#### 所支持的 Invoke 命令意图及参数

Intent | 描述
---- | ----
MOBVOI_SDS_SET_PARAM | 设置语音检测相关参数
MOBVOI_SDS_START| 开启语音检测服务
MOBVOI_SDS_FEED_SPEECH| 发送语音数据给语音检测服务
MOBVOI_SDS_STOP|停止语音检测服务

##### MOBVOI_SDS_SET_PARAM 参数：

Param | 类型 | 描述 | 是否必选
---- | ---- | ----| ----
MOBVOI_SDS_CALLBACK | CallBackBase | 设置语音检测回调 | 是
MOBVOI_SDS_VAD_TYPE | String | 设置语音检测类型。<br>可选值有"DNN"和"Energy"。<br>默认值为"DNN" | 否
MOBVOI_SDS_VAD_START_SILENCE | int | 设置语音检测start silence阈值。<br>单位为毫秒。<br>默认值为5000 | 否
MOBVOI_SDS_VAD_END_SILENCE | int | 设置语音检测end silence阈值。<br>单位为毫秒。<br>默认值为600 | 否
MOBVOI_SDS_VAD_SPEECH_THRESHOLD | int | 设置语音检测speech detected阈值。<br>单位为毫秒。<br>默认值为60 | 否

##### MOBVOI_SDS_START 参数：

无

##### MOBVOI_SDS_FEED_SPEECH 参数：
Param | 类型 | 描述 | 必选
---- | --- | ---| ---
MOBVOI_SDS_AUDIO_BUF|Buf|设置语音Buf | 是

##### MOBVOI_SDS_STOP 参数：

无

### 服务调用流程

#### 1 获取服务

```java
// SpeechSDS sds;

Service vad = sds.getService(MobvoiSDSConstants.MOBVOI_SDS_VAD);
```

#### 2 调用服务

##### 设置语音检测相关参数
```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_SET_PARAM);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_CALLBACK, eventHandler);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_VAD_TYPE, "DNN");
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_VAD_START_SILENCE, 4000);
params.setParam(MobvoiSDSConstants.MOBVOI_SDS_VAD_END_SILENCE, 700);

Parameter ret = vad.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

其中，传递给 eventHandler 的，是如下的回调消息：

Param | 类型 | 描述 | 是否必选
---- | ---- | ----| ----
MOBVOI_SDS_CB_TYPE | String | 指示回调的类型 | 是

例如
```java
private class VadEventHandler extends CallBackBase {
  @Override
  public void CallBack(Parameter parameter) {
    String cbType = parameter.getParam(MobvoiSDSConstants.MOBVOI_SDS_CB_TYPE).asString();
    if (cbType.equals(MobvoiSDSConstants. MOBVOI_SDS_CB_START_SILENCE)) {
      Log.i(TAG, "start silence detected")
    } else if (cbType.equals(MobvoiSDSConstants.MOBVOI_SDS_CB_END_SILENCE)) {
      Log.i(TAG, "end silence detected")
    } eles if (cbType.equals(MobvoiSDSConstants.MOBVOI_SDS_CB_SPEECH)) {
      Log.i(TAG, "speech detected")
    }
  }
}
```

##### 开启语音检测服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_START);

Parameter ret = vad.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 发送语音数据给语音检测服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_FEED_SPEECH);

// Must use direct buffer.
// speech size: speech buffer length, recommended value is 320 (10ms long).
ByteBuffer buffer = ByteBuffer.allocateDirect(<speech size>);
Buf audioBuf = new Buf(buffer, <speech size>);

params.setParam(MobvoiSDSConstants.MOBVOI_SDS_AUDIO_BUF, audioBuf);

Parameter ret = vad.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

##### 停止语音检测服务

```java
Parameter params = new Parameter(MobvoiSDSConstants.MOBVOI_SDS_STOP);

Parameter ret = vad.invoke(params);

if (ret.getParam(MobvoiSDSConstants.MOBVOI_SDS_ERROR_CODE).asInt() != MobvoiSDSConstants.MOBVOI_SDS_SUCCESS) {
  // Error handling code goes here
}
```

#### 3 释放服务

```java
sds.releaseService(vad);
```

---

[//]: # (05-sds-services-end)
