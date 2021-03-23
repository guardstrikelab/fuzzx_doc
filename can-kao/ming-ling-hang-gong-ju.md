---
description: 该命令行工具需要安装在用户的开发机器上，以便与FuzzX平台配合进行上传前的监测和发现缺陷后的修复缺陷过程。
---

# 命令行工具

## FuzzX CLI 的本地配置

测试环境：`Ubuntu:18.04`

### 1. 直接使用可执行文件（推荐）

1. 从代码仓库下载编译过后的二进制文件
2. 将该二进制文件放在工程项目的根目录下\(即与`fuzzx.yaml`处于同一目录\)
3. 在该目录执行如下命令即可看到 `fuzzx` 的提示：

{% tabs %}
{% tab title="Ubuntu" %}
```bash
$ mv fuzzx_for_Ubuntu fuzzx
$ ./fuzzx
```
{% endtab %}

{% tab title="MacOS" %}
```text
$ mv fuzzx_for_MacOS fuzzx
$ ./fuzzx
```
{% endtab %}
{% endtabs %}

> 务必在执行每句代码时在`fuzzx`之前加上`./`来指明您要运行的是放在当前目录下的`fuzzx`文件。

### 2.源码编译

1. 确保你的机器有Golang语言环境:

   ```text
   $ go version
   ```

   如果没有，登陆[Golang中文网站](https://golang.google.cn/)下载。

   然后为Go更换国内proxy：

   ```text
   $ go env -w GOPROXY=https://goproxy.cn,direct
   ```

2. 创建Go语言工作空间：\

   **我们默认工作空间路径为`~/go/`,请根据您的实际情况填写**

   ```text
   $ mkdir ~/go/src 
   $ cd ~/go/src
   ```

3. 拉取命令行程序源码： \(_目前仓库为private_\)

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

```bash
$ echo "export GOBIN=~/go/bin" >> ~/.bashrc
$ echo "export PATH=$PATH:$GOBIN" >> ~/.bashrc
$ source ~/.bashrc
```

## FuzzX CLI的使用

> 默认您直接使用二进制文件，如果您是自行从源码编译，请在执行命令前添加环境变量，并去掉命令中的\`./\`。

### login

使用该命令进行身份确认，完成`login`的用户才能使用CLI的其他命令

具体命令如下：

```bash
$ ./fuzzx login <id> <key>
```

_&lt;id&gt; 和 &lt;key&gt; 就是您在`FuzzX`平台的_[_个人信息_](https://guardstrikelab.gitbook.io/fuzzx/ping-tai/nei-bu-xi-jie#8-yong-hu-xin-xi)_处所看到的。_

### logout

登出功能

```bash
$ ./fuzzx logout
```

### validate

当您在配置项目时，使用该命令可以验证本地编辑的`fuzzx.yaml`文件的正确性：

```bash
$ ./fuzzx validate
```

### build

该命令用于在本地重现出我们在服务端提供给您的错误信息：

（在bug详情页可以看到复现所需的命令）

```bash
$ ./fuzzx build <bugkey>
```

