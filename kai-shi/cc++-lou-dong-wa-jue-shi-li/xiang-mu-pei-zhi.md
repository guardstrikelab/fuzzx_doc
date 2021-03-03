# 项目配置

为了实现自动化测试，我们需要为自己的项目提供两个文件：
    
    `fuzzx.yaml`
    `target.cc`

`fuzzx.yaml`文件（后称`yaml`文件）为项目的配置文件，`fuzzx`将按照此文件的指示来执行您规定的`fuzz`任务。关于此文件的编写，请参考[这里](can-kao/pei-zhi-wen-jian.md)。


`target.cc`文件（后称`target`文件）内包含您对测试工具的初始化选项以及您需要测试的函数的描述。

---

我们为您提供了一个样例来帮助您更快上手。[点击获取样例](https://github.com/guardstrikelab/fuzzx_cpp_demo)

```
fuzzx_cpp_demo  
    |---openssl  
    |   └---  项目内的文件   
    |        
    |---fuzzx.yaml  
    |  
    └---target.cc 
```

[点击查看yaml文件](https://github.com/guardstrikelab/fuzzx_cpp_demo/blob/master/fuzzx.yaml)

[点击查看target文件](https://github.com/guardstrikelab/fuzzx_cpp_demo/blob/master/target.cc)

