##开始

只需要两行代码，你就可以完成一个贴吧的签到：

    $test = new BaiduUtil($cookie);
	$test->sign("chrome");

下面这几行代码展示了如何完成连续的一系列动作，在这个示例里，我们传入了一个客户端$client(可选)：

	$test = new BaiduUtil($cookie, $client);
	$test->returnThis()->zan("chrome")->zan("firefox")->multiSign()->meizhi("星弦雪")->post("显卡");

在这个示例里，我们通过开启链式调用，完成了点赞、客户端一键签到，并为@星弦雪 投了妹纸票，最后在显卡吧水了一贴:)。

当然，这只是一小部分功能，详细请参见方法手册。

##返回值

- 大多数方法都返回一个数组['status'=>(int),'msg'=>(string),'data'=>(array)]
- status = 0 ,则此方法执行成功，否则返回错误码
- msg是错误信息
- data数组中返回主要数据
- 以下方法手册中的返回值，若未特别说明，都是指data数组中的返回值

##方法手册

###登录

**原型**

	public function login($un,$passwd,$vcode = NULL,$vcode_md5 = NULL)

**返回值(array)**

如果登陆成功

- uid……用户的uid
- un……用户名
- bduss……BDUSS
- cookie

如果需要验证码

- un……用户名
- passwd……密码
- need_vcode……需要验证码时为1，否则为0
- vcode_md5……验证码MD5【可以存在session中】
- vcode_pic_url……验证码地址

----------

###签到

**原型**

	public function sign($kw, $fid = NULL)

**参数**

- $fid……重要，签到贴吧的tid
- $kw……必须，签到贴吧名

**返回值**

- fid……签到贴吧的tid
- kw……签到贴吧名

----------

###发帖

**原型**

	public function post($kw,$fid = NULL,$tid = NULL,$content = NULL)

**参数**

虽然大部分参数可以省略，但为了减少HTTP请求并加快速度，请尽量传入$fid,$tid,$kw

**返回值**

##异常

百度工具类在内部处理大部分异常并返回负值的错误码，能在外部捕获的异常只有构造函数中的异常

- -10 fetch() 网络连接失败
- -11 fetch() 未收到正确数据
- -12 getForumInfo() 无可赞的帖子
- -13 getForumInfo() 无法取得pid
- -14 fetchWebTbs() 获取webtbs失败
- -15 clientRelogin() clientRelogin失败
- -16 doTdouLottery() 免费抽奖机会已经用完 
- -17 doMultiSign() 没有可以一键签到的贴吧
- -18 getForumInfo() 获取的贴吧页面没有点赞信息
- -99 __construct() 请输入合法的cookie