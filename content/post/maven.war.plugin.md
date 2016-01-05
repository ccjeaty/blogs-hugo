+++
date = "2016-01-05T11:44:21+08:00"
draft = true
title = "maven.war.plugin"

+++

# maven war plugin 使用说明

[TOC]

## intro
主要用于处理在构建war包时的一些操作, 目前只用于对资源文件的处理.把目前使用到过的一些配置记录下来. 用以备忘.  
直接贴一个比较完整的配置, 一看便知:
```xml
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>2.6</version>
        <configuration>
		  <filters>
			<!-- 集中管理变量值, -->
            <filter>properties/config.prop</filter>
          </filters>
          <nonFilteredFileExtensions>
            <!-- default value contains jpg,jpeg,gif,bmp,png -->
            <nonFilteredFileExtension>conf</nonFilteredFileExtension>
          </nonFilteredFileExtensions>
          <webResources>
            <resource>
              <!-- this is relative to the pom.xml directory -->
              <directory>resource2</directory>
			  <includes> <!-- excludes -->
				<include>**/*.properties</include>
			  </includes>
			  <!-- this is relative to war root dir, and it will replace all of the duplicate files -->
			  <targetPath>WEB-INF/classes</targetPath>
			  <filtering>true</filtering>
            </resource>
          </webResources>
        </configuration>
      </plugin>
    </plugins>
</build>
```
## 资源文件处理

执行`compile`命令的时候可以对项目的资源文件进行处理, 常用的处理有以下几种:
-   替换变量(*与profile配合使用*)  
在插件中的变量也是以**`${prop.name}`**方式来书写的, 如果文件内容中有相应的变量, 正好maven的ctx中又有相应的值,则会对变量进行替换.  
-   copy非标准结构目录中的文件到war指定的文件中并指定destination
-