#1、		Maven项目修改仓库地址

```xml
<repositories>
    <repositorie>
       <id>spring-snapshots</id>
       <url>http://repo.spring.io/snapshot</url>
       <snapshots>
         <enabled>true</enabled>
       </snapshots>
    </repositorie>
</repositories>
<pluginRepositories>
	<pluginRepository>
        <id>spring-snapshots</id>
        <url>http://repo.spring.io/snapshot</url>
    </pluginRepository>
</pluginRepositories>
```

