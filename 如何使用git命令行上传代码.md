
#如何使用git命令行上传代码#

`…or create a new repository on the command line`

<pre><code>
echo "# HelloWorld" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/MrZhangy/HelloWorld.git
git push -u origin master
</code></pre>

`…or push an existing repository from the command line`

<pre><code>
git remote add origin https://github.com/MrZhangy/HelloWorld.git
git push -u origin master
…or import code from another repository
</code></pre>
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

Import code


[mac使用github](http://www.cnblogs.com/heyonggang/p/3462191.html)

[mac使用github参考](http://blog.csdn.net/hcbbt/article/details/11651229)

[廖雪峰的github教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743858312764dca7ad6d0754f76aa562e3789478044000)
