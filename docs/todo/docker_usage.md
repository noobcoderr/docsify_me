# 《Docker使用场景》
## 一、和docker容器进行文件交互
	1、获取docker容器id
	2、本机往docker容器内复制 docker cp 本机路径/文件名 容器id:/目标路径/文件名
	2、docker往本机复制文件 docker cp 容器id:/目标路径/文件名 本机路径/文件名

## 二、docker 容器内配置修改
	docker内的文件是只读的，但是提供了一种方式
	KONG_ + 配置名 = 期望值
	比如我要设置go插件可执行目录为/usr/local/bin
	那么启动docker kong的时候
	-e "KONG_GO_PLUGINSERVER_EXE=/usr/local/bin/go-pluginserver" \

前2步可以将生成的插件文件复制到容器内的指定目录，然后通过启动时的配置，完成要求的配置更改。

## 三、配置了插件后，konga连接kong失败，如何查看原始日志
	1、docker启动容器立马失败，查看该容器的日志
		docker logs 容器id
		提示sh: /usr/local/bin/go-pluginserver: not found
		分析：我们启动容器用的是镜像kong:latest，为官方镜像，内部不含有我们的修改设置。
		思路：
			1、使用官方镜像为基础，新增我们需要的文件，打包成新的镜像，并使用新的镜像进行启动
			2、按官方镜像进行启动容器，启动后，在容器内修改对应配置后，重启容器	
				1、文档描述配置文件为/etc/kong/kong.conf，但是kong目录下只有kong.conf.default 和kong.logrotate俩文件，且都是只读文件，且无法通过命令新增文件
	改变思路，对docker镜像相关操作不太熟悉，所以使用原文件安装kong、以及konga，先在本地测试




## kong源文件安装
1、mac使用brew安装kong出错，且无法解决，转为使用原文件安装,相关文档较少
	后续，brew用的国内清华源，执行brew update后，重新执行，安装成功
	export PATH="$PATH:/usr/local/Cellar/kong/2.5.1/bin"
	export PATH="$PATH:/usr/local/Cellar/openresty@1.19.3.2/1.19.3.2/bin"
	退出后 记得source .zshrc 生效

转换思路，使用现有的lua脚本插件

1、明明已经安装canary，但是启动kong提示未找到
	kong要求的  /usr/local/share/lua/5.1/kong/init.lua:515:,需要安装在 /usr/local/share/lua/5.1/kong/plugins
	通过查询文件夹 find ./local -name canary -type d (查找文件用的是 find / -name 'server.xml' -print)
	发现canary安装在了 usr/local/share/lua/5.4/kong/plugins/canary
	发现我们使用了5.4版本的lua，所以重新下载5.1的lua（https://blog.triplez.cn/posts/install-lua-and-luarocks-in-macos/）
	三个地方有canary
	./local/lib/luarocks/rocks-5.4/canary
	./local/Cellar/luarocks/3.7.0/share/lua/5.4/kong/plugins/canary
	./local/share/lua/5.4/kong/plugins/canary
	把最后一个复制一份到5.1试试,cp -r ./5.4/kong/plugins/canary ./5.1/kong/plugins/canary
	失败，一堆依赖报错
	nginx: [error] init_by_lua error: /usr/local/share/lua/5.1/kong/tools/utils.lua:705: error loading module 'kong.plugins.canary.handler':
.../share/lua/5.1/kong/plugins/canary/policies/IPCanary.lua:6: module 'resty.iputils' not found:

	cp ./go-pluginserver ~/usr/local/bin/go-pluginserver
	修改kong.conf报只读错误，更改权限为chmod 777
	报错Error: /usr/local/share/lua/5.1/kong/cmd/start.lua:64: /usr/local/bin/go-pluginserver flag redefined: kong-prefix
panic: /usr/local/bin/go-pluginserver flag redefined: kong-prefix
	可能为kong版本的问题，目前版本为2.5.1，选择2。0版本重新安装，brew安装指定版本的软件https://www.jianshu.com/p/b8909508bb8e
	无法选择版本进行安装。这篇教如何使用go安装https://www.bilibili.com/s/video/BV1Z44y1q7Ko


