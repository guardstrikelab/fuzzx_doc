---
description: 目标程序从FuzzX接收测试用例，运行并报告发现的缺陷。本页介绍如何构建一个目标程序。
---

# 目标程序

每个目标程序包含两部分：

1. 一个可以让`FuzzX`进行传参的方法————像是单元测试框架。
2. `fuzzx.yaml`文件中的target字段————描述如何运行目标程序，定义错误的显示格式。

## 测试方法

{% tabs %}
{% tab title="C" %}
要Fuzz您的C语言代码，创建一个使用下述方法的文件：

{% code title="target.c" %}
```c
int LLVMFuzzerTestOneInput(const uint8_t *Data, size_t Size) {
    //第一步：读取所需的数据类型
    //第二步：运行您的代码（方法）
    //第三步：查错，发现错误立即终止并报告
    //字节数组长度不够则返回-1
}
```
{% endcode %}
{% endtab %}

{% tab title="C++" %}
要Fuzz您的C++语言代码，创建一个使用下述方法的文件：

{% code title="target.cc" %}
```cpp
extern "C" int LLVMFuzzerTestOneInput(const uint8_t *Data, size_t Size) {
    //第一步：读取所需的数据类型
    //第二步：运行您的代码（方法）
    //第三步：查错，发现错误立即终止并报告
    //字节数组长度不够则返回-1
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

_您也可以像是在单元测试一样————使用断言查错，反正`FuzzX`会把断言失败也当成错误。_

该方法接收数组`Data`中定义的测试用例并将其转化为目标所需求的测试数据。 例如：如果你的代码（或者说是你要测试的方法）需要两个整数作为输入，则从字节数组中解析两个整数。

这样的接口十分灵活：你可以从输入的字节数组中解析出任意类型的数据、结构体或是对象，进而生成复杂的测试结构体。

当然，如果输入的字节数组不够长，会立马返回`-1`。

而我们的`FuzzX`将会马上意识到数组长度不够并及时做出调整！

FuzzTarget写法可以[参考这里](https://llvm.org/docs/LibFuzzer.html#fuzz-target)。

## 配置方法

> 此部分仅提供一部分参考，具体教程，请您[点击这里](https://guardstrikelab.gitbook.io/fuzzx/can-kao/pei-zhi-wen-jian)。

目标程序需要在`fuzzx.yaml`文件中用`target`字段描述,以下是我们提供的[c/c++样例](https://guardstrikelab.gitbook.io/fuzzx/kai-shi/cc++-lou-dong-wa-jue-shi-li)中的配置详情：

{% code title="fuzzx.yaml" %}
```yaml
# --------  全局配置
language: c++
target:
  name: heartbleed      #名称，目标文件的唯一标识
  setup:                #构建步骤
    - cd openssl && CC="$CC -g -fsanitize=address,fuzzer-no-link" ./config && make clean && make && cd ..
    - $CXX -g ./target.cc -fsanitize=address,fuzzer openssl/libssl.a  openssl/libcrypto.a -std=c++17 -DCERT_PATH=\"$PWD/runtime\" -I openssl/include -lstdc++fs -ldl -lstdc++ -o ./target
  corpus: ./corpus      #语料库路径
  harness: ./target     #生成的可执行文件路径
```
{% endcode %}

我们在此对`target.setup`字段中的各条命令做出简要的解释说明，以帮助理解：

在全局配置部分，我们已将环境变量设置为：`CC=clang`,`CXX=clang++` ；所以第一条的命令等价于：

```bash
$ cd openssl
$ CC="$CC -g -fsanitize=address,fuzzer-no-link" ./config 
$ make clean 
$ make 
$ cd ..
```

1.  进入`openssl`文件夹，该文件夹即对应您要测试的项目的文件夹；
2. 将环境变量`CC`重新设置为`clang -g -fsanitize=address,fuzzer-no-link`，执行当前文件夹下的`config`（当然，您可以在我们给出的C/C++示例中的对应目录下找到到我们所说的文件）。即`config`中所有出现`CC`的地方都换为了新值，而新增flag的含义是：
   * `-g` 的含义为保留调试信息（该选项为非必须选项，也可以去掉）；
   * `-fsanitize=address,fuzzer-no-link` 选项声明了需要启动`libFuzzer`，同时启动了地址检查。_（由于此处的目的仅仅是编译，所以选择了`fuzzer-no-link`而不是`fuzzer`  ，但实际实验发现二者在编译期的功能完全一致，只不过`fuzzer-no-link`强制在链接阶段不生效，而后者在链接阶段也生效。当然，如果您追求简单统一，也可以直接使用`fuzzer`关键词替换掉`fuzzer-no-link`。）_
3. 而`make clean`就是清除之前的`make`信息；（别忘了，我们已经在全局配置中添加了`make`的安装）
4. 然后运行`make`，生成`Makefile`；
5. 退回FuzzX的根目录 

第二条的命令，也就等价于：

```bash
#别忘了，CXX的值已设置为clang++，所以 $CXX 会被自动替换为 clang++
$ $CXX -g ./target.cc -fsanitize=address,fuzzer openssl/libssl.a  openssl/libcrypto.a -std=c++17 -DCERT_PATH=\"$PWD/runtime\" -I openssl/include -lstdc++fs -ldl -lstdc++ -o ./target
```

./target.cc 即为我们在目录下已经写好的目标文件

-fsanitize=address,fuzzer 如前所说，开启了libFuzzer和 地址分析 



做完这些准备工作之后，把项目提交到`FuzzX`，它就会自行检测您的目标程序了。

了解具体操作，请[点击这里](https://guardstrikelab.gitbook.io/fuzzx/kai-fa-wen-dang/yun-hang-ce-shi)。

