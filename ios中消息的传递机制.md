##IOS中消息的传递机制


[iOS中消息的传递机制 参考连接](http://www.cocoachina.com/industry/20131216/7543.html)

delegate, block , kvo , Notification, target--action, 

本文将`介绍所有可用的消息传递机制`，并通过示例来介绍`这些机制在苹果的Framework中如何使用`，同时，`还介绍了一些最佳实践建议，告诉你什么时机该选择使用什么机制`。


###kvo###
`	KVO提供了一个这样的机制`：当对象中的某个属性发生了变化，可以对这些值的观察者做出通知。

`注意事项`: 1、接收着必须知道发送者， 2、接收者同样还需要知道发送者的生命周期，因为在销毁发送者对象之前，需要取消观察者的注册。如果这两个要求都满足了，消息传递过程中可以是1对多(多个观察者可以注册某个对象中的值)。

`使用时机`:如果对某个对象中值的改变情况感兴趣，那么可以使用KVO消息传递机制。

###Notification###
在不相关的两部分代码中要想进行消息传递，通知（notification）是非常好的选择机制，他可以对消息进行广播。特别是想要传递丰富的信息，并且不一定指望有谁对此消息关心。

`实践建议`：通知可以用来发送任意的消息，甚至包含一个userInfo字典，或者是NSNotifacation的一个子类。通知的独特之处就在于发送者和接收者双方并不需要相互知道。这样就可以在非常松耦合的模块间进行消息的传递。记住，这种消息传递机制是单向的，作为接收者是不可以回复消息的。

`使用时机`：在不相关的两部分代码中要想进行消息传递

###delegare###
delegation允许我们定制某个对象的行为，并且可以收到某些确定的事件。

`实践建议`:消息的发送者需要知道消息的接收者(delegate)，反过来就不用了。这里的发送者和接收者是比较松耦合的，因为发送者只知道它的delegate是遵循某个特定的协议。

`实践建议`：delegate协议可以定义任意的方法，因此你可以准确的定义出你所需要的类型。你可以用函数参数的形式来处理消息内容，delegate还可以通过返回值的形式给发送者做出回应。如果只需要在相对接近的两个模块之间进行消息传递，那么Delegation是一种非常灵活和直接方式。

 `使用时机`:在相对接近的两个模块之间进行消息传递，消息的发送者需要知道消息的接收者(delegate)，那么Delegation是一种非常灵活和直接方式。
 
###Block###

 `历史`：Block相对来说，是一种比较新的技术，它首次出现是在OS X 10.6和iOS 4中。
 
 一般情况下，block可以满足用delegation实现的消息传递机制。不过这两种机制都有各自的需求和优势。
 
  `使用时机`:在2个相对接近的两个模块间进行消息传递。
  
  `实践建议`：block的循环引用
  
###Target-Action###
`Target-action`主要被用于相应用户界面中事件的消息传递。iOS中的UIControl和Mac中的NSControl/NSCell都支持这种机制。

`实践建议`:
消息的接收者不知道发送者，甚至消息的发送者不需要预先知道消息的接收者。如果target是nil，action会在响应链(responder chain)中被传递，知道找到某个能够响应该aciton的对象。在iOS中，每个控件都能关联多个target-action。

基于target-action消息传递的机制有一个局限就是发送的消息不能携带自定义的payload。




####＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝###
###做出正确的选择###
###KVO###
NSOperationQueue就是lion给了KVO来观察队列中operation状态属性的改变情况(isFinished, isExecuting, isCancelled)。当状态发生了改变，队列会受到一个KVO通知。

###Notifications####

Core Data使用notification来传递事件(例如一个managed object context内部的改变——NSManagedObjectContextDidChangeNotification)。
 
change notification是由managed object context发出的，所以我们不能确定消息的接收者一定知道发送者。如果消息并不是一个UI事件，而有可能多个接收者对该消息感兴趣，并且消息的传递属于单向(one-way communication channel)，那么notification是最佳选择。

###delegate
able view的delegate有多种功能，从accessory view的管理，到屏幕中cell显示的跟踪，都与delegate的功劳。例如，我们来看看 tableView:didSelectRowAtIndexPath: 方法。为什么要以delegate调用的方式来实现？而又为啥不用target-action方式？
 
正如我们在流程图中看到的一样，使用target-action时，不能传递自定义的数据。而在选中table view的某个cell时，collection view不仅仅需要告诉我们有一个cell被选中了，还需要告诉我们是哪个cell被选中了(index path)。按照这样的一种思路，那么从流程图中可以看到应该使用delegation机制。


###Target-Action###
Target-Action用的最明显的一个地方就是button(按钮)。button除了需要发送一个click事件以外，并不需要再发送别的信息了。所以Target-Action在用户界面事件传递过程中，是最佳的选择。

如果taget已经明确指定了，那么action消息回直接发送给指定的对象。如果taget是nil，action消息会以冒泡的方式在响应链中查找一个能够处理该消息的对象。此时，我们拥有一种完全解耦的消息传递机制——发送者不需要知道接收者，以及其它一些信息。
 