# Gradle

## Gradle 构建可执行 Jar 包

```gradle
jar {
    baseName "%JarName%"
    manifest {
        attributes 'Main-Class': '%ClassName%'
    }
    from {
        configurations.compile.collect {
            it.isDirectory() ? it : zipTree(it)
        }
    }
}
```

## 创建空目录

```gradle
task "create-dirs" << {
    sourceSets*.java.srcDirs*.each { it.mkdirs() }
    sourceSets*.resources.srcDirs*.each { it.mkdirs() }
}
```

## Problems

- 编码 GBK 的不可映射字符

    ```gradle
    [compileJava,compileTestJava]*.options*.encoding = 'UTF-8'
    或
    tasks.withType(JavaCompile) { options.encoding = "UTF-8" }
    ```

- `Invalid signature file digest for Manifest main attributes`

    ```groovy
    jar {
        manifest {
            ...
        }
        from (configurations.compile.collect { entry -> zipTree(entry) }) {
            exclude 'META-INF/MANIFEST.MF'
            exclude 'META-INF/*.SF'
            exclude 'META-INF/*.DSA'
            exclude 'META-INF/*.RSA'
        }
    }
    ```

* [Gradle官网](https://gradle.org/ "Gradle Build Tool")
* 项目\[`project`\]:构建\[`Gradle build`\]由1到多个project构成。project代表你要使用Gradle做的一件事。
* 任务\[`task`\]:每个project由1到多个task构成。task代表一些更加细化的构建\[编译classes,创建JAR包等\]
* 构建脚本\[`build script`\]:定义一个project及其tasks。gradle命令会在当前目录中查找build.gradle文件

## 下载和安装

1. [下载Gradle](https://gradle.org/releases "下载Gradle")
2. 解压并添加`GRADLE_HOME\bin`到环境变量`Path`
3. 测试Gradle是否安装成功

   ![测试Gradle是否安装成功](http://note.youdao.com/yws/public/resource/a5134f52d1502f37d1756c776adf7e3e/xmlnote/33EDF5CA001B498D9630C4A02433766A/11760)
    ![自动生成Gradle目录](http://note.youdao.com/yws/public/resource/a5134f52d1502f37d1756c776adf7e3e/xmlnote/AF47CC4D69DA464FBEE384B668B2A2CE/11957)

## Hello Gradle

1. 新建Gradle构建脚本`build.gradle`

   ```
    // 1. 定义一个独立的task
    task hello {
        // 添加一个action[一个包含了Groovy代码的闭包]
        doLast {
            // 输出 Hello World!
            println 'Hello World!'
        }
    }

    // 2. 定义task的简洁写法:<<是action[doLast]的简写
    task hello << {
        println 'Hello Gradle!'
    }
   ```

2. 执行构建脚本`gradle -q taskName`

   ![执行构建脚本](http://note.youdao.com/yws/public/resource/a5134f52d1502f37d1756c776adf7e3e/xmlnote/WEBRESOURCE486d562d1e348e4193a24ed2ea6a8fbc/12126)

   * -q代表quite模式，该模式不生成Gradle的日志信息，因此执行脚本后只能看到task的输出

## Gradle构建Java应用

1. 新建一个`Java Project`,文件结构如下:
    1. 项目名:HelloGradle
    2. `HelloGradle/src/main/java/hello/HelloWorld.java`

   ```
        package hello;

        public class HelloGradle {
          public static void main(String[] args) {
            Greeter greeter = new Greeter();
            System.out.println(greeter.sayHello());
          }
        }
   ```

   1. `HelloGradle/src/main/java/hello/Greeter.java`

      ```
       package hello;

       public class Greeter {
         public String sayHello() {
           return "Hello Gradle!";
         }
       }
      ```

2. 执行gradle命令`gradle tasks`:查看当前gradle能做的task

   ![查看当前gradle能做的task](http://note.youdao.com/yws/public/resource/a5134f52d1502f37d1756c776adf7e3e/xmlnote/WEBRESOURCEf4b9d60f0f34ab0a4383ff2025e1e658/12130)

3. 新建gradle配置文件:`HelloGradle\build.gradle`
4. 增加文件内容:`apply plugin: 'java'`--&gt;往project中添加java插件
   1. 插件默认在src/main/java中查找源代码
   2. 在src/test/java中查找测试代码
   3. 在src/main/resources的文件都会被包含在JAR文件中
   4. 在src/test/resources的文件会被加入到classpath中以运行测试代码
   5. 所有输出文件被创建在构建目录中,JAR文件存放在build/libs文件夹里
5. 重新查看当前gradle能做的task\[新增构建项目/运行测试/生成注释文档等java相关task\]
6. 执行gradle命令`gradle build`:构建项目
7. 修改文件`HelloWorld.java`,使该类依赖JAR包Joda Time

   ```
    package hello;

    import org.joda.time.LocalTime;

    public class HelloGradle {
      public static void main(String[] args) {
        LocalTime currentTime = new LocalTime();
        System.out.println("The current local time is: " + currentTime);

        Greeter greeter = new Greeter();
        System.out.println(greeter.sayHello());
      }
    }
   ```

8. 重新构建项目:构建失败,缺少依赖的包
9. 修改`build.gradle`并重新执行`gradle build`:

   ```
    apply plugin: 'java'

    // 指定外部依赖[第三方库]的来源
    repositories {
        // Maven中央仓库-->提取/存放依赖文件[JAR包]
        mavenCentral()
    }

    // 声明外部依赖
    dependencies {
        compile  "joda-time:joda-time:2.2"
    }
   ```

   ![Gradle主动下载缺少的依赖文件](http://note.youdao.com/yws/public/resource/a5134f52d1502f37d1756c776adf7e3e/xmlnote/D24C0735EC42493CA938061679C67B22/12145)

10. 使用`Gradle Wrapper`构建项目
    1. 执行`gradle wrapper --gradle-version 3.5`:往project中添加包装器\[wrapper\]支持

    ```
    ![gradle wrpper](http://note.youdao.com/yws/public/resource/a5134f52d1502f37d1756c776adf7e3e/xmlnote/337C11AE1AA34F4EA24C5DE2C7BB8115/11963)
    ```

    1. 执行`gradlew build`
       * 初次构建Gradle会主动下载gradle-version-bin.zip并放在生成的特殊目录下
       * 可以将下载好的压缩包直接粘贴在生成的目录下
       * ![跳过下载直接解压](http://note.youdao.com/yws/public/resource/a5134f52d1502f37d1756c776adf7e3e/xmlnote/A93F172BFBF94E7AB3B76EB1F31F7B16/12183)
       * ![解压完成的目录](http://note.youdao.com/yws/public/resource/a5134f52d1502f37d1756c776adf7e3e/xmlnote/65EEAD0284B5418DA033E25771E5BB58/12188)

11. 执行`gradlew build`后生成的jar包中依然不包含外部依赖文件,即应用还不能运行
12. 向`build.gradle`添加代码
    ```
    apply plugin: 'application'
    mainClassName = 'hello.HelloGradle'
    ```
13. 运行`gradlew run`
14. 完整的`build.gradle`文件

    ```
    // 添加java插件
    apply plugin: 'java'

    apply plugin: 'application'

    mainClassName = 'hello.HelloGradle'

    // 指定外部依赖[第三方库]的来源
    repositories {
        // Maven中央仓库-->提取/存放依赖文件[JAR包]
        mavenCentral()
    }

    // JAR包属性
    jar {
        baseName = 'gs-gradle'
        version =  '0.1.0'
    }

    // JDK版本
    sourceCompatibility = 1.8
    targetCompatibility = 1.8

    // 项目版本号
    version='1.0'

    // 添加依赖
    dependencies {
        // 编译阶段
        compile  "joda-time:joda-time:2.2"
        // 测试编译阶段:仅在构建和运行测试代码时需要该依赖项
        testCompile "junit:junit:4.12"
        // providedCompile:编译期间需要该依赖项,运行期间可能由容器提供相关组件[API]
    }
    ```

15. 参考链接
    * [Build Java Project with Gradle-Spring.io](https://spring.io/guides/gs/gradle/#scratch "Spring.io")
    * [使用Gradle构建Java项目-字节技术](http://www.4byte.cn/learning/119960/shi-yong-gradle-gou-jian-java-xiang-mu.html "字节技术")