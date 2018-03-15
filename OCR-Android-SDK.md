
# OCR Android SDK 开发者文档

# 简介

本文档主要介绍OCR Android SDK的安装和使用。在使用本文档前，您需要先了解Optical Character Recognition(OCR)的基础知识，并已经开通了OCR服务。

## 接口能力

### 远程API能力

SDK提供了下列百度AI开放平台RESTful接口的封装

| 接口名称            | 接口能力简要描述                                     |
| :-------------- | :-------------------------------------------------- |
| 通用文字识别          | 识别图片中的文字信息                               |
| 通用文字识别（高精度版）    | 更高精度地识别图片中的文字信息                          |
| 通用文字识别（含位置信息版）  | 识别图片中的文字信息（包含文字区域的坐标信息）                  |
| 通用文字识别（高精度含位置版） | 更高精度地识别图片中的文字信息（包含文字区域的坐标信息）             |
| 通用文字识别（含生僻字版）   | 识别图片中的文字信息（包含对常见字和生僻字的识别）                |
| 网络图片文字识别        | 识别一些网络上背景复杂，特殊字体的文字                      |
| 身份证识别           | 识别身份证正反面的文字信息                            |
| 银行卡识别           | 识别银行卡的卡号并返回发卡行和卡片性质信息                    |
| 驾驶证识别           | 识别机动车驾驶证所有关键字段                           |
| 行驶证识别           | 识别机动车行驶证所有关键字段                           |
| 车牌识别            | 对小客车的车牌进行识别                              |
| 营业执照识别          | 对营业执照进行识别                                |
| 通用票据识别          | 对各类票据图片（医疗票据，保险保单等）进行文字识别，并返回文字在图片中的位置信息 |

### 本地质量控制能力

除了包含远程API调用能力外，安卓SDK中还集成了身份证识别的本地质量控制能力，提供给开发者本地检测身份证的功能。


## 版本更新记录

| 上线日期      | 版本号   | 更新内容                    |
| --------- | ----- | ----------------------- |
| 2017.2.8 | 1.4.2 | 修复高精度通用文字识别调用api错误的问题|
| 2017.2.1 | 1.4.1 | 优化和修复了一些引起奔溃的问题；身份证本地扫描新增一个用户手动加和释放模型的类，强烈推荐用户参照demo中手动初始化和释放模型|
| 2017.11.23 | 1.4.0 | 新增高精度版通用文字，营业执照，通用票据接口|
| 2017.11.2 | 1.3.3 | 修复一个本地代码内存泄露问题，优化代码结构|
| 2017.10.17 | 1.3.2 | 修复token对象expireTime时间异常的问题|
| 2017.9.21 | 1.3.1 | 修复了一些机型下autofocus fail的错误；添加了请求接口token过期前10秒自动获取新token的逻辑；对demo界面文案做了微调|
| 2017.8.15 | 1.3.0 | 增加驾驶证，行驶证，车牌识别功能；修复了一个潜在内存泄露问题；身份证本地质量控制模型升级，加入完整性保证|
| 2017.8.1 | 1.2.3 | ui库输出格式RGB565压缩，身份证识别参数加入压缩质量，对焦实现改为间隔自动对焦，修复了一些问题|
| 2017.7.14 | 1.2.2 | 配合添加身份证本地能力升级SDK的安全性，身份证识别支持自动质量控制扫描模式以及默认的拍照识别模式|
| 2017.6.30 | 1.2.1 |1.对SDK的安全性作出优化 2.对本地身份证输入校验功能进行升级，该功能暂时不可用|
| 2017.6.20 | 1.2.0 | ocr_ui库身份证识别升级，交互修改为基于本地模型实现实时扫描判断后自动上传识别身份证|
| 2017.5.18 | 1.1.0 | 增加通用文字识别基础版，生僻字，网图接口的SDK接口和demo演示；移除okhttp依赖；支持x86架构CPU；略微优化了demo的交互|
| 2017.4.13 | 1.0.2 | 修复部分用户使用ak，sk方式无法获取token的问题 |
| 2017.3.23 | 1.0.1 | 更新demo获取token失败的错误提示的交互 |
| 2017.3.16 | 1.0.0 | 在线OCR第一版！               |

# 快速入门

支持的系统和硬件版本

- 系统：支持 Android 4.0（API Level 15）到Android7.0（API Level 25）系统。需要开发者通过minSdkVersion来保证支持系统的检测。
- CPU架构：armeabi，arm64-v8a，armeabi-v7a，x86
- 机型：手机和平板皆可
- 硬件要求：要求设备上有相机模块。
- 网络：支持WIFI及移动网络，移动网络支持使用NET网关及WAP网关（CMWAP、CTWAP、UNIWAP、3GWAP）。

## 开发包说明

    aip-ocr-android-sdk.zip               // OCR SDK包，包括文档，demo工程，SDK核心库
    	|- OCRDemo                         // demo示例工程
        |- libs                            // lib 库,包括各平台的so库及 jar包。
        |- ocr-ui                          // ocr UI模块
        |- OCR-android-SDK.md              // 使用说明文档

sdk的包含的UI部分和demo工程以Android Studio方式提供，sdk部分则可以较方便的集成到eclipse工程中。

 1. 前往[SDK下载页面](http://ai.baidu.com/sdk/#ocr)下载Android SDK压缩包。
 2. (必须)将下载包libs目录中的ocr-sdk.jar文件拷贝到工程libs目录中，并加入工程依赖。
 3. (必须)将libs目录下armeabi，arm64-v8a，armeabi-v7a，x86文件夹按需添加到android studio工程`src/main/jniLibs`目录中， eclipse用户默认为libs目录。
 4. (可选)如果需要使用UI模块，请在Android studio中以模块方式导入下载包中的ocr-ui文件夹。
 
### 为您自己的工程添加必要的权限

如果您在自己的工程中集成SDK，请确保已经在工程AndroidManifest.xml文件中添加如下权限：
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```
各个权限的用途说明见下表：

| 名称                     | 用途                      |
| ---------------------- | ----------------------- |
| INTERNET               | 应用联网，发送请求数据至服务器，获得识别结果。 |
| CAMERA                 | 调用相机进行拍照（仅UI部分需要）       |
| WRITE_EXTERNAL_STORAGE | 图片裁剪临时存储                |
| READ_EXTERNAL_STORAGE  | 图片裁剪临时存储                |

### Proguard配置

如果您在自己的工程中集成SDK，请在Proguard配置文件中增加, 防止release发布时打包报错：

```
-keep class com.baidu.ocr.sdk.**{*;}
-dontwarn com.baidu.ocr.**
```

## DEMO使用说明

安卓SDK包中提供了一个可快速运行的demo工程，该工程已经集成了sdk，UI库，您只需直接在Android Studio中导入开发包OCRDemo目录即可运行。

若运行提示"身份验证错误"，可能是您还未填写正确的Api Key和Secret Key，或者是还未绑定您安卓应用的包名，如何绑定包名请参考下一章节：***身份验证与安全*** 章节

![](https://ai.bdstatic.com/file/22808038CABB4D87A523E00A63F86A3F)

## 身份验证与安全

百度AI开放平台使用OAuth2.0授权调用开放API，调用API时必须在URL中带上accesss_token参数。AccessToken可用AK/SK或者授权文件的方式获得。安卓SDK中已经为您做了封装，当初始化完毕后，所有API请求会自动带上accesss_token参数，您也可以通过initAccessTokenWithAkSk，initAccessToken这两个函数的回调中查看。

OCR Android SDK提供了以下2种AccessToken管理方法.

### API Key / Secret Key

此种身份验证方案使用AK/SK获得AccessToken。

虽然SDK对网络传输的敏感数据进行了二次加密，但由于AK/SK是明文填写在代码中，在移动设备中可能会存在AK/SK被盗取的风险。有安全考虑的开发者可使用第二种授权方案。

使用步骤：

1. 在[管理控制台](https://console.bce.baidu.com/ai/?fromai=1&_=1488766023093#/ai/ocr/app/list)中新建一个OCR应用，并且请填写正确的包名
![](https://ai.bdstatic.com/file/E0FE42DB27494CBC895C6F24DBC1FE54)
![](https://ai.bdstatic.com/file/36B5703778884B73AE6E9241730B1772)
2. 在***应用详情***页面查看并复制应用的Api Key（简称AK） 和 Secret Key（简称SK），初始化`OCR`单例：

```
OCR.getInstance().initAccessTokenWithAkSk(new OnResultListener<AccessToken>() {
    @Override
    public void onResult(AccessToken result) {
        // 调用成功，返回AccessToken对象
        String token = result.getAccessToken();
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError子类SDKError对象
    }
}, getApplicationContext(), "您的应用AK", "您的应用SK");
```

由于AK/SK是明文填写在代码中，在移动设备中可能会存在AK/SK被盗取的风险。有安全考虑的开发者可使用第二种授权方案。

### 授权文件（安全模式）

此种身份验证方案使用授权文件获得AccessToken，缓存在本地。***建议有安全考虑的开发者使用此种身份验证方式。***

在您的移动APP分发出去之后，APP存在被反编译的可能，所以直接将AK / SK 置于APP源码之中，存在被盗取的风险。采用授权文件的身份验证方法，可有效保护AK/SK在移动设备中的安全。攻击者即使拦截了流量，盗取了授权文件，也难以盗用您的配额。

使用步骤：

1. 在[官网](https://console.bce.baidu.com/ai/?fromai=1&_=1488766023093#/ai/ocr/app/list)中配置应用
![](https://ai.bdstatic.com/file/E0FE42DB27494CBC895C6F24DBC1FE54)
![](https://ai.bdstatic.com/file/36B5703778884B73AE6E9241730B1772)
2. 在***应用详情***页面下载对应应用的授权文件
![](https://ai.bdstatic.com/file/6E928A2EBAE744E59D8D0CE2984AAC57)
3. 将授权文件添加至工程assets文件夹，文件名必须为`aip.license`
![](https://ai.bdstatic.com/file/54D522AC76AA44B9BBE6E98FEEAD79EE)
4. 调用initAccessToken方法，初始化OCR单例：

```
OCR.getInstance().initAccessToken(new OnResultListener<AccessToken>() {
    @Override
    public void onResult(AccessToken result) {
        // 调用成功，返回AccessToken对象
        String token = result.getAccessToken();
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError子类SDKError对象
    }
}, getApplicationContext());
```

# 接口调用说明

## OCR-UI模块

OCR-UI模块提供了一套默认的UI。如需使用，请将ocr_ui模块包含到您的工程，具体使用可参考SDK包中附带的示例工程。

### OCR-UI模块调用示例

调用拍摄activity，更详细的类别请参考demo工程

```
// 生成intent对象
Intent intent = new Intent(IDCardActivity.this, CameraActivity.class);

// 设置临时存储
intent.putExtra(CameraActivity.KEY_OUTPUT_FILE_PATH,                     FileUtil.getSaveFile(getApplication()).getAbsolutePath());

// 调用除银行卡，身份证等识别的activity
intent.putExtra(CameraActivity.KEY_CONTENT_TYPE, CameraActivity.CONTENT_TYPE_GENERAL);
startActivityForResult(intent, REQUEST_CODE_CAMERA);
// 通过参数确定接口类型
startActivityForResult(intent, REQUEST_CODE_GENERAL_BASIC);

// 调用拍摄银行卡的activity
intent.putExtra(CameraActivity.KEY_CONTENT_TYPE, CameraActivity.CONTENT_TYPE_BANK_CARD);
startActivityForResult(intent, REQUEST_CODE_CAMERA);

// 调用拍摄身份证正面（不带本地质量控制）activity
intent.putExtra(CameraActivity.KEY_CONTENT_TYPE, CameraActivity.CONTENT_TYPE_ID_CARD_FRONT);
startActivityForResult(intent, REQUEST_CODE_CAMERA);

// 调用身份证本识别（带本地质量控制）activity
 Intent intent = new Intent(IDCardActivity.this, CameraActivity.class);
// 使用本地质量控制能力需要授权，需要在OCR调用initAccessToken或者
// initAccessTokenWithAkSk成功返回后才能获取License授权本地质量控制能力
intent.putExtra(CameraActivity.KEY_NATIVE_TOKEN,
                        OCR.getInstance().getLicense());
// 使用本地质量控制能力需要设置开启
intent.putExtra(CameraActivity.KEY_NATIVE_ENABLE,
                        true);
// 开启身份证正面本地识别                    
intent.putExtra(CameraActivity.KEY_CONTENT_TYPE, CameraActivity.CONTENT_TYPE_ID_CARD_FRONT);

```



通过onActivityResult获取拍摄结果，更详细的类别请参考demo工程

```
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // 获取调用参数
    String contentType = data.getStringExtra(CameraActivity.KEY_CONTENT_TYPE);
    // 通过临时文件获取拍摄的图片
    String filePath = FileUtil.getSaveFile(getApplicationContext()).getAbsolutePath();
    // 判断拍摄类型（通用，身份证，银行卡等）
    if (requestCode == REQUEST_CODE_GENERAL && resultCode == Activity.RESULT_OK) {
    // 判断是否是身份证正面
        if (CameraActivity.CONTENT_TYPE_ID_CARD_FRONT.equals(contentType)){
        // 获取图片文件调用sdk数据接口，见数据接口说明
        }
    }
}
```


## 数据接口

### 通用文字识别

* 调用示例

```
// 通用文字识别参数设置
GeneralBasicParams param = new GeneralBasicParams();
param.setDetectDirection(true);
param.setImageFile(new File(filePath));

// 调用通用文字识别服务
OCR.getInstance().recognizeGeneralBasic(param, new OnResultListener<GeneralResult>() {
    @Override
    public void onResult(GeneralResult result) {
        // 调用成功，返回GeneralResult对象
        for (WordSimple wordSimple : result.getWordList()) {
            // wordSimple不包含位置信息
            wordSimple word = wordSimple;
            sb.append(word.getWords());
            sb.append("\n");
        }
        // json格式返回字符串
        listener.onResult(result.getJsonRes());
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数详情**

| 参数                    | 是否必选  | 类型      | 可选值范围                                   | 说明                                       |
| --------------------- | ----- | ------- | --------------------------------------- | ---------------------------------------- |
| image                 | true  | string  | -                                       | 图像数据，base64编码，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |
| language_type         | false | string  | CHN_ENG、ENG、POR、FRE、GER、ITA、SPA、RUS、JAP | 识别语言类型，默认为CHN_ENG。可选值包括：<br/>- CHN_ENG：中英文混合；<br/>- ENG：英文；<br/>- POR：葡萄牙语；<br/>- FRE：法语；<br/>- GER：德语；<br/>- ITA：意大利语；<br/>- SPA：西班牙语；<br/>- RUS：俄语；<br/>- JAP：日语 |
| detect_direction      | false | boolean | true、false                              | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括:<br/>- true：检测朝向；<br/>- false：不检测朝向。 |
| detect_language       | FALSE | string  | true、false                              | 是否检测语言，默认不检测。当前支持（中文、英语、日语、韩语）           |


* 结果返回

| 字段                 | 必选   | 类型      | 说明                                       |
| ------------------ | ---- | ------- | ---------------------------------------- |
| direction          | 否    | int32   | 图像方向，当detect_direction=true时存在。<br/>- -1:未定义，<br/>- 0:正向，<br/>- 1: 逆时针90度，<br/>- 2:逆时针180度，<br/>- 3:逆时针270度 |
| log_id             | 是    | uint64  | 唯一的log id，用于问题定位                         |
| words_result_num   | 是    | uint32  | 识别结果数，表示words_result的元素个数                |
| words_result       | 是    | array() | 定位和识别结果数组                                |
| +words             | 否    | string  | 识别结果字符串                                  |

```json
// 示例
返回格式参考通用文字识别
```

### 通用文字识别（高精度版）

* 调用示例

```
// 通用文字识别参数设置
GeneralBasicParams param = new GeneralBasicParams();
param.setDetectDirection(true);
param.setImageFile(new File(filePath));

// 调用通用文字识别服务
OCR.getInstance().recognizeAccurateBasic(param, new OnResultListener<GeneralResult>() {
    @Override
    public void onResult(GeneralResult result) {
        // 调用成功，返回GeneralResult对象
        for (WordSimple wordSimple : result.getWordList()) {
            // wordSimple不包含位置信息
            wordSimple word = wordSimple;
            sb.append(word.getWords());
            sb.append("\n");
        }
        // json格式返回字符串
        listener.onResult(result.getJsonRes());
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数详情**

| 参数                    | 是否必选  | 类型      | 可选值范围                                   | 说明                                       |
| --------------------- | ----- | ------- | --------------------------------------- | ---------------------------------------- |
| image                 | true  | string  | -                                       | 图像数据，base64编码，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |
| detect_direction      | false | boolean | true、false                              | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括:<br/>- true：检测朝向；<br/>- false：不检测朝向。 |


* 结果返回

| 字段                 | 必选   | 类型      | 说明                                       |
| ------------------ | ---- | ------- | ---------------------------------------- |
| direction          | 否    | int32   | 图像方向，当detect_direction=true时存在。<br/>- -1:未定义，<br/>- 0:正向，<br/>- 1: 逆时针90度，<br/>- 2:逆时针180度，<br/>- 3:逆时针270度 |
| log_id             | 是    | uint64  | 唯一的log id，用于问题定位                         |
| words_result_num   | 是    | uint32  | 识别结果数，表示words_result的元素个数                |
| words_result       | 是    | array() | 定位和识别结果数组                                |
| +words             | 否    | string  | 识别结果字符串                                  |

```json
// 示例
{
"log_id": 2471272194, 
"words_result_num": 2,
"words_result": 
    [
        {"words": " TSINGTAO"}, 
        {"words": "青島睥酒"}
    ]
}
```

### 通用文字识别（含位置信息版）

* 调用示例

```
// 通用文字识别参数设置
GeneralParams param = new GeneralParams();
param.setDetectDirection(true);
param.setImageFile(new File(filePath));

// 调用通用文字识别服务（含位置信息版）
OCR.getInstance().recognizeGeneral(param, new OnResultListener<GeneralResult>() {
    @Override
    public void onResult(GeneralResult result) {
        StringBuilder sb = new StringBuilder();
            for (WordSimple wordSimple : result.getWordList()) {
                // word包含位置
                Word word = (Word) wordSimple;
                    sb.append(word.getWords());
                sb.append("\n");
            }
        // 调用成功，返回GeneralResult对象，通过getJsonRes方法获取API返回字符串
        listener.onResult(result.getJsonRes());
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数详情**

| 参数                    | 是否必选  | 类型      | 可选值范围                                   | 说明                                       |
| --------------------- | ----- | ------- | --------------------------------------- | ---------------------------------------- |
| image                 | true  | string  | -                                       | 图像数据，base64编码，要求base64编码后大小不超过1M，最短边至少15px，最长边最大2048px,支持jpg/png/bmp格式 |
| recognize_granularity | false | string  | big、small                               | 是否定位单字符位置，big：不定位单字符位置，默认值；small：定位单字符位置 |
| language_type         | false | string  | CHN_ENG、ENG、POR、FRE、GER、ITA、SPA、RUS、JAP | 识别语言类型，默认为CHN_ENG。可选值包括：<br/>- CHN_ENG：中英文混合；<br/>- ENG：英文；<br/>- POR：葡萄牙语；<br/>- FRE：法语；<br/>- GER：德语；<br/>- ITA：意大利语；<br/>- SPA：西班牙语；<br/>- RUS：俄语；<br/>- JAP：日语 |
| detect_direction      | false | boolean | true、false                              | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括:<br/>- true：检测朝向；<br/>- false：不检测朝向。 |
| detect_language       | false | string  | true、false                              | 是否检测语言，默认不检测。当前支持（中文、英语、日语、韩语）           |
| vertexes_location     | false | string  | true、false                              | 是否返回文字外接多边形顶点位置，不支持单字位置。默认为false         |


* 结果返回

| 字段                 | 必选   | 类型      | 说明                                       |
| ------------------ | ---- | ------- | ---------------------------------------- |
| direction          | 否    | int32   | 图像方向，当detect_direction=true时存在。<br/>- -1:未定义，<br/>- 0:正向，<br/>- 1: 逆时针90度，<br/>- 2:逆时针180度，<br/>- 3:逆时针270度 |
| log_id             | 是    | uint64  | 唯一的log id，用于问题定位                         |
| words_result       | 是    | array() | 定位和识别结果数组                                |
| words_result_num   | 是    | uint32  | 识别结果数，表示words_result的元素个数                |
| +vertexes_location | 否    | array() | 当前为四个顶点: 左上，右上，右下，左下。当vertexes_location=true时存在 |
| ++x                | 是    | uint32  | 水平坐标（坐标0点为左上角）                           |
| ++y                | 是    | uint32  | 垂直坐标（坐标0点为左上角）                           |
| +location          | 是    | array() | 位置数组（坐标0点为左上角）                           |
| ++left             | 是    | uint32  | 表示定位位置的长方形左上顶点的水平坐标                      |
| ++top              | 是    | uint32  | 表示定位位置的长方形左上顶点的垂直坐标                      |
| ++width            | 是    | uint32  | 表示定位位置的长方形的宽度                            |
| ++height           | 是    | uint32  | 表示定位位置的长方形的高度                            |
| +words             | 否    | string  | 识别结果字符串                                  |
| +chars             | 否    | array() | 单字符结果，recognize_granularity=small时存在     |
| ++location         | 是    | array() | 位置数组（坐标0点为左上角）                           |
| +++left            | 是    | uint32  | 表示定位位置的长方形左上顶点的水平坐标                      |
| +++top             | 是    | uint32  | 表示定位位置的长方形左上顶点的垂直坐标                      |
| +++width           | 是    | uint32  | 表示定位定位位置的长方形的宽度                          |
| +++height          | 是    | uint32  | 表示位置的长方形的高度                              |
| ++char             | 是    | string  | 单字符识别结果                                  |

```json
// 示例
{
    direction : 2,
    log_id : 676709620,
    words_result : [ {
            location : {
                height : 20;
                left : 86;
                top : 387;
                width : 22;
            };
            words : "N";
        },
    ],
    words_result_num : 1;
}
```

### 通用文字识别（高精度含位置信息版）

* 调用示例

```
// 通用文字识别参数设置
GeneralParams param = new GeneralParams();
param.setDetectDirection(true);
param.setImageFile(new File(filePath));

// 调用通用文字识别服务（含位置信息版）
OCR.getInstance().recognizeAccurate(param, new OnResultListener<GeneralResult>() {
    @Override
    public void onResult(GeneralResult result) {
        StringBuilder sb = new StringBuilder();
            for (WordSimple wordSimple : result.getWordList()) {
                // word包含位置
                Word word = (Word) wordSimple;
                    sb.append(word.getWords());
                sb.append("\n");
            }
        // 调用成功，返回GeneralResult对象，通过getJsonRes方法获取API返回字符串
        listener.onResult(result.getJsonRes());
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数详情**

| 参数                    | 是否必选  | 类型      | 可选值范围                                   | 说明                                       |
| --------------------- | ----- | ------- | --------------------------------------- | ---------------------------------------- |
| image                 | true  | string  | -                                       | 图像数据，base64编码，要求base64编码后大小不超过1M，最短边至少15px，最长边最大2048px,支持jpg/png/bmp格式 |
| vertexes_location     | false | string  | true、false | 是否返回文字外接多边形顶点位置，不支持单字位置。默认为false         |
| recognize_granularity | false | string  | big、small                               | 是否定位单字符位置，big：不定位单字符位置，默认值；small：定位单字符位置 |
| detect_direction      | false | boolean | true、false                              | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括:<br/>- true：检测朝向；<br/>- false：不检测朝向。 |

* 结果返回

| 字段                 | 必选   | 类型      | 说明                                       |
| ------------------ | ---- | ------- | ---------------------------------------- |
| direction          | 否    | int32   | 图像方向，当detect_direction=true时存在。<br/>- -1:未定义，<br/>- 0:正向，<br/>- 1: 逆时针90度，<br/>- 2:逆时针180度，<br/>- 3:逆时针270度 |
| log_id             | 是    | uint64  | 唯一的log id，用于问题定位                         |
| words_result       | 是    | array() | 定位和识别结果数组                                |
| words_result_num   | 是    | uint32  | 识别结果数，表示words_result的元素个数                |
| +vertexes_location | 否    | array() | 当前为四个顶点: 左上，右上，右下，左下。当vertexes_location=true时存在 |
| ++x                | 是    | uint32  | 水平坐标（坐标0点为左上角）                           |
| ++y                | 是    | uint32  | 垂直坐标（坐标0点为左上角）                           |
| +location          | 是    | array() | 位置数组（坐标0点为左上角）                           |
| ++left             | 是    | uint32  | 表示定位位置的长方形左上顶点的水平坐标                      |
| ++top              | 是    | uint32  | 表示定位位置的长方形左上顶点的垂直坐标                      |
| ++width            | 是    | uint32  | 表示定位位置的长方形的宽度                            |
| ++height           | 是    | uint32  | 表示定位位置的长方形的高度                            |
| +words             | 否    | string  | 识别结果字符串                                  |
| +chars             | 否    | array() | 单字符结果，recognize_granularity=small时存在     |
| ++location         | 是    | array() | 位置数组（坐标0点为左上角）                           |
| +++left            | 是    | uint32  | 表示定位位置的长方形左上顶点的水平坐标                      |
| +++top             | 是    | uint32  | 表示定位位置的长方形左上顶点的垂直坐标                      |
| +++width           | 是    | uint32  | 表示定位定位位置的长方形的宽度                          |
| +++height          | 是    | uint32  | 表示位置的长方形的高度                              |
| ++char             | 是    | string  | 单字符识别结果                                  |

```json
// 返回结果参考通用文字识别（含位置信息版）
```

### 通用文字识别(含生僻字版)

* 调用示例

```
// 通用文字识别(含生僻字版)参数设置
GeneralBasicParams param = new GeneralBasicParams();
param.setDetectDirection(true);
param.setImageFile(new File(filePath));

// 调用通用文字识别服务
OCR.getInstance().recognizeGeneralEnhanced(param, new OnResultListener<GeneralResult>() {
    @Override
    public void onResult(GeneralResult result) {
        // 调用成功，返回GeneralResult对象
        for (WordSimple wordSimple : result.getWordList()) {
            // wordSimple不包含位置信息
            wordSimple word = wordSimple;
            sb.append(word.getWords());
            sb.append("\n");
        }
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数详情**

| 参数                    | 是否必选  | 类型      | 可选值范围                                   | 说明                                       |
| --------------------- | ----- | ------- | --------------------------------------- | ---------------------------------------- |
| image                 | true  | string  | -                                       | 图像数据，base64编码，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |
| language_type         | false | string  | CHN_ENG、ENG、POR、FRE、GER、ITA、SPA、RUS、JAP | 识别语言类型，默认为CHN_ENG。可选值包括：<br/>- CHN_ENG：中英文混合；<br/>- ENG：英文；<br/>- POR：葡萄牙语；<br/>- FRE：法语；<br/>- GER：德语；<br/>- ITA：意大利语；<br/>- SPA：西班牙语；<br/>- RUS：俄语；<br/>- JAP：日语 |
| detect_direction      | false | boolean | true、false                              | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括:<br/>- true：检测朝向；<br/>- false：不检测朝向。 |
| detect_language       | FALSE | string  | true、false                              | 是否检测语言，默认不检测。当前支持（中文、英语、日语、韩语）           |



* 结果返回

| 字段                 | 必选   | 类型      | 说明                                       |
| ------------------ | ---- | ------- | ---------------------------------------- |
| direction          | 否    | int32   | 图像方向，当detect_direction=true时存在。<br/>- -1:未定义，<br/>- 0:正向，<br/>- 1: 逆时针90度，<br/>- 2:逆时针180度，<br/>- 3:逆时针270度 |
| log_id             | 是    | uint64  | 唯一的log id，用于问题定位                         |
| words_result_num   | 是    | uint32  | 识别结果数，表示words_result的元素个数                |
| words_result       | 是    | array() | 定位和识别结果数组                                |
| +words             | 否    | string  | 识别结果字符串                                  |


```json
// 示例
参考通用文字识别接口
```

### 网络图片文字识别

* 调用示例

```
// 网络图片文字识别参数设置
GeneralBasicParams param = new GeneralBasicParams();
param.setDetectDirection(true);
param.setImageFile(new File(filePath));

// 调用通用文字识别服务
OCR.getInstance().recognizeWebimage(param, new OnResultListener<GeneralResult>() {
    @Override
    public void onResult(GeneralResult result) {
        // 调用成功，返回GeneralResult对象
        for (WordSimple wordSimple : result.getWordList()) {
            // wordSimple不包含位置信息
            wordSimple word = wordSimple;
            sb.append(word.getWords());
            sb.append("\n");
        }
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数详情**

| 参数                    | 是否必选  | 类型      | 可选值范围                                   | 说明                                       |
| --------------------- | ----- | ------- | --------------------------------------- | ---------------------------------------- |
| image                 | true  | string  | -                                       | 图像数据，base64编码，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |
| detect_direction      | false | boolean | true、false                              | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括:<br/>- true：检测朝向；<br/>- false：不检测朝向。 |
| detect_language       | false | string  | true、false                              | 是否检测语言，默认不检测。当前支持（中文、英语、日语、韩语）           |



* 结果返回

| 字段                 | 必选   | 类型      | 说明                                       |
| ------------------ | ---- | ------- | ---------------------------------------- |
| direction          | 否    | int32   | 图像方向，当detect_direction=true时存在。<br/>- -1:未定义，<br/>- 0:正向，<br/>- 1: 逆时针90度，<br/>- 2:逆时针180度，<br/>- 3:逆时针270度 |
| log_id             | 是    | uint64  | 唯一的log id，用于问题定位                         |
| words_result_num   | 是    | uint32  | 识别结果数，表示words_result的元素个数                |
| words_result       | 是    | array() | 定位和识别结果数组                                |
| +words             | 否    | string  | 识别结果字符串                                  |

```json
// 示例
参考通用文字识别接口
```

### 银行卡识别

* 调用示例

```
// 银行卡识别参数设置
BankCardParams param = new BankCardParams();
param.setImageFile(new File(filePath));

// 调用银行卡识别服务
OCR.getInstance().recognizeBankCard(param, new OnResultListener<BankCardResult>() {
    @Override
    public void onResult(BankCardResult result) {
        // 调用成功，返回BankCardResult对象
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

* 结果返回

| 参数                 | 类型     | 描述                           |
| :----------------- | :----- | :--------------------------- |
| log_id             | Uint64 | 唯一的log id，用于问题定位             |
| result             | Object | 定位和识别结果数组                    |
| \+bank_card_number | String | 银行卡识别结果                      |
| \+bank_name        | String | 银行名，不能识别时为空                  |
| \+bank_card_type   | uint32 | 银行卡类型，0:不能识别; 1: 借记卡; 2: 信用卡 |

```json
 // 示例
 {
    "log_id": 3207866271;
    result: {
        "bank_card_number": "6226 2288 8888 8888",
        "bank_card_type": 1,
        "bank_name": "\U5de5\U5546\U94f6\U884c"
    };
}
```

### 身份证识别

* 调用示例

```
// 身份证识别参数设置
IDCardParams param = new IDCardParams();
param.setImageFile(new File(filePath));

// 调用身份证识别服务
OCR.getInstance().recognizeIDCard(param, new OnResultListener<IDCardResult>() {
    @Override
    public void onResult(IDCardResult result) {
        // 调用成功，返回IDCardResult对象
    }
    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数**

| 参数               | 必选    | 范围               | 类型      | 说明                                       |
| ---------------- | ----- | ---------------- | ------- | ---------------------------------------- |
| image            | true  |                  | String  | 图像数据，支持本地图像文件路径，图像文件二进制数据，要求base64编码后大小不超过1M，最短边至少15px，最长边最大2048px,支持jpg/png/bmp格式 |
| isFront          | true  | true、false       | Boolean | true：身份证正面，false：身份证背面                   |
| detect_direction | false | true、false       | string  | 是否检测图像朝向，默认不检测，即：false。可选值为：true - 检测图像朝向；false - 不检测图像朝向。朝向是指输入图像是正常方向、逆时针旋转90/180/270度 |
| accuracy         | false | auto、normal、high | string  | 精准度，精度越高，速度越慢。default：auto               |


* 结果返回

| 参数               | 类型     | 描述                                       |
| :--------------- | :----- | :--------------------------------------- |
| direction        | Int32  | 图像方向，当detect_direction=true时存在。-1:未定义，0:正向，1: 逆时针90度， 2:逆时针180度， 3:逆时针270度 |
| log_id           | Uint64 | 唯一的log id，用于问题定位                         |
| words_result     | Array  | 定位和识别结果数组，数组元素的key是身份证的主体字段（正面支持：住址、公民身份号码、出生、姓名、性别、民族，背面支持：签发机关、签发日期、失效日期）。只返回识别出的字段。若身份证号码校验不通过，则不返回 |
| words_result_num | Uint32 | 识别结果数，表示words_result的元素个数                |
| \+location       | Array  | 位置数组（坐标0点为左上角）                           |
| \+\+left         | Uint32 | 表示定位位置的长方形左上顶点的水平坐标                      |
| \+\+top          | Uint32 | 表示定位位置的长方形左上顶点的垂直坐标                      |
| \+\+width        | Uint32 | 表示定位位置的长方形的宽度                            |
| \+\+height       | Uint32 | 表示定位位置的长方形的高度                            |
| \+words          | String | 识别结果字符串                                  |

```json
//示例
{
    "log_id": 7037721,
    "direction": 0,
    "words_result_num": 2,
    "words_result": {
        "住址": {
            "location": {
                "left": 227,
                "top": 235,
                "width": 229,
                "height": 51
            },
            "words": "湖北省天门市渔薪镇杨咀村一组2号",
        }
        ...
    }
}

```

### 行驶证识别

* 调用示例

```
// 行驶证识别参数设置
OcrRequestParams param = new OcrRequestParams();

// 设置image参数
param.setImageFile(new File(filePath));
// 设置其他参数
param.putParam("detect_direction", true);
// 调用行驶证识别服务
OCR.getInstance().recognizeVehicleLicense(param, new OnResultListener<OcrResponseResult>() {
    @Override
    public void onResult(OcrResponseResult result) {
        // 调用成功，返回OcrResponseResult对象
        listener.onResult(result.getJsonRes());
    }

    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数**

| 参数               | 必选  | 类型      | 可选值范围      | 说明                                       |
| ---------------- | ----- | ------- | ---------- | ---------------------------------------- |
| image            | 是  | File对象  | -          | 图像数据, 要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |
| detect_direction | 否 | boolean | true、false | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括:- true：检测朝向；- false：不检测朝向。 |
| accuracy         | 否  | string  | normal，缺省  | normal 使用快速服务，1200ms左右时延；缺省或其它值使用高精度服务，1600ms左右时延 |


* 结果返回

| 字段    | 说明         | 是否必选 | 类型                                |
| ----- | --------- | ---- | ---------------------------------- |
| log_id           | 是    | number  | 唯一的log id，用于问题定位          |
| words_result_num | 是    | number  | 识别结果数，表示words_result的元素个数 |
| words_result     | 是    | array | 识别结果数组                    |
| +words           | 否    | string  | 识别结果字符串                   |


```json
//示例
{
	"errno": 0,
	"msg": "success",
	"data": {
		"words_result_num": 10,
		"words_result": {
			"品牌型号": {
				"words": "保时捷GT37182RUCRE"
			},
			"发证日期": {
				"words": "20160104"
			},
			"使用性质": {
				"words": "非营运"
			},
			"发动机号码": {
				"words": "20832"
			},
			"号牌号码": {
				"words": "苏A001"
			},
			"所有人": {
				"words": "圆圆"
			},
			"住址": {
				"words": "南京市江宁区弘景大道"
			},
			"注册日期": {
				"words": "20160104"
			},
			"车辆识别代号": {
				"words": "HCE58"
			},
			"车辆类型": {
				"words": "小型轿车"
			}
		}
	}
}

```

### 驾驶证识别

* 调用示例

```
// 驾驶证识别参数设置
OcrRequestParams param = new OcrRequestParams();

// 设置image参数
param.setImageFile(new File(filePath));
// 设置其他参数
param.putParam("detect_direction", true);
// 调用驾驶证识别服务
OCR.getInstance().recognizeDrivingLicense(param, new OnResultListener<OcrResponseResult>() {
    @Override
    public void onResult(OcrResponseResult result) {
        // 调用成功，返回OcrResponseResult对象
        listener.onResult(result.getJsonRes());
    }

    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数**

| 参数               | 必选  | 类型      | 可选值范围      | 说明                                       |
| ---------------- | ----- | ------- | ---------- | ---------------------------------------- |
| image            | 是  | File对象  | -          | 图像数据，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |
| detect_direction | 否 | boolean | true、false | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括:- true：检测朝向；- false：不检测朝向。 |

* 结果返回

| 字段               | 必选 | 类型                                |
| ---- | ------------ | ---- | -------------------------------- |
| log_id           | 是    | number  | 唯一的log id，用于问题定位          |
| words_result_num | 是    | number  | 识别结果数，表示words_result的元素个数 |
| words_result     | 是    | array | 识别结果数组                    |
| +words           | 否    | string  | 识别结果字符串                   |


```json
//示例
{
	"errno": 0,
	"msg": "success",
	"data": {
		"words_result_num": 10,
		"words_result": {
			"证号": {
				"words": "3208231999053090"
			},
			"有效期限": {
				"words": "6年"
			},
			"准驾车型": {
				"words": "B2"
			},
			"有效起始日期": {
				"words": "20101125"
			},
			"住址": {
				"words": "江苏省南通市海门镇秀山新城"
			},
			"姓名": {
				"words": "小欧欧"
			},
			"国籍": {
				"words": "中国"
			},
			"出生日期": {
				"words": "19990530"
			},
			"性别": {
				"words": "男"
			},
			"初次领证日期": {
				"words": "20100125"
			}
		}
	}
}

```

### 车牌识别

* 调用示例

```
// 车牌识别参数设置
OcrRequestParams param = new OcrRequestParams();

// 设置image参数
param.setImageFile(new File(filePath));

// 调用车牌识别服务
OCR.getInstance().recognizeLicensePlate(param, new OnResultListener<OcrResponseResult>() {
    @Override
    public void onResult(OcrResponseResult result) {
        // 调用成功，返回OcrResponseResult对象
        listener.onResult(result.getJsonRes());
    }

    @Override
    public void onError(OCRError error) {
        // 调用失败，返回OCRError对象
    }
});
```

**options参数**

| 参数               | 必选  | 类型      | 可选值范围      | 说明                                       |
| ---------------- | ----- | ------- | ---------- | ---------------------------------------- |
| image            | 是  | File对象  | -          | 图像数据，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |

* 结果返回

|参数 | 是否必须 | 类型| 说明|
| --- | --- | ---| --- |
| log_id| 是 | number |请求标识码，随机数，唯一 |
| words_result| 是 | object | 暴恐结果置信度 |
| +color　|是 |　string | 车牌颜色，如"blue" |
| +number | 是 | string | 车牌号码，示例："苏HS7766" |


```json
//示例
{
  "words_result":{
     "color":"blue",
     "number":"粤FQ0000"
  },
  "log_id":2783673432
}

```

### 营业执照识别

* 调用示例

```
// 营业执照识别参数设置
OcrRequestParams param = new OcrRequestParams();

// 设置image参数
param.setImageFile(new File(filePath));

// 调用营业执照识别服务
OCR.getInstance().recognizeBusinessLicense(param, new OnResultListener<OcrResponseResult>() {
    @Override
    public void onResult(OcrResponseResult result) {
        listener.onResult(result.getJsonRes());
    }

    @Override
    public void onError(OCRError error) {
        listener.onResult(error.getMessage());
    }
});
```

**options参数**

| 参数               | 必选  | 类型      | 可选值范围      | 说明                                       |
| ---------------- | ----- | ------- | ---------- | ---------------------------------------- |
| image            | 是  | File对象  | -          | 图像数据，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |

* 结果返回

| 参数               | 是否必须    | 类型     | 说明                        |
| ---------------- | ------- | ------ | ------------------------- |
| log_id           | 是       | uint64 | 请求标识码，随机数，唯一。             |
| words_result_num | 是       | uint32 | 识别结果数，表示words_result的元素个数 |
| words_result     | array() | 识别结果数组 |                           |
| left             | 是       | uint32 | 表示定位位置的长方形左上顶点的水平坐标       |
| top              | 是       | uint32 | 表示定位位置的长方形左上顶点的垂直坐标       |
| width            | 是       | uint32 | 表示定位位置的长方形的宽度             |
| height           | 是       | uint32 | 表示定位位置的长方形的高度             |
| words            | 否       | string | 识别结果字符串                   |


```json
//示例
{
    "log_id": 490058765,
    "words_result": {
        "单位名称": {
            "location": {
                "left": 500,
                "top": 479,
                "width": 618,
                "height": 54
            },
            "words": "袁氏财团有限公司"
        },
        "法人": {
            "location": {
                "left": 938,
                "top": 557,
                "width": 94,
                "height": 46
            },
            "words": "袁运筹"
        },
        "地址": {
            "location": {
                "left": 503,
                "top": 644,
                "width": 574,
                "height": 57
            },
            "words": "江苏省南京市中山东路19号"
        },
        "有效期": {
            "location": {
                "left": 779,
                "top": 1108,
                "width": 271,
                "height": 49
            },
            "words": "2015年02月12日"
        },
        "证件编号": {
            "location": {
                "left": 1219,
                "top": 357,
                "width": 466,
                "height": 39
            },
            "words": "苏餐证字(2019)第666602666661号"
        },
        "社会信用代码": {
            "location": {
                "left": 0,
                "top": 0,
                "width": 0,
                "height": 0
            },
            "words": "无"
        }
    },
    "words_result_num": 6
}

```

### 通用票据识别

* 调用示例

```
// 通用票据识别参数设置
OcrRequestParams param = new OcrRequestParams();

// 设置image参数
param.setImageFile(new File(filePath));

// 设置额外参数 
param.putParam("detect_direction", "true");

// 调用通用票据识别服务
OCR.getInstance().recognizeReceipt(param, new OnResultListener<OcrResponseResult>() {
    @Override
    public void onResult(OcrResponseResult result) {
        listener.onResult(result.getJsonRes());
    }

    @Override
    public void onError(OCRError error) {
        listener.onResult(error.getMessage());
    }
});
```

**options参数**

| 参数               | 必选  | 类型      | 可选值范围      | 说明                                       |
| ---------------- | ----- | ------- | ---------- | ---------------------------------------- |
| image            | 是  | File对象  | -          | 图像数据，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |
| recognize_granularity | false | string | big、small  | 是否定位单字符位置，big：不定位单字符位置，默认值；small：定位单字符位置 |
| probability           | false | string | true、false | 是否返回识别结果中每一行的置信度                         |
| accuracy              | false | string | normal,缺省  | normal 使用快速服务;缺省或其它值使用高精度服务              |
| detect_direction      | false | string | true、false | 是否检测图像朝向，默认不检测，即：false。可选值包括true - 检测朝向；false - 不检测朝向。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。 |

* 结果返回

| 字段               | 是否必选 | 类型      | 说明                                       |
| ---------------- | ---- | ------- | ---------------------------------------- |
| log_id           | 是    | uint64  | 唯一的log id，用于问题定位                         |
| words_result_num | 是    | uint32  | 识别结果数，表示words_result的元素个数                |
| words_result     | 是    | array() | 定位和识别结果数组                                |
| location         | 是    | object  | 位置数组（坐标0点为左上角）                           |
| left             | 是    | uint32  | 表示定位位置的长方形左上顶点的水平坐标                      |
| top              | 是    | uint32  | 表示定位位置的长方形左上顶点的垂直坐标                      |
| width            | 是    | uint32  | 表示定位位置的长方形的宽度                            |
| height           | 是    | uint32  | 表示定位位置的长方形的高度                            |
| words            | 是    | string  | 识别结果字符串                                  |
| chars            | 否    | array() | 单字符结果，recognize_granularity=small时存在     |
| location         | 是    | array() | 位置数组（坐标0点为左上角）                           |
| left             | 是    | uint32  | 表示定位位置的长方形左上顶点的水平坐标                      |
| top              | 是    | uint32  | 表示定位位置的长方形左上顶点的垂直坐标                      |
| width            | 是    | uint32  | 表示定位定位位置的长方形的宽度                          |
| height           | 是    | uint32  | 表示位置的长方形的高度                              |
| char             | 是    | string  | 单字符识别结果                                  |
| probability      | 否    | object  | 识别结果中每一行的置信度值，包含average：行置信度平均值，variance：行置信度方差，min：行置信度最小值 |

```
{
    "log_id": 2661573626,
    "words_result": [
        {
            "location": {
                "left": 10,
                "top": 3,
                "width": 121,
                "height": 24
            },
            "words": "姓名:小明明",
            "chars": [
                {
                    "location": {
                        "left": 16,
                        "top": 6,
                        "width": 17,
                        "height": 20
                    },
                    "char": "姓"
                },
                {
                    "location": {
                        "left": 35,
                        "top": 6,
                        "width": 17,
                        "height": 20
                    },
                    "char": "名"
                },
                {
                    "location": {
                        "left": 55,
                        "top": 6,
                        "width": 11,
                        "height": 20
                    },
                    "char": ":"
                },
                {
                    "location": {
                        "left": 68,
                        "top": 6,
                        "width": 17,
                        "height": 20
                    },
                    "char": "小"
                },
                {
                    "location": {
                        "left": 87,
                        "top": 6,
                        "width": 17,
                        "height": 20
                    },
                    "char": "明"
                },
                {
                    "location": {
                        "left": 107,
                        "top": 6,
                        "width": 17,
                        "height": 20
                    },
                    "char": "明"
                }
            ]
        }
    ],
    "words_result_num": 2
}
```

# FAQ

```
Q：使用demo为何发生 App identifier unmatch 错误
A：使用demo工程请在百度云控制台中绑定demo工程的包名（com.baidu.ocr.demo），然后选择license或者ak，sk方式授权。

Q: demo工程为何提示token还未获取成功，无法使用识别功能
A：识别需要token作为参数，token需要通过网络获取，网络环境较差的情况下返回较慢。

Q：关于身份证识别的两种模式
A：身份证识别依赖本地库，如果您不需要本地能力可以在传入activity的参数中选择关闭，并且移除ui模块中的本地so文件和ui模块中assets下的模型文件

```
# 错误码表

**验证错误**

| 错误码    | 错误信息                                    | 说明                            | 备注                                       |
| ------ | --------------------------------------- | ----------------------------- | ---------------------------------------- |
| 110    | Access token invalid or no longer valid | Access Token过期失效              | 请重新获得有效的Token                            |
| 283501 | License file check error                | 授权文件不匹配                       | 请在[控制台](https://console.bce.baidu.com/ai/#/ai/ocr/overview/index)中配置正确的包名，并确认使用了正确的授权文件 |
| 283502 | App identifier unmatch                  | BundleId不匹配                   | 请在[控制台](https://console.bce.baidu.com/ai/#/ai/ocr/overview/index)中配置正确的包名，并确认使用了正确的授权文件 |
| 283503 | License file not exists                 | 请确认aip.licence文件存在于assets文件夹中 |                                          |
| 283504 | Network error                           | 网络请求失败                        | 请授权App网络权限并保证网络通畅                        |
| 283505 | Server illegal response                 | 服务器返回数据异常                     |                                          |
| 283506 | Load jni so library error               | JNI加载异常                       | 请确认开发包中的so库被正确加载                         |
| 283601 | Server authentication error             | 身份验证错误。                       | 请在[控制台](https://console.bce.baidu.com/ai/#/ai/ocr/overview/index)中配置应用，并确认填写了正确的AK/SK，或使用了正确的授权文件 |
| 283602 | Authentication time error               | 时间戳不正确，可能是设备时间异常。             | 请确保不要改变调用设备的本地时间                         |
| 283604 | App identifier unmatch                  | 错误的PackageName或者BundleId      | 请在[控制台](https://console.bce.baidu.com/ai/#/ai/ocr/overview/index)中配置正确的包名，并确认使用了正确的授权文件 |
| 283700 | Server internal error                   | 服务器内部错误                       | 您可以在工单系统中提交错误信息中的logId，我们将尝试帮您排查错误原因     |



**服务错误**

| 错误码    | 错误信息                         | 描述            |
| ------ | ---------------------------- | ------------- |
| 216015 | module closed                | 模块关闭          |
| 216100 | invalid param                | 非法参数          |
| 216101 | not enough param             | 参数数量不够        |
| 216102 | service not support          | 业务不支持         |
| 216103 | param too long               | 参数太长          |
| 216110 | appid not exist              | APP ID不存在     |
| 216111 | invalid userid               | 非法用户ID        |
| 216200 | empty image                  | 空的图片          |
| 216201 | image format error           | 图片格式错误        |
| 216202 | image size error             | 图片大小错误        |
| 216300 | db error                     | DB错误          |
| 216400 | backend error                | 后端系统错误        |
| 216401 | internal error               | 内部错误          |
| 216500 | unknown error                | 未知错误          |
| 216600 | id number format error       | 身份证的ID格式错误    |
| 216601 | id number and name not match | 身份证的ID和名字不匹配  |
| 216630 | recognize error              | 识别错误          |
| 216631 | recognize bank card error    | 识别银行卡错误（通常为检测不到银行卡）       |
| 216632 | ocr                          | unknown error |
| 216633 | recognize idcard error       | 识别身份证错误（通常为检测不到身份证）       |
| 216634 | detect error                 | 检测错误          |
| 216635 | get mask error               | 获取mask图片错误    |
| 282000 | logic internal error        | 业务逻辑层内部错误 |
| 282001 | logic backend error         | 业务逻辑层后端服务错误 |
| 282100 | image transcode error    | 图片压缩转码错误         |
