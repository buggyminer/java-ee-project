# Web Tier



## Web Application 入门

Web 应用有 2 类：

* 面向表示
    * 面向表示的 web 应用程序，生成包含各种标记语言(HTML、 XHTML、 XML 等)和响应请求的动态内容的交互式 web 页面
* 面向服务
    * 面向表示的应用程序通常是面向服务的 web 应用程序的客户端

### web 组件

功能：为 web 服务器提供动态扩展功能

类型：

* Java servlet
    * 是 Java 编程语言类
    * 功能：动态处理请求 & 构造响应
    * 最适合：应用程序的控制功能。如：分发请求，处理非文本数据
* 使用 JavaServer Faces 和 Facelets 技术实现的 Web 页面
    * 适合：生成基于文本的标记，如 XHTML
* Web 服务端点
* JSP 页面

Web 组件由称为 `Web 容器`的运行时平台的服务支持

### web 容器

提供以下服务：

* 请求分发
* 安全性
* 并发性
* 生命周期管理
* 使 web 组件能够访问诸如命名、事务和电子邮件等 api
* 其它

由于 web 应用程序必须考虑这些服务，因此创建和运行 web 应用程序的过程与传统的独立 Java 类不同

当应用程序安装或部署到 web 容器时，可以**配置 web 应用程序行为的某些方面**

配置信息的 2 种指定方式：

* 使用 javaee 注释
* 以 XML 格式的文本文件来维护
    * 这种文本文件称为 web 应用部署描述符（Deployment Descriptor, DD）
    * Web 应用程序 DD 必须符合 javaservlet 规范中描述的模式



### 应用程序生命周期

一个 web 应用程序，包括：

* web 组件
* 静态资源文件，如图像和层叠样式表文件(CSS)
* 辅助类
* 库

创建、部署和执行 web 应用程序的过程：

1. 开发 web 组件代码。
2. 如果有必要，开发 web 应用部署描述符。
3. 编译组件引用的 web 应用组件和 helper 类。
4. （可选）将应用程序打包成可部署单元
5. 将应用程序部署到 web 容器中。
6. 访问引用 web 应用程序的 URL。

例子：Hello 应用程序，包含两个 web 组件

* hello1
    * 基于 JavaServer Faces 技术
    * 使用两个 XHTML 页面和一个托管 bean(managed bean)
* hello2
    * 基于 servlet 
    * 其中的组件由两个 servlet 类实现



### 例子：hello1（使用 Facelets）

* `index.xhtml` 是 Facelets 应用程序的默认登陆页面

* 在典型的 Facelets 应用程序中，web 页面是用 XHTML 创建的。

* web page 通过 Expression Language (EL) 表达式 `#{hello.name}` 连接到该项目的`托管bean`
* 在`托管 bean`中，大多数情况下有一个 `@Named` 注解和一个描述 scope 的注解。
    * 如果`@Named` 没有指定名称，则访问这个`托管bean`时，需要将它的首字母变为小写。例如：如果要访问 Hello 类，则使用 hello 来访问。
* XHTML的 `action` 属性，其作用是：跳转到指定页面。例如：`action="response"` 将跳转到 `response.xhtml`
* 如果 `托管bean` 的属性被调用，那么`托管bean`需要提供 getter 和 setter 方法
* `scope 作用域`
    * 功能：定义了应用程序数据如何持久化和共享
    * JavaServer Faces 应用程序中最常用的范围如下:
        * Request (`@RequestScoped`)
            * 请求范围在 web 应用程序中的单个 HTTP 请求期间持久化
        * Session (`@SessionScoped`)
            * 跨 web 应用程序中的多个 HTTP 请求持久存在
        * Application (`@ApplicationScoped`)
            * Application 作用域跨所有用户与 web 应用程序的交互保持
* `@Model` 等价于 `@Named` 和 `@RequestScoped` 叠加使用
    * `@Model` 也称为 stereotype，是术语，表示封装了其他注释的注释
* Web Pages 节点下的 WEB-INF 节点的 `Web.xml` 文件
    * 包含 Facelets 应用程序所需的几个元素
    * 是 IDE 自动创建的
    * 包括
        * `<context-param>`
            * 指定项目阶段的上下文参数
            * 提供 web 应用程序所需的配置信息
            * 可以自定义
            * JavaServer Faces 技术和 Java Servlet 技术定义了应用程序可以使用的上下文参数
        * `<servlet>` 和 `<servlet-mapping>`
            * 指定 FacesServlet
        * `<welcome-file-list>`
            * 指定登陆页位置的欢迎文件列表元素
* 在某些部署场景中，以及无论何时您想要分发 web 模块，都必须将 web 模块打包到 WAR 中。

* 将一个 WAR 文件部署到 GlassFish 服务器的 4 种方式：
    * 使用 NetBeans IDE
    * 使用 `asadmin` 命令
    * 使用管理控制台
    * 将 WAR 文件复制到 domain-dir/autodeploy/目录中
* 构建和打包 web 模块的 2 种方法：
    * 使用 NetBeans IDE
        * 找到项目，然后 Build
    * 使用 Maven
        * 在项目的路径中打开 cmd，输入以下命令：`mvn install`
* 查看已部署的 Web 模块的 2 种方法：
    * 使用 asadmin 命令
        * `asadmin list-applications`
    * 使用 NetBeans IDE
        * 在服务器的节点中展开“Applications”节点
* 运行 Deployed hello1 Web 模块
    * 默认情况下，应用程序被部署到端口8080上的本地主机。Web 应用程序的上下文根是 hello1。
    * 在浏览器中输入：`http://localhost:8080/hello1/`
* 取消部署的 2 种方法：
    * 使用 NetBeans IDE
        * 在服务器节点的application节点的对应模块中，右键，undeploy
    * 使用 Maven
        * 在项目的路径中打开 cmd，输入以下命令：`mvn cargo:undeploy`
        * 如果要删除类文件和其他构建工件，输入以下命令：`mvn clean`

### 例子：hello2（使用 Servlet）

当web 容器接收到一个请求时，必须决定哪个 web 组件应该处理这个请求

* 通过将请求中包含的 URL 路径映射到 web 应用程序和 web 组件来实现
* 一个 `URL 路径`包含`context root 上下文根`（和一个 `URL pattern URL 模式`）

```oac_no_warn
http://host:port/context-root[/url-pattern]
```

* 设置 servlet 的 URL 模式的方式：使用 servlet 源文件中的 `@WebServlet` 注释
    * 例子：`@WebServlet("/greeting")`
    * 这个注释表明 URL 模式/greeting 位于上下文根之后。因此，当 servlet 在本地部署时，它是通过以下 URL 访问的:`http://localhost:8080/hello2/greeting`
    * 要仅使用上下文根访问 servlet，请指定“/”作为 URL 模式
* servlet 类覆盖 doGet 方法，实现 HTTP 的 GET 方法

### 配置 Web 应用程序

* 设置`上下文参数（Context Parameters）`

    * Web 模块中的 web 组件共享一个表示其应用程序上下文的对象
    * 上下文参数可用于整个应用程序
    * 可以：
        * 将上下文参数传递到上下文
        * 将初始化参数传递给 servlet
    * 在 NetBeans IDE 中创建 web.xml
        * File -> New File，Categories 选 Web，File Types 选 Standard Deployment Descriptor(web.xml)
    * 在 NetBeans IDE 中添加上下文参数
        * 单击编辑器窗口顶部的“常规 General”，展开上下文参数节点，在“添加上下文参数”对话框的“参数名称”字段中，输入指定上下文对象的名称。在 Parameter Value 字段中，输入要传递给上下文对象的参数。点击确定。

* 声明`欢迎文件`

    * 欢迎文件机制允许您指定一个文件列表，web 容器可以将其附加到对`未映射到 web 组件的 URL`(称为有效部分请求)的请求中
    * 如果 web 容器接收到有效的部分请求，web 容器检查`欢迎文件列表`，**按指定的顺序**将每个欢迎文件追加到部分请求，并检查 WAR 中的静态资源或 servlet 是否映射到该请求 URL。然后 web 容器将请求发送到 WAR 中**匹配的第一个资源**。

* 将错误映射到错误屏幕

    * 根据错误类型，显示特定的错误屏幕
    * 使用 NetBeans IDE 设置错误映射
        * 打开 web.xml 文件，单击编辑器窗口顶部的“页面”，展开“错误页面”节点，在“添加错误页”对话框中，单击“浏览”以定位要充当错误页的页，指定错误代码或异常类型
            * 要指定一个错误代码，在 Error Code 字段中输入错误 HTTP状态码，这将导致错误页面被打开，或者保留空白字段以包含所有错误代码。
            * 若要指定异常类型，请在“异常类型”字段中输入将导致加载错误页的异常
            * 若要指定所有可调用的错误和异常，请输入 `java.lang.Throwable`

* 声明资源引用

    * 使用`注解`，将 enterprise beans, data sources, 和 web services 等资源注入到应用程序之中。

        * 优点：消除了许多以前版本的 javaee 所需的样板查找代码和配置元素
        * 限制：
            * 只能将资源注入到容器管理的对象中。
                * 因为容器必须控制组件的创建，以便能够执行对组件的注入。
            * 不能将资源注入简单 javabean 组件这样的对象中
            * 但是，托管 bean 由容器管理; 因此，它们可以接受资源注入
        * 接受资源注入的 Web 组件
            * Servlets
            * Servlet filters
            * Event listeners
            * Managed beans

    * `@resource` 注释

        * 功能：声明对**资源**的引用，例如 data source, enterprise bean, 或 an environment entry

        * 在哪里声明？

            * 类
            * 方法
            * 字段

        * 容器负责注入对@resource 注释声明的资源的引用，并将其映射到适当的 JNDI 资源

        * 例子：

            ```java
            @Resource javax.sql.DataSource catalogDS;
            public getProductsByCategory() {
                // get a connection and execute the query
                Connection conn = catalogDS.getConnection();
                ...
            }
            ```

        * 如果有多个资源需要注入到一个组件中，那么需要使用 `@resources` 注释来包含它们

            ```java
            @Resources ({
                @Resource(name="myDB" type=javax.sql.DataSource.class),
                @Resource(name="myMQ" type=javax.jms.ConnectionFactory.class)
            })
            ```

    * 声明对 **Web 服务**的引用

        * @WebServiceRef 注释



## JavaServer Faces 技术简介

### JSF 技术包括以下几部分:

* API
    * 表示组件并管理其状态
    * 处理事件
    * 服务器端验证
    * 数据转换
    * 定义页面导航
    * 支持国际化和可访问性
    * 为所有这些特性提供扩展性
* 标记库
    * 向网页添加组件
    * 将组件连接到服务器端对象

### 使用 JSF 完成的常见任务

* 创建一个网页
* 通过添加组件标记，将组件放到网页上
* 将页面上的组件绑定到服务器端数据
* 将组件生成的事件连接到服务器端应用程序代码
* 在服务器请求生命周期之后保存和恢复应用程序状态
* 通过定制，重用和扩展组件

### 一个JSF 应用程序，包括

* 一组网页，其中包含若干个组件
* 向网页添加组件的一组标记
* 一组托管 bean (managed bean)，它们是轻量级的容器管理对象(pojo)
    * 在 JavaServer Faces 应用程序中，托管 bean 充当 backing bean，它为页面上的 UI 组件定义属性和函数
* 一个 web 部署描述符文件(web.xml 文件)
* （可选）若干个应用程序配置资源文件
    * 如 faces-config.xml 文件，该文件可用于定义页面导航规则和配置 bean 和其他定制对象，如定制组件
* （可选）一组自定义对象
    * 可以包括应用程序开发人员创建的自定义组件、验证器、转换器或监听器
* （可选）一组用于在页面上表示自定义对象的自定义标记

### 一个典型的 JSF 应用程序中客户机和服务器之间的交互

![image-20200902145544703](C:\Users\Gwan\AppData\Roaming\Typora\typora-user-images\image-20200902145544703.png)

* 网页 `myfacelet.xhtml` 是使用 JSF组件标记构建的
    * 组件标记用于向视图中添加组件
    * 该视图是页面的服务器端表示
    * 除了组件之外，网页还可以引用对象，例如
        * 在组件上注册的任何事件监听器、验证器和转换器
        * Javabean 组件捕获数据并处理组件的特定于应用程序的功能
* 根据客户机的请求，视图作为响应呈现
    * 渲染，是web 容器生成可以被客户端(如浏览器)读取的输出(如 HTML 或 XHTML)的过程。它基于服务器端视图。

### JSF 技术的优势

* 为 web 应用程序提供了**行为**和**表示**之间的清晰分离
    * 将 HTTP 请求映射到特定于组件的事件处理
    * 并且，将组件作为服务器上的`有状态对象`进行管理
* 利用熟悉的组件和网络层概念，但不限制使用特定的脚本技术或标记语言
    * Java Web 应用技术的分层
        ![image-20200902150254910](C:\Users\Gwan\AppData\Roaming\Typora\typora-user-images\image-20200902150254910.png)
    * 可以使用不同的表示技术，直接从组件类创建您自己的定制组件，以及为各种客户端设备生成输出

### Facelets 技术及其优势

Facelets 技术是 JSF 技术的一部分，是构建基于 JSF 技术的 web 应用程序的首选表示技术

* 代码可以通过模板和复合组件特性，对组件进行重用和扩展
* 可以使用注释，来自动注册 `托管 bean` 作为 JSF 应用程序的可用资源
* 隐式导航规则允许开发人员快速配置页面导航
* JSF 技术为管理组件状态、处理组件数据、验证用户输入和处理事件提供了丰富的体系结构

### 开发一个简单的 JSF 应用程序，需要完成以下任务

* 使用组件标记创建网页
* 开发托管 bean
* 映射 FacesServlet 实例

### 每个 web 应用程序都有一个生命周期

常见的任务都是在 web 应用生命周期中执行的

一些 web 应用程序框架对您隐藏生命周期的细节，而另一些则要求您手动管理它们

* JavaServer Faces 自动为你处理大部分的生命周期操作，但它也公开了请求生命周期的各个阶段

JSF 服务器应用程序的生命周期简介

* 开始：客户机对网页发出请求
* 主要阶段1：执行（Execute）
    * 可以执行的操作
        * 生成或还原应用程序视图
            * 对于第一个(初始)请求，只构建视图
            * 对于后续(回发)请求，可以执行部分或全部其他操作。
        * 应用请求参数值
        * 对组件值执行转换和验证
        * 使用组件值更新托管 bean
        * 调用应用程序逻辑
* 主要阶段2：渲染（Render）
    * 请求的视图作为对客户端的响应而呈现
    * 呈现通常是生成输出(如 HTML 或 XHTML)的过程，客户机(通常是浏览器)可以读取这些输出
* 结束：服务器用网页进行响应

### 用户界面组件模型

`JSF 组件`是 `JSF 视图`的组成部分

* 组件可以是用户界面(UI)组件，也可以是非 UI 组件

* 界面组件是可配置的、可重用、可简单也可复合的元素

JSF 技术的组件架构包括：

* 一组 `javax.faces. com ponent.uicomponent` 类
    * 用于指定 UI 组件的状态和行为
    * 所有组件的抽象基类是 `javax.faces.component.UIComponent`
    * JavaServer Faces UI 组件类扩展了 UIComponentBase 类(UIComponent 的一个子类) 
        * 该类定义了组件的默认状态和行为
* 一个渲染模型 Component Rendering Model
    * 定义如何以各种方式呈现组件
    * JSF 组件架构的设计使得：
        * 组件的功能由组件类定义
        * 组件呈现可以由一个单独的呈现类（Render Class）定义
    * 优点
        * 可以编写一个组件类，但是创建多个呈现器
    * 有一个渲染工具包，定义组件类如何映射到适合特定客户端的组件标记
        * 通过定义一组 `javax.faces.render.Renderer 类`  的方式，用于它支持的每个组件
* 一个转换模型 Conversion Model
    * 定义如何将数据转换器注册到组件上
    * 应用程序可以选择将`组件`与`服务器端对象数据`关联起来
        * 这个对象是 javabean 组件，例如：托管bean
        * 应用程序通过调用组件的适当对象属性来获取和设置组件的对象数据。
    * 当一个组件绑定到一个对象时，应用程序拥有该组件数据的 2 个视图
        * 模型视图
            * 其中数据表示为数据类型，如 int 或 long
        * 表示视图
            * 其中数据以用户可以读取或修改的方式表示，其中数据以用户可以读取或修改的方式表示
* 一个事件与侦听器模型
    * 定义如何处理组件事件
    * 具有强类型的事件类和监听器接口
    * JSF 事件规范定义了3种类型的事件：
        * 应用程序事件
            * 由 UIComponent 生成
            * 绑定到特定的应用程序
            * 它们代表了以前版本的 JavaServer Faces 技术中可用的标准事件
        * 系统事件
            * 由 Object 而不是 UIComponent 生成
            * 在应用程序执行期间按预定义的时间生成
            * 适用于整个应用程序，而不是特定的组件
        * 数据模型事件
            * 当选择 UIData 组件的新行时，将发生数据模型事件
    * 一个事件对象，会做以下工作：
        * 标识生成事件的组件
        * 存储有关事件的信息
    * 要获得事件通知，需要做到：
        * 应用程序必须提供侦听器类的实现
        * 必须在生成事件的组件上注册它
    * JSF 支持 2 种类型的应用程序事件:
        * 动作事件 `javax.faces.event.ActionEvent 类`
            * 当用户激活一个实现 ActionSource 的组件时发生 ActionEvent
            * 这些组件包括按钮和链接
        * 值更改事件 `javax.faces.event.ValueChangeEvent 类`
    * 使应用程序对`标准组件`发出的`动作事件`或`值更改事件`作出反应的2 种方法:
        * 实现一个事件侦听器类来处理事件，并通过在组件标记内嵌套 `f:valueChangeListener 标记`或 `f: actionListener 标记`来注册组件上的侦听器
        * 实现托管 bean 的方法来处理事件，并从组件标记的适当属性引用带有方法表达式的方法
* 一个验证模型
    * 定义如何将验证器注册到组件
    * 支持验证可编辑组件(如文本字段)的本地数据
        * 此验证在更新相应的模型数据以匹配本地值之前进行
    * 与转换模型一样，验证模型定义了一组用于执行通用数据验证检查的标准类
    * 实现自定义验证的 2 种方法
        * 实现执行验证的 `Validator` 接口
            * 并且，还要执行：
                * 向应用程序注册 Validator 实现
                * 创建自定义标记或使用 `f:validator` 标记在组件上注册验证器
        * 实现执行验证的托管 bean 方法



### 导航模型

* 导航是一组规则，用于选择应用程序操作之后要显示的下一个页面或视图，比如单击按钮或链接时。

* 导航可以是隐式的，也可以是用户自定义的
    * 隐式导航
        * 生效情景：当应用程序配置资源文件中，**没有**配置用户定义的导航规则时
        * 当您向 Facelets 页面添加一个组件(如 commandButton) ，并为其 `action 属性`分配另一个页面的值时，默认导航处理程序将尝试隐式匹配应用程序中的一个合适页面。
    * 用户自定义
        * 处理如下：
            * 在应用程序配置资源文件中定义规则
            * 从按钮或链接组件的 `action` 属性引用结果字符串。这个结果字符串被 JSF 实现用来选择导航规则

### JSF 应用程序的生命周期

* 请求 -> 响应

* 执行 -> 渲染

`执行阶段` 可细分为多个子阶段，从而支持复杂的**组件树**

**组件树**要求：

* 转换和验证组件数据
* 处理组件事件

* 以有序的方式将组件数据传播到 bean

一个 **JSF 组件页面**由一个组件树表示，称为**视图**

* 在生命周期中，JavaServer Faces 实现必须构建视图
* 同时考虑从以前提交的页面中保存的状态

处理 2 种类型的请求：

* 初始请求 initial requests
    * 发生情景：用户第一次请求某个页面时
    * 只执行 Restore View 和 Render Response 阶段，不需要处理用户输入或操作
* 回发 postback
    * 发生情景：用户提交页面上包含的表单时，该表单以前作为执行初始请求的结果加载到浏览器中
    * 当生命周期处理回发时，它会执行所有阶段

各个子阶段：

![Flow diagram of Faces request and Faces response, including event and validation processing, error handling, model updating, application invocation.](https://javaee.github.io/tutorial/img/javaeett_dt_016.png)

* 恢复视图阶段 Restore View Phase
    * 将做的工作：
        * 构建页面视图
        * 将事件处理器和验证器连接到视图中的组件
        * 将视图保存在 FacesContext 实例中
            * 该实例包含处理单个请求所需的所有信息
            * 所有应用程序的组件、事件处理程序、转换器和验证器都可以访问 FacesContext 实例
    * 如果对页面的请求是一个初始请求
        * 在这个阶段，JSF 实现将创建一个空视图
        * 然后进入 Render Response 阶段，用页面中标记所引用的组件填充空视图
    * 如果对页面的请求是回发
        * FacesContext 实例中已经存在与此页面对应的视图
        * JSF 实现通过使用保存在客户端或服务器上的状态信息来恢复视图
* 应用请求值阶段 Apply Request Values Phase
    * 在回发请求期间恢复组件树之后，树中的每个组件使用它的`decode` (`processDecodes ()`)方法从请求参数中提取它的新值。然后将值存储在每个组件的本地。
    * 如果任何解码方法或事件侦听器在当前 FacesContext 实例上调用了 renderResponse 方法，那么 JavaServer Faces 实现将跳到 Render Response 阶段。
    * 如果在这个阶段中有任何事件已经排队，那么 JSF 实现将事件广播给感兴趣的侦听器
    * 如果页面上的某些组件的 `immediate` 属性(请参阅即时属性)设置为 true，那么在这个阶段将处理与这些组件相关的验证、转换和事件。
    * 在这个阶段结束时，组件被设置为它们的新值，消息和事件已经排队

* 流程验证阶段 Process Validations Phase
* 更新模型值阶段 Update Model Values Phase
* 调用应用程序阶段 Invoke Application Phase
* 渲染响应阶段 Render Response Phase

### JSF 支持部分处理和部分渲染





## Facelets 入门

### 什么是 Facelets

本质：页面声明语言

目的：

* 使用 HTML 格式的模板来构建 JSF 视图
* 构建组件树

特点：

* 使用 XHTML 创建网页
* 支持 Facelets 标签库，还有 JSF 标签库和 JSTL 标签库
* 支持表达式语言(EL)，用于引用托管 bean 的属性和方法，或者将组件对象/值与托管beans的属性或方法绑定在一起
* 组件和页面的模板化

### Facelets 应用程序的生命周期

1. 当客户机(如浏览器)对**使用 Facelets 创建的页面**发出新请求时，将创建一个新组件树或 `javax.faces.component.UIViewRoot` ，并将其放置在 `FacesContext` 中
2. `UIViewRoot` 应用于 Facelets，视图中填充了用于呈现的组件
3. 新构建的视图作为对客户端的响应而呈现回来
4. 呈现时，该视图的状态将为下一个请求而存储起来。此外，输入组件和表单数据的状态也会存储起来
5. 客户端可能会与视图交互，并从 JavaServer Faces 应用程序请求另一个视图或更改。此时，将从存储状态还原保存的视图
6. 恢复的视图再次通过 JavaServer Faces 生命周期，如果没有验证问题和没有触发任何操作，最终将生成一个新视图或重新呈现当前视图
7. 如果请求相同的视图，则再次呈现存储的视图
8. 如果请求新视图，则继续执行步骤2中描述的流程
9. 然后将新视图作为对客户机的响应呈现回来

## 开发简单的 Facelets 应用程序: guessnumber-jsf 示例应用程序

开发 JSF 程序需要完成的任务：

* 开发托管 bean
    * 在 JSF 程序中，每个页面都连接到一个托管bean，这个托管bean充当 Backing bean
    * Backing bean 定义与组件关联的方法和属性
* 使用组件标记创建页面
    * 使用各种**标签**将组件插入到网页中
        * 以 `h:`开头的标记，是Facelets HTML tags，用于添加组件
        * `f:validateLongRange`，是 Facelets core tag，用于验证用户输入
        * 详情参见：\web\jsf\guessnumber-jsf\src\main\java\jakarta\tutorial\guessnumber\UserNumberBean.java
* 定义页面导航
* 映射 `FacesServlet` 实例
* 添加托管 bean 声明

* 配置应用程序
    * 将 Faces Servlet 映射到 web 部署描述符文件中，比如 `web.xml`
    * 在应用程序配置资源文件 Faces-config.xml 中，加入
        * 托管 bean 声明
        * 导航规则
        * 资源束声明（resource bundle declarations）
    * 注意：在 web.xml 中，有一个上下文参数 `PROJECT_STAGE`，他表示 JSF 程序在软件生命周期中的状态
        * 如果是 `Development`，则生成调试信息
        * 如果没有定义，则为 `Production`

### 使用 Facelets 模板

Facelets 模板标签:

![image-20200904094327634](C:\Users\Gwan\AppData\Roaming\Typora\typora-user-images\image-20200904094327634.png)

### 复合组件

特点：

* 由一组标记标记和其他现有组件组成
* 可以像任何其他组件一样，在它身上加上将验证器、转换器和侦听器
* 以 `composite`开头
* `cc`是表示复合组件的保留字
* 复合组件的本地库定义在 `xmlns`命名空间中，并且声明为 `xmlns:em="http://xmlns.jcp.org/jsf/composite/emcomp"`，通过 `em:email` 标签进行访问

![image-20200904102420514](C:\Users\Gwan\AppData\Roaming\Typora\typora-user-images\image-20200904102420514.png)

### web 资源

必须在标准位置放置资源：

* 如果要求资源的访问路径是 web 应用程序的根目录，那么它必须放置在根目录的 resources 文件夹下面
* 如果要求资源的访问路径是 web 应用程序的classpath，那么它必须放置在 META-INF/resources 文件夹下面。可以使用这个文件结构，将资源打包到捆绑在 web 应用程序中的 JAR 文件中

资源标识符的格式：

* ```oac_no_warn
    [locale-prefix/][library-name/][library-version/]resource-name[/resource-version]
    ```

* 带 [] 的都是可选的，可以借助这些属性进行定位

访问资源的方式：

* 使用 library 和 name 属性
    `<h:outputStylesheet library="css" name="default.css"/>`
    web/resources/css
* 使用 EL
    `<h:graphicImage value="#{resource['images:wave.med.gif']}"/>`
    web/resources/images

### 可重新定位的资源

作用：将资源标记放置在页面的 A 处，却使用标记的 target 属性指定它呈现在 B 处

### 资源库合约 Resource Library Contracts

作用：允许您为应用程序的不同部分定义不同的外观

需要做的工作：给 web 应用程序创建 `contracts` 部分

* 指定任意数量的命名区域，每个区域称为 `contracts`
* 在每个 `contracts` 中，您可以指定资源，如模板文件、样式表、 JavaScript 文件和图像

* 在 WEB-INF 文件夹下新建一个 `contracts` 文件夹，然后在里面分配构建不同的 `contract` 文件夹，各自表示一个合约，里面再放置相应的 xhtml, css, gif 和 js 文件

![image-20200904105438133](C:\Users\Gwan\AppData\Roaming\Typora\typora-user-images\image-20200904105438133.png)

* 可以将资源库契约打包到 JAR 文件中，然后放在 WEB-INF/lib/ 下

注意：当应用程序使用多个合约时，需要在 faces-config.xml 文件中的 `resource-library-contracts` 元素下指定合约的用法

#### hello1-rlc 示例

faces-config.xml 文件包含以下元素：

```oac_no_warn
<resource-library-contracts>
    <contract-mapping>
        <url-pattern>/reply/*</url-pattern>
        <contracts>reply</contracts>
    </contract-mapping>
    <contract-mapping>
        <url-pattern>*</url-pattern>
        <contracts>hello</contracts>
    </contract-mapping>
</resource-library-contracts>
```

### Html5-友好的标记

JSF 允许直接使用 HTML5标记，并且允许 HTML5元素中使用 JavaServer Faces 属性

JavaServer Faces 技术对 HTML5的支持分为两类：

- Pass-through elements 传递元素

    - 要使非 JavaServer Faces 元素成为传递元素，请使用 `http://xmlns.jcp.org/jsf` 名称空间指定其至少一个属性

- Pass-through attributes 传递属性

    - Pass-through 属性与 Pass-through 元素是相反的。它们允许你将非 JavaServer Faces 属性传递到浏览器而不需要解释

    - 指定传递属性的办法：

        - 使用 JavaServer Faces 名称空间作为传递属性的前缀

            ```oac_no_warn
            <html ... xmlns:p="http://xmlns.jcp.org/jsf/passthrough"
            ...
            
            <h:form prependId="false">
            <h:inputText id="nights" p:type="number" value="#{bean.nights}"
                         p:min="1" p:max="30" p:required="required"
                         p:title="Enter a number between 1 and 30 inclusive.">
                    ...
            ```

        - 将 `f:passthoutattribute` 标记嵌套在组件标记中

            ```o
            <h:inputText value="#{user.email}">
                <f:passThroughAttribute name="type" value="email" />
            </h:inputText>
            ```

        - 要传递一组属性，将 `f:passThroughAttributes` 标记嵌套在组件标记中，指定一个 EL 值，但是该值必须等价于 Map<String, Object> 

```
<html lang="en"
      xmlns="http://www.w3.org/1999/xhtml"
      xmlns:f="http://xmlns.jcp.org/jsf/core"
      xmlns:h="http://xmlns.jcp.org/jsf/html"
      xmlns:p="http://xmlns.jcp.org/jsf/passthrough"
      xmlns:jsf="http://xmlns.jcp.org/jsf">
```

```
<h:outputStylesheet name="css/stylesheet.css" target="head"/>
```

```
<input type="email" jsf:id="email" name="email" 
                       value="#{reservationBean.email}" required="required"
                       title="Enter email."/>
```

```
<h:inputText id="tickets" value="#{reservationBean.tickets}">
                    <f:passThroughAttributes value="#{reservationBean.ticketAttrs}"/>
                    <f:ajax event="change" render="total" 
                            listener="#{reservationBean.calculateTotal}"/>
                </h:inputText>
```

```
<h:inputText id="price" p:type="number" 
                             value="#{reservationBean.price}" 
                             p:min="80" p:max="120"
                             p:step="20" p:required="required" 
                             p:title="Enter a price: 80, 100, 120, or 140.">
                    <f:ajax event="change" render="total" 
                            listener="#{reservationBean.calculateTotal}"/>
                </h:inputText>
```





## 表达式语言 EL

作用：使表示层(web 页面)能够与应用程序逻辑(托管 bean)通信

EL 可被多种 JavaEE 技术所使用：

* JSF
* JSP
* Java EE 的上下文和依赖注入技术
* 独立环境

功能：

* 动态读取存储在 javabean 组件、各种数据结构和隐式对象中的应用程序数据
* 将数据(如用户输入)动态地写入 javabean 组件
* 调用任意的静态和公共方法
* 动态执行算术、布尔和字符串运算
* 动态构造集合对象并对集合执行操作

### 即时和延迟求值语法

支持表达式的

* 即时求值 Immediate Evaluation
    * 使用 `${}`语法
* 延迟求值 Deferred Evaluation
    * 使用 `#{}`语法

### 3 种表达式

* 值表达式

    * 功能：

        * 计算值
        * 引用对象
        * 引用对象属性或集合元素
        * 引用字面值
        * 参数化方法调用

    * 可细分为：

        * 左值表达式
        * 右值表达式

    * 可使用值表达式的场景：

        * 静态文本
        * 任何可以接受表达式的标准或自定义标记属性

    * 设置 tag 属性的办法：

        * 使用一个表达式构造:

            ```
            <some:tag value="${expr}"/>
            ```

        * 用一个或多个表达式分隔或用文本包围。嵌入在复合表达式中的每个表达式都被转换为 String，然后与任何中间的文本连接。然后，生成的 String 将转换为属性的预期类型。

            ```
            <some:tag value="some${expr}${expr}text${expr}"/>
            ```

        * 只有文字。属性的 String 值转换为属性的预期类型。

            ```
            <some:tag value="sometext"/>
            ```

* 方法表达式

    * 特点：
        * 因为可以在生命周期的不同阶段调用方法，所以方法表达式必须始终使用延迟计算语法
        * `#{object.method}` 等价于 `#{object["method"]}`

* Lambda 表达式

### 对集合对象的操作

3大类集合对象：

* 集合 sets

    * ```oac_no_warn
        {1,2,3}
        ```

* 列表 lists

    * ```oac_no_warn
        [1, "two", [three,four]]
        ```

* 映射 maps

    * ```oac_no_warn
        {"one":1, "two":2, "three":3}
        ```

处理元素时，使用 *流* 的形式。可以通过管道的形式，将操作链接在一起

一个 stream pipeline 由以下部分组成：

* 一个源（`Stream` 对象）
* 若干个中间操作，其中每个操作返回一个 stream
* 一个终端操作，不返回 stream

`stream` 方法从 2 个地方获取 `Stream`对象

* `java.util.Collection`
* 数组

`stream` 方法不修改原来的对象

http://docs.oracle.com/javase/8/docs/API/





## 在网页中使用 JSF 技术

一个典型的 JSF 网页包含以下元素:

- 一组声明 JSF 标签库的名称空间声明
- （可选）HTML head (`h:head`) 和 body (`h:body`) tags
- 表示用户输入组件的表单标记 `h: form`

为了在你的网页上添加 JavaServer Faces 组件，你需要提供对 2 个*标准标记库*的页面访问

* JavaServer Faces HTML 渲染工具包库
    * 定义了表示常见 HTML 用户界面组件的标签
    * https://javaee.github.io/tutorial/jsf-page002.html
* JavaServer Faces 核心标记库
    * 定义了执行核心操作的标签，这些标签独立于特定的渲染工具包
    * https://javaee.github.io/tutorial/jsf-page003.html

要使用任何 JavaServer Faces 标签，您需要在**每个页面的顶部**包含适当的指令来**指定**标签库

* 对于 Facelets 应用程序，XML 命名空间指令（xmlns）唯一地标识标记库 URI 和标记前缀



## 使用转换器，监听器和验证器

### 功能

* 转换器：转换从输入组件接收的数据
* 监听器：监听页面中发生的事件，并执行定义的操作
* 验证器：验证从输入组件接收的数据

### 使用标准转换器

JSF 实现提供了一套*转换器实现*，位于 javax.faces.convert 包中

目的：获取来自 Servlet API 的基于 string 的数据，并将其转换为适合业务领域的强类型 Java 对象

要使用特定的转换器转换组件的值，需要将转换器注册到组件上，注册方法有：

* 在组件的 tag 中，嵌套一个标准转换器 tag
* （最常用）将组件的值绑定到与转换器相同类型的托管 bean 属性
* 在组件 tag 的转换器属性引用转换器，指定转换器类的 ID
* 在组件 tag内嵌入一个 `f:converter` tag，并使用 `f:converter` tag 的 `converterId `属性或其绑定属性来引用转换器

### 在组件上注册侦听器

可以将侦听器实现为:

* 类
    * 组件可以从以下属性引用侦听器：
        * `f:valueChangeListener` 属性
        * `f:actionListener` 属性
    * 在组件上注册监听器的方法：
        * 将标记嵌套在组件标记中
* 托管 bean 的方法
    * 组件可以从以下属性引用侦听器：
        * `valueChangeListener` 属性
        * `actionListener` 属性

### 使用标准验证器

标准验证器类都实现了 `Validator` 接口，如果要自定义验证器，也要实现该接口。

### 引用托管 Bean 方法

引用托管 Bean 方法的组件标记属性有如下几个：

* `action`
    * 引用一个托管 bean 方法，该方法有可以执行以下 2 种操作之一：
        * 导航：指定一个逻辑输出字符串，该字符串告诉应用程序下一个访问哪个页面
        * 引用一个执行某种处理并返回逻辑输出字符串的托管 bean 方法
* `actionListener`
    * 引用处理操作事件的托管 bean 方法
    * 如果页面上的某个组件生成了一个action event（由 `action`属性指定），并且该事件由托管 bean 方法（由 `actionListener`属性指定）处理，则可以使用组件的 actionListener 属性引用该方法
* `validator`
    * 引用对组件值执行验证的托管 bean 方法
* `valueChangeListener`
    * 引用处理值更改事件的托管 bean 方法



## 使用 JSF 技术开发

### 创建托管 Bean

一个合格的托管 bean，包括：

* 无参构造函数
* 一组属性
    * 每个属性 = 一个 private 字段 + 一组getter/setter方法
    * 每个托管 bean 属性都可以绑定到以下属性之一：
        * 组件值
        * 组件实例
        * 转换器实例
        * 侦听器实例
        * 验证器实例
    * 当 bean 属性被绑定到组件的值时，bean 属性必须是应用程序可以按照适当转换器进行转换的类型
    * 当 bean 属性绑定到组件实例时，要求该属性的类型必须与组件对象相同
* 一组为组件执行函数的方法
    * 托管 bean 方法执行的最常见的功能包括:
        * 验证组件的数据
        * 处理由组件触发的事件
        * 执行处理以确定应用程序必须导航到的下一页

### 编写 Bean 属性

在组件的 tag 中，使用：

* `value` 属性，将组件的 `值` 绑定到托管 bean 属性
* `binding` 属性，将组件的 `实例` 绑定到托管 bean 属性

### 编写 Bean 方法

托管 bean 的方法可以为页面上的组件执行多个应用程序特定的函数：

* 执行与导航相关的处理
    * 无参
    * 返回一个 `Object`，用来确定下一个显示的页面的逻辑结果
    * 使用组件tag的action属性引用此方法
* 处理 action 事件
    * 参数：`ActionEvent`
    * 返回 void
    * 使用组件tag的actionListener属性引用此方法
        * 要求：该组件必须实现 `javax.faces.component.ActionSource`
* 对组件的值执行验证
    * 参数3个：
        * `javax.faces.context.FacesContext`，即上下文
        * 组件
        * 待验证的值
    * 返回 void
* 处理值更改事件
    * 参数：`ActionEvent`
    * 返回 void



## 使用 Ajax 和 JSF 技术

### 概述

目的：局部加载

Ajax  = JS & XML & 其它技术

当 JS 函数从客户机向服务器发送一个异步请求时，服务器会发回一个响应，用于更新页面的 DOM，这个响应通常是 XML 格式（或 JSON）

优点：

* 表单数据实时验证，无需提交表单进行验证
* 增强 web 页面的功能，如用户名和密码提示
* 部分更新网页内容，避免完整的页面重载

如何在 JSF 程序中使用 Ajax？

* 将所需的 JavaScript 代码添加到应用程序中（JSF 通过使用一个内置的 JavaScript 资源库来支持 Ajax）
    * 在 Facelets 程序的标准组件中，使用 `f:ajax` 标签
        * 如果该标签的 `event` 属性没有定义，那么将使用它的默认事件
    * 在 Facelets 应用程序中，直接使用 JS api 方法 `jsf.ajax.request()`
        * 此方法提供对 Ajax 方法的直接访问，并允许对组件行为进行自定义控制
    * 在视图中，通过使用 `<h:commandScript>`组件，执行服务器端的任意方法。
        * 该组件生成一个具有给定名称的 JavaScript 函数，当调用该名称时，该名称反过来通过 Ajax 调用给定的服务器端方法。
* 使用内置的 Ajax 资源库

### 发送 Ajax 请求

在后台，`jsf.ajax.request()`搜集由 `f:ajax` 标签提供的数据，并将请求发送到 JSF 生命周期

* 使用 `event` 属性
    * 功能：定义了触发 Ajax action 的事件
    * 例如：`click`, `keyup`, `mouseover`, `focus`, `blur` 等等
    * 如果未声明，则使用组件的默认事件
    * 这些事件其实来自于 JS 事件，只不过少了前缀on
* 使用 `execute` 属性
    * 功能：定义了在服务器将要执行的组件
    * 组件由它的 `id` 属性定义
* 使用 `immediate` 属性
    * 功能：决定了属性是否在程序的生命周期早期或稍后执行

* 使用 `listener` 属性
    * 功能：指向了服务器端的一个方法表达式，作为对 Ajax action 的一个响应

### 监视客户端上的事件

`f:ajax` 标记的 `onevent` 属性，指向一个 JS 函数。在处理一个 Ajax 请求的3个阶段（begin, complete, success）都会调用这个 onevent 函数

### 处理错误

调用 `onerror` 属性指向的 JS 函数

### 接收 Ajax 响应

`f:ajax`标签的`render`属性定义了处理响应的方式

### Ajax请求的生命周期

### 组件分组

### 加载 JS 作为资源

在 `javax.faces` 库里，有一个与 JSF 技术绑定的 JS 资源文件称为 `jsf.js`

`javax.faces` 资源库支持 JSF 程序中的 Ajax 功能

在 Facelets 应用程序中使用 JavaScript API，需要完成以下任务：

* 通过 `h:outputScript` 标记确定页面的默认 JavaScript 资源

    ```oac_no_warn
    <h:form>
        <h:outputScript name="jsf.js" library="javax.faces" target="head"/>
    </h:form>
    ```

* 确定要附加 Ajax 功能的组件

在 Bean 类中使用 `@ResourceDependency` 注释



















## 复合组件: 高级主题和示例

## 创建自定义 UI 组件和其他自定义对象

## 配置 JSF 应用程序

## 使用 JSF服务技术的 websocket

## Servlet 技术

## Java API for WebSockets

## JSON Processing

## JSON Binding

## Web 应用程序的国际化和本地化

