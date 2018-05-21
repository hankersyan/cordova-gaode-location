# cordova-gaode-location V1.0.3
由于Android Webkit的安全限制，只有Https网站才能访问HTML5的定位功能，
因此，提供基于高德地图的Android定位插件；iOS使用HTML5定位即可(navigator.geolocation.getCurrentPosition)。
本分支基于 https://github.com/ww-gh/cordova-gaode-location ，并修复了在cordova 7.0编译失败的BUG。

## 1、添加插件方式：
- 直接通过 url 安装：
```shell
cordova plugin add https://github.com/hankersyan/cordova-gaode-location.git --variable API_KEY="高德的API_KEY"
```
> 注意：使用前先到高德官网去申请自己的API Key；

## 3、使用说明：
- javascript语法
```javascript
cordova.plugins.GaoDeLocation.getCurrentPosition(function(resp){
                    console.log(resp);
                }, function(err){
                    console.log(err);
                });
```
- typescript语法
```typescript
cordova.plugins.GaoDeLocation.getCurrentPosition((resp: GaoDeposition) => {
                    console.log(resp);
                }, (err: ErrorInfo) => {
                    console.log(err);
                });

//GaoDeposition描述
class GaoDeposition {
    locationType: number;
    latitude: number;
    longitude: number;
    hasAccuracy: boolean;//是否有精确度
    accuracy: number;//精确度
    address: string;//地址
    province: string;//省份
    road: string;//街道
    speed: number;// 移速单位：米/秒
    bearing: number;// 角度
    satellites: number;// 星数
    time: number;// 时间
}

//ErrorInfo描述 
class ErrorInfo {
    errorCode: string;
    errorInfo: string;
}

 /**
  * 自定义的截取高德地图返回的错误描述
  * 返回的提醒信息格式是：
  * 问题说明
  * +空格
  * +请到http://lbs.amap.com/api/android-location-sdk/guide/utilities/errorcode/查看错误码说明,
  * +错误详细信息:问题排查策略
  * +#错误码
  * @param err
  * @return {string}
  */
 static getErrorInfo(err: ErrorInfo): string {
     let errInfo = '';
     if (err) {
         if (err.errorInfo.indexOf(' ') > 0) {//问题说明
             errInfo = err.errorInfo.substring(0, err.errorInfo.indexOf(' '));
         }
         if (err.errorInfo.indexOf('错误详细信息') > 0) {//问题排查策略
             errInfo = errInfo + err.errorInfo.substring(err.errorInfo.indexOf('错误详细信息'), err.errorInfo.length)
                     .replace("错误详细信息", "");
         } else {
             errInfo = err.errorInfo;
         }
     } else {
         errInfo = "定位出现未知错误，请稍后重试！"
     }
     return errInfo;
 }
```

## Support
- [故障反馈地址：](https://github.com/hankersyan/cordova-gaode-location/issues)
