---
title: mac配置JDK
date: 2024-3-21 16:03:13
tags:
- 后端
- Java
categories: 
- 笔记
---

### 下载

https://www.oracle.com/java/technologies/downloads/archive/

找到

https://www.oracle.com/java/technologies/downloads/#jdk17-mac

下载安装

### 环境配置

```shell
cd /Library/Java/JavaVirtualMachines
ls #拿到路径
cd ~
open .bash_profile
# 第一次配置环境变量，就要先
# touch .bash_profile
# 复制如下内容，JAVA_HOME 替换为自己目录
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH:.
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.

# 配置文件立即生效
source .bash_profile

# 查看 JAVA_HOME 目录
echo $JAVA_HOME
```

### 验证

```shell
java -version
```

