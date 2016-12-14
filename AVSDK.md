# AVSDK-接口介绍及使用说明

*注：* **重构后与之前的版本差异主要在主接口文件名，sdk库名，回调函数上的差异（重构后回调sdp的类型已经加上）；新增一个参数回调(需多次回调显示,客户端加定时器)；具体看文档:**
# 一, 目录结构差异：
    1.头文件差异
    ![这里写图片描述](fff) ![这里写图片描述](fff)
    2.新增AVConfig处理所有枚举和常量值(原来放在AVSipObject.h中)；
    3.删除枚举中多余的无用的定义；和其他端枚举值统一;

# 二, 主接口文件差异:
1.	文件名重构前（AVSipObject），重构后(AVObject);
2.	主要功能接口差异：
功能接口 | AVSDK | AVSipSDK
------------|-----------------------------------|----------------------------
初始化参数设置  setAVParam:(AVParamsModel*)          setAVParam:(AVParamsModel*)


呼叫 |createOffer  | createOffer;
 
接听|createAnswer|createAnswer
前后置摄像|switchCamera|switchCamera
静音设置|setMicrophone:(BOOL) isEnableMic|setMicrophone:(BOOL) isEnableMic
外音听筒切换|setSpeaker:(BOOL)|setSpeaker:(BOOL)
视频采集开关|setCaptureEnable:(BOOL)|setCaptureEnable:(BOOL)
SDP参数设置|setRemoteSDP:(NSString *) type:(NSString*);|setRemoteSDP:(NSString *) type:(NSString*)；
资源释放|destroy|destroy
音视频数据获取|getStatsWithBlock:(void (^)(NSDictionary *));|无



# 三,回调接口差异:
类型 | AVSDK | AVSipSDK
------------|-----------------------------------|----------------------------

回调返回状态 | objectClient:(AVObject*) didReceiveCallBackStatus:(MSG_TYPE);| sipObject:(AVSipObject*) didReceiveCallBackStatus:(MSG_TYPE)
sdp数据 | objectClient:(AVObject*) didReceiveCallBackSDPString:(NSString*) withSDPType:(NSString*);| sipObject:(AVSipObject*) didReceiveCallBackSDPString:(NSString*) withSDPType:(NSString*);
错误信息返回 | objectClient:(AVObject*) errorStatu:(NSError*) | sipObject:(AVSipObject*) errorStatu:(NSString*)

