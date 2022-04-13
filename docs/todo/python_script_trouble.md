# Python跑数遇到的问题
1、技术方案
	多线程：速度慢，
	多进程+多协程：控制不住，耗尽机器所有内存
	单进程+多协程(协程池)：速度可控，大约60个请求/s，但是会超过65536个链接，会有很多失败，

2、请求返回json打印成日志后，无法使用json.loads
	使用eval()解决。https://www.jianshu.com/p/3a7dbd17e7b9
3、gevent导入问题：https://blog.csdn.net/a19990412/article/details/82966452
4、超过最大链接数：maximum recursion depth exceeded while calling a Python object
	
5、Python写入json文件到excel
	https://blog.csdn.net/destinymf/article/details/78096678
6、多进程+协程示例：https://segmentfault.com/a/1190000038437451
7、提高爬虫效率-多线程：https://zhuanlan.zhihu.com/p/55057487
8、golang操作excel：https://segmentfault.com/a/1190000037585645



参考
[Python requests接口测试小技巧](https://testerhome.com/topics/19899)
[基于协程的python网络库gevent](http://www.bjhee.com/gevent.html)