---
description: 目标程序从FuzzX接收测试用例，运行并报告发现的缺陷。本页介绍如何构建一个目标程序。
---

# 目标程序

>   此页告诉您如何为FuzzX编写新的目标程序。如果您已经有了LibFuzzer的目标程序，请忽略此页，直接看[这里](https://gustrikelab.gitbook.io/fuzzx/kai-fa-wen-dang/qian-yi-dao-fuzzx)


每个目标程序包含两部分：

1.  一个可以让`FuzzX`进行传参的方法————像是单元测试框架。
2.  `fuzzx.yaml`文件中的target字段————描述如何运行目标程序，定义错误的显示格式。


##  测试方法

{% tabs %} {% tab title="C" %}
要Fuzz您的C语言代码，创建一个使用下述方法的文件：

{% code-tabs %} {% code-tabs-item title="target.c" %}

```c
int LLVMFuzzerTestOneInput(const uint8_t *Data, size_t Size) {
    //第一步：读取所需的数据类型
    //第二步：运行您的代码（方法）
    //第三步：查错，发现错误立即终止并报告
    //字节数组长度不够则返回-1
}
```

{% endcode-tabs-item %} {% endcode-tabs %}
  
{% endtab %}

{% tab title="C++" %}
要Fuzz您的C++语言代码，创建一个使用下述方法的文件：

{% code-tabs %} {% code-tabs-item title="target.cc" %}

```cpp
extern "C" int LLVMFuzzerTestOneInput(const uint8_t *Data, size_t Size) {
    //第一步：读取所需的数据类型
    //第二步：运行您的代码（方法）
    //第三步：查错，发现错误立即终止并报告
    //字节数组长度不够则返回-1
}
```
  
{% endcode-tabs-item %} {% endcode-tabs %}

{% endtab %} {% endtabs %}

*您也可以像是在单元测试一样————使用断言查错，反正`FuzzX`会把断言也当成错误。*


该方法接收数组`Data`中定义的测试用例并将其转化为目标所需求的测试数据。
例如：如果你的代码（或者说是你要测试的方法）需要两个整数作为输入，则从字节数组中解析两个整数。

这样的接口十分灵活：你可以从输入的字节数组中解析出任意类型的数据、结构体或是对象，进而生成复杂的测试结构体。

当然，如果输入的字节数组不够长，会立马返回`-1`。

而我们的`FuzzX`将会马上意识到数组长度不够并及时做出调整！


##  配置方法

>   此部分仅提供一部分参考，具体教程，请您[点击这里](https://gustrikelab.gitbook.io/fuzzx/can-kao/pei-zhi-wen-jian)。

目标程序需要在`fuzzx.yaml`文件中用`target`字段描述,以下是我们提供的[c/c++样例](https://gustrikelab.gitbook.io/fuzzx/kai-shi/cc++-lou-dong-wa-jue-shi-li)中的配置详情：


{% tabs %} {% tab title="C" %}
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
{% endtab %} {% endtabs %}


//
待补充······
//


做完这些准备工作之后，把项目提交到`FuzzX`，它就会自行检测您的目标程序了。

了解具体操作，请[点击这里](https://gustrikelab.gitbook.io/fuzzx/kai-fa-wen-dang/yun-hang-ce-shi)。

