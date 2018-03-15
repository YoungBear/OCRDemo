# 百度OCR 官方Demo

[百度文字识别官网](http://ai.baidu.com/tech/ocr)

[OCR Android SDK 开发者文档](http://ai.baidu.com/docs#/OCR-Android-SDK/top)

[百度sdk下载](http://ai.baidu.com/sdk)

OCR: Optical Character Recognition 光学字符识别

## 一、 [管理控制台](https://console.bce.baidu.com/ai/?fromai=1&_=1488766023093#/ai/ocr/app/list)申请应用

可以使用两种方式来完成身份验证与安全：

1. ak/sk方式。
2. license授权文件方式。（推荐）

具体方式请参考[官方文档](http://ai.baidu.com/docs#/OCR-Android-SDK/top)。

## 二、 下载SDK及demo文件

可以在[百度sdk下载](http://ai.baidu.com/sdk)下载对应的**文字识别**模块下载AndroidSDK包。

![下载ORC Android SDK](https://github.com/YoungBear/OCRDemo/blob/master/pic_files/baidu_orc_download_android_sdk.png)

下载完成，解压文件后，目录结构为：

![SDK目录结构](https://github.com/YoungBear/OCRDemo/blob/master/pic_files/baidu_ocr_sdk_directory.png)

其中
- libs为库文件目录
- ocr_ui为ui模块
- OCRDemo为一个单独的模块，使用Android Studio只需要该目录。
- OCR-Android-SDK.md为OCR Android 官方文档。

## 三、 使用Android Studio导入工程文件

1. 使用AS导入工程OCRDemo模块。
2. 将获取的授权文件**`aip.license`**拷贝到`OCRDemo/app/src/main/assets`目录下，然后使用授权文件的方式初始化token。

```
        // 请选择您的初始化方式
         initAccessToken();
//        initAccessTokenWithAkSk();
...
    private void initAccessToken() {
        OCR.getInstance().initAccessToken(new OnResultListener<AccessToken>() {
            @Override
            public void onResult(AccessToken accessToken) {
                String token = accessToken.getAccessToken();
                hasGotToken = true;
            }

            @Override
            public void onError(OCRError error) {
                error.printStackTrace();
                alertText("licence方式获取token失败", error.getMessage());
            }
        }, getApplicationContext());
    }
```

然后，就可以运行工程，查看效果。

车牌识别结果：

![车牌识别结果](https://github.com/YoungBear/OCRDemo/blob/master/pic_files/baidu_ocr_license_plate_result.jpg)

## [项目Github地址](https://github.com/YoungBear/OCRDemo)

## [更多文章](https://github.com/YoungBear/MyBlog/blob/master/README.md)




