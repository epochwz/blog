# POM 配置文件

项目描述、组织管理、依赖管理、构建信息的管理

```xml
<!-- POM 约束信息 -->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/maven-v4_0_0.xsd">

    <!-- POM 文件版本（固定) -->
    <modelVersion>4.0.0</modelVersion>

    <!-- 项目坐标 -->
    <!-- 主项目标识 -->
    <groupId>网址反写。项目名</groupId>
    <!-- 项目模块 -->
    <artifactId>项目名 + 模块名</artifactId>
    <!-- 项目版本 -->
    <version>1.0-SNAPSHOT</version>
    <!-- 项目打包方式（默认 JAR，WAR/ZIP/POM) -->
    <packaging>war</packaging>

    <!-- 项目描述名（生成项目文档时使用) -->
    <name>mmall Maven Webapp</name>
    <!-- 项目描述信息 -->
    <description></description>
    <!-- 项目地址 -->
    <url>http://maven.apache.org</url>
    <!-- 开发人员列表 -->
    <developers></developers>
    <!-- 许可证信息 -->
    <linceses></linceses>
    <!-- 组织信息 -->
    <organization></organization>

    <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
      <maven.compiler.encoding>UTF-8</maven.compiler.encoding>

      <org.springframework.version>4.0.0.RELEASE</org.springframework.version>
      <org.mybatis.version>3.4.1</org.mybatis.version>
      <org.mybatis.spring.version>1.3.0</org.mybatis.spring.version>
    </properties>

    <!-- 所有依赖项 -->
    <dependencies>
        <!-- 依赖项 -->
        <dependency>
            <!-- 依赖的项目坐标 -->
            <groupId>org.apache.tomcat</groupId>
            <artifactId>tomcat-servlet-api</artifactId>
            <version>7.0.64</version>
            <!-- 依赖的使用范围 -->
            <scope>test</scope>
            <!-- 设置依赖是否可选 -->
            <optional>true</optional>
            <!-- 排除依赖传递列表 -->
            <exclusions>
                <!-- 排除的依赖项 -->
                <exclusion></exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <!-- 依赖管理 -->
    <dependenciesManagement>
        <dependencies></dependencies>
    </dependenciesManagement>

    <!-- 项目构建 -->
    <build>
        <finalName>mmall_learning</finalName>
        <!-- 插件列表 -->
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.2</version>
                <configuration>
                    <verbose>true</verbose>
                    <overwrite>true</overwrite>
                </configuration>
            </plugin>
        </plugins>
    </build>
    <parent></parent>
    <modules></modules>

</project>
```

**项目版本规范**

```bash
大版本号。分支版本号。小版本号 - 版本类型
『版本类型』
SNAPSHOT     快照
Alpah        内测
Beta         公测
Release      稳定版本
GA           正式发布
```