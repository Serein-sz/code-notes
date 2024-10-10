# Servlet

## 1.概述

### 1.1 什么是Servlet

​		Servlet 是基于 Jakarta 技术的 Web 组件，由容器管理，用于生成动态内容。与其他基于 Jakarta 技术的组件一样，Servlet 是独立于平台的 Java 类，它们被编译为平台中立的字节代码，这些字节代码可以动态加载到支持 Jakarta 技术的 Web 服务器中并由其运行。容器（有时称为 Servlet 引擎）是提供 Servlet 功能的 Web 服务器扩展。Servlet 通过 Servlet 容器实现的请求/响应范例与 Web 客户端进行交互。

### 1.2 什么是Servlet容器

​		Servlet 容器是 Web 服务器或应用程序服务器的一部分，它提供发送请求和响应的网络服务，对基于 MIME 的请求进行解码，并对基于 MIME 的响应进行格式化。Servlet 容器还在 Servlet 的整个生命周期中包含和管理 Servlet。

​		Servlet 容器可以构建到主机 Web 服务器中，也可以通过该服务器的本机扩展 API 作为附加组件安装到 Web 服务器。Servlet 容器也可以内置于支持 Web 的应用程序服务器中，或者可能安装在支持 Web 的应用程序服务器中。

​		所有 Servlet 容器都必须支持 HTTP 和 HTTPS 作为请求和响应的协议，但可能支持其他基于请求/响应的协议。容器必须实现的 HTTP 规范的必需版本是 HTTP/1.1 和 HTTP/2。当支持 HTTP/2 时，Servlet 容器必须支持“h2”和“h2c”协议标识符（如 HTTP/2 RFC 的第 3.1 节中所指定）。这意味着所有 servlet 容器都必须支持 ALPN。由于容器可能具有 RFC 9111（HTTP 缓存）中描述的缓存机制，因此它可以在将来自客户端的请求传送到 Servlet 之前修改这些请求，可以在将 Servlet 发送到客户端之前修改 Servlet 生成的响应，或者可能在符合 RFC 9111 的情况下响应请求而不将其传送到 Servlet。

​		Servlet 容器可能会对 Servlet 的执行环境施加安全限制。

​		Java SE 17 是底层 Java 平台的最低版本，必须使用该版本构建 servlet 容器。