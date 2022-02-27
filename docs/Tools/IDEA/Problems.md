# IDEA Problems

## 中文乱码

### IDEA 控制台 中文乱码

1. IDEA 全局编码和项目编码都设置成 UTF-8
2. IDEA `VM Options`: 在 `idea64.exe.vmoptions` 中追加 `-Dfile.encoding=UTF-8`
3. Tomcat `VM options`: 在设置界面追加 `-Dfile.encoding=UTF-8`

### IDEA 终端中 Git 中文乱码

设置 Git 编码：在文件 `C:\Program Files\Git\etc\bash.bashrc` 中追加下面内容

```bash
export LANG="zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8"
```

## Indexing 无限循环

清除缓存和索引：`File` -> `Invalidate Caches/Restart` -> `Invalidate and Restart`