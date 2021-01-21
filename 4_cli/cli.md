# cli

# FuzzX CLI 

doc of Command Line Interface for FuzzX.



## 如何配置

 测试环境：`Ubuntu:18.04`


1.	确保你的机器有Golang语言环境:

	```shell
	$ go version
	```

2.	拉取命令行程序源码：

	*目前仓库为private*

	```shell
	$ git clone https://github.com/guardstrikelab/fuzzx_cli.git
	```

3.	将源码拷贝至`go`工作空间的`src`目录下：

		**默认工作路径为`~/go/`,请根据实际情况填写**

	```shell
	$ cp fuzzx_cli/ ~/go/src/fuzzx/
	```
	
4.	进入项目文件夹，安装：

	```shell
	$ cd ~/go/src/fuzzx
	$ go install
	```

5.	添加环境变量

	```shell
	$ export GOBIN=~/go/bin
	$ export PATH=$PATH:$GOBIN
	```

6.	测试命令

	```shell
	$ fuzzx
	```


注意：5.中添加环境变量命令更换终端后则失效，替换为以下命令则可在当前用户下永久有效。
```shell
$ echo "export GOBIN=~/go/bin" >> ~/.bashrc
$ echo "export PATH=$PATH:$GOBIN" >> ~/.bashrc
$ source ~/.bashrc
```


