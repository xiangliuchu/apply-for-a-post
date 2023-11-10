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

## 分布式锁        [商品详情优化](D:\Java\java50th\java50-course-materials\04-微服务\01-课件\13_商品详情页2\商品详情优化.md)

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



### @annotation

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







