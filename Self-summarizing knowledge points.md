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

### @RequestBody，表示接收时是json

**注意： 方法的形参列表只能写一个@RequestBody，因为HTTP请求的请求体只能被读取一次，如果实在要读取多个参数，可以将他们封装成一个更复杂的对象。**

Json请求参数仍然是在形参中进行接收，可以使用以下几种类型来接收

- **String**
- **引用类型对象**
- **Map**

**形参前需要增加一个注解@RequestBody**

> @RequestBody和@ResponseBody这两个注解都是和Json打交道的时候使用的注解
>
> 接收的时候用的是@RequestBody
>
> 响应的时候用的是@ResponseBody



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

### @PostConstruct

`@PostConstruct` 是一个 Java 注解，用于标记一个方法，**在 Spring 容器初始化 bean 之后立即执行该方法**。它被用作初始化回调方法，用于在 bean 的依赖注入完成后执行一些必要的初始化逻辑。

**当一个 bean 被创建并且其依赖关系已经注入完成后，Spring 容器会调用带有 `@PostConstruct` 注解的方法。这个方法可以用来执行任何初始化操作，例如初始化成员变量、建立数据库连接、加载配置等。**

使用 `@PostConstruct` 注解的方法必须满足以下条件：
- 方法不能有任何参数。
- 方法的访问修饰符可以是 public、protected 或者默认的包内可见性。
- 方法不能被 static、final 或者 abstract 修饰。
- 方法只能在单例的 bean 上使用。

示例代码如下所示：

```java
@Component
public class MyBean {

    private String name;

    @PostConstruct
    public void init() {
        // 执行初始化逻辑
        this.name = "Initialized name";
        // 其他初始化操作...
    }

    // 其他方法...
}
```

在上面的例子中，`init()` 方法被标记为 `@PostConstruct`，当 Spring 容器创建 `MyBean` 实例并完成依赖注入后，会自动调用 `init()` 方法。在 `init()` 方法中，可以执行任何需要在 bean 初始化后进行的操作。

总而言之，`@PostConstruct` 注解可以确保在 Spring bean 的依赖注入完成后立即执行指定的初始化方法，以便进行一些必要的初始化操作。



### @value

**@Value 注解允许你将配置文件中的值注入到 Spring 管理的组件中，可以灵活地注入到字段、构造函数、方法参数或者注解上。**



@Value 是一个注解，用于在 Spring 中注入配置属性的值。它可以用于将配置文件中的值注入到类的字段、构造函数、方法参数或者注解上。

使用 @Value 注解时，你可以指定一个表达式或者直接指定一个属性的值。这个注解可以用于任何 Spring 管理的组件，如 Bean、Controller、Service 等。

下面是一些使用 @Value 注解的示例：

1. 注入配置文件中的值到字段：

```java
@Component
public class MyComponent {
    @Value("${my.property}")
    private String myProperty;

    // 其他代码...
}
```

在上面的例子中，my.property 是在配置文件中定义的属性，使用 @Value 注解将其注入到 myProperty 字段中。



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
- **在高并发场景下，页面中的动态数据，可能对应着多次异步请求，比如商品详情页中，商品的图片列表url，商品的名称，商品的销售属性等等，这些数据可能变化很少，但只要加载一次页面就都会发送请求，给后端服务器带来比较大的压力**

```
发起请求：浏览器向服务器发送请求，请求一个HTML文档。
下载HTML文档：服务器将HTML文档作为响应返回给浏览器。
解析HTML：浏览器开始解析HTML文档，并构建DOM（文档对象模型）树，表示网页的结构。
加载CSS和JavaScript：浏览器解析HTML文档时，如果遇到CSS和JavaScript的引用，会开始下载这些资源。
执行JavaScript：浏览器执行已下载的JavaScript代码。在执行过程中，JavaScript可以通过DOM API修改DOM树、添加事件处理程序等。
请求数据：JavaScript代码可以通过AJAX或Fetch等方式向服务器请求数据，通常是使用API或后端接口。
数据处理和渲染：一旦数据返回，JavaScript代码会使用这些数据来生成或更新页面的内容。这可能涉及数据处理、模板渲染和DOM操作。
更新DOM：根据JavaScript代码生成的新内容，浏览器会更新DOM树。这可能包括插入、删除、修改DOM元素等操作。
呈现页面：浏览器根据更新后的DOM树和CSS样式进行布局和渲染，将页面呈现给用户。
监听事件：页面呈现完毕后，浏览器会监听用户的交互事件（如点击、滚动等），并触发相应的事件处理程序。
```

如何**优化上述问题**呢？采用后端渲染，其核心思想如下：

- 与客户端渲染页面不同，后端渲染指的是，在返回页面的时候就将动态数据直接写入到返回给客户端的HTML文件中
- 这样一来，**客户端就不用在获取HTML页面后，再次通过异步请求来获取这些动态数据，而是直接使用包含所需数据的HTML，对于前端，少了请求数据的延迟，对于后端减少了处理请求的数量**
- 对于另外的一些需要根据用户的行为，来实时获取的动态数据，前端仍然可以使用ajax向后端发起动态请求实时获取

## 分布式锁 （缓存击穿）       [商品详情优化](D:\Java\java50th\java50-course-materials\04-微服务\01-课件\13_商品详情页2\商品详情优化.md)

### 基于setnx来实现    保证服务实例进来后用的都是同一把锁

```
# set not exists,设置一个键值对，不会覆盖原来的值
# 当key不存在的时候，再去设值，不会覆盖原来的值
setnx key value
```

要实现一把锁，最主要是要模拟一把锁加锁和释放锁的状态。我们可以在Redis中定义一个string类型的值，把这个值的key当做是锁的名字，于是我们可以用是否有该key对应的值，当做是锁是否上锁的状态：

- key对应的value值存在，说明锁被上锁了，不能重复加锁
- key对应的value值不存在，说明锁还没有被上锁，可以加锁(就是在redis中添加该key对应的value值)

![](D:/Java/java50th/java50-course-materials/04-微服务/01-课件/13_商品详情页2/商品详情优化.assets/商品详情页-setnx 加锁.png)

所以这样的加锁操作，刚刚好可以用SETNX来完成，所以可以改造我们的代码如下：

```java
@Service
public class TestServiceImpl implements TestService {

    @Autowired
    RedissonClient redissonClient;

    @Override
    public void incrWithLock() {
        // 在操作Redis中的数据之前先加锁, lock:number 对应的值可以是任意的
        RBucket<String> lockBucket = redissonClient.getBucket("lock:number");
        // trySet方法等价于SETNX
        boolean exists = lockBucket.trySet("lockObj");
        if (!exists) {
            // 如果锁已存在，即已经加锁, 则稍后重试
            try {
                Thread.sleep(100);
                incrWithLock();
                return;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }

        // 如果加锁成功，则自增key number对应的值
        try {
            RBucket<Integer> bucket = redissonClient.getBucket("number");
            // 获取key为number的value值
            int number = bucket.get();
            // 自增1
            number++;
            // 在放回redis
            bucket.set(number);
        } finally {
            // 访问完数据之后，释放锁，即删除lock:number这个key
            lockBucket.delete();
        }

    }
}

```

![](D:/Java/java50th/java50-course-materials/04-微服务/01-课件/13_商品详情页2/商品详情优化.assets/商品详情页-使用setnx结果.png)

看起来，好像也没啥问题，**但是这种实现方式有一个潜在的问题，就是如果在某一个商品服务实例中，加锁成功之后，因为某些原因，在还未释放锁之前，该实例挂了(java进程挂了)，那就意味着这把锁永远不会被释放，那么其他服务实例就再也访问不到这把锁了**。

### 增加过期时间     防止进程挂掉，永远不释放锁，这样其他实例，依然可以加锁成功

针对上述的可能存在的问题，我们可以增加一个解决方案就是，在利用SETNX加锁成功之后，给锁(给key)设置过期时间，这样一来，如果因为意外情况没有释放锁，到了锁的过期时间，其他服务实例，依然可以加锁成功。

![](D:/Java/java50th/java50-course-materials/04-微服务/01-课件/13_商品详情页2/商品详情优化.assets/setnx 增加过期时间.png)

所以，结合过期时间，我们改造代码如下：

```java
@Service
public class TestServiceImpl implements TestService {

    @Autowired
    RedissonClient redissonClient;

    @Override
    public void incrWithLock() {
        // 在操作Redis中的数据之前先加锁, lock:number 对应的值可以是任意的
        RBucket<String> lockBucket = redissonClient.getBucket("lock:number");
        // trySet方法等价于SETNX
        boolean exists = lockBucket.trySet("lockObj", 3, TimeUnit.SECONDS);
        if (!exists) {
            // 如果锁已存在，即已经加锁, 则稍后重试
            try {
                Thread.sleep(100);
                incrWithLock();
                return;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }

        // 如果加锁成功
        try {
            RBucket<Integer> bucket = redissonClient.getBucket("number");
            // 获取key为number的value值
            int number = bucket.get();
            // 自增1
            number++;
            // 在放回redis
            bucket.set(number);
        } finally {
            // 访问完数据之后，释放锁，即删除lock:number这个key
            lockBucket.delete();
        }

    }
}
```

以上代码如果测试是没问题的。但是这种实现方式，仍然有**潜在**的**问题**：

- **假设商品服务实例1先加锁成功，开始执行了，但是它执行4秒中，才会释放锁**
- **但是过了3秒后，锁过期了，商品服务实例2加锁成功**
- **又过了1s商品服务实例1执行完，释放锁，但是服务实例2还在执行，此时相当于没加锁**

### 增加UUID防止误删   实例1还未执行完时，锁已过期，实例2加锁后，被实例1误删掉锁

所以，为了防止锁被误删，所以在加锁的时候，我们给锁key对应的value，设置为一个uuid，并保存这个uuid。在释放锁的时候，如果获取到了锁，还要看看锁的value

- 如果锁key对应的value和释放锁的线程锁持有的uuid是不是同一个，说明是加锁线程在释放锁没有问题
- 但是如果不一致，说明加锁线程和释放锁的线程不是同一个，不能释放锁

![](D:/Java/java50th/java50-course-materials/04-微服务/01-课件/13_商品详情页2/商品详情优化.assets/商品详情页 增加uuid.png)

这样一来，就可以解决，锁的误删问题，代码如下

```java
@Service
public class TestServiceImpl implements TestService {

    @Autowired
    RedissonClient redissonClient;

    @Override
    public void incrWithLock() {
        // 在操作Redis中的数据之前先加锁, lock:number 对应的值可以是任意的
        RBucket<String> lockBucket = redissonClient.getBucket("lock:number");
        String uuid = UUID.randomUUID().toString();
        // trySet方法等价于SETNX，设置锁key对应的值
        boolean exists = lockBucket.trySet(uuid, 3, TimeUnit.SECONDS);
        if (!exists) {
            // 如果锁已存在，即已经加锁, 则稍后重试
            try {
                Thread.sleep(100);
                incrWithLock();
                return;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }

        // 如果加锁成功
        try {
            RBucket<Integer> bucket = redissonClient.getBucket("number");
            // 获取key为number的value值
            int number = bucket.get();
            // 自增1
            number++;
            // 在放回redis
            bucket.set(number);
        } finally {
            if (uuid.equals(lockBucket.get())) {
                // 说明是加锁线程在释放锁，可以正确释放
                lockBucket.delete();
            }

        }

    }
}

```

但是增加uuid防止误删就完美了吗？当然不是，因为还是有可能会有问题：

- **假设商品服务实例1，先加锁，访问完Redis数据后，刚刚执行完uuid.equals(lockBucket.get())发现结果为true，准备释放锁了**
- **但是在释放锁之前，刚好锁也过期了，商品服务实例2继续加锁成功**
- **然后，商品服务实例1删除锁，测试商品服务实例2在访问Redis数据时相当于没有加锁**

**究其原因，就是因为判断和释放锁不是原子操作。**

### Redisson实现的分布式锁    以上还有问题，判断和释放锁不是原子操作，还可能误删

所以，我们还需要让判断和释放锁成为原子操作，怎么样让它们成为原子操作呢？用Lua脚本来实现它们即可。因为Redis的工作线程是单线程，且Lua脚本可以直接在Redis中运行，所以一段Lua脚本中运行的必然是一个原子操作。而Redisson底层，就是利用Lua脚本来加锁和释放锁的。

如果使用Redisson，则代码如下：

```java
@Service
public class TestServiceImpl implements TestService {

    @Autowired
    RedissonClient redissonClient;

    @Override
    public void incrWithLock() {
        // 获取锁
        RLock redisLock = redissonClient.getLock("lock:number");
        try {
            // 加锁，失败会在这里阻塞
            redisLock.lock();
            // 加锁成功，代码执行到这里
            RBucket<Integer> bucket = redissonClient.getBucket("number");
            // 获取key为number的value值
            int number = bucket.get();
            // 自增1
            number++;
            // 在放回redis
            bucket.set(number);
        } finally {
            // 释放锁
            redisLock.unlock();
        }

    }
}

```

为了确保**分布式锁可用**，我们要确保锁的实现同时满足以下**四个条件**：

**\- 互斥性。在任意时刻，只有一个客户端能持有锁。**

**\- 不会发生死锁。即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。**

**\- 解铃还须系铃人。加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了。**

**\- 加锁和解锁必须具有原子性**

**而这四个条件，Redisson实现的分布式锁都可以满足，同时Redisson实现的分布式锁，还是可重入的。**



## 自定义注解

### 语法：

```
权限修饰符 @interface 注解名字{
    // 注解体定义
    属性类型 属性名();
    属性类型 属性名();
    属性类型 属性名();
    ......
}

例如：

// 定义注解
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface Login{
    // 属性
    String username() default "admin";

    String password() default "123456";
}
```

### 元注解

**@Retention元注解，来定义我们自己定义的注解的保留级别.**

- RetentionPolicy.RUNTIME
- RetentionPolicy.CLASS  默认
- RetentionPolicy.SOURCE

**@Target元注解，注解可以作用的目标**

对于注解而言，可以作用的目标：

1. 整个类    ElementType.TYPE
2. 成员变量   ElementType.FIELD
3. 构造方法   ElementType.CONSTRUCTOR
4. 成员方法   ElementType.METHOD

### 注意事项

**使用的时候注解的每个属性都要赋值**

```java
@注解名(属性1=属性值,属性2=属性值)
```

**注意事项:**

- 每个属性都要赋值
- 可以不赋值,但是要有默认值, default
- 数组形式赋值 {}
- 如果只有1个属性, 名字叫value, 可以简化赋值
- 如果属性类型是引用类型, 不能是null

### 注解的属性值

自定义注解的属性可以用于在注解中存储和**传递信息**，以便在需要时能够根据这些信息进行相应的处理。通过注解的属性，你可以在注解中提供一些可配置的选项，使得注解能够适应不同的使用场景。

## AspectJ

​	切面组件

- 增加AspectJ的注解开关 → 配置类上增加一个注解**@EnableAspectJAutoProxy**
- 把组件标记为切面组件 → **@Aspect**

### 切入点表达式

引入Aspectjweaver依赖

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
</dependency>
```

在切面组件中使用**@Pointcut**

- value属性：切入点表达式
- 方法名：作为切入点id

```java
 @Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
public @interface Pointcut {
    String value() default "";

    String argNames() default "";
}
```

###  execution

- 能否省略

- 能否通配

- 特殊用法

修饰符：可以省略，省略代表任意修饰符

返回值：不能省略，POJO类要写全类名；可以是用*作为通配符

包名、类名、方法名：使用..部分省略，头和尾不能省略，中间的任意一部分都可以省略；可以使用*作为通配符

形参：可以使用..或* 来进行通配，<font color=red>..代表任意数量任意类型的参数，*代表任意类型的参数；POJO类要写全类名</font>

```java
<!--execution 👉 aop:pointcut 👉 指定的是容器中的组件中的方法-->

<!--语法：execution(访问修饰符 返回值 包名.类名.方法名(参数列表))

        1、能否省略不写
        2、能否通配
        3、是否有特殊用法

        访问修饰符：可以省略、省略不写代表任意修饰符
        返回值：不可以省略、可以使用*来通配、JavaBean要写全类名（基本类型、包装类以及String可以直接写）
        包名、类名、方法名：可以部分省略，头和尾不能省略，中间的任意一部分都可以省略 👉 ..来进行省略
                         *来通配 com.*、com.cskao*，头和尾也可以使用*作为通配符                                          *.cskaoyan.service.UserService.say*
        参数列表：可以省略 👉 代表的是无参的方法
                如果想要写多个参数，写参数的全类名 👉 第一参数是String，第二个参数是int 👉 (String,int)
                可以通配：* 👉 代表的单个任意参数
                        .. 👉 任意参数 👉 任意数量的任意类型的参数
                JavaBean要写全类名（基本类型、包装类以及String可以直接写）
-->
/**
     * execution(修饰符 返回值 包名、类名、方法名(形参))
     * 可以增强多个方法
     *     越具体，匹配范围越小；越宽泛，匹配范围越大。
     *     通配符
     *     - 修饰符： 可以省略不写，如果省略不写代表任意修饰符
     *     - 返回值： 不能省略，可以使用通配符* 。*代表任意值
     *               如果是引用类型，要写全限定类名;全限定类名中也可以出现通配符*
     *     - 包名、类名、方法名： 可以使用*来通配，也可以使用..来通配
     *                         头和尾的位置不能使用..  但是可以使用*
     *     - 形参： 省略不写代表无参方法
     *             可以使用*来通配 → 代表单个任意类型的参数
     *             也可以使用..来通配 → 代表任意参数 → 数量任意，类型也任意
     *             如果是引用类型，要写全限定类名;全限定类名中也可以出现通配符*
     */
    //@Pointcut("execution(public void com.cskaoyan.demo3.service.UserServiceImpl.sayHello(java.lang.String))")
    //@Pointcut("execution(public * com.cskaoyan.demo3.service.UserServiceImpl.sayHello(java.lang.String))")
    //@Pointcut("execution(public com.cskaoyan.demo3.bean.User com.cskaoyan.demo3.service.UserServiceImpl.query(Integer))")
    //@Pointcut("execution(public com.cskaoyan.demo3.bean.* com.cskaoyan.demo3.service.UserServiceImpl.query(Integer))")
    //@Pointcut("execution(public com.cskaoyan.demo3.bean.* com.cskaoyan.*.ser*.*ServiceImpl.query(Integer))")
    //@Pointcut("execution(public com.cskaoyan.demo3.bean.* com..*ServiceImpl.query(Integer))")
    // 增强service层 以say作为开头的方法(暂时先没管参数)
    //@Pointcut("execution(* com..service.*ServiceImpl.say*(String))")
    @Pointcut("execution(* com..service.*ServiceImpl.*(..))")
    public void pointcut1(){}
```

```java
@Pointcut("execution(public void com.cskaoyan.service.UserServiceImpl.sayHello(String))")
public void mypointcut1(){}
```

**execution和@annotation有什么区别：**

`execution`和`@annotation`是用于定义切点表达式（pointcut expression）的两种不同方式。

- `execution`是一种切点表达式，它基于方法的签名和访问修饰符来匹配方法。它可以定义在切面中使用的切点，以便选择特定的方法进行增强或拦截。例如，`execution(public void com.example.MyClass.myMethod())`表示选择`MyClass`类中的`myMethod`方法，而`public`是访问修饰符。
  
- `@annotation`是一种切点指示器（pointcut designator），它基于注解的存在来匹配方法。它可以用于切面中的切点表达式，以便选择被特定注解标记的方法进行增强或拦截。例如，`@annotation(com.example.MyAnnotation)`表示选择被`MyAnnotation`注解标记的方法。

区别在于：
- `execution`是基于方法的签名和访问修饰符进行匹配，而`@annotation`是基于注解的存在进行匹配。
- `execution`可以匹配方法的任何特征（如方法名、参数类型等），而`@annotation`只匹配被指定注解标记的方法。
- `execution`可以选择多个方法进行匹配，而`@annotation`只能选择被特定注解标记的方法。

在使用切面编程时，你可以根据需要选择使用`execution`还是`@annotation`，**具体取决于你想要匹配哪些方法和条件**。

### @annotation   (只是一些不同的匹配方式)

写上自定义注解的全类名

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface CountTime {
}
```

```java
@Pointcut("@annotation(com.cskaoyan.CountTime)")
public void mypointcut2(){}
```

### @target

写上自定义注解的全类名

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.Type)
public @interface CountTimeAllMethod {
}
```

```java
@Pointcut("@target(com.cskaoyan.CountTimeAllMethod)")
public void mypointcut3(){}
```

### Aspect切面

在切面类中配置切面组件和通知方法

AspectJ的注解实现，首先要标记前面开关

然后在切面类中使用AspectJ相关的注解，表达切面、切入点、通知

```java
/**
 * 增加对应通知的方法 ： 将这样的一些方法配置为对应的通知方法
 * 方法 👉 通知
 *  before
 *  after
 *  around
 *  afterReturning
 *  afterThrowing
 */
@Component
@Aspect
public class CustomAspect {
  /**
     * pointcut方法
     * 返回值：void
     * 方法名：任意写 👉 用于作为pointcut组件的id
     * 形参：不用写
     * 方法体：不用写
     * @Pointcut的value属性写切入点表达式
     */
  @Pointcut("execution(* com.cskaoyan.service..*(..))")
  public void mypointcut1(){}
  @Pointcut("@annotation(com.cskaoyan.CountTime)")
  public void mypointcut2(){}
  @Pointcut("@annotation(com.cskaoyan.CountTimeAllMethod)")
  public void mypointcut3(){}
  /**
     * 通知注解@Before、@After、@Around、@AfterReturning、@AfterThrowing
     * value属性：
     *      1、对应的切入点表达式
     *      2、引用的切入点表达式的方法名
     */

  /**
     * before通知
     * 返回值：void
     * 方法名：任意去写
     * 形参：joinPoint连接点 (可写可不写)
     */
  //@Before("execution(* com.cskaoyan.service..*(..))")
  @Before("mypointcut()")
  public void before(JoinPoint joinPoint){
    System.out.println("before");
  }
  /**
     * after通知
     * 返回值：void
     * 方法名：任意去写
     * 形参：joinPoint连接点 (可写可不写)
     */
  @After("mypointcut()")
  public void after(){
    System.out.println("after");
  }
  /**
     * around通知
     * 返回值：Object
     * 方法名：任意去写
     * 形参：ProceedingJoinPoint连接点 (必须写) 👉 提供了proceed方法，执行的是委托类的代码
     */
  @Around("mypointcut()")
  public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
    System.out.println("around的before");

    Object proceed = proceed = joinPoint.proceed();//该方法执行的是委托类的方法

    System.out.println("around的after");
    return proceed;
  }
  /*public Object around(ProceedingJoinPoint joinPoint){
        System.out.println("around的before");
        Object proceed = null;//该方法执行的是委托类的方法
        try {
            proceed = joinPoint.proceed();
        } catch (Throwable throwable) {
            throwable.printStackTrace();
        }
        System.out.println("around的after");
        return proceed;
    }*/
  /**
     * afterReturning通知
     * 返回值：void
     * 方法名：任意去写
     * 形参：Object(委托类方法的执行结果)
     */
  @AfterReturning(value = "mypointcut()",returning = "result")
  public void afterReturning(Object result){
    System.out.println("afterReturning");
  }
  /**
     * afterThrowing通知
     * 返回值：void
     * 方法名：任意去写
     * 形参：Exception/Throwable(委托类方法执行过程中抛出异常)
     */
  @AfterThrowing(value = "mypointcut()",throwing = "exception")
  public void afterThrowing(Exception exception){//Throwable
    System.out.println("afterThrowing");
    System.out.println(exception.getMessage());
  }

}
```

### JoinPoint连接点

获取增强过程中的一些值

- Signature 方法
- Arguments 参数
- This 代理对象
- Target 委托类对象

在通知方法的形参中，传入JoinPoint形参，通过JoinPoint获得对应的一些参数

```java
@Before("mypointcut()")
public void before(JoinPoint joinPoint){
    //Signature 方法的描述
    //This 代理对象
    //Target 委托类对象
    //Arguments 参数
    Signature signature = joinPoint.getSignature();
    Object proxy = joinPoint.getThis();
    Object target = joinPoint.getTarget();
    Object[] args = joinPoint.getArgs();

    System.out.println("signature:" + signature.getName());
    System.out.println(proxy.getClass().getName());
    System.out.println(target.getClass().getName());
    System.out.println(Arrays.asList(args));
}
```

## AOP+自定义注解+分布式锁   [商品详情优化](D:\Java\java50th\java50-course-materials\04-微服务\01-课件\13_商品详情页2\商品详情优化.md)

通过自定义注解+aop将与业务逻辑无关的功能模块化，提高代码的可维护性和可重用性。

整个流程的逻辑：在需要被增强的方法（**比如getSkuInfo()方法等**）上面添加自定义注解如@RedisCache， 然后再单独写一个切面，通知方法使用环绕通知 @Around("**@annotation**(com.cskaoyan.mall.common.cache.**RedisCache**)")，其中@annotation匹配自定义注解@RedisCache；那么在所有有@RedisCache注解的方法就会执行增强方法：

**1. 先访问Redis，有数据直接放回，没有数据则加锁；**  **（around通知）**

**2. 然后执行自己本身的方法，从数据库中拿数据；**  **（自身方法）**

**3.因为是around通知， 最后再执行增强逻辑， 将拿到的数据，放入Redis中，并释放锁。**  **（around通知）**

=================================================================================================================================================================================================================================================================================================================

我们在前面说过，**商品详情页时访问量比较大的，所以我们需要给整个商品详情页的数据增加缓存(除了商品价格这种实时数据)。因此我们不仅仅需要对获取SKU基本信息的getSkuInfo方法做改造，还需要对其他获取商品详情页数据的方法做改造（其他方法看下文代码）。**

但是，无论对于哪个方法做改造，改造的逻辑几乎都是一样的，即我们需要给多个方法做增强，增加一段通用处理逻辑，此时你会 想到什么呢？当然是AOP

既然要是用AOP，自然需要定义切面，但在定义切面之前，我们需要思考以下两个问题：

- 我们的切入点应该如何定义
- 我们应该使用什么样的通知类型

针对第一个问题，我们可以结合自定义注解，给**需要增强的方法加自定义注解**，所以我们的切入点使用@annotation注解找具有目标自定义注解的方法即可。

针对第二个问题，得根据我们的增强逻辑来决定，被增强的方法是访问数据库的，而我们的增强逻辑需要在访问数据库之前先访问缓存，如果缓存中没有，在被增强方法访问数据库之后，我们还需要将数据库中的查询结果，放入Redis，所以很显然，我们应该使用环绕通知。

![](D:\Java\java50th\java50-course-materials\04-微服务\01-课件\13_商品详情页2\商品详情优化.assets\商品详情页优化-通知.png)

![](D:\Java\java50th\java50-course-materials\04-微服务\01-课件\13_商品详情页2\商品详情优化.assets\aop cache.png)

所以我们可以定义自定义注解如下：

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface RedisCache {

    // 给缓存数据增加前缀，以区分不同的缓存数据
    String prefix() default "cache:";

}
```

定义切面如下：

```java
@Component
@Aspect
public class RedisCacheAspect {

    @Autowired
    private RedissonClient redissonClient;

    //  定义一个环绕通知！
    @Around("@annotation(com.cskaoyan.mall.common.cache.RedisCache)")
    public Object gmallCacheAspectMethod(ProceedingJoinPoint point) {
        //  定义一个对象
        Object obj = null;
        MethodSignature methodSignature = (MethodSignature) point.getSignature();
        RedisCache redisCache = methodSignature.getMethod().getAnnotation(RedisCache.class);
        //   获取到注解上的前缀
        String prefix = redisCache.prefix();
        //  组成缓存的key！ 获取方法传递的参数
        String key = prefix + Arrays.asList(point.getArgs()).toString();
        RLock lock = null;
        try {
            //  可以通过这个key 获取缓存的数据
            obj = this.redissonClient.getBucket(key).get();
            if (obj != null) {
                // 获取到了直接返回
                return obj;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            //  加锁
            lock = redissonClient.getLock(key + ":lock");
            lock.lock(RedisConst.SKULOCK_EXPIRE_PX2, TimeUnit.SECONDS);
            Object redisData = this.redissonClient.getBucket(key).get();
            // double check
            if (obj != null) {
                // 获取到了直接返回
                return obj;
            }

            //  执行业务逻辑：直接从数据库获取数据
            obj = point.proceed(point.getArgs());

              // 将结果放入redis
            obj = putInRedis(obj, key, methodSignature);

            return obj;

        } finally {
            //  解锁
            if (lock != null) {
                lock.unlock();
            }
        }
    }

   /*
         将数据放入Redis缓存
     */
    private Object putInRedis(Object obj, String key, MethodSignature methodSignature) {
        try {
            if (obj == null) {
                //  防止缓存穿透
                //obj = new Object();
               Class returnType = methodSignature.getReturnType();
                if (returnType.isAssignableFrom(List.class)) {
                    // 返回值是Collection或List类型
                    obj = new ArrayList();
                } else if (Map.class.equals(returnType)) {
                    // 返回值是Map类型
                    obj = new HashMap();
                } else {
                    // 其他类型
                    Constructor declaredConstructor = returnType.getDeclaredConstructor();
                    declaredConstructor.setAccessible(true);
                    obj = declaredConstructor.newInstance();
                }
                //  将缓存的数据变为 Json 的 字符串,默认值的过期时间是1分钟
                this.redissonClient.getBucket(key).set(obj, RedisConst.SKUKEY_TEMPORARY_TIMEOUT, TimeUnit.SECONDS);
            } else {
                //  将缓存的数据变为 Json 的 字符串
                this.redissonClient.getBucket(key).set(obj, RedisConst.SKUKEY_TIMEOUT, TimeUnit.SECONDS);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

        return obj;
    }

}

```

定义好切面后，我们可以在获取详情页数据的时候使用了

```java
    @RedisCache(prefix = RedisConst.SKUKEY_PREFIX)
    @Override
    public SkuInfoDTO getSkuInfo(Long skuId) {
        ...
    }

    @RedisCache(prefix = "spuSaleAttrListCheckBySku:")
    @Override
    public List<SpuSaleAttributeInfoDTO> getSpuSaleAttrListCheckBySku(Long skuId, Long spuId) {
	   ...
    }

    @Override
    @RedisCache(prefix = "skuValueIdsMap:")
    public Map<String, Long> getSkuValueIdsMap(Long spuId) {
        ...
    }

    @Override
    @RedisCache(prefix = "categoryHierarchyByCategory3Id:")
    public CategoryHierarchyDTO getCategoryViewByCategoryId(Long category3Id) {
	   ...
    }
    
    @Override
    @RedisCache(prefix = "SpuPosterList:")
    public List<SpuPosterDTO> findSpuPosterBySpuId(Long spuId) {
	   ...
    }

    @RedisCache(prefix = "platformAttributeInfoList:")
    @Override
    public List<PlatformAttributeInfoDTO> getPlatformAttrInfoBySku(Long skuId) {
		...
    }
```

## 布隆过滤器    解决缓存穿透问题

**在商品上架的时候（onSale（）方法），向布隆过滤器中添加元素；然后在获取商品详情时判断，该元素是否在布隆过滤器中。**

方法二：空对象缓存（若使用空对象缓存会暂用一定的空间）

如果我们想要判断，一个skuId是否存在于数据库，我们只需要在构造布隆过滤器时，将目标集合变为数据库中所有的SKU商品的id集合即可。

**Redisson本身对实现了基于Redis的布隆过滤器，可以非常方便的使用**

```java
       //  根据指定名称(key)获取布隆过滤器
	   RBloomFilter rbloomFilter = redissonClient.getBloomFilter("xxx");
        // 初始化布隆过滤器，预计统计元素数量为100000，期望误判率为0.01
       rbloomFilter.tryInit(100000, 0.01);
        
         
       // 向布隆过滤器中添加目标元素
       rbloomFilter.add(目标元素);

       // 判断目标元素是否存在，返回false表示不存在
       boolen exists = rbloomFilter.contains(目标元素);
```

在我们的商品服务中，**我们需要在服务启动的时候就执行对于布隆过滤器的初始化(即仅仅需要执行一个操作)**，所以我们可以将布隆过滤器的初始化，可以使用**CommandLineRunner**接口

```java
public interface CommandLineRunner {

	/**
	   该方法会被SpringBoot在初始化完Spring容器之后自动调用
	 * Callback used to run the bean.
	 * @param args incoming main method arguments
	 * @throws Exception on error
	 */
	void run(String... args) throws Exception;

}
```

所以我们可以这样使用

```java
/*
    这个BloomFilterRunner因为加了Component注解，所以会被放到Spring容器中，
    Spring容器初始化完毕后，该对象的run方法会被自动调用
*/
@Component
public class BloomFilterRunner implements CommandLineRunner {
    
    @Autowired
    RedissonClient redissonClient;
    
    @Override
    public void run(String... args) throws Exception {
        RBloomFilter<Long> rbloomFilter = redissonClient.getBloomFilter(RedisConst.SKU_BLOOM_FILTER);
        // 初始化布隆过滤器，预计统计元素数量为100000，期望误差率为0.01
        rbloomFilter.tryInit(100000, 0.01);
    }
}
```

然后我们在上架SKU商品的时候，向布隆过滤器中添加元素

```java
    @Override
    public void onSale(Long skuId) {
		
        /* 
           ...
        */
        
        //向添加布隆过滤器添加元素
        RBloomFilter<Long> rbloomFilter = redissonClient.getBloomFilter(RedisConst.SKU_BLOOM_FILTER);
        rbloomFilter.add(skuId);

    }
```

在处理获取详情页商品信息的请求时，首先判断布隆过滤器中是否存在该元素，如果不存在，则直接返回默认值

```java
@Override
    public ProductDetailDTO getItemBySkuId(Long skuId) {

        ProductDetailDTO productDetailDTO = new ProductDetailDTO();
        RBloomFilter<Long> bloomFilter = redissonClient.getBloomFilter(RedisConst.SKU_BLOOM_FILTER);
        if (!bloomFilter.contains(skuId)) {
            // 如果不存在，则返回默认值
            return productDetailDTO;
        }

		/*
		  ....
		*/
        return productDetailDTO;
    }
```

## 倒排索引和正排索引

**倒排索引：“关键词” →“文档Id”，即关键词到文档id的映射。**



正排索引和倒排索引在不同的场景下具有不同的优势，无法简单地说哪个更出色，而取决于具体的需求和使用场景。

**正排索引（Forward Index）的优势：**

- 正排索引适用于范围查询和有序查询，因为数据按照索引顺序进行排序。
- 正排索引在查询时可以直接定位到对应的数据行，因为叶子节点包含了数据。
- 正排索引对于频繁的等值查询非常高效。

**倒排索引（Inverted Index）的优势：**

- 倒排索引适用于关键词搜索和全文搜索，因为它可以快速找到包含特定关键词的文档。
- 倒排索引可以支持高效的词频统计和相关性排序。
- 倒排索引适用于大规模文本检索和搜索引擎等应用。

在实际应用中，可以根据具体的需求和场景选择适合的索引类型。有些情况下可能需要同时使用正排索引和倒排索引，以充分利用它们的优势，提高查询效率和准确性。



## OpenFeign

### 基本使用

**openfeign只在服务的消费端使用，对于服务的提供端来说是无感的，只需要关注自身的逻辑业务即可。**

**只需要在需要在服务消费端写一个xxxClient接口，添上@FeignClient注解，接口里面是想要使用的方法（要一模一样），然后再在启动类上添加@EnableFeignClients注解即可**



在使用@FeignClient注解时，value属性用于指定要调用的目标服务的名称。**该名称通常是服务注册中心中注册的服务名**。

```java
// 注意，这里FeignClient的名字是调用的服务的名称 / 服务中心注册的服务名
@FeignClient("feign-provider-8005")
public interface DemoServiceClient{
    @GetMapping("/feign/hello")
    String sayHello(@RequestParam(name = "name")String name);
}
```

在启动类上加注解@EnableFeignClients，才能让我们定义的FeignClient生效

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableFeignClients
public class FeignConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(FeignConsumerApplication.class, args);
    }
}
```

### FeignInterceptor拦截器   通过拦截器在服务与服务之间传递用户信息



1. 网关路由分发到web-all时有用户的信息（网关通过cookie中的token去获取）；
2. web-all远程调用order服务时没有用户的信息（因为这是另一个请求） ；
3. 通过FeignInterceptor（会在远程调用发起请求前生效，也就是web-all在远程调用前生效），从web-all阶段获取用户的信息，然后把信息添加到请求头中；这样web-all调用order服务的请求头里有就有用户的信息了。



## Elastic Search       [Elastic Search2](D:\Java\java50th\java50-course-materials\04-微服务\01-课件\15_elasticsearch-2\Elastic Search2.md)  ←Java api 

### match all

match all 相当于不加查询条件搜索所有的文档 

```json
GET product/_search
{
  "query": {
    "match_all": {}
  },
  "from": 0,
  "size": 100
}
```

### term查询

erm查询和字段类型有关系，首先回顾一下ElasticSearch两个数据类型

 ElasticSearch两个数据类型：

- text：会分词，不支持聚合
- keyword：不会分词，将全部内容作为一个词条，支持聚合

**term查询：不会对查询条件进行分词。但是注意，term查询，查询text类型字段时，文档中类型为text类型的字段本身仍然会分词**

```json
GET product/_search
{
  "query": {
    "term": {
      "title": {
        "value": "手机充电器"
      }
    }
  }
}
```

### match查询

match查询的特征：

•会对查询条件进行分词。

•然后将分词后的查询条件和目标字段分词后的词条进行等值匹配

•默认取并集（OR），即只要查询条件中的一个分词和目标字段值的一个分词(词条)匹配，即认为匹配查询条件

```json
# match查询
GET product/_search
{
  "query": {
    "match": {
      "title": "手机充电器"
    }
  },
  "size": 500
}
```

**match 的默认搜索（or 并集）**例如：华为手机，会分词为 “华为”，“手机” 只要出现其中一个词条都会认为词条匹配

**match的 and（交集） 搜索**，例如：例如：华为手机，会分词为 “华为”，“手机”  但要求“华为”，和“手机”同时出现在词条中，才算词条匹配

```json
GET product/_search
{
  "query": {
    "match": {
      "title": {
        "query": "华为手机",
        "operator": "and"    #如果是or，那么"小米"或 "手机"任何一个匹配，都会匹配成功 
      }
    }
  },
  "size": 500
}
```

###  模糊查询

####  wildcard查询

wildcard查询: wildcard查询：会对查询条件进行分词。还可以使用通配符 ?（任意单个字符） 和  * （0个或多个字符）

```json
# wildcard 查询。查询条件分词，模糊查询
GET product/_search
{
  "query": {
    "wildcard": {
      "title": {
        "value": "手机*"
      }
    }
  }
}
```

#### 正则查询

```
[A-Z a-z 0-9_] 表示一个大小写英文字符，或者0-9的数字字符，或者下划线字符_

+号多次出现

(.)*为任意字符
正则查询取决于正则表达式的效率
```

```json
GET product/_search
{
  "query": {
    "regexp": {
      "title": "[A-Z a-z 0-9_]+(.)*"
    }
  }
}
```

#### 前缀查询

 对keyword类型支持比较好

```json
# 前缀查询 对keyword类型支持比较好
GET product/_search
{
  "query": {
    "prefix": {
      "brandName": {
        "value": "三"
      }
    }
  }
}
```

####  模糊查询Java API

```java
//模糊查询
WildcardQueryBuilder wildQuery = QueryBuilders.wildcardQuery("title", "充电*");//充电后多个字符
//正则查询
 RegexpQueryBuilder regexQuery = QueryBuilders.regexpQuery("title", "[A-Z a-z 0-9_]+(.)*");
 //前缀查询
 PrefixQueryBuilder prefixQuery = QueryBuilders.prefixQuery("title", "充电");
```

### querystring

queryString 多条件查询

1. 会对查询条件进行分词。
2. 然后将分词后的查询条件和词条进行等值匹配
3. 默认取并集（OR）
4. 可以指定多个查询字段

**query_string：可以识别query中的连接符（or 、and）**

```json
# queryString

GET product/_search
{
  "query": {
    "query_string": {
      "fields": ["title","sell_point"], 
      "query": "耳机 AND 充电器"
    }
  }
}
```

java代码

```java
QueryStringQueryBuilder query = QueryBuilders.queryStringQuery("耳机充电器").field("title").field("sellPoint");
```

**simple_query_string：不识别query中的连接符（or 、and）**，查询时会将 “耳机”、"and"、“充电器”分别进行查询

```json
GET product/_search
{
  "query": {
    "simple_query_string": {
      "fields": ["title","sell_point"], 
      "query": "耳机 AND 充电器"
    }
  }
}
```

java代码

```java
QueryStringQueryBuilder query = QueryBuilders.queryStringQuery("耳机充电器").field("title").field("sellPoint")
```



### 范围& 排序查询

```json
GET product/_search
{
  "query": {
    "range": {
      "price": {
        "gte": 100,
        "lte": 1000
      }
    }
  },
  "sort": [
    {
      "price": {
        "order": "desc"
      }
    }
  ]
}	
```

```java
 //范围查询 以price 价格为条件
RangeQueryBuilder query = QueryBuilders.rangeQuery("price");

//指定下限
query.gte(100);
//指定上限
query.lte(1000);

sourceBuilder.query(query);

//排序  价格 降序排列
sourceBuilder.sort("price",SortOrder.DESC);
```

### 复合查询 bool

 boolQuery：对多个查询条件连接。其组成主要分为如下四个部分：

1. must（and）：条件必须成立
2. must_not（not）：条件必须不成立
3. should（or）：条件可以成立
4. filter：条件必须成立，性能比must高。不会计算得分

```json
# must
GET product/_search
{
  "query": {
    "bool": {
      "must": [
        {
           "term": {
             "title": {
               "value": "充电器"
             }
           }
        },
        {
          "match": {
            "sellPoint": "快充"
          }
        }
      ]
    }
  }
}

```



```json
# must_not
GET product/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
           "match": {
             "title": "充电器"
           }
        }
      ]
    }
  }
}
```



```json
# should 中的多个条件是or关系
GET product/_search
{
  "query": {
    "bool": {
      "should": [
          {
           "term": {
             "title": {
               "value": "充电器"
             }
           }
        },
        {
          "term": {
             "sellPoint": {
               "value": "小菜鸡"
             }
           }
        }
      ]
    }
  }
}
```



```json
# filter
GET product/_search
{
  "query": {
    "bool": {
      "filter": [
          {
           "term": {
             "title": {
               "value": "充电器"
             }
           }
        },
        {
          "match": {
            "sellPoint": "快充"
          }
        }
      ]
    }
  }
}
```

这里有几点需要注意：

- 一个复合查询中，可以同时包含must，must not，should，filter中的一个或多个部分
- 每一部分，都可以包含多个查询条件(只有should中的多个查询条件是or关系)
- 当存在must，或者filter的时候，should中的条件默认不生效
- must和filter都可以表示同时满足多个条件的查询，但是不同的地方在于must会计算文档的近似度得分，filter不会(must_not也不会)

```json
# boolquery 包含多个部分
GET product/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "title": {
              "value": "充电器"
            }
          }
        }
      ],
      "filter":[ 
        {
        "term": {
          "title": "原装"
        }
       },
       {
         "range":{
          "price": {
            "gte": 40,
            "lte": 100
         }
         }
       }
      
      ]
    }
  }
}

```

JAVA API:

布尔查询：boolQuery 

1. 查询商品为(title): 充电器 
2. 查询过滤条件：原装
3. 查询价格在：40-100 

```java
        //1.构建boolQuery
        BoolQueryBuilder boolQuery = QueryBuilders.boolQuery();
        //2.构建各个查询条件
        //2.1 查询品牌名称为:华为
        TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", "耳机");
        boolQuery.must(termQueryBuilder);
        //2.2. 查询标题包含：手机
        MatchQueryBuilder matchQuery = QueryBuilders.matchQuery("title", "原装");
        boolQuery.filter(matchQuery);

        //2.3 查询价格在：40-100
        RangeQueryBuilder rangeQuery = QueryBuilders.rangeQuery("price");
        rangeQuery.gte(40);
        rangeQuery.lte(100);
        boolQuery.filter(rangeQuery);

        sourceBuilder.query(boolQuery);
```

### 聚合查询

聚合查询分为两种类型:

- 指标聚合：相当于MySQL的聚合函数。max、min、avg、sum等
- 桶聚合：相当于MySQL的 group by 操作。不要对text类型的数据进行分组，会失败。

```json
# 聚合查询

# 指标聚合 聚合函数

GET product/_search
{
  "query": {
    "match": {
      "title": "耳机"
    }
  },
  "aggs": {
    "max_price": {
      "max": {
        "field": "price"
      }
    }
  }
}

# 桶聚合  分组
GET product/_search
{
  "query": {
    "match": {
      "title": "充电器"
    }
  },
  "aggs": {
    "price_bucket": {
      "terms": {
        "field": "price",
        "size": 100
      }
    }
  }
}
```

### 高亮查询

高亮三要素：

1. 高亮字段
2. 前缀
3. 后缀

默认前后缀 ：em

```html
<em>手机</em>
```

```json
GET product/_search
{
  "query": {
    "match": {
      "title": "充电器"
    }
  },
  "highlight": {
    "fields": {
      "title": {
        "pre_tags": "<font color='red'>",
        "post_tags": "</font>"
      }
    }
  }
}
```

## Elastic Search       Java中的使用

### 总结 （共四步如下）

**1. 先引入依赖spring-boot-starter-data-elasticsearch ， 然后在配置文件中添加配置；2. 然后定义一个实体类去映射ES索引中的文档；3 . 然后定义一个接口去继承ElasticsearchRepository，注入其对象后就可以快速进行crud（有内置的方法更简便）、分页、排序等简单操作 （上架、下架、更新热点）；4.引入ElasticsearchRestTemplate用来进行更复杂的自定义查询（crud要自己构造更复杂），如构造自定义分页，高亮，nested以及聚合查询等 **



### 基础配置 

要使用Elasticsearch我们需要引入如下依赖： 

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
            <version>2.1.7.RELEASE</version>
        </dependency>
```

还需要在配置文件中增加如下配置

```yml
spring:
  elasticsearch:
    rest:
      # es server的地址
      uris: 192.168.0.102:9200
      # 连接超时时间
      connection-timeout: 6s
      # 访问超时时间
      read-timeout: 10s
```

### 定义实体类映射ES文档

类比于MyBatis-Plus可以定义实体类去映射数据库中的表中的数据，使用Spring Data Elasticsearch时，我们也可以通过定义一个实体类映射ES索引中的文档。

**注意@Id注解**  **这里的类型是后面继承ElasticsearchRepository接口时需要的第二个泛型的类型；也就是这个Long类型**

```java
@Data
@Document(indexName = "goods" , shards = 1,replicas = 0)  //一个分片，0个副本
public class Goods {
    // 商品Id skuId _id
    @Id
    private Long id;

    @Field(type = FieldType.Keyword, index = false)
    private String defaultImg;

    //  es 中能分词的字段，这个字段数据类型必须是 text！keyword 不分词！
    @Field(type = FieldType.Text, analyzer = "ik_max_word")
    private String title;
    
         .
         .
         .
             

    @Field(type = FieldType.Keyword)
    private String thirdLevelCategoryName;

    //  商品的热度！ 我们将商品被用户点查看的次数越多，则说明热度就越高！
    @Field(type = FieldType.Long)
    private Long hotScore = 0L;

    // 平台属性集合对象
    // Nested 支持嵌套查询
    @Field(type = FieldType.Nested)
    private List<SearchAttr> attrs;

}
```

```java
/*
   该类映射nested平台属性
*/
@Data
public class SearchAttr {
    // 平台属性Id
    @Field(type = FieldType.Long)
    private Long attrId;
    // 平台属性值名称
    @Field(type = FieldType.Keyword)
    private String attrValue;
    // 平台属性名
    @Field(type = FieldType.Keyword)
    private String attrName;
}
```

**注意：**在Goods类上，通过添加@Document注解，我们将Goods类映射的文档所属的索引：

- @Document注解的index属性，用来定义实体类所映射的文档所属的目标索引名称
- @Document的shards属性，表示目标索引的住分片数量
- @Document的replicas属性，表示每个主分片所拥有的副本分片的数量

**在Goods类的成员变量Id上通过添加@Id注解指定，Id成员变量映射到Goods索引中文档的id字段，同时也映射到文档的唯一表示_id字段。**

在Goods类的其他成员变量上，通过添加@Field注解，定义成员变量和文档字段的映射关系：

- 默认同名成员变量，映射到文档中的同名字段(也可以由@Field注解的name属性显示指定)
- 通过@Field注解的type属性指定文档中同名字段的数据类型
- 通过@Field注解的analyzer属性，指定成员变量所映射的文档字段所使用的的分词器



### xxxRepository继承ElasticsearchRepository

**定义一个接口去继承ElasticsearchRepository（其他的不用写），注入其对象后就可以快速进行crud（有内置的方法更简便）、分页、排序等简单操作 （上架、下架、更新热点）**

类比于Mybatis-Plus中定义BaseMaper子接口即可对单表做增删改查的操作，Spring Data Elastisearch中我们可以通过定义ElasticsearchRepository子接口，迅速实现对索引中的文档数据的增删改查，以及通过自定义方法，实现自定义查询。

```java
public interface GoodsRepository extends ElasticsearchRepository<Goods,Long> {

}
```

**ElasticsearchRepository**接口需要接收**两个泛型，第一个泛型即映射实体类，第二个泛型是在实体类中加了@Id注解的成员变量的数据类型，即映射到文档唯一标识_id字段的成员变量类型。**

一旦我们定义好了ElasticsearchRepository的子接口，马上就可以实现对goods索引中文档的增删改查功能

```java
    // 注入repository对象
    @Autowired
    private GoodsRepository goodsRepository;

    
    // 保存单个文档对象
    Goods good = ....
    goodsRepository.save(good);



    // 批量保存多个文档对象
    List<Goods> goods = ...
    goodsRepository.save(goods);

   // 根据id查询
   goodsRepository.findById(id);

   // 根据id删除
   goodsRepository.deleteById(id);
    
```

同时，我们还需要注意一点，一旦我们定义好了ElasticsearchRepository接口，而且被SpringBoot启动类扫描到，那么在应用**启动**的时候，如果ElasticsearchRepository子接口所访问的索引在ES中不存在，Spring Data Elasticsearch会在ES中**自动创建索引，并根据映射实体类定义索引的映射**。

Repository好用，但是具有一定的局限性，如果面对比较复杂的查询，此时就只能使用Spring Data Elasticsearch提供的另外一个工具ElasticsearchRestTemplate了。



### 引入ElasticsearchRestTemplate

为了构造更复杂的查询，引入ElasticsearchRestTemplate。 **直接@Autowired注入即可使用**。然后构造自定义分页，高亮，nested以及聚合查询，并发起请求

```java

    @Autowired
    ElasticsearchRestTemplate restTemplate;

    @Test
    public void testRestTemplate() {
        // 该Builder包含所有搜索请求的参数
        NativeSearchQueryBuilder queryBuilder = new NativeSearchQueryBuilder();


        // 获取bool查询Builder
        BoolQueryBuilder boolQueryBuilder = QueryBuilders.boolQuery();


        // 构造bool查询中match查询
        MatchQueryBuilder matchQuery = QueryBuilders.matchQuery("title", "小米手机");
        // 将该查询加入bool查询must中
        boolQueryBuilder.must(matchQuery);

        TermQueryBuilder subQueryForAttrNested = QueryBuilders.termQuery("attrs.attrValue", "8G");
        // 构造nested查询
        NestedQueryBuilder attrsNestedQuery = QueryBuilders.nestedQuery("attrs", subQueryForAttrNested, ScoreMode.None);
        // 将nested查询作为一个过滤条件
        boolQueryBuilder.filter(attrsNestedQuery);


        // 将整个bool查询添加到NativeSearchQueryBuilder
        queryBuilder.withQuery(boolQueryBuilder);

        // 构造分页参数
        //PageRequest price = PageRequest.of(0, 10, Sort.by(Sort.Order.desc("price")));
        PageRequest price = PageRequest.of(0, 10);
        // 向NativeSearchQueryBuilder添加分页参数
        queryBuilder.withPageable(price);

        // 按照指定字段值排序
        FieldSortBuilder priceSortBuilder = SortBuilders.fieldSort("price").order(SortOrder.ASC);
        queryBuilder.withSort(priceSortBuilder);

        // 构造高亮参数
        HighlightBuilder highlightBuilder = new HighlightBuilder();
        highlightBuilder.field("title").preTags("<font color='red'>").postTags("</font>");
        // 向NativeSearchQueryBuilder添加高亮参数
        queryBuilder.withHighlightBuilder(highlightBuilder);

        // 设置品牌聚合(平台属性等的聚合也是相同的方式)
        TermsAggregationBuilder termsAggregationBuilder = AggregationBuilders.terms("tmIdAgg").field("tmId")
                .subAggregation(AggregationBuilders.terms("tmNameAgg").field("tmName"))
                .subAggregation(AggregationBuilders.terms("tmLogoUrlAgg").field("tmLogUrl"));


        // 向NativeSearchQueryBuilder添加聚合参数
        queryBuilder.addAggregation(termsAggregationBuilder);

        // 结果集过滤，只包含原始文档的id，defaultImg，title，price
        queryBuilder.withFields("id", "defaultImg", "title", "price");

        // 使用ElasticsearchRestTemplate发起搜索请求
        NativeSearchQuery build = queryBuilder.build();
        SearchHits<Goods> search = restTemplate.search(build, Goods.class);

        //封装所有的查询数据
        SearchResponseDTO searchResponseDTO = new SearchResponseDTO();

        // 获取满足条件的总文档数量
        long totalHits = search.getTotalHits();
        // 设置查询到的总文档条数
        searchResponseDTO.setTotal(totalHits);


        // 获取包含所有命中文档的SearchHit对象
        List<SearchHit<Goods>> searchHits = search.getSearchHits();


        // 处理搜索到的结果集即SearchHit<Goods>集合, 并使用高亮字符串替换
        List<GoodsDTO> goodsList = searchHits.stream().map(hit -> {
            // 获取命中的文档
            Goods content = hit.getContent();

            //获取高亮字段
            List<String> title = hit.getHighlightField("title");

            // 用高亮字段替换
            content.setTitle(title.get(0));
            // 将Goods对象转化为GoodsDTO对象
            GoodsDTO goodsDTO = goodsConverter.goodsPO2DTO(content);
            return goodsDTO;
        }).collect(Collectors.toList());

        // 设置查询到的结果列表
        searchResponseDTO.setGoodsList(goodsList);

        // 从品牌聚合中获取品牌集合

        // 根据id获取品牌id terms聚合结果
       Terms terms = search.getAggregations().get("tmIdAgg");
        List<SearchResponseTmDTO> trademarkList = terms.getBuckets().stream().map(tmIdBucket -> {
            // 封装品牌数据
            SearchResponseTmDTO searchResponseTmDTO = new SearchResponseTmDTO();

            String tmIdStr = tmIdBucket.getKeyAsString();
            // 获取品牌id
            Long tmId = Long.parseLong(tmIdStr);
            // 设置品牌id
            searchResponseTmDTO.setTmId(tmId);

            // 获取品牌名称聚合(子聚合)
            Terms tmNameAgg = tmIdBucket.getAggregations().get("tmNameAgg");
            // 通过聚合桶的名称获取品牌名称
            String tmName = tmNameAgg.getBuckets().get(0).getKeyAsString();
            // 设置品牌名称
            searchResponseTmDTO.setTmName(tmName);


            // 获取品牌logo聚合(子聚合)
            Terms tmLogoUrlAgg = tmIdBucket.getAggregations().get("tmLogoUrlAgg");
            // 通过聚合桶的名称获取品牌名称
            String tmLogoUrl = tmLogoUrlAgg.getBuckets().get(0).getKeyAsString();
            // 设置品牌名称
            searchResponseTmDTO.setTmLogoUrl(tmLogoUrl);

            return searchResponseTmDTO;
        }).collect(Collectors.toList());

        // 设置聚合品牌数据
        searchResponseDTO.setTrademarkList(trademarkList);
        
        // .....

    }
```

### ElasticsearchRestTemplate 的search()方法

`ElasticsearchRestTemplate` 是 Spring Data Elasticsearch 提供的一个类，用于与 Elasticsearch 进行交互。

**`search()` 方法是 `ElasticsearchRestTemplate` 类中的一个方法，用于执行 Elasticsearch 的搜索操作。它接受一个 `SearchQuery` 对象作为参数，并返回一个 `SearchHits` 对象，该对象包含了搜索结果中的命中（hits）信息。**

**`SearchQuery` 是一个接口，用于构建 Elasticsearch 的查询条件。你可以使用 `SearchQuery` 的实现类，如 `NativeSearchQuery` 或 `CriteriaQuery`，来构建不同类型的查询。这些查询可以包含各种条件、过滤器、排序等。**

**当调用 `search()` 方法时，`ElasticsearchRestTemplate` 会将 `SearchQuery` 对象转换为 Elasticsearch 的查询语句，并发送给 Elasticsearch 服务器执行搜索操作。然后，它会解析 Elasticsearch 返回的结果，并将结果封装在 `SearchHits` 对象中返回。**

你可以使用 `SearchHits` 对象获取搜索结果的详细信息，如总命中数、每个命中文档的得分、文档的字段值等。

需要注意的是，`ElasticsearchRestTemplate` 是基于 Elasticsearch 的 REST API 进行操作的，因此在使用之前，你需要配置好 Elasticsearch 的连接信息，包括主机、端口等。

这些是关于 `ElasticsearchRestTemplate` 的 `search()` 方法的基本说明，具体的用法和更多细节可以参考 Spring Data Elasticsearch 的文档或者相关的 API 文档。



## 热度排名

逻辑是这样的：**1.** 当你搜索商品，点进商品详情页之后，**商品服务**就会远程**调用 搜索服务**的增加热度方法；**2.** 搜索服务就会先将热度数据保存到Redis中（因为点击量大，减小压力）; **3.** 等到比如有10次了，然后再写入到ES中。**4.** ES在自定义的搜索条件中， 会有根据热度降序排序，这样就完成了热度排名。



## 共享会话解决单点登录的思路    [单点登录](D:\Java\java50th\java50-course-materials\04-微服务\01-课件\17_单点登录\单点登录.md)

### 解决思路

这个思路的核心要点，当用户登录成功之后，将用户的会话信息，存储在一个共享的地方(比如Redis)，而不是某个服务的内存中，这样一来就可以通过访问共享会话来判断用户的登录状态。



**对于用户服务而言:**

- 一旦判断用户登录成功，就将用户信息存储到Redis，这就相当于将用户登录之后的会话，存储到Redis中。
- 同时，随机生成Token，作为Redis中，会话信息的key，通过该Token获取Redis中对应的共享会话信息
- 该Token还需要返回给前端。

**对于前端而言**，只要用户登陆成功，就会接收到Redis中，登录会话对应的Token，前端就会在请求Cookie中携带该Token，发起请求的时候，会在请求中携带该Cookie。

**对于服务而言：**

- 因为所有的请求都是经过网关的，所以登录身份认证的工作，没有必要在每个服务中完成，而是统一在网关中完成即可
- 所以网关会拦截那些需要做登录身份认证的请求，并根据请求中携带是否携带Cookie，以及通过Cookie中的Token是否能在Redis中获取登录会话，来判断用户是否登录过。

### 验证逻辑

```java
// 全局过滤器，对请求进行拦截校验，逻辑是什么呢？
    // 1. 获取当前访问的Path
    // 2. 判断当前这个path是否需要登录
    // 3. 如果需要登录，那么判断用户是否已经登录
    // 4. 如果 当前path需要登录且 未登录，拦截
    // 5. 其他的情况，放行    
          // a) 当前访问的path 不需要登录
          // b) 需要登录且用户已经登录
```



**补充：**Nginx 的 IP Hash 策略可以在一定程度上解决单点登录问题。通过使用 IP Hash 策略，当用户进行认证并获得一个会话后，该会话将被绑定到一个特定的后端服务器，即使用户的后续请求经过负载均衡也会被路由到同一台服务器上。这样可以确保用户的会话状态在整个系统中保持一致，从而实现单点登录的效果。需要注意的是，Nginx 的 IP Hash 策略并不能解决所有与单点登录相关的问题，特别是在分布式系统中，还需要考虑会话的同步、跨域访问等其他方面的问题。因此，在实际应用中，可能需要结合其他技术和策略来完善单点登录的解决方案。



## RocketMQ 

RocketMQ 是一个开源的分布式消息中间件系统，由阿里巴巴集团开发和维护。它具有高吞吐量、低延迟、高可靠性和强大的扩展性，广泛应用于大规模分布式系统中的消息通信场景。

RocketMQ 的核心概念包括：

1. Producer（生产者）：负责发送消息到 RocketMQ 的消息队列中。

2. Consumer（消费者）：从 RocketMQ 的消息队列中接收和处理消息。

3. Topic（主题）：消息的逻辑分类，类似于消息的主题或者标签。

4. Message Queue（消息队列）：每个主题可以被划分为多个消息队列，用于存储消息。

5. Broker（消息代理）：负责存储消息，接收和传递消息的中间件组件。

RocketMQ 提供了丰富的特性，包括：

- 可靠的消息传递：RocketMQ 提供了多种消息传递模式，包括同步、异步和单向传递，可以根据需求选择不同的传递方式。

- 高吞吐量和低延迟：RocketMQ 的设计目标之一是实现高吞吐量和低延迟的消息传递，适用于高并发场景。

- 消息顺序性：RocketMQ 支持按照消息的顺序进行传递和处理，保证消息的有序性。

- 消息事务：RocketMQ 支持消息事务，允许在消息发送和消息确认之间执行本地事务。

- 消息过滤：RocketMQ 提供了灵活的消息过滤机制，可以根据消息的属性进行过滤，只选择感兴趣的消息进行处理。

RocketMQ 可以用于构建各种分布式系统中的消息通信模块，例如订单系统、支付系统、日志系统等。它有着广泛的社区支持和活跃的开发维护，可以满足大规模分布式系统中的消息传递需求。

## RocketMQ的代码中的实际用法



1. 在**common服务**里有个一个BaseProducer里面有init（）方法，sendMessage（）方法，sendDelayMessage（）方法。

2. 在**商品服务**的SkuServiceImpl中**onSale（）**方法里，会调用一个common服务的BaseProducer来生产一个**上架消息**(注入进来，调用相应的方法即可)，**offSale（）**方法会发送一个**下架消息**。

3. 在**搜索服务**里会单独写两个消费者类，创建消息消费者，然后设置相同的注册中心，订阅相同的主题，然后再设置好消息监听器，监听到上架的消息后，便会调用searchService中的upperGoods（）方法，用消息中的skuId，将相应的商品储存es中。

4. 延迟消息是用在订单服务里， 超时30分钟自动取消订单



## yaml配置文件的解读示例

```yaml
spring:
  application:
    name: server-gateway
  profiles:
    active: dev
  cloud:
    nacos:
      discovery:
        server-addr: 120.46.174.115:8848
      config:
        server-addr: 120.46.174.115:8848
        file-extension: yaml
        shared-configs:
          - data-id: common.yaml
        timeout: 10000
# 网关会使用 配置中心里面哪些配置文件？
# common.yaml
# server-gateway-dev.yaml
```

**解读：**

这段 YAML 配置文件用于配置 Spring Boot 应用程序的属性和 Nacos 服务注册与配置中心。

```yaml
spring:
  application:
    name: service-product
```

- `spring.application.name`：指定 Spring Boot 应用程序的名称为 "service-product"。这个名称将用于服务注册和其他相关配置。

```yaml
profiles:
  active: dev
```

- `profiles.active`：指定当前活动的配置文件为 "dev"。这表示应用程序将使用 "dev" 配置文件中的属性和配置。
- **dev**表示开发环境，**test**表示测试环境，**prod**表示生产环境

```yaml
cloud:
  nacos:
    discovery:
      server-addr: 120.46.174.115:8848
    config:
      server-addr: 120.46.174.115:8848
      file-extension: yaml
      shared-configs:
        - data-id: common.yaml
```

- `cloud.nacos.discovery.server-addr`：指定 Nacos 服务注册中心的地址为 "120.46.174.115:8848"。这将告诉应用程序在此地址上注册和发现服务。
- `cloud.nacos.config.server-addr`：指定 Nacos 配置中心的地址为 "120.46.174.115:8848"。这将告诉应用程序从此地址获取配置属性。
- `cloud.nacos.config.file-extension`：指定配置文件的扩展名为 "yaml"。这表示应用程序将从 Nacos 配置中心读取以 YAML 格式存储的配置文件。
- `cloud.nacos.config.shared-configs`：指定共享的配置文件列表。在这个例子中，只有一个共享配置文件 "common.yaml"。

总结起来，这个配置文件指定了应用程序的名称为 "service-product"，活动配置文件为 "dev"，并设置了 Nacos 的服务注册和配置中心地址。它还指定了应用程序将从 Nacos 配置中心读取以 YAML 格式存储的共享配置文件 "common.yaml"。



## 商品服务将公共服务当作依赖进入了进来，那么公共服务里面的@value注解，就会从商品服务的配置文件中读取想要的值对吗？公共服务里没有配置文件

是的，如果商品服务将公共服务作为依赖注入，并且公共服务中使用了 `@Value` 注解来注入配置文件中的值，那么公共服务将会从商品服务的配置文件中读取相应的属性值。

在 Spring 中，属性值的注入是基于容器的，而不是基于类。当商品服务将公共服务作为依赖注入时，公共服务也会成为 Spring 容器中的一个组件。因此，当公共服务使用 `@Value` 注解时，它将查找 Spring 容器中的配置文件，并读取相应的属性值。

这意味着，公共服务可以通过 `@Value` 注解来访问商品服务的配置文件中的属性值，而不需要自己拥有独立的配置文件。这种方式可以方便地将公共配置与特定服务的配置分离，使得公共服务更加灵活和可复用。

需要确保商品服务的配置文件中包含了公共服务所需的属性值。如果商品服务的配置文件中没有相关的属性定义，那么公共服务将无法正确注入属性值。



## redis相关问题

### redis使用场景

- 缓存：穿透、击穿、雪崩 ； 双写一致性、持久化 ； 数据过期、淘汰策略

- 分布式锁： setnx、redisson

- 保存token：string类型

- 消息队列：List类型

- 延迟类型：zset类型

- 计数器



### （购物车问题）如果我当前的数据触发了redis的持久化策略写入到了磁盘上；然后我内存里的该数据被删除了，等到下次我再查询该数据的时候redis会怎么操作

如果你在 Redis 中设置了持久化策略，并且数据已经被成功写入到磁盘上，当你在内存中删除了该数据后，当你再次查询该数据时，Redis 会按照以下步骤进行操作：

1. 当你查询已被删除的数据时，Redis 会发现该键在内存中不存在。

2. Redis 会检查持久化文件（RDB 文件）中是否存在该键的数据。由于你之前已经触发了持久化策略并将数据写入磁盘，所以 Redis 可能会在持久化文件中找到该键的数据。

3. 如果 Redis 在持久化文件中找到了该键的数据，它会将数据从磁盘加载到内存中，并返回给你查询结果。

需要注意的是，如果你在查询已被删除的数据之前进行了 Redis 服务器重启，Redis 会重新加载持久化文件并将数据恢复到内存中。在这种情况下，即使你已经在内存中删除了该数据，Redis 仍然可以从持久化文件中恢复数据并返回给你查询结果。

因此，Redis 的持久化机制可以确保即使在数据被删除后，仍然可以从磁盘中恢复数据，并且保证数据的持久性和可靠性。

### redis队列（用List数据结构），解决商品超卖问题

****

1. 将元素添加到队列中，可以使用 Redis 的 **LPUSH** 或 **RPUSH** 命令。LPUSH 将元素添加到队列的头部，RPUSH 将元素添加到队列的尾部。
2. 从队列中获取元素，可以使用 Redis 的 **LPOP** 或 RPOP 命令。LPOP 从队列的头部移除并返回元素，RPOP 从队列的尾部移除并返回元素。



使用 Redis 实现队列时，需要**注意**以下几点：

- 队列的元素可以是任意类型的数据，如字符串、数字、对象等。
- 队列是持久化的，即使 Redis 重启，队列中的数据也不会丢失。
- Redis 的 List 数据结构是线程安全的（**原子性**），多个客户端可以同时对队列进行操作。





## 购物车流程



使用redis的**hash数据结构**（hset key field value）来存储用户的购物车,  **key存用户的id； field存商品id； value存CartInfoDTO**



**1. 添加购物车**操作，直接用redissonClient.getMap()方法，就可以得到这个用户的整个购物车。判断是否已经有传入的商品id了， 如果有就更新数量，没有的话就添加商品到购物车。

**2. 查询购物车**的话有两点需要注意：

- 用户先用临时id添加了购物车，然后登陆正式id，那么需要将临时的购物车和正式的购物车合并起来，然后删掉临时的购物车

- 购物车列表排序要按照**添加时间**进行**排序**







### 购物车扣库存问题

购物车不扣，生成订单时也不扣，完成付款时才扣库存。





## 整个流程复盘一下 （待完成 ...  先把问题都解决）





## Ribbon

nacos 已经整合了ribbon，配置文件中设置相关策略

待补充 ...  看收藏 微服务篇	











## nginx  （反向代理、负载均衡、动静分离）

正向代理代理客户端 ； **反向代理代理代理服务端**   

并发性好 ，官方说可以有50000个请求， tomcat 也就4、500个请求。



**集群及负载均衡配置**

```
#负载均衡策略 
# 1 轮询（默认）
# 2 weight
# 3 ip_hash
# 4 least_conn 最少连接方式
# 5 fair(第三方) 响应时间
# 6 url_hash (第三方) 
```

**反向代理的配置**    **(重要)**

```bash
http{
	...
	...
	#负载均衡策略   此处是权重
	upstream mysvr{
		#服务器资源
		server 192.168.45.151:8080 weight=2;
		server 192.168.45.151:8081 weight=1;
	}
	
  #这部分是nginx作为反向代理服务器的配置
  server{
  	  #nginx监听的端口
      listen  80;
      #虚拟服务器的识别标记，一般配置为本机ip
      server_name 192.168.45.151;
	  #代理设置地址
      location / {
          proxy_pass http://mysvr; 
      }
      
       # 定义静态资源的访问路径和目录  /path/to/static/files 是静态资源文件的存储路径，需要将其替换为实际的路径。
       location /static {
          alias /path/to/static/files;
      }
    
    
  }
  
  #可以配置多个server 比如再配置一个监听443端口
  server{
   ...
  }
  
}
```



## 高并发的一些解决方案以及秒杀相关

### redis缓存

**缓存预热**，商品秒杀时，提前将相关的数据放入缓存中。

缓存预热方案落地：

- 什么时候存入Redis？凌晨放入当天所有的秒杀商品数据
- 如何实现？**Spring Scheduling 定时任务**

### 消息队列，限流削峰

在高并发场景下把请求存入消息队列，利用排队思想降低系统瞬间峰值

具体场景：购物网站开展秒杀活动，一般由于瞬时访问量过大，服务器接收过大，会导致流量暴增，相关系统无法处理请求甚至崩溃。而加入消息队列后，系统可以从消息队列中取数据，相当于消息队列做了一次缓冲。

![img](D:\Java\java50th\java50-course-materials\04-微服务\01-课件\19. RocketMQ\RocketMQ课件.assets\1506330074201_6290_1506330076616.png)

**优点**： 

1. **请求先入消息队列**，而不是由业务处理系统直接处理，做了一次缓冲,极大地减少了业务处理系统的压力；
2. 事实上，秒杀时，后入队列的用户无法秒杀到商品，这些**请求可以直接被抛弃**，返回活动已结束或商品已售完信息；

## （待补充）MybatisPlus





