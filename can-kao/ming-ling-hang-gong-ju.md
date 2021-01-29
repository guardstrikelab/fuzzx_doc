---
description: doc of Command Line Interface for FuzzX.
---

# 命令行工具

## 如何配置

测试环境：`Ubuntu:18.04`

1. 确保你的机器有Golang语言环境:

   ```text
   $ go version
   ```

2. 为`FuzzX`创建工作路径：

   ```text
   $ cd ~/go/src 
   $ mkdir fuzzx
   ```

   **我们默认工作路径为`~/go/`,请根据实际情况填写**

3. 拉取命令行程序源码：

   _目前仓库为private_

   ```text
   $ git clone https://github.com/guardstrikelab/fuzzx_cli.git ./fuzzx
   ```

4. 进入项目文件夹，安装：

   ```text
   $ cd fuzzx
   $ go install
   ```

5. 添加环境变量

   ```text
   $ export GOBIN=~/go/bin
   $ export PATH=$PATH:$GOBIN
   ```

6. 输入命令进行测试，根据提示操作即可。

   ```text
   $ fuzzx
   ```

注意：5.中添加环境变量命令更换终端后则失效，替换为以下命令则可在当前用户下永久有效。

```text
$ echo "export GOBIN=~/go/bin" >> ~/.bashrc
$ echo "export PATH=$PATH:$GOBIN" >> ~/.bashrc
$ source ~/.bashrc
```

