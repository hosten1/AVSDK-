# AVSDK-接口介绍及使用说明

*注意:AVSDK库使用说明*
 * 音视频的库需要依赖系统类库，在使用前必须导入以下的类库 


![AVSDK](https://github.com/hosten1/AVSDK-/blob/master/AVSDK_framework.png)   

 * iOS10 需要添加访问权限

```

 Privacy - Camera Usage Description //相机权限
 Privacy - Microphone Usage Description//麦克风权限
 Privacy - Speech Recognition Usage Description//麦克风权限
 Privacy - Bluetooth Peripheral Usage Description//蓝牙权限
```
# 一, AVSDK使用流程：
 1. 项目中引入*AVSDK.framework*（*导库的时候请仔细检查库是否导入，以及库的路劲是否准确*），需要使用音视频的地方导入* #import <AVSipSDK/AVSipSDK.h> *即可；
 2. 创建AVObject对象(主接口文件)：

    ```
      @property (nonatomic, strong)AVObject *avObject;

    ```
 3. 实例化，并设置相关代理

    ```
       self.avObject  = [[AVObject alloc]init];
       [self.avObject setDelegate:self];
       //<AVObjectDelegate>

    ```
 4. 参数设置(iOS是使用对象来存储需要的参数)：

     ```
         AVParamsModel *model = [[AVParamsModel alloc]init];
         if (self.isVideo) {//音频不用设置
        //这里对远端和本地视频View的创建，推荐使用纯代码创建
              self.videoView.frame  =  CGRectMake(0, 0, SCREEN_WIDTH, SCREEN_HEIGHT);
              self.cameraView.frame =  CGRectMake(0, kLocalVideoViewMarginY, SCREEN_WIDTH/4, SCREEN_HEIGHT/6);
             //(1) 传递远端和本地视频View
              model.paramLocalView = self.cameraView;
              model.paramRemoteView = self.videoView;
            //(2) 视频通话时，该参数必须设置
               model.paramOpenVideo = YES;

               }
        //(3) 打洞服务器（可以不用设置 默认为公网服务器）
        model.paramIceServer = @"210.14.155.83";
        model.paramIcePort   = @"3478";
        model.paramIceUser   = @"admin";
        model.paramIcePwd    = @"admin";
       //(4) 参数设置
       [self.avObject setAVParam:model];


     ```
  5.主接口调用

    (1) 主叫端(发起通话请求)调用接口;

        ```
         [self.avObject createOffer];

        ```
    (2) 被叫端(接听方)调用接口:

      ```
      /******************
        1. 这里的接口调用顺序一定不能反；
        2. 接听端的最后一个参数一定要是“offer”；
        3. sdpString(该值在后面有说明)是需要呼叫方发过来的参数；
      ********************/
       [self.avObject setRemoteSDP:self.sdpString type:@"offer"];
       [self.avObject createAnswer];//接听


      ```
  6. 参数回调(sdp):

     主叫端调用**createOffer**或被叫端调用**createAnswer**后会回调该方法：


```
-(void)objectClient:(AVObject *)obj didReceiveCallBackSDPString:(NSString *)sdpString withSDPType:(NSString *)type{
    //发送一条消息
    if (![sdpString isEqualToString:@" " ] || !sdpString) {
                     
        }else{
            [self sendTextMessageWithVideo:sdpString msgType:DOODMESSAGE_CALLBACK_SDP session:self.sessionId];
        }
    }

}

```

# 二, AVSDK头文件：
1.所有接口文件

![AVSDK](https://github.com/hosten1/AVSDK-/blob/master/AVSDKinclude.png)   


2.新增AVConfig处理所有枚举和常量值(原来放在AVSipObject.h中)；

3.删除枚举中多余的无用的定义；和其他端枚举值统一;

# 三, 主接口文件:

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

```

- (void)setCaptureEnable:(BOOL)isEnableCapture; 

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



# 四,回调接口:

* 回调返回状态

```
-(void) objectClient:(AVObject*)obj didReceiveCallBackStatus:(MSG_TYPE)status; 

``` 


* sdp数据

``` 
-(void) objectClient:(AVObject*)obj didReceiveCallBackSDPString:(NSString*)sdpString withSDPType:(NSString*)type; 


```


* 错误信息返回

```

-(void) objectClient:(AVObject*)obj errorStatu:(NSError*)error;
 


```


