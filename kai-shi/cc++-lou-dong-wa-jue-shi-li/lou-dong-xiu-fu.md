# 漏洞修复

`fuzzx`已经发现了我们的漏洞，接下来我们在我们的开发端查看该漏洞并进行修复。

1. 选择主目录或是您自行指定的文件夹，克隆我们的`fuzzx`正在运行的代码：

   ```text
   git clone https://github.com/guardstrikelab/fuzzx_cpp_demo.git
   ```

2. 然后请您按照提示配置好我们的[本地命令行工具](https://gustrikelab.gitbook.io/fuzzx/can-kao/ming-ling-hang-gong-ju)。

   **可执行文件`fuzzx`应该放在`fuzzx_cpp_demo`文件夹内**

3. 点击`fuzzx`平台的个人中心（页面右上角），您可以看到您的`id`和`key`。在终端输入如下格式的指令：

   ```text
   ./fuzzx login 您的id 您的key
   ```

   登陆成功会有相应提示。

4. 之后到`fuzzx`平台查看错误的提示，根据提示输入命令，即可在本地复现出错误。
5. 根据错误的提示修改代码的缺陷，重新提交。
6. 在`fuzzx`平台重新进行`fuzzx`，可以看到该缺陷已被修复。

