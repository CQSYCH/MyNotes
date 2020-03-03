# 			Maven安装

### 1、Maven的作用

######一、Maven 采用了不同方式对项目构建进行抽象，比如源码位置总是在 src/main/java 中，配置 文件则是在 src/main/resources 中，编译好的类总是放在项目的 target 目录下，总的来说， Maven 实现了以下目标：

- 使构建项目变得很容易， Maven 屏蔽了构建的复杂过程。比如，你只需要输入 maven package 就可以构建整个 Java 项目

- 统一了构建项目的方式，不同人、不同公司的项目都有同样的描述项目和构建项目的 方式， Maven 通过 pom.xml 来描述项目，井提供一系列插件来构建项目 。只要熟悉了 Maven，就熟悉了所有项目的构建方式

- 提出了一套开发项目的最佳实践，而不用每个项目都有不同结构和构建方式，比如源 代码在 src/main/iava 中，测试代码则在 src/test/iava 中，项目需要的配置文件则放在 src/main/resources 中

- 解决了类库依赖的问题，只需要声明使用的类库， Maven 会自动从仓库下载依赖的 jar 包，并能协助你管理 jar 包之间的冲突

  

### 2、Maven的核心文件

###### 一、Maven 的核心是 pom.xrnl，用 XML 方式描述了项目模型，pom通常有以下元素

- groupId：表示项目所属的组，通常是一个公司或者组织的名称，如 org.Springframework

- artifactld：项目唯一的标识，比如，有 spring-boot-starter-web、 spring-boot-devtools
- packaging： 项目的类型，常用的有 jar 和 war 两种， jar 表示项目 会打包成一个 jar 包， 这是 Spring Boot 的默认使用方式
- version： 项目的版本号，比如 0β.1割APSHOT、 1.5.2.RELEASE