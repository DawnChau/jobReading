### 1. 什么是Spring的MVC框架？

- Spring 配备构建Web 应用的全功能MVC框架。Spring可以很便捷地和其他MVC框架集成，如Struts，Spring 的MVC框架用控制反转把业务对象和控制逻辑清晰地隔离。它也允许以声明的方式把请求参数和业务对象绑定。
- Spring的MVC框架是围绕DispatcherServlet来设计的，它用来处理所有的HTTP请求和响应。
- WebApplicationContext 继承了ApplicationContext  并增加了一些WEB应用必备的特有功能，它不同于一般的ApplicationContext ，因为它能处理主题，并找到被关联的servlet。

### 2. Springmvc的工作流程

- 用户发送请求至前端控制器DispatcherServlet

- DispatcherServlet收到请求**调用HandlerMapping处理器映射器**。

- 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。

- DispatcherServlet通过HandlerAdapter处理器适配器调用处理器

- 执行处理器(Controller，也叫后端控制器)。

- Controller执行完成返回ModelAndView

- HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet

- DispatcherServlet将ModelAndView传给ViewReslover视图解析器

- ViewReslover解析后返回具体View
- DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）。