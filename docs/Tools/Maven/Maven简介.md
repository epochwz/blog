# Maven 简介

**Maven:** Java 项目的构建和管理工具，基于项目对象模型 (Project Object Model)，通过一小段描述信息来管理项目的构建、报告和文档

- 基于 **archetype** 可以方便地创建多种类型的 java 项目
- Maven 仓库对 jar 包 (**artifact**) 进行统一管理，避免 jar 包的重复拷贝和版本冲突
- 团队开发，Maven 管理项目的 RELEASE 和 SNAPSHOT 版本，方便多模块 (**Module**) 项目的各个模块之间的快速集成

**Maven 的文件目录**

```bash
bin    mvn 的可执行命令
boot   类加载器的框架（负责加载 Maven 自身所需类库)
lib    Maven 运行时用到的类库 (Maven 类库、第三方类库)
conf   配置文件 (settings.xml)
```

**Mavne 配置文件**`${MAVEN_HOME}/conf/settings.xml`

**Maven 项目的规定目录结构**

```bash
project
    -src    # 源代码
        -main
            -java
                -package
        -test
            -java
                -package
        -resource
```

**项目目录结构的自动构建**

```bash
mvn archetype:generate
mvn archetype:generate -DgroupId= 组织名 . 网址反写 . 项目名
                       -DartifactId= 项目名 - 模块名
                       -Dversion= 版本快照
                       -Dpacakge= 代码所在包名
```

**常用命令**

```bash
# 验证版本信息
mvn -v
# 清除命令
mvn clean
# 编译命令
mvn compile
# 打包命令
mvn package
# 编译时跳过单元测试
mvn compile -Dmaven.test.skip=true
# 打包时跳过单元测试
mvn package -Dmaven.test.skip=true
```