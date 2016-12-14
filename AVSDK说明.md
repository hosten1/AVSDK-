# AVSDK-接口介绍及使用说明

*注：* **kon**
# 一, 目录结构差异：
1.头文件差异

![AVSDK](https://github.com/hosten1/AVSDK-/blob/master/AVSDKinclude.png)   


2.新增AVConfig处理所有枚举和常量值(原来放在AVSipObject.h中)；

3.删除枚举中多余的无用的定义；和其他端枚举值统一;

# 二, 主接口文件:

1.	文件名重构前（AVSipObject），重构后(AVObject);

2.	主要功能接口差异：


* 初始化参数设置  

```
- (void)setAVParam:(AVParamsModel*)params;
```
* 呼叫

```
- (void)createOffer;
``` 

*  接听

```
- (void)createAnswer;

```

* 前后置摄像


```
- (void)switchCamera; 

```
* 静音设置

``` 
-(void)setMicrophone:(BOOL) isEnableMic;

``` 

* 外音听筒切换

``` 
- (void)setSpeaker:(BOOL) isEnableSpeaker;

``` 

* 视频采集开关

``` - (void)setCaptureEnable:(BOOL)isEnableCapture; 

``` 

* SDP参数设置 

```
-(void)setRemoteSDP:(NSString *)sdp type:(NSString*)type; 

``` 


* 资源释放

``` 
- (void)destroy;

```


* 音视频数据获取

```

- (void)getStatsWithBlock:(void (^)(NSDictionary *stats))response; 


```



# 三,回调接口:

<div class="body">
<ul style="color: red; margin-left: 20px" >
<li style="color: red; margin-left: 20px" >回调返回状态<br>
<code>
-(void) objectClient:(AVObject*)obj didReceiveCallBackStatus:(MSG_TYPE)status;
</code>
</li>
<li>sdp数据<br>
<code>
-(void) objectClient:(AVObject*)obj didReceiveCallBackSDPString:(NSString*)sdpString withSDPType:(NSString*)type; 
</code>
</li>
<li>错误信息返回<br>
<code>
-(void) objectClient:(AVObject*)obj errorStatu:(NSError*)error;
</code>
</li>
</ul>
</div>

