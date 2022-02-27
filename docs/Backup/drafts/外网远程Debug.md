# 外网远程 debug

1. 远程代码和本地代码保持一致
2. 编辑 `${CATALINA_HOME}/bin/catalina.sh`

    ```bash
    CATALINA_OPTS="-Xdebug -Xrunjdwp:transport=dt_socket,address=60222,suspend=n,server=y"
    ```

3. 防火墙开放远程 debug 端口
4. 完成 debug 后应该关闭端口