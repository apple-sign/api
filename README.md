# api

## 签名
所有的第三方API接口需要附带 `account` 和 `timestamp` 公共参数，然后所有的参数和登陆密码参与签名计算，然后将签名后的MD5结果填入 `secret` 参数中。
* `account` 登陆账号
* `timestamp` 当前的[Unix Time](https://en.wikipedia.org/wiki/Unix_time)时间戳，单位为秒

签名算法如下：
* 将所有参数根据 key 的字典顺排序
* 每个参数用 "{key}={value}" 字符串表示，然后将所有的字符串用 "&" 连接
* 在字符串尾部链接 ".{password}"，password 为登陆密码
* 返回字符串的 MD5 小写字符串

可以参考 JavaSDK 中 [MD5Sign.java](https://github.com/apple-sign/JavaSDK/blob/master/src/main/java/org/applesign/utils/MD5Sign.java) 的实现。

## 应用管理

### 获取应用列表
* url: `/api/third/appList`
* method: POST
* Content-Type: application/x-www-form-urlencoded
参数：
* `name` 应用名称搜索关键词，如果返回所有，为空字符串
* `size` 每页的数量
* `current` 当前的页码

可参考 JavaSDK 中 [AppList.java](https://github.com/apple-sign/JavaSDK/blob/master/src/main/java/org/applesign/api/AppList.java) 的实现。

上传应用流程比较复杂，需要先通过 API 获取阿里云OSS参数，上传 .ipa 文件后，再调用 API 解析。可参考 JavaSDK 中 [AppUpload.java](https://github.com/apple-sign/JavaSDK/blob/master/src/main/java/org/applesign/api/AppUpload.java) 的实现。

## 授权码

### 生成授权码
* url: `/api/third/authcode`
* method: POST
* Content-Type: application/x-www-form-urlencoded
参数：
* `alias` 应用唯一标识  必传
* `udid` 授权的udid 可以为空，为空表示可以给任意设备下载一次，下载的时候绑定udid
* 公共参数：`account`, `timestamp`, `secret`

### 获取授权码列表
* url: `/api/third/authcodeList`
* method: POST
* Content-Type: application/x-www-form-urlencoded
参数：
* `appId` 应用唯一标识  必传
* `udid` 授权的udid 可以为空，为空表示可以给任意设备下载一次，下载的时候绑定udid
* `size` 每页的数量
* `current` 当前的页码
* 公共参数：`account`, `timestamp`, `secret`
*tets
可参考 JavaSDK 中 [AuthCode.java](https://github.com/apple-sign/JavaSDK/blob/master/src/main/java/org/applesign/api/AuthCode.java) 的实现。
