---
title: Tomcat 常见问题
permalink: problems-of-tomcat
description: 本文主要记录 Tomcat 的常见问题
keywords: tomcat,java
quicklink: true
categories:
- Problems
tags:
- Problems
- Tools
- Tomcat
- Java
---

## Tomcat8 启动超慢

**报错信息**

```bash
org.apache.catalina.util.SessionIdGeneratorBase.createSecureRandom Creation of SecureRandom instance for session ID generation using [SHA1PRNG] took [366,876] milliseconds.
```

**解决方法**

将 `$JAVA_HOME/jre/lib/security/java.security` 中的 `securerandom.source=file:/dev/random` 或者 `securerandom.source=file:/dev/urandom` 注释掉或者修改成 `securerandom.source=file:/dev/./urandom`

**参考文章**

- <https://www.jianshu.com/p/c690e791c408>
- <https://blog.csdn.net/raintungli/article/details/42876073>

## Tomcat8 扫描 TLDs

**报错信息**

```bash
org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.
```

**解决方法**

将 `$CATALINA_HOME/conf/catalina.properties` 中的 `tomcat.util.scan.StandardJarScanFilter.jarsToSkip` 赋值成 `*.jar`