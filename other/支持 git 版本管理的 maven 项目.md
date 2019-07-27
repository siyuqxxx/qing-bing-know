# 支持 git 版本管理的 maven 项目

[TOC]

# 参考资料

《maven实战》
[Nexus Repository Manager OSS 3.x - Windows 安装配置](https://blog.csdn.net/clanguage2012/article/details/86600473)
[Nexus 3.X（Maven仓库私服）仓库迁移与备份](https://www.cnblogs.com/nethrd/p/9554163.html)

# 配置

## scm git 仓库配置

在 pom.xml 中添加如下配置：

```xml
<project>
  
  ...
  
  <scm>
    <connection>scm:git:git@github.com:qiansiyu/excel.git</connection>
  </scm>

  <distributionManagement>
    <repository>
      <id>nexus-releases</id>
      <name>Private Release Repository</name>
      <url>http://192.168.0.11:8081/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
      <id>nexus-snapshots</id>
      <name>Private Snapshot Repository</name>
      <url>http://192.168.0.11:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
  </distributionManagement>
  
  ...

  <build>
    <pluginManagement>
      <plugins>
      
        ...
      
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.5.3</version>
          <configuration>
            <tagNameFormat>V@{project.version}</tagNameFormat>
          </configuration>
        </plugin>
        
        ...
        
      </plugins>
    </pluginManagement>
  </build>
</project>
```

两个关键配置，一个 `scm` 一个 `maven-release-plugin` 

执行一次版本发布

```sh
mvn clean release:prepare release:perform
```