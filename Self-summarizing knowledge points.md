# 自总结知识点
## springMVC中不同注解的作用   详细见：[springMVC](D:\Java\java50th\java50-course-materials\03-JavaEE&Spring框架\02-笔记\Day22-26-SpringMVC.md)

### @RequestMapping : 用于请求路径和处理方法的注解 ， 主要实现一些限定

```java
1.★★★value: url路径映射，窄化请求； 如： @RequestMapping(value={"hello"})//映射URL → localhost:8080/hello
2.method: 请求方法的限定；      如： @RequestMapping(value = "get",method = RequestMethod.GET)
3.params: 请求参数的限定：      如： @RequestMapping(value = "login",params = {"username","password"})
4.headers: 请求头的限定；       如： @RequestMapping(value = "limit",headers = {"abc","def"})
5.consumes: content-type这个请求头的限定→ 这个请求头的含义：请求携带的 正文的类型（请求体） 如一个jpg文件：image/jpeg如文本：text/html 如json：application/json
6.produces: 限定的是Accept这个请求头的值 → 这个请求的含义 → 客户端希望接收到的服务端响应的正文类型   比如一个jpg文件：image/jpeg  比如文本：text/html  比如json：application/json 
```

### 放在方法的形参中的一些注解：

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

### @ResponseBody 如果写在类上，意味着当前类下所有的方法响应的都是字符串或Json字符串 （附 ：@RestController）

引申注解：<span style='color:yellow;background:red;font-size:文字大小;font-family:字体;'>**@RestController **</span>**= @Controller + @ResponseBody**

@RestController的用途：

1. **自动转换为响应体格式**：@RestController 注解会自动将方法返回的对象转换为JSON或XML格式的响应体，根据请求的 Accept 头部信息确定响应的格式。
2. **常用于构建RESTful API**：@RestController 注解通常用于构建RESTful风格的API，可以方便地处理HTTP请求和返回结构化数据。
3. **与 @RequestMapping 结合使用**：@RestController 注解可以与 @RequestMapping 注解结合使用，以定义控制器类或方法的请求映射路径和处理逻辑。
4. **支持常见的HTTP方法**：@RestController 注解支持常见的HTTP方法，如GET、POST、PUT、DELETE等，通过不同的请求映射路径和方法来处理不同的请求。
5. **无需额外的视图解析器**：由于 @RestController 注解主要用于返回数据而不是视图，因此不需要额外配置视图解析器。

###  @Autowired用于组件的注入，如方法注入、成员变量等     详细见：[SpringIOC](D:\Java\java50th\java50-course-materials\03-JavaEE&Spring框架\02-笔记\Day18-20-Spring-IOC.md)

###  @Qualifier

**`@Qualifier` 是一个用于解决依赖注入中多个候选对象的歧义性的Spring框架注解。它可以与 `@Autowired` 注解一起使用，通过指定限定符来精确选择要注入的bean对象。**

当有多个类型匹配的候选bean对象时，Spring无法确定要注入哪个对象。这时，可以使用 `@Qualifier` 注解结合自定义的限定符来明确指定要注入的bean对象。

以下是关于 `@Qualifier` 注解的一些要点：

1. **限定符概念**：限定符是一个用于标识bean对象的标签，可以自定义命名。通过使用 `@Qualifier` 注解，可以将一个限定符与被注入的bean对象相关联。

2. **与 `@Autowired` 结合使用**：`@Qualifier` 注解通常与 `@Autowired` 注解一起使用，以指定要注入的bean对象的限定符。

3. **按照限定符匹配**：当存在多个匹配的bean对象时，Spring会尝试匹配被 `@Qualifier` 注解指定的限定符与bean对象的限定符进行匹配，从而确定要注入的对象。

4. **自定义限定符**：可以通过自定义注解或者使用字符串来定义限定符。自定义注解需要使用元注解 `@Qualifier` 进行标注，以确保其被用作限定符。

以下是一个使用 `@Qualifier` 注解的示例：

```java
@Service
public class UserService {

    @Autowired
    @Qualifier("userDaoImpl")
    private UserDao userDao;

    // ...
}
```

在上述示例中，`UserService` 类使用 `@Autowired` 注解将 `UserDao` 对象注入到 `userDao` 字段中。通过在 `@Qualifier` 注解中指定限定符 `"userDaoImpl"`，可以确保注入的是具有相应限定符的 `UserDao` bean 对象。

总而言之，`@Qualifier` 注解是Spring框架中用于解决依赖注入中多个候选对象歧义性的关键注解之一。它允许精确指定要注入的bean对象，以确保正确的依赖关系被建立。

## mybatis-plus中的分页插件  注意看getSize(), getPages()是表示的什么

MyBatis Plus自带分页插件，只要简单的配置即可实现分页功能

添加配置类

```java
@Configuration
//@MapperScan("com.cskaoyan.mybatisplus.mapper") //可以将主类中的注解移到此处
public class MybatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
	}
}
```

```java
@Test
public void testPage(){
    //设置分页参数
    // 注意页数从1开始
    Page<User> page = new Page<>(1, 2);
    userMapper.selectPage(page, null);
    //获取分页中的每一条记录
    List<User> list = page.getRecords();
    list.forEach(System.out::println);
    System.out.println("当前页：" + page.getCurrent());
    System.out.println("每页显示的条数：" + page.getSize());
    System.out.println("总记录数：" + page.getTotal());
    System.out.println("总页数：" + page.getPages());
    System.out.println("是否有上一页：" + page.hasPrevious());
    System.out.println("是否有下一页：" + page.hasNext());
}
```

## thymleaf渲染引擎的使用

为了优化客户端渲染，使用**后端渲染**(服务端渲染)

客服端渲染过程：

- 浏览器发起请求，加载html页面
- 在成功获取html页面;，并在浏览器中生成DOM的时候，如果遇到js，css标签，又会去下载所需的js，css脚本
- 获取到js脚本后，如果js脚本又需要访问后端数据，那么又会向后端发起请求

这种渲染生成页面的方式，我们称之为客户端渲染，稍微思考一下，我们就会发现，从效率的角度出发，客户端渲染可能存在问题：

- 客户端渲染至少发起http请求三次，第一次是请求HTML页面，第二次是请求页面里的js脚本（渲染dom和进行事件绑定，而且这个过程时同步阻塞的），第三次是通过js请求获取动态数据，所以首次加载一个页面可能是比较慢的，就有可能出现白屏现象
- 在高并发场景下，页面中的动态数据，可能对应着多次异步请求，比如商品详情页中，商品的图片列表url，商品的名称，商品的销售属性等等，这些数据可能变化很少，但只要加载一次页面就都会发送请求，给后端服务器带来比较大的压力

如何**优化上述问题**呢？采用后端渲染，其核心思想如下：

- 与客户端渲染页面不同，后端渲染指的是，在返回页面的时候就将动态数据直接写入到返回给客户端的HTML文件中
- 这样一来，客户端就不用在获取HTML页面后，再次通过异步请求来获取这些动态数据，而是直接使用包含所需数据的HTML，对于前端，少了请求数据的延迟，对于后端减少了处理请求的数量
- 对于另外的一些需要根据用户的行为，来实时获取的动态数据，前端仍然可以使用ajax向后端发起动态请求实时获取





