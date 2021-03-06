# java-ee-project

### 项目文档

##### 项目建模
[需求文档](doc/tutorial/需求分析.md)

[数据库建模]()

[前端原型]()

###### 项目架构
![项目架构](./doc/images/jave-ee-architect.png)

##### 代码结构
```
root:project
|--.gitignore
|--.iml
|--pom.xml
|--README.md
|--doc:文档
|   |--images
|   |--tutorial:整理文档
|--src:源文件
|   |--main
|   |    |--java:源文件
|   |    |   |--web:业务逻辑
|   |    |   |--ejb:数据库操作
|   |    |   |--entity:实体
|   |    |--resources:资源文件
|   |    |   |--META-INF
|   |    |   |   |--beans.xml
|   |    |   |   |--persistence.xml:数据库配置
|   |    |--webapp:web应用
|   |    |   |--resources
|   |    |   |--WEB-INF
|   |    |   |   |--web.xml:web应用配置
|   |    |   |   |--face-config.xml:JSF配置
|   |    |   |--index.xhtml
|   |--test:测试文件
|--target:目标文件
```

##### 参考文档

[jakarta-tutorial](https://javaee.github.io/tutorial/toc.html)

[整理文档](./doc/tutorial)

[JSF文档](https://www.w3cschool.cn/java/inject-managed-beans.html)

##### 项目配置
web.xml web应用配置

persistence.xml 数据库配置

glassfish-{web/resources}.xml 服务器资源配置

faces-config.xml JSF配置

##### 项目日志
- 09/06/2020

分析项目需求，完成项目架构、数据库建模、前端原型
