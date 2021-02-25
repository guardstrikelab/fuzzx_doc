---
description: doc of Command Line Interface for FuzzX.
---

# 命令行工具

FuzzX CLI 的本地配置方法

测试环境：`Ubuntu:18.04`


---


## 1.直接使用可执行文件

1.	从代码仓库下载编译过后的二进制文件	  

2.	将该二进制文件放在工程项目的根目录下(即与`fuzzx.yaml`处于同一目录)  

3.	在该目录执行如下命令即可看到 `fuzzx` 的提示：

	```shell
	$ ./fuzzx
	```

---


## 2.源码编译



1.	确保你的机器有Golang语言环境:

	```shell
	$ go version
	```
	
	如果没有，登陆[Golang中文网站](https://golang.google.cn/)下载。
	

	**别忘记为Go更换国内proxy：**

	```shell
	$ go env -w GOPROXY=https://goproxy.cn,direct
	```


2.	创建Go语言工作空间：

	
	```shell
	$ mkdir ~/go/src 
	$ cd ~/go/src 
	```
	
	**我们默认工作空间路径为`~/go/`,请根据您的实际情况填写**

3.	拉取命令行程序源码：

	*目前仓库为private*

	```shell
	$ git clone https://github.com/guardstrikelab/fuzzx_cli.git ./fuzzx
	```

4.	进入项目文件夹，安装：

	```shell
	$ cd fuzzx
	$ go install
	```

5.	添加环境变量

	```shell
	$ export GOBIN=~/go/bin
	$ export PATH=$PATH:$GOBIN
	```

6.	输入命令进行测试，根据提示操作即可。

	```shell
	$ fuzzx
	```



注意：5.中添加环境变量命令更换终端后则失效，替换为以下命令则可在当前用户下永久有效。
```shell
$ echo "export GOBIN=~/go/bin" >> ~/.bashrc
$ echo "export PATH=$PATH:$GOBIN" >> ~/.bashrc
$ source ~/.bashrc
```

