##APP 提交上架流程

[原文连接](http://blog.sina.com.cn/s/blog_6ca8877f0101g2u0.html)

由于苹果的机制，在非越狱机器上安装应用必须通过官方的Appstore，
开发者开发好应用后上传Appstore，也需要通过审核等环节。
AppCan作为一个跨主流平台的一个开发平台，也对ipa包上传Appstore作了支持。
本文从三个流程来介绍如何实现AppCan在
线编译出ipa包，以及上传到苹果Appstore。

一、证书的导出

1.1、前期工作

首先你需要有一个苹果的开发者帐号，一个mac系统。
如果没有帐号可以在打开http://developer.apple.com/申请加入苹果的开发者
计划。支付99美元每年，怎么申请网上有详细的介绍，在此不多做介绍。
如果你已经有了一个IDP，打开http://developer.apple.com/并登录到苹果MemberCenter，见下图

![](http://s15.sinaimg.cn/mw690/001Zn3qLty6FCQ9oxcyde&690)

登录以后可以看到下面这个界面，列出了你开发需要的一些工具，支持，itunes app管理等内容。

![](http://s7.sinaimg.cn/mw690/001Zn3qLty6FCQb53jUe6&690)

选择第二项:Certificates, Identifiers & Profiles，进入，所有证书相关的都在这里进行。

在下图的左边选择 Identifiers，我们先创建一个AppId，对于要发布到Appstore上的程序，
都有一个唯一的AppId，下面会列出你当前所有的AppId
我们点击右上角的New App ID

![](http://s13.sinaimg.cn/mw690/001Zn3qLty6FCQl9OEs1c&690)

其中有两项需要你自己填：
第一个Description，用来描述你的appid，这个随便填，没有什么限制；
第二项Bundle Identifier (App ID Suffix)，这是你appid的后缀，这个需要仔细，
因为这个内容和你的程序直接相关，后面很多地方要用到，最好是
 com.yourcompany.yourappname的格式，当然没有公司名的个人开发者，
第二项可以用你自己的英文名字或者拼音，如下图
appcan.cn在线ipa包编译时需要填写的iapp IDs就是你再此输入的第二项内容

![](http://s2.sinaimg.cn/mw690/001Zn3qLty6FCQk9Bi971&690)

填完后submit，如下图，可以看见我们已经生成的appid：ebook appid。想要支持推送服务和icould等也可以在这儿配置：![](http://s14.sinaimg.cn/mw690/001Zn3qLty6FCQoRuqh2d&690)
1.3、申请发布证书

1.3.1、先创建一个证书请求文件

这儿需要一个mac系统。以下内容以雪豹系统为例，其他版本差别不是很大。
首先打开应用程序-实用工具-钥匙串访问（KEY CHAIN）,在证书助理中，选择"从证书颁发机构求证书"，如下图
![](http://s2.sinaimg.cn/mw690/001Zn3qLty6FCQqdyrT01&690)
在下图所示的界面，你的电子邮件地址：填你申请idp的电子邮件地址，常用名称，默认就好，CA空，
选择存贮到磁盘，点击"继续"：

![](http://s10.sinaimg.cn/mw690/001Zn3qLty6FCQrdf9n49&690)
![](http://s9.sinaimg.cn/mw690/001Zn3qLty6FCQsnVPid8&690)

下一步点击完成，你就可以看到你的桌面多了一个CertificateSigningRequest.certSigningRequest的证
书请求文件。


生成对应的发布配置概要文件（授权文件）
选择Provisioning Profiles----> Distribution
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/317394-be89361a04023c56.png)
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/317394-af723f16cae4917e.png)
下载完成后，双击安装，安装成功后，可以在你的钥匙串里面的证书下面看到这个中级证书。

1.3.3、请求一个发布证书

OK，现在来请求一个真正的发布证书，还是在这个页面，点击request certificate

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/317394-428ce378af0c7275.png)

这个页面告诉你怎么生成发布证书，点击下面的"选取文件"，选择你在第一步创建的证书请求文件，
然后点击"submit"

![](http://s9.sinaimg.cn/mw690/001Zn3qLty6FCQxOQru78&690)

OK。现在你有一个证书可以下载了，如下图（不能下载请刷新页面）

![](http://s13.sinaimg.cn/mw690/001Zn3qLty6FCQzXn2scc&690)

1.3.4、安装和导出

点击"download"下载你生成的证书，下载完成后双击安装，如果有如下提示，选择login，OK

![](http://s9.sinaimg.cn/mw690/001Zn3qLty6FCQBAYEU88&690)

这时再查看你的钥匙串，应该有下面这一行Iphone Distribution的证书，注意，这个证书有一个小三角可以点击，
展开后有一个对应的密钥。如果你没有这个钥匙，那么请检查上面那一步做错了。

![](http://s10.sinaimg.cn/mw690/001Zn3qLty6FCQCo8aZe9&690)

现在发布证书已经安装了，我们选择这个证书，右击，选择，导出"xxxxxxx"，如下图

![](http://s5.sinaimg.cn/mw690/001Zn3qLty6FCQDxM2w44&690)

给你要导出的证书起个名字，选择一个存的位置，注意，保存成P12的信息交换文件

![](http://s10.sinaimg.cn/mw690/001Zn3qLty6FCQK2ex389&690)

输入密码，如果mac系统有密码，后面还会要求你输入系统密码。

![](http://s1.sinaimg.cn/mw690/001Zn3qLty6FCQKUSkg60&690)

现在你就有了发布程序需要的p12文件。