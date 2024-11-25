# 新建项目加载失败解决方案

创建程序持续失败，基本上就是gradle的问题。

```properties
repositories {
    maven{
        url 'https://maven.aliyun.com/nexus/content/groups/public/'
    }
```

就可以了。