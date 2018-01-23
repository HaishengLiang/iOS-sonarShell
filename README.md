#移动端代码审查方案



## Sonar简介

​        Sonar 是一个用于代码质量管理的开放平台。通过插件机制，Sonar 可以集成不同的测试工具，代码分析工具，以及持续集成工具。与持续集成工具（例如 Hudson/Jenkins 等）不同，Sonar 并不是简单地把不同的代码检查工具结果（例如 FindBugs，PMD 等）直接显示在 Web 页面上，而是通过不同的插件对这些结果进行再加工处理，通过量化的方式度量代码质量的变化，从而可以方便地对不同规模和种类的工程进行代码质量管理。
​        在对其他工具的支持方面，Sonar 不仅提供了对 IDE 的支持，可以在 Eclipse 和 IntelliJ IDEA 这些工具里联机查看结果；同时 Sonar 还对大量的持续集成工具提供了接口支持，可以很方便地在持续集成中使用 Sonar。
​        此外，Sonar 的插件还可以对 Java 以外的其他编程语言提供支持，对国际化以及报告文档化也有良好的支持。
主要特点
　　· 代码覆盖：通过单元测试，将会显示哪行代码被选中
　　· 改善编码规则
　　· 搜寻编码规则：按照名字，插件，激活级别和类别进行查询
　　· 项目搜寻：按照项目的名字进行查询
　　· 对比数据：比较同一张表中的任何测量的**趋势**



## Sonar的安装

官网：https://www.sonarqube.org/downloads/

最新版本：SonarQube 6.7.1 (LTS *)

直接下载后解压即可运行，Mysql配置（可选，建议配置）

自带Tomcat，直接执行对应环境下bin目录中的执行文件即可启动，Web控制台地址：http://localhost:9000 默认账户 admin 密码 admin



## iOS集成方案（Xcode 8.x+）

sonar支持26种语言的扫描，但是Objective-C的验证插件是收费版本

免费版本的插件再这里下载：https://github.com/HaishengLiang/iOS-sonarShell/blob/master/backelite-sonar-objective-c-plugin-0.6.2.jar

插件执行原理是通过ocLint来扫描oc的代码包 然后通过sonar-runner 上报

具体的集成过程如下：

#### 基础环境

1、xcodebuild （不再使用xctool）

2、使用Homebrew来安装xctool、oclint、gcovr

```
brew tap oclint/formulae
brew install oclint
brew install gcovr
```

3、tee、xcpretty、oclint

```
gem install xcpretty
```

#### 程序配置

1、扫描脚本run-sonar.sh

2、程序配置文件sonar-project.properties

下载地址：https://github.com/HaishengLiang/iOS-sonarShell/

#### 问题&&解答

1、找不到compile_commands.json文件

配置错误使用 output参数

xcpretty -r json-compilation-database --output compile_commands.json

2、去除不需要扫描的范围

使用 -e 参数 比如去除Pods目录

oclint-json-compilation-database -e Pods 

3、ERROR: The rule 'OCLint:compiler warning' does not exist.

插件错误 使用 参数 -extra-arg=-Wno-everything -rc LONG_LINE=250

Eg: oclint-json-compilation-database -e Pods -- -extra-arg=-Wno-everything -rc LONG_LINE=250 -max-priority-1 

4、svn 错误

登录后台-配置管理 -gcm- 账户密码 增加



## Android集成方案(AS)








