# xcx_QQMapWX
小程序使用腾讯地图，计算距离


1. 申请开发者密钥（key）：申请密钥

2. 下载微信小程序JavaScriptSDK，微信小程序JavaScriptSDK v1.0

3. 安全域名设置，需要在微信公众平台添加域名地址https://apis.map.qq.com


```
//计算距离
ctdistance: function (data, i, callback = function () { }) {
    var that = this;
    //调用腾讯地图接口计算距离
    var demo = new QQMapWX({
      key: 'HYEBZ-Z5QK3-VBV33-YWWNV-IJDOS-5XFZ7' // 必填
    });
    //对下标为i数组的值进行计算距离
    data[i].clinicimg = data[i].img[0]; //取第一张图片
    var latitude = data[i]['latitude']; //经度
    var longitude = data[i]['longitude']; //纬度
    // 调用接口方法
    demo.calculateDistance({  //计算一个点到多点的步行距离。
      to: [{
        latitude: latitude,
        longitude: longitude
      }],
      success: function (res) {
        //结果为m，处理成km
        var distance = res.result.elements[0]['distance'] / 1000
        data[i].distance = distance.toFixed(3);

      },
      fail: function (res) {
        data[i].distance = 99999;
      },
      complete: function (res) {  //不管是否成功都会执行
        if (i < data.length - 1) {  //当i小于数组长度时，继续执行计算距离
          i++;
          that.ctdistance(data, i, callback)
        } else {  //数组的距离全部计算完成，回调结果
          callback(data);
        }
      }
    });
}
```

```
//调用
that.ctdistance(res.data, 0, function (data) {
            //回调的结果带有距离参数
});
```
