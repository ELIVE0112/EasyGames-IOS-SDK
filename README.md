# EasyGame IOS SDK

## 简介
欢迎使用 EasyGames IOS SDK，當前最新版本為4.8.50

## 开发环境

EasyGames SDK for iOS 现有版本需要Xcode 10.0以上，iOS版本为9.0以上，支持适配iPhone，iPad，支持所有方向游戏。


## 参数配置
所需具体参数请参见我方运营配置表，这里列出所有参数在各版本中的填写必要性，请根据运营地区选择填入
#### 项目plist直接配置参数
|SDK国家版本/PlistKey|大陆版本| 港澳台欧美俄（新加坡）|
|:--:|:--:|:--:|
|EGLSAppleID（ItunsConnect AppleID string类型）|**需要**|**需要**|
|EGLSCurrencyCode（货币代号 string类型）|**需要**|**需要**|
|EGLSDevelopmentTeamID（APPLE DEVELOPMENT_TEAM ID string类型）|**需要**|**需要**|
|EGLSAFDevKey（AppsFlyer开发者Key string类型）|如接入独立AF账号则需要|如接入独立AF账号则需要|
|WechatAppID（微信开发者ID string类型）|**需要**|不需要|
|FacebookAppID（Facebook开发者ID string类型）|不需要|**需要**|
|GoogleAppID（Google开发者ID string类型）|不需要|**需要**|
|NaverLoginClientId（Naver开发者ID NaverLoginClientIdCafeId string类型）|不需要|不需要|
|NaverLoginClientSecret（Naver开发者Secret NaverLoginClientSecret string类型 ）|不需要|不需要|
|CafeId（NaverCafe开发者ID string类型）|不需要|不需要|
|LineSDKConfig（line登录开ID string类型）|不需要|**需要**|
|LSApplicationQueriesSchemes（白名单功能参数见下方 Array）|**需要**|**需要**|
|FaceBookPermissions（fb额外权限  无特殊要求可忽略  Array类型  ）|不需要|**需要**|
|IsNeedFirebase（是否启用推送 ）|不需要|**需要**|
|SupportedChannels（额外渠道登录 (cr[目前只有美国],line,phone[只有大陆和台湾才有此登录]) Array类型）|不需要|**需要**|


#### 项目plist的scheme中需要添加的参数
这里的参数是在plist中打开URL Types选项卡以后需要点击“+”号添加的每一组参数，其中每组需要添加的参数包括了Identifier和URL Schemes（Schemes）2项，如果没有对identifier做说明则不需要填入任何参数，如有疑问也请善用搜索或者直接联系帮助  
line3rdp.$(PRODUCT_BUNDLE_IDENTIFIER)
|SDK国家版本/参数说明|大陆版本| 港澳台欧美俄（新加坡）|
|:--:|:--:|:--:|
|Schemes:WechatAppID值|**需要**|不需要|
|Schemes:"FB"+FacebookAppID值|不需要|**需要**|
|Identifier:项目bundleID值 Schemes:项目bundleID值|不需要|**需要**|
|Schemes:lien(line3rdp.$(PRODUCT_BUNDLE_IDENTIFIER))|不需要|**需要**|
|Schemes:GoogleAppID按“.”分割反序排列值*|不需要|**需要**|
*：[Google对填写scheme的说明](https://developers.google.com/identity/sign-in/ios/start-integrating)




#### 项目plist中LSApplicationQueriesSchemes数组中需要添加的参数（暂无）
|LSApplicationQueriesSchemes|参数|
|:--:|:--:|
|line|lineauth2,line|
|faceboob|fbapi,fb-messenger-share-api,fbauth2,fbshareextension|
|cafa|naversearchthirdlogin,naversearchapp,navercafe|

## Xcode编译

按步骤设置你的Xcode工程来保证编译成功

- **加入SDK库文件**  
  分别在Xcode工程里拖入3rdlib（通用），EGLSSDK_Framework.framework，EGLSSDK_Framework.bundle（分发行地区选择导入）
- **Capabilities设置**  
  1--打开 Keychain Sharing 权限
- **Build Settings设置**  
  1--在Other Linker Flags 选项中追加-ObjC  
  2--SDK不支持bitcode 在Enable Bitcode选项中关闭它  
  3--Search Paths 在Framework Search Paths、Header Search Paths、Library Search Paths中加入3rdLib文件夹路径，并在路径右侧的参数值选择里选择recursive，使路径遍历搜索  
  4--在Framework Search Paths中加入EGLSSDK_Framework.framework路径
- **Build Phases设置**  
  1--如果工程整体设置了ARC则不需要再对具体文件设置ARC，否则请将SDK导入可编译的文件在Compile Sources选项卡中单独加入-fobjc-arc选项设置  
  2--增加SDK所需系统库，列表如下，如已有不需重复添加

> libxml2.tbd  
> libsqlite3.tbd  
> libz.tbd  
> libsqlite3.0.tbd  
> Photos.framework  
> UserNotifications.framework  
> CoreTelephony.framework  
> CFNetwork.framework  
> GameKit.framework  
> Social.framework  
> iAd.framework  
> AVKit.framework  
> WebKit.framework  
> QuartzCore.framework  
> ImageIO.framework  
> ReplayKit.framework  
> CoreGraphics.framework  
> Foundation.framework  
> Security.framework  
> UIKit.framework  
> AVFoundation.framework  
> AssetsLibrary.framework  
> MediaPlayer.framework  
> CoreMedia.framework  
> SystemConfiguration.framework  
> MobileCoreServices.framework  
> AddressBook.framework  
> SafariServices.framework  
> StoreKit.framework  
> AdSupport.framework  
> CoreGraphics.framework  
> Foundation.framework  
> MessageUI.framework  
> Accelerate.framework

## 功能代码
### 初始化
- （必选）SDK初始化
```
- (void)sdkInitWithAppID:(NSString *)appID
      withClientVersion:(NSString *)clientVersion
    withPassportCountry:(PassportCountry)passportCountry
            withIsDebug:(BOOL)isDebug
   withCallBackDelegate:(id<EGLSSDKDelegate>)delegate;
```

- （可选）初始化回调
```
- (void)eglsSdkInitCallBack;
```

### IOS自定事件上报
- （可选）设置trackKey
```
-(void)setTrackKey:(NSString*)trackKey;


- （可选）传递自定义的追踪事件

-(void)trackEventCustom:(NSString *)eventKey eventValue:(NSString *)eventValue
```

### iOS通知接口
- （必选）SDK iOS代理接口
```

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)dictionary;
 
- (void)applicationDidBecomeActive:(UIApplication *)application;
 
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation;
 
- (void)applicationWillTerminate:(UIApplication *)application;
 
- (BOOL)application:(UIApplication*)app openURL:(NSURL *)url options:(NSDictionary *)options;
 
- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken;
 
- (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler;
 
- (void)application:(UIApplication*)application didReceiveLocalNotification:(UILocalNotification *)notification;

```


### 设置绘制层
- （必选）设置程序最上层用于显示的Controller,如果在游戏运行中rootViewController有变化也请在调用SDK方法之前调用这个方法
```
+ (void)setEGLSRootViewController:(UIViewController *)rootVC;
```


### 一般模式(带ui的SDK方法)
#### 注册登陆
- （必选）登陆功能
```
- (void)eglsLogin:(CPLoginType)type;
```


- （必选）登陆回调成功
```
- (void)loginSuccessCallBackWithUid:(NSString *)uid
                           andToken:(NSString *)token 
                         andChannel:(NSString *)channel
                        andNickname:(NSString *)nick;
```


- （必选）登陆回调失败
```
- (void)loginFailCallBackWithChannel:(NSString *)channel;
```


- （可选）注册回调
```
- (void)registerSuccessCallBackWithUid:(NSString *)uid;
```

- （可选）关闭登陆页面回调
```
- (void)loginCancel;
```


- （可选）悬浮框登出按钮回调
```
//目前回调参数不可用
- (void)eglsLogOut:(int)state andMessage:(NSString *) messge;
```

- （可选）悬浮框登出按钮显示隐藏
```
//YES为隐藏
+ (void)setLoginOutOnFloatButon:(BOOL)showEnable
```

- （可选）主动调用游客绑定
```
- (void)eglsGuestBind;
```

- （可选）设置登陆后Banner是否隐藏更换账号按钮
```
- (void)setBannerSwitchHidden;
```

#### 充值
- （必选）苹果充值 (CP可根据需求选择相应的充值接口)
```
// 玩家信息提交接口,一般自研游戏需要在充值前调用
-(void)updateRoleData:(NSString * )subgame
               roleID:(NSString *)roleID
               level:(NSString *)level
               vipLevel:(NSString *)vipLevel;
               
               
//公共充值接口            
#define  FLAG_PURCHASE_DEFAULT    0    //默认渠道支付
#define  FLAG_PURCHASE_WEB_MYCARD    1    //Mycard网页支付
#define  FLAG_PURCHASE_WEB_EC    2    //绿界网页支付
#define  FLAG_PURCHASE_WEB_EB  3     //蓝新网页支付


- (void)applePurchaseCommunal:(int)payType 
                     productID:(NSString *)productID
                     orderID:(NSString *)extraData
                     productName:(NSString*)productName
                     price:(double)price

                    
```
- （必选）充值回调
```
- (void)applePurchaseSuccessCallBackWithOrderID:(NSString *)orderID 
                                       andMoney:(NSString *)money;
```
#### 分享
- （可选）通过第三方SDK实现游戏内容分享
```
- (void)shareWithType:(int)type 
            withTitle:(NSString *)title 
             withText:(NSString *)text 
            withImage:(NSString *)image 
             withLink:(NSString *)link 
           isTimeLine:(BOOL)isTL;
```

- （可选）分享回调
```
- (void)shareCallBack:(int)typeCode 
           withResult:(int)result 
          withMessage:(NSString *)message;
```


#### 第三方信息获取(目前只有faceBook)

##### 获取接口
```
/**
 *    facebook邀请好友
 *
 */
- (void)facebookInviteFriends;

/**
 *    玩家信息
 *
 */
- (void)facebookUserInfo;


/**
 *    玩家好友列表
 *
 */
- (void)facebookFriendList;
```
##### 回掉接口
```
-(void)FBloginSuccessCallBackWithUid:(NSString *)uid name:(NSString *)name picture:(NSString *)picture
{
     NSLog(@"EGLS Login Success Call Back!\n uid is : %@\n name is : %@ picture:%@", uid, name,picture);
}
-(void)FBloginSuccessCallBackWithFields:(NSArray *)fields
{
    NSLog(@"field is:%@",fields);
}
```
#### 运营活动
- （可选）运营活动页面，接入需与我方运营人员确认
```

- (void)ratingActivity;

- (void)facebookActivity:(BOOL)enableJoinfans withEnableShare:(BOOL)enableShare;

- (void)linePromotionActivity;
```
- （可选）运营回调
```
- (void)activityCallBack:(int)typeCode isSuccess:(BOOL)isSuc;
```


### 轻量级SDK(不带UI)
#### 登录
```
/**
 轻量级登录

 @param accountType 登录类型
 */
-(void)channelLoginLightly:(NSString*)accountType;


/**
手机号 登录

@param mobile 手机号
@param passWord 密码
*/
-(void)mobileLoginLightly:(NSString*)mobile passWord:(NSString *) password;
/**
 邮箱 登录
@param mail 电子邮箱
@param passWord 密码
*/
-(void)mailLoginLightly:(NSString*)mail passWord:(NSString *) password;

```
#### 登录回调
```
/**
登录回掉(最新)
@param state SDK登录完成后返回的状态码，0为成功，1为取消，2为失败
@param uid 用户ID
@param token 校验用TOKEN
@param channel 登录方式："0"表示游客，"1"表示EGLS，“2”表示google，“3”表示facebook, "4"表示微信登陆
@param nick 用于显示的账号名称
@param message SDK登录完成后返回的平台消息
*/
-(void)loginCallback:(int)state andToken:(NSString *)token anduid: (NSString *)uid accountType:(NSString *)channel andNickname:(NSString *)nick andMassage:(NSString *) massage;
```

#### 绑定
```
/**
 轻量级绑定

 @param accountType accountType 绑定类型
 */
-(void)channelBindLightly:(NSString*)accountType;



/**
  手机号绑定验证

 @param mobile  手机号码
 */
-(void)mobileBindVerifyLightly:(NSString*)mobile;

/**
邮箱绑定验证

 @param mail  邮箱验证
 */
-(void)mailBindVerifyLightly:(NSString*)mail;


/**
 手机号绑定请求

 @param mobile  手机号
 @param verificationCode  验证码
 @param password  密码
 */
-(void)mobileBindRequestLightly:(NSString*)mobile verificationCode:(NSString*)verificationCode passWord:(NSString*)password;


/**
 邮箱绑定请求

 @param mail  邮箱号
 @param verificationCode  验证码
 @param password  密码
 */
-(void)mailBindRequestLightLy:(NSString*)mail verificationCode:(NSString*)verificationCode passWord:(NSString*)password;
```

#### 绑定回调
```
/**
 用户绑定回调

 @param accountType 绑定对应账户类型 1:EGLS 2:Google 3:Facebook 4:Wechat
 @param nickName 用户昵称
 */
- (void)channelBindCallBack:(NSString *)accountType withNickName:(NSString *)nickName withBindState: (int) state withMessage:(NSString*)massage;

/**
用户绑定验证

@param state  0为成功，1为取消，2为失败
@param message 服务器返回的消息
*/
- (void)verifyCallbackFromBind:(int)state andMessage:(NSString*)message;
```
#### 注册
```
/**
手机号注册验证
@param mobile  手机号
*/

-(void)mobileRegisterVerifyLightly:(NSString*) mobile;

/**
邮箱注册验证
@param mobile  手机号
*/

-(void)mailRegisterVerifyLightly:(NSString*) mail;


/**
手机注册请求
@param mobile  手机号
@param verificationCode  验证码
@param passWord  密码
*/

-(void)mobileRegisterRequestLightly:(NSString*) mobile verificationCode:(NSString*)verificationCode passWord:(NSString*)password;

/**
邮箱注册请求
@param mail  邮箱
@param verificationCode  验证码
@param passWord  密码
*/

-(void)mailRegisterRequestLightly:(NSString*) mail verificationCode:(NSString*)verificationCode passWord:(NSString*)password;
```
#### 注册回调
```
/**
用户注册验证回调

@param state SDK“手机号/邮箱注册验证”接口调用后返回的状态码：0为成功，1为取消，2为失败
@param message SDK“手机号/邮箱注册验证”接口调用后返回的平台消息
*/
- (void)verifyCallbackFromRegister:(int)state andMessage:(NSString*)message;

/**
登录回掉(最新) 注册成功后会直接登录 因此会走登录回调接口 
@param state SDK登录完成后返回的状态码，0为成功，1为取消，2为失败
@param uid 用户ID
@param token 校验用TOKEN
@param channel 登录方式："0"表示游客，"1"表示EGLS，“2”表示google，“3”表示facebook, "4"表示微信登陆
@param nick 用于显示的账号名称
@param message SDK登录完成后返回的平台消息
*/
-(void)loginCallback:(int)state andToken:(NSString *)token anduid: (NSString *)uid accountType:(NSString *)channel andNickname:(NSString *)nick andMassage:(NSString *) massage;


```

#### 密码相关服务
```
/**
密码修改
@param passWord  密码
*/
-(void)pwdModifyLightly:(NSString*)password;

/**
密码重置鉴权
@param userAccount  账号（手机号/邮箱）
*/
-(void)pwdResetCaptchaLightly:(NSString*)userAccount;


/**
密码重置请求
@param userAccount  账号（手机号/邮箱）
 @param captcha  鉴权码
*/
-(void)pwdResetRequestLightly:(NSString*)userAccount captcha:(NSString*) captcha;
```

#### 密码服务回调
```
/**
鉴权回调接口

@param state SDK“密码重置鉴权”接口调用后返回的状态码：0为成功，1为取消，2为失败
@param message SDK“密码重置鉴权”接口调用后返回的平台消息
*/
- (void)captchaCallbackFromPassWord:(int)state andMessage:(NSString*)message;

/**
密码重置

@param state SDK“密码重置请求”接口调用后返回的状态码：0为成功，1为取消，2为失败
@param message SDK“密码重置鉴权”接口调用后返回的平台消息
*/
- (void)requestCallbackFromResetPassWord:(int)state andMessage:(NSString*)message;


/**
密码修改

@param state SDK“密码重置请求”接口调用后返回的状态码：0为成功，1为取消，2为失败
@param message SDK“密码重置鉴权”接口调用后返回的平台消息
*/
- (void)requestCallbackFromChangePassWord:(int)state andMessage:(NSString*)message;
```

#### 支付
```
/**
 轻量级支付
 @param amount  金额（手机号/邮箱）
 @param productId  档位编号
 @param productName  档位名称
 @param orderInfo  订单信息
 @param flag  支付标识
                    PAYTYPE_COMMON, //一般充值接口
                    PAYTYPE_MC, //myCard
                    PAYTYPE_EC, //绿戒
                    PAYTYPE_EB //蓝心
                    
*/
-(void)channelPurchaseLightly:(NSString *)amount productId:(NSString*)productId productName:(NSString*)productName
                    orderInfo:(NSString*)orderInfo flag:(int)falg;
                    
```

#### 支付回调
```
/**
 *    苹果充值成功回调
 *    @param     state          0为成功，1为取消，2为失败
 *    @param    orderID         EGLS平台订单号
 *    @param    otherMessage    额外信息, 目前充值成功为 充值金额 后续可能为json
 *
 */
- (void)applePurchaseCallBackWithOrderID:(int)state andOrderId:(NSString *)orderID andOtherMessage:(NSString *)otherMessage;


```



