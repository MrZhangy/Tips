
#LLDB经常会用到的命令

###命令p 和 po
在调试器中最常用到的命令是p（用于输出基本类型）或者po（用于输出 Objective-C 对象）。

p (int)[[[self view] subviews] count]




###命令 expr
可以在调试时动态执行指定表达式，并将结果打印出来。常用于在调试过程中修改变量的值。
expr a=2 打印结果为2

expr a=2

expr int $b=2

p $b
###命令 call
动态修改程序属性
如执行 call [self.view setBackgroundColor:[UIColor redColor]] 继续运行程序，view的背景颜色将会变成红色

实践经验：在调试的时候灵活运用call命令可以起到事半功倍的作用。


参考网址 [LLDB调试命令初探](http://blog.csdn.net/chaoyuan899/article/details/21514079)
