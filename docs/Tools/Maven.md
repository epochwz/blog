# Maven

Maven 是 Apache 旗下的一款开源项目管理工具，对 Java 项目提供统一的构建和依赖管理，已成为业界标准。

解决的问题

- 工程结构不统一：遵循统一的项目设置规则，保证不同开发环境的兼容性
- 依赖查找困难：强大的依赖管理，自动下载、更新项目依赖组件
- 项目打包困难：可扩展的插件机制，使用简单、功能丰富

坐标

- `GroupId` 机构或团体的英文，建议采用“逆向域名”的形式书写
- `ArtifactId` 项目名称，说明项目用途
- `Version` 版本号，建议采用“版本+单词”形式，如 `1.0.0.RELEASE`

Maven 工程标准目录结构

| 目录                            | 用途                                      |
|:--------------------------------|:-----------------------------------------|
| `${basedir}`                    | 项目根目录，存放 pom.xml                   |
| `${basedir}/pom.xml`            | 项目对象模型文件(Project Object Model,POM) |
| `${basedir}/src/main/java`      | 源代码目录，存放Java 源代码                |
| `${basedir}/src/main/resources` | 资源目录，存放配置文件、静态资源等          |
| `${basedir}/src/test/java`      | 测试源代码目录，存放Java 源代码            |
| `${basedir}/src/test/resources` | 测试资源目录，存放配置文件、静态资源等      |
| `${basedir}/src/target`         | 输出目录，存放 jar,war等                   |
| `${basedir}/src/target/classes` | 编译输出目录，存放字节码文件等              |

本地仓库和远程仓库

Maven 下载依赖的过程

1. 加载 pom.xml
2. 在本地仓库 `~/.m2/repository` 中查找
3. 如果不存在则在远程仓库（默认是 `repo.maven.apache.org`）中下载

自定义远程仓库

```xml
<repositories>
    <repository>
        <id>aliyun</id>
        <id>aliyun</id>
        <url>https://maven.aliyun.com/repository/public</url>
    </repository>
</repositories>
```

常用命令

```bash
mvn archetype:generate  # 创建 Maven 工程目录结构
mvn compile             # 编译
mvn test                # 执行测试用例
mvn clean               # 清除项目输出文件
mvn package             # 打包
mvn install             # 安装至本地仓库

```

[阿里云 Maven 私有仓库服务](https://maven.aliyun.com/mvn/view)