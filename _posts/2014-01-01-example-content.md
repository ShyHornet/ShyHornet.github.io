---
layout: post
title: swift实现一个与智能机器人聊天的app(一)
---


![截图](http://upload-images.jianshu.io/upload_images/727794-e4b4dd2fe687ab5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 你在本系列文章中将会学到
- 如何安装和使用cocoapods来集成第三方库
- 如何搭建一个类似于iOS短信app的界面,以及使用SnapKit来用代码设置autolayout
- 如何使用Parse云服务平台存储和同步聊天信息,学习相应地数据库知识
- 如何使用Parse的远程推送功能
- 如何使用Alamofire实现与智能机器人聊天功能
**初始项目下载地址:**
[百度网盘下载地址](http://pan.baidu.com/s/1z71vg)
#配置初始项目


**1.cocoapods的安装**
cocoapods的安装是通过ruby，幸运的是Mac电脑都是默认安装ruby的，所以安装ruby的过程就省去了，唯一的前提就是安装Xcode的CommandLineTools。
commandLineTools的安装也很简单,只要在终端输入以下命令:
```
$ xcode-select --install
```<br>
如果确实没有安装commandLineTools会提示你要安装它，点安装就可以开始下载，然后等待下载完成后安装即可
下面开始安装cocoapods，本来只需要简单地在终端输入以下命令即可:<br>
```
$ sudo gem install cocoapods
```<br>
但是由于中国的互联网是"自由的"。。咳咳，所以呢，你要改变gem的默认下载源:<br>
```
$ gem sources -a http://ruby.taobao.org
```<br>
看到以下结果
```
http://ruby.taobao.org added to sources
```
将淘宝的ruby源加入进来，看来淘宝也不光是卖东西哈，也是对开发者做了一些贡献的~
删除原来的下载源:
```
$ gem sources -r https://rubygems.org/
```
看到以下结果就说明已经成功
```
https://rubygems.org/ removed from sources
```
然后呢，就可以愉快地安装上cocoapods了！
 ```
$ sudo gem install cocoapods
```
```
Password:(你的管理员密码，这里不会显示出来)
Fetching: cocoapods-core-0.38.2.gem (100%)
Successfully installed cocoapods-core-0.38.2
Fetching: claide-0.9.1.gem (100%)
Successfully installed claide-0.9.1
Fetching: xcodeproj-0.26.3.gem (100%)
Successfully installed xcodeproj-0.26.3
Fetching: cocoapods-downloader-0.9.3.gem (100%)
Successfully installed cocoapods-downloader-0.9.3
Fetching: cocoapods-stats-0.5.3.gem (100%)
Successfully installed cocoapods-stats-0.5.3
Fetching: cocoapods-try-0.4.5.gem (100%)
Successfully installed cocoapods-try-0.4.5
Fetching: cocoapods-trunk-0.6.4.gem (100%)
Successfully installed cocoapods-trunk-0.6.4
Fetching: molinillo-0.3.1.gem (100%)
Successfully installed molinillo-0.3.1
Fetching: cocoapods-0.38.2.gem (100%)
Successfully installed cocoapods-0.38.2
Parsing documentation for cocoapods-core-0.38.2
Installing ri documentation for cocoapods-core-0.38.2
Parsing documentation for claide-0.9.1
Installing ri documentation for claide-0.9.1
Parsing documentation for xcodeproj-0.26.3
Installing ri documentation for xcodeproj-0.26.3
Parsing documentation for cocoapods-downloader-0.9.3
Installing ri documentation for cocoapods-downloader-0.9.3
Parsing documentation for cocoapods-stats-0.5.3
Installing ri documentation for cocoapods-stats-0.5.3
Parsing documentation for cocoapods-try-0.4.5
Installing ri documentation for cocoapods-try-0.4.5
Parsing documentation for cocoapods-trunk-0.6.4
Installing ri documentation for cocoapods-trunk-0.6.4
Parsing documentation for molinillo-0.3.1
Installing ri documentation for molinillo-0.3.1
Parsing documentation for cocoapods-0.38.2
Installing ri documentation for cocoapods-0.38.2
9 gems installed
```
OK,cocoapods顺利安装完毕！
**2.cocoapods的使用**
那么cocoapods怎么用呢，当然第一次使用会觉得它非常麻烦，但是呢渐渐地你会发现这是一个非常好用的工具，可以说是iOS开发者必备！
首先创建我们的Xcode工程:File/New/Project.../Single View Application
起名叫图灵聊天。
>我们将要使用图灵机器人的api进行开发:
[图灵机器人官网](http://www.tuling123.com/openapi/cloud/home.jsp)

打开项目，新建一个空文件:File/New/File.../ iOS/Others/Empty
**起名叫Podfile，这一点非常重要,因为这是cocoapods的配置文件，也就是指定你要使用哪些第三方库!**
我们要使用以下几个库:
- Alamofire，网络请求库，用来调用图灵机器人的api
- SnapKit,用代码进行autolayout设置
- Parse ,Parse云服务平台的SDK
- ParseUI,Parse提供的便捷UI组件
在Podfile中,输入以下代码:
```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.4'
use_frameworks!
```
指定下载源，指定平台版本，使用framework进行集成
>由于swift的特殊性，某些第三方库必须使用framework来集成,但是这样也有一个好处，我也是最近才发现，就是Parse和ParseUI其实是OC编写的库，但是呢却不需要OC-Swift的桥接文件了！可以直接当做swift库来使用！

```
pod 'Alamofire', '~> 1.3'
pod 'SnapKit', '~> 0.12.0'
pod 'Parse','~>1.7.1'
pod 'ParseUI','~>1.1.3'
```
选择指定的第三方库及其版本

开始安装第三方库,打开终端,将当前目录转到Podfile所在目录:
```
$ cd <Podfile所在目录>
```
输入以下命令开始配置第三方库:
```
$ pod install
```
```
Analyzing dependencies
Downloading dependencies
Using Alamofire (1.3.1)
Using Bolts (1.2.1)
Using Parse (1.7.5.3)
Using ParseUI (1.1.4)
Using SnapKit (0.12.0)
Generating Pods project
Integrating client project
Sending stats
```
然后等待几分钟，如果一切正常，没有出现错误的话，打开项目文件后你会看到workspace的文件，以后都要使用这个文件来打开项目。
![图1](http://upload-images.jianshu.io/upload_images/727794-c6ecdc03eb346dab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
打开项目，看一下项目的结构:

![项目结构.png](http://upload-images.jianshu.io/upload_images/727794-f482d9b61f5672e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点一下Pods项目,你会发现所需的framework已经编译好了,只要在使用前import他们就可以了:
![Pods](http://upload-images.jianshu.io/upload_images/727794-0ceaff51b1b19ca4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
OK,到此我们的项目就配置好了，在我们开始搭建UI之前，先了解一下Parse的使用和一些必要配置
#配置Parse
首先打开Parse的官网：
[点我](https://www.parse.com)
注册一个新的用户,点击右上角的sign up :

![Parse注册.png](http://upload-images.jianshu.io/upload_images/727794-5f775e08f036c7b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
以上用户名只是示例，但是app名称输入TuringChat。
注册完毕后，用你刚才注册的用户名登陆，应该会出现以下界面:

![主界面](http://upload-images.jianshu.io/upload_images/727794-1306b7039726bea0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后导入我们的示例数据:
[点我下载](http://pan.baidu.com/s/1gdsBuWb)
点击import按钮:

![导入数据](http://upload-images.jianshu.io/upload_images/727794-864bef1a10e20d0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
选择刚才下载的文件:
![数据导入成功.png](http://upload-images.jianshu.io/upload_images/727794-946156866d970bf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后刚才导入的数据就会显示出来，并自动新建了一个数据库类:Messages
![数据.png](http://upload-images.jianshu.io/upload_images/727794-b08f51b1306050ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们来看一眼Messages类里都有什么:


| 名称  | 类型| 含义  |备注|
| :----- |:------:|:-----|--|
| objectId | String | 系统默认键 |每一条数据都对应一个独一无二的id|
| **incoming** | Boolean      | 用来确定该条信息是发送给我们的还是发送出去的|true就是发送来的反之就是我们发送出去的|
| **sentDate** | Date      |  消息发送时间|无|
|**text**|     String    | 消息的内容  |无|
|createdAt|Date|系统默认键|数据创建时间|
|updatedAt|Date|系统默认键|数据上一次更新的时间|
|ACL|ACL|系统默认键|数据的读写模式|


接下来我们来测试一下能否读取到这些数据，首先要获得该app的application ID和Client Key:

![获取Key](http://upload-images.jianshu.io/upload_images/727794-243da2025a3f18f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
红线划掉的那两行就是我们需要的。
然后打开项目中的**AppDelegate.swift**，增加对Parse库的引用:
```
import Parse
```
找到以下方法
```
  func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool 
```
在里面添加代码:
```
    Parse.setApplicationId("CYdFL9mvG8jHqc4ZA5PJsWMInBbMMun0XCoqnHgf", clientKey: "6tGOC1uIKeYp5glvJE6MXZOWG9pmLtMuIUdh2Yzo")
```
连接Parse的服务器
```
        var query = PFQuery(className: "Messages")
        query.orderByAscending("sentDate")
        query.findObjectsInBackgroundWithBlock { (objects,error) -> Void in
            for object in objects as! [PFObject]{
            let incoming:Bool = object["incoming"] as! Bool
            let text:String = object["text"] as! String
            let sentDate:NSDate = object["sentDate"] as! NSDate
            println("\(object.objectId!)\n\(incoming)\n\(text)\n\(sentDate)")
        }
     }
```
新建查询，查询我们刚才所建的`Messages`类，用`findObjectsInBackgroundWithBlock`方法取出查询结果，并用一个循环全部打印出来。
cmd+R运行一下，如果没有问题会输出类似下面的内容:
```
oYtildSAOz
false
你叫什么名字？
2015-08-28 06:42:00 +0000
LX7kxmmiEp
true
我叫灵灵，聪明又可爱的灵灵
2015-08-28 06:43:00 +0000
p62dmgGIAS
false
你爱不爱我？
2015-08-28 06:43:00 +0000
oWReOM43Nf
true
爱你么么哒
2015-08-28 06:44:00 +0000
mtl2BGt3Mu
false
今天北京天气如何？
2015-08-29 03:59:00 +0000
DikAu5P2Nn
true
北京:08/29 周六,20-29° 28° 雷阵雨 微风小于3级;08/30 周日,19-27° 雷阵雨 微风小于3级;08/31 周一,19-27° 雷阵雨 微风小于3级;09/01 周二,20-26° 雷阵雨 微风小于3级;
2015-08-29 03:59:01 +0000
```
很好，我们的数据库连接没有问题,那么下面开始搭建我们的UI。

#搭建UI
我们需要搭建的UI只是聊天页面，我们首先来看一看聊天页面的结构:
界面主要由以下三个部分组成
![UI结构](http://upload-images.jianshu.io/upload_images/727794-03d57b8222eede17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
那么这三部分怎样去实现呢，我先向大家做一些简单的介绍:
**1.导航栏**
这一部分实现比较简单，只要把视图控制器嵌套在一个导航控制器(**UINavigationController**)中即可，然后对其外观进行一些定制化操作。
**2.聊天窗口**
这一部分用UITableView来构建。仔细观察你会发现这里一共有三种UITableViewCell:
- 用来显示消息发送日期的cell
- 发送消息气泡的cell
- 接收消息气泡的cell
但其实我们只需要两个，因为后两种cell区别只是是颜色和位置，我们只要判断一下该消息是发送的还是接收的，然后相应进行处理即可!
两种cell都是用的以下这个素材:
![MessageBubble.png](http://upload-images.jianshu.io/upload_images/727794-b37e5cf608b0c975.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
但是，你会问，它为啥是黑色的！怎么让他变成图中的两种颜色呢？还有明明聊天气泡的大小是不定的，这样一张图怎么能满足所有尺寸呢？
有疑问很好，因为它可以成为你学习的动力，我们会在接下来向大家解释这是如何实现的！Be patient!
**3.输入框**
这里我们要通过重写`UIResponder`类的`inputAccessoryView`属性来自定义我们的输入框，这样做的好处是我们的输入框会和系统的键盘结合起来，可以让其成为第一响应者(first responder)，一旦它成为第一响应者，我们自定义的输入框会跟随键盘一同弹出和收回，就像真正的短信app那样，这个方法比我有一篇文章所写的[实现类似微信的输入框跟随键盘弹出的效果](http://www.jianshu.com/p/4e755fe09df7)的方法还要更好一些,所以说方法不是绝对的，因为你总是能够找到更好的方法，所以,编程的时候要经常在脑子里想"嗯，一定还有更好的方法"。

**嗯好嘞，废话不多说，下面我们就来一步一步地一一实现它们！**

首先从最简单的做起,实现自定义导航栏:
打开初始项目你会看到模板文件已经全部建好:
找到**AppDelegate.swift**文件中的以下方法:
``swift 
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool
``
在其中添加如下代码：
```
        var ChatVC:ChatViewController = ChatViewController()
        ChatVC.title = "灵灵"
        
        UINavigationBar.appearance().tintColor = UIColor(red: 0.05, green: 0.47, blue: 0.91, alpha: 1.0)
        UINavigationBar.appearance().barTintColor = UIColor(red: 0.05, green: 0.47, blue: 0.91, alpha: 1.0)
        UINavigationBar.appearance().titleTextAttributes = [NSForegroundColorAttributeName: UIColor.whiteColor()]
        UIApplication.sharedApplication().statusBarStyle = UIStatusBarStyle.LightContent
        var navigationVC:UINavigationController = UINavigationController(rootViewController: ChatVC)
        
        let frame = UIScreen.mainScreen().bounds
        window = UIWindow(frame: frame)
        window!.rootViewController = navigationVC
        window!.makeKeyAndVisible()
```
设置app启动时显示我们自定义的视图控制器,并设置一下导航栏的外观。
ok,第一部分完成。
接下来我们来实现一下第三部分:输入框，我们要把最难的第二部分留在最后( ⊙ o ⊙ )
打开**ChatViewController.swift**文件:
添加一些全局常量,在import下面class的定义之上:
```
let messageFontSize: CGFloat = 17
let toolBarMinHeight: CGFloat = 44
```
第一个是消息所用的字体大小,第二个是我们输入框的高度。
添加一些组成输入框的组件:
```
    var toolBar: UIToolbar!
    var textView: UITextView!
    var sendButton: UIButton!
```
toolBar用来承载输入框中的组件,之所以用UIToolbar是因为它默认出现在屏幕最下方，就像你的短信输入框那样。
textView是我们输入文字的地方，而sendButton则是我们的发送按钮。
下面实现我们重写的**inputAccessoryView**,在这之前先让我们的视图控制器遵循`UITextViewDelegate`协议:
```
class ViewController: UIViewController,UITextViewDelegate {
....
....
}

```
下面添加以下代码来声明对**inputAccessoryView**的重写:
```
 override var inputAccessoryView: UIView! {

}
```
用get的方式将输入框的组件进行配置:
在大括号内部添加代码:
```swift
        get {
            if toolBar == nil {
                
                toolBar = UIToolbar(frame: CGRectMake(0, 0, 0, toolBarMinHeight-0.5))
                
                textView = InputTextView(frame: CGRectZero)
                textView.backgroundColor = UIColor(white: 250/255, alpha: 1)
                textView.delegate = self
                textView.font = UIFont.systemFontOfSize(messageFontSize)
                textView.layer.borderColor = UIColor(red: 200/255, green: 200/255, blue: 205/255, alpha:1).CGColor
                textView.layer.borderWidth = 0.5
                textView.layer.cornerRadius = 5
                //            textView.placeholder = "Message"
                textView.scrollsToTop = false
                textView.textContainerInset = UIEdgeInsetsMake(4, 3, 3, 3)
                toolBar.addSubview(textView)
                
                sendButton = UIButton.buttonWithType(.System) as! UIButton
                sendButton.enabled = false
                sendButton.titleLabel?.font = UIFont.boldSystemFontOfSize(17)
                sendButton.setTitle("发送", forState: .Normal)
                sendButton.setTitleColor(UIColor(red: 142/255, green: 142/255, blue: 147/255, alpha: 1), forState: .Disabled)
                sendButton.setTitleColor(UIColor(red: 0.05, green: 0.47, blue: 0.91, alpha: 1.0), forState: .Normal)
                sendButton.contentEdgeInsets = UIEdgeInsets(top: 6, left: 6, bottom: 6, right: 6)
                sendButton.addTarget(self, action: "sendAction", forControlEvents: UIControlEvents.TouchUpInside)
                toolBar.addSubview(sendButton)
                
                // 对组件进行Autolayout设置
                textView.setTranslatesAutoresizingMaskIntoConstraints(false)
                sendButton.setTranslatesAutoresizingMaskIntoConstraints(false)
   
                toolBar.addConstraint(NSLayoutConstraint(item: textView, attribute: .Left, relatedBy: .Equal, toItem: toolBar, attribute: .Left, multiplier: 1, constant: 8))
                toolBar.addConstraint(NSLayoutConstraint(item: textView, attribute: .Top, relatedBy: .Equal, toItem: toolBar, attribute: .Top, multiplier: 1, constant: 7.5))
                toolBar.addConstraint(NSLayoutConstraint(item: textView, attribute: .Right, relatedBy: .Equal, toItem: sendButton, attribute: .Left, multiplier: 1, constant: -2))
                toolBar.addConstraint(NSLayoutConstraint(item: textView, attribute: .Bottom, relatedBy: .Equal, toItem: toolBar, attribute: .Bottom, multiplier: 1, constant: -8))
                toolBar.addConstraint(NSLayoutConstraint(item: sendButton, attribute: .Right, relatedBy: .Equal, toItem: toolBar, attribute: .Right, multiplier: 1, constant: 0))
                toolBar.addConstraint(NSLayoutConstraint(item: sendButton, attribute: .Bottom, relatedBy: .Equal, toItem: toolBar, attribute: .Bottom, multiplier: 1, constant: -4.5))
            }
            return toolBar
        }
```

你会发现有一个错误,这是因为我们的InputTextView是一个单独定义的类,它还没有定义，我们在之后会对他做一些操作，目前先不用管它，不过我们先把它定义出来，在视图控制器类之外定义该类:
```
class InputTextView: UITextView {
    
    
    
}
```
还有一个问题，用系统默认的代码实现autolayout看起来很难理解，所以这里可以用第三方库SnapKit来实现,把上面设置autolayout的代码替换成以下代码:
```
textView.setTranslatesAutoresizingMaskIntoConstraints(false)             
sendButton.setTranslatesAutoresizingMaskIntoConstraints(false)

 textView.snp_makeConstraints({ (make) -> Void in
                    
                    make.left.equalTo(self.toolBar.snp_left).offset(8)
                    make.top.equalTo(self.toolBar.snp_top).offset(7.5)
                    make.right.equalTo(self.sendButton.snp_left).offset(-2)
                   make.bottom.equalTo(self.toolBar.snp_bottom).offset(-8)
                  
                
                })
                sendButton.snp_makeConstraints({ (make) -> Void in
                    make.right.equalTo(self.toolBar.snp_right)
                     make.bottom.equalTo(self.toolBar.snp_bottom).offset(-4.5)
                    
                })

```
是不是看起来简洁多了?我们来解释一下这段代码:
每一个组件都有一个 `snp_makeConstraints`的闭包方法，用来设置约束,`textView.snp_makeConstraints`就是来设置textView的约束
闭包中` make.left.equalTo(self.toolBar.snp_left).offset(8)`这行代码可以用公式来表示:
也就是`textView.left = self.toolBar.left + 8`，这样一看就很直观了，文字框的左侧距输入框左侧8点。
 `make.top.equalTo(self.toolBar.snp_top).offset(7.5)`可以用公式
`textView.top = self.toolBar.top +7.5`表示，剩下的代码以此类推，如下图所示:
![autoLaout](http://upload-images.jianshu.io/upload_images/727794-31cebfa57f9e484f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
sendButton的部分也是如此:
`make.right.equalTo(self.toolBar.snp_right)`表示发送按钮右侧直接贴输入框的右侧,没有位移
`make.bottom.equalTo(self.toolBar.snp_bottom).offset(-4.5)`发送按钮底部距离输入框底部4.5点
这样是不是让autoLayout变得简单很多了？后面的项目我们就一直使用它来进行autoLayout设置了！
现在没有错误了,cmd+R运行一下，啊哦，为啥是空白！作者你骗人！( ⊙ o ⊙ )
=  =好吧，我们还差一步，记得吗，它要变成第一响应者才能弹出键盘哦,我们要重写一个方法它才能生效!在视图控制器类中增加以下方法:
```
   override func canBecomeFirstResponder() -> Bool {
        return true
    }
```
告诉我们的系统我们自定义的输入框可以成为第一响应者，我们也是有身份证的！
然后在运行一下,如果没有错误，应该会有以下效果:

![chat.gif](http://upload-images.jianshu.io/upload_images/727794-631ea320c1b221a9.gif?imageMogr2/auto-orient/strip)
忽略黑洞洞的背景，因为我们还没有添加内容。。。
但是你会发现一个问题，键盘怎么回来啊。。不管怎么点都没有反应啊！

好吧，下面我们来用一个巧妙的办法来解决它。由于聊天页面是一个**UITableView**，所以我们可以使用**UITableViewContoller**来替代我们的**UIViewContoller**，这样我们的页面中就默认有了一个**UITableView**,然后它有一个非常实用的属性---`keyboardDismissMode`，我们把它设置为`.Interactive`也就是键盘的弹出和收回状态可以根据你对tableView的拖拽进行改变,也就是你的手指拖到哪里你的键盘就到哪里，是不是很酷。
改变视图控制器的类型:
```swift
class ChatViewController:UITableViewController,UITextViewDelegate {
....
....
....
}
```
在viewDidLoad里添加一行代码来设置`keyboardDismissMode`:
```
tableView.keyboardDismissMode = .Interactive
```
再次运行，你会发现黑洞洞的背景不见了,取而代之的是空白的TableView！而且键盘也实现了炫酷的效果！

![chat.gif](http://upload-images.jianshu.io/upload_images/727794-96018225631cff0a.gif?imageMogr2/auto-orient/strip)
[文章本部分源代码](https://github.com/ShyHornet/TuringchatProject/tree/step1)
好的，第三部分顺利实现！第二部分是我们的重头戏，内容较多，所以我把它放到教程的第二部分中。
第二部分教程已经出炉，欢迎围观！
[swift实现一个与智能机器人聊天的app(二)](http://www.jianshu.com/p/f2488a659688)
##如果该文章对你有帮助，请点一下喜欢！您的支持是我继续写作的动力！