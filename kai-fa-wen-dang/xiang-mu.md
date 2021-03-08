# 项目

##  链接项目

项目需要被连接到`FuzzX`，这样我们的`FuzzX`才可以自动化获取到您的代码提交。

在登录到`FuzzX`主页之后，您可以看到如下图所示的界面：











##  配置项目

每个项目的根目录下都需要放置一个`fuzzx.yaml`文件，该文件定义了如何搭建环境和如何进行`fuzzing`。我们给出在`c/c++`示例中的配置文件：

```yaml
base: ubuntu:18.04
environment:
  - CC=clang
  - CXX=clang++
setup:
  - sudo apt-get -y update
  - sudo apt-get -y install git clang make 
language: c++
target:
  name: heartbleed
  setup:
    - cd openssl && CC="$CC -g -fsanitize=address,fuzzer-no-link" ./config && make clean && make && cd ..
    - $CXX -g ./target.cc -fsanitize=address,fuzzer openssl/libssl.a  openssl/libcrypto.a -std=c++17 -DCERT_PATH=\"$PWD/runtime\" -I openssl/include -lstdc++fs -ldl -lstdc++ -o ./target
  corpus: ./corpus
  harness: ./target
```

`base`字段声明了为您运行代码的系统环境：`ubuntu:18.04`；
`environment`字段声明了您运行前需要设置的环境变量；
`setup`字段声明了您的项目需要安装的依赖；
`language`字段声明了您所使用的编程语言；
`target`字段内的各项信息描述了您的`target`，`name`为名称，`setup`为编译和插桩指令，`corpus`为语料库路径，`harness`为插桩后生成的可执行文件路径。



想为您的项目编写`fuzzx.yaml`文件，请[点击这里](https://gustrikelab.gitbook.io/fuzzx/can-kao/pei-zhi-wen-jian)。