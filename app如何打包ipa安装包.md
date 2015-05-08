###app如何打包ipa安装包###

1、把运行目标改为ios Device

2、选择“Product”->"Edit Scheme..."打开如下界面

3、在“Build Configuration”中选择“Release”,单击"OK"

4、选择菜单栏中的"Product"->"Archive"

5、之后等等待几秒钟出现如下操作框。选择“Distribute...”

6、弹出如下提示框，选择第二项，点击“Next”

7、在弹出的界面中选择和第2步中相同的证书，点击“Next”

8、等待几秒，弹出保存界面设置包名称，点击“Save”




###iOS --﻿No provisioning profiles with a valid signing identity 一种解决方法###

1、删除原有“钥匙串访问”中疑是过期的的证书；

2、在Member Center中Certificate中删除疑是有问题的Certificate，重新添加新的Certificate；

3、在“钥匙串访问” - 证书处理 - 从证书颁发机构请求证书，生成新的CertificateSigningRequest.certSigningRequest；

4、在2中选取3中的CSR文件，生成新的Certificate；

5、在Target 中的Team 选择，Fix issue；

问题得到解决。

http://developer.apple.com
###发布应用到App Store 详细教程
http://blog.csdn.net/songrotek/article/details/8566479


###ios设置应用程序图标
http://www.tuicool.com/articles/IFBFny

###iphone开发必知点之－－app图标
http://www.cnblogs.com/xiaodao/archive/2012/09/28/2707652.html