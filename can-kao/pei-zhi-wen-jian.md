---
description: 为了让 FuzzX 知晓如何编译和运行目标程序，需要用户准备一个配置文件，这个文件必须命名为 fuzzx.yaml，且必须放到目标程序根目录下。
---

# 目标程序配置

下文详细描述了 **`fuzzx.yaml`** 文件的配置格式，各个字段用法及作用。

### base

**类型: `string`**

**是否必须: yes**

**有效值: `ubuntu:18.04`**

代码运行操作系统环境，FuzzX使用 docker 镜像作为代码运行环境，目前仅支持 `ubuntu:18.04` 。

#### 用法:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
base: ubuntu:18.04
```
{% endtab %}
{% endtabs %}

### environment

**类型: `Array`**

**是否必须: no**

目标程序运行需要的环境变量。

#### 用法:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
environment:
    - ENV_VAR=value
    - ENV_VAR2=value2
```
{% endtab %}
{% endtabs %}

### setup

**类型: `Array`**

**是否必须: no**

目标程序编译运行前需要运行的命令，或者需要安装的依赖。

#### Usage:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
setup:
    - sudo apt-get update
    - sudo apt-get install my-dependency
```
{% endtab %}
{% endtabs %}

### language

**类型: `string`**

**是否必须: yes**

**有效值: `c, c++, rust`**

目标程序语言，目前仅支持c，c++，rust 。

#### 用法:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
language: c++
```
{% endtab %}
{% endtabs %}

### version

**类型: `string`**

**是否必须: yes \(仅限rust语言\)**

rust语言版本。

#### 用法:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
target:
    name: my-target
    language: rust
    version: 1.49.0
```
{% endtab %}
{% endtabs %}

### target

**类型: `map`**

**是否必须: yes**

目标程序配置参数

#### 用法:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
target:
    name: target-name
    # --- Other target configuration options omitted
```
{% endtab %}
{% endtabs %}

### target.name

**类型: `string`**

**是否必须: yes**

目标程序命名，命名规则为：仅可包含字母，数字和短横线。

#### 用法:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
targets:
    name: my-target
```
{% endtab %}
{% endtabs %}

### 

### target.setup

**类型: `Array`**

**是否必须: no**

用于告诉FuzzX如何编译目标程序的命令列表。

#### 用法:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
target:
    name: my-target
    setup:
        - sudo apt-get install my-lib
        - CC=$FUZZX_CC CXX=$FUZZX_CXX make my-target
```
{% endtab %}
{% endtabs %}

### target.corpus

**类型: `string`**

**是否必须: 否**

目标程序运行的初始化测试用例所在目录，可以不设置，但是强烈建议根据目标程序实际情况提供初始化用例，这样可以提高模糊测试运行效率。

#### 用法:

{% tabs %}
{% tab title="fuzzx.yaml" %}
```yaml
targets:
    name: my-target
    corpus: ./my-target/corpus
```
{% endtab %}
{% endtabs %}

### target.harness

**类型: `Map`**

**是否必须: yes**

此参数告诉 FuzzX 如何运行目标程序。

{% tabs %}
{% tab title="C" %}
指明目标程序如何运行。

#### 用法:

```yaml
target:
    name: my-target
    harness: ./target
```
{% endtab %}

{% tab title="C++" %}
指明如何运行目标程序。

#### 用法:

```yaml
target:
    name: my-target
    harness: ./target
```
{% endtab %}
{% endtabs %}

