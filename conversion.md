# 1.点击回调

当TapTap用户发生“点击下载按钮”行为时，系统会通知填写到后台的回调地址，广告主可以获取回调请求中的设备信息进行转化匹配。

回调地址需要是合法的http或https链接，长度不能超过1000个字符。

回调地址应当返回2xx或3xx的http code。当回调地址调用失败时，我方系统会进行多次尝试，多次尝试失败会丢弃该回调。

回调地址支持下列宏替换：

宏 | 说明
--- | ---
{IDFA} | iOS设备的IDFA，使用原始的大写的值，不存在时传递空字符串
{TIME} | 时间戳，1970年到当前时间的秒数
{IP} | 用户的IP地址
{ORG_ID} | 公司或代理商ID
{ORG_NAME} | 公司或代理商名称
{GAME_ID} | 游戏ID
{GAME_NAME} | 游戏名称
{CREATIVE_ID} | 广告ID
{CREATIVE_NAME} | 广告名称
{CREATIVE_TYPE} | 广告类型，taptap表示taptap原生广告
{DEVICE} | 设备类型，ios表示iOS设备
{CALLBACK_HTTP} | 安装激活回调地址，HTTP协议，如果成功匹配到安装激活，可通过此链接回传数据
{CALLBACK_HTTPS} | 安装激活回调地址，HTTPS协议，如果成功匹配到安装激活，可通过此链接回传数据

示例：

`https://xx.xxx.com/track?idfa={IDFA}&time={TIME}&ip={IP}&org_id={ORG_ID}&org_name={ORG_NAME}&game_id={GAME_ID}&game_name={GAME_NAME}&creative_id={CREATIVE_ID}&creative_name={CREATIVE_NAME}&creative_type={CREATIVE_TYPE}&deivce={DEVICE}&callback_http={CALLBACK_HTTP}&callback_https={CALLBACK_HTTPS}`

# 2.激活数据回传

当广告主通过`点击回调`链接成功匹配到安装激活时，应该将激活数据回传给TapTap广告系统，可以使用下面两种方法回传。

### 2.1.动态回传地址

在`点击回调`中，可以设置`{CALLBACK_HTTP}`或`{CALLBACK_HTTPS}`宏，它们会被替换为激活回传地址，分别为HTTP协议和HTTPS协议，选择支持的协议，直接调用对应的地址完成数据回传。

### 2.2.手动配置回传地址

调用地址`https://sense.tapdb.net/api/v2/track/activate`，HTTP Method为GET，若只支持HTTP协议，可以将地址前缀修改为`http://`，需要传递如下参数：

参数 | 是否必须 | 说明
--- | --- | ---
tapAdId | 必须 | iOS设备的IDFA，原始的大写的值
product | 必须 | 安装激活的游戏在TapTap上的游戏ID
device | 必须 | 1表示iOS设备

示例：

`https://sense.tapdb.net/api/v2/track/activate?tapAdId=F2F589F4-CCB2-41F4-8657-00011AC62395&product=0&device=1`
