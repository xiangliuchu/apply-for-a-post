# 自总结知识点
## springMVC中不同注解的作用   详细见：[springMVC](D:\Java\java50th\java50-course-materials\03-JavaEE&Spring框架\02-笔记\Day22-26-SpringMVC.md)

### 1 @RequestMapping : 用于请求路径和处理方法的注解 ， 主要实现一些限定

```java
1.★★★value: url路径映射，窄化请求； 如： @RequestMapping(value={"hello"})//映射URL → localhost:8080/hello
2.method: 请求方法的限定；      如： @RequestMapping(value = "get",method = RequestMethod.GET)
3.params: 请求参数的限定：      如： @RequestMapping(value = "login",params = {"username","password"})
4.headers: 请求头的限定；       如： @RequestMapping(value = "limit",headers = {"abc","def"})
5.consumes: content-type这个请求头的限定→ 这个请求头的含义：请求携带的 正文的类型（请求体） 如一个jpg文件：image/jpeg如文本：text/html 如json：application/json
6.produces: 限定的是Accept这个请求头的值 → 这个请求的含义 → 客户端希望接收到的服务端响应的正文类型   比如一个jpg文件：image/jpeg  比如文本：text/html  比如json：application/json 
```

### 2  放在方法的形参中的一些注解：

- 请求URL的信息 → @PathVariable     获得请求url中占位符所在的值

  ```java
  //localhost:8080/path/songge
  //@PathVariable → 获得请求URL的一部分值 → 在@RequestMapping的value属性写占位符{}
  // 获得指定占位符位置的值给到所对应的形参 → 形参通过注解接收指定占位符位置的值
  // 通过这个注解，就可以把一部分请求参数写到URL中，比如豆瓣、CSDN
  @RequestMapping("path/{username}")
  public BaseRespVo path(@PathVariable("username")String name) {
      System.out.println("name = " + name);
      return BaseRespVo.ok();
  }
  ```

  

- 请求参数信息 → @RequestParam

- 请求头信息 → @RequestHeader

- Cookie信息 → @CookieValue

- Session信息 → @SessionAttribute

### 3. @ResponseBody 如果写在类上，意味着当前类下所有的方法响应的都是字符串或Json字符串 （附 ：@RestController）

引申注解：<span style='color:yellow;background:red;font-size:文字大小;font-family:字体;'>**@RestController **</span>= @Controller + @ResponseBody

@RestController的用途：

1. **自动转换为响应体格式**：@RestController 注解会自动将方法返回的对象转换为JSON或XML格式的响应体，根据请求的 Accept 头部信息确定响应的格式。
2. **常用于构建RESTful API**：@RestController 注解通常用于构建RESTful风格的API，可以方便地处理HTTP请求和返回结构化数据。
3. **与 @RequestMapping 结合使用**：@RestController 注解可以与 @RequestMapping 注解结合使用，以定义控制器类或方法的请求映射路径和处理逻辑。
4. **支持常见的HTTP方法**：@RestController 注解支持常见的HTTP方法，如GET、POST、PUT、DELETE等，通过不同的请求映射路径和方法来处理不同的请求。
5. **无需额外的视图解析器**：由于 @RestController 注解主要用于返回数据而不是视图，因此不需要额外配置视图解析器。
