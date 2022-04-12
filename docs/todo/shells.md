#《shell命令》

## xargs


## grep
	基础用法
	grep xxx filename
	或用法
	使用\|
		grep 'A\|B' filename
	使用-E 开启正则
		grep -E 'pattern1|pattern2' filename
	使用 egrep ，等价于 grep -E
	https://blog.csdn.net/jackaduma/article/details/6900242

## sed：利用脚本来处理、编辑文本文件
	
	例子：去除字符串内的所有空格
	"my name" | sed 's/ //g'
	此处替换使用到的是  
	s/old/new/g：将当前行所有的old替换成new，s是substitute，g是global



## tee：
	
	例子：将数据写入文件
	"pay log" | tee pay.log


## vim
	例子：将第二行开始的xxx改为squash
	:2,$s/pick/squash/g
	此处替换使用到的是：
	:1,10s/old/new/g：将第1到第10行所有的old替换成new，最后一行的话将10换为$


## 参考链接
[vim文本替换](https://segmentfault.com/a/1190000004443210)
[linux sed命令](https://www.runoob.com/linux/linux-comm-sed.html)
