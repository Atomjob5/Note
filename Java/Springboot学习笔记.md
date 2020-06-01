# Validtion

## NotNull、NotEmpty、NotBlank的区别

- `@NotEmpty`  String，Collection、Map、数组，是不能为null或者长度为0的（String、Collection、Map的`isEmpth()`方法）

- `@NotBlank`  忽略尾部空格，所以空字符串也不符合规则。

> The difference to `@NotEmpty` is that trailing whitespaces are getting ignored. 

- `@NotNUll`   不能为null

```markdown
1. @NotEmpty用在集合上面(不能注释枚举)

2. @NotBlank用在String上面

3. @NotNull用在所有类型上面
```



```java
String name = null;
@NotNull: false
@NotEmpty: false
@NotBlank: false
 
String name = "";
@NotNull: true
@NotEmpty: false
@NotBlank: false
 
String name = " ";
@NotNull: true
@NotEmpty: true
@NotBlank: false
 
String name = "Great answer!";
@NotNull: true
@NotEmpty: true
@NotBlank: true
```



## 其他的注解

```markdown
# JSR提供的校验注解
@Null   被注释的元素必须为 null    
@NotNull    被注释的元素必须不为 null    
@AssertTrue     被注释的元素必须为 true    
@AssertFalse    被注释的元素必须为 false    
@Min(value)     被注释的元素必须是一个数字，其值必须大于等于指定的最小值    
@Max(value)     被注释的元素必须是一个数字，其值必须小于等于指定的最大值    
@DecimalMin(value)  被注释的元素必须是一个数字，其值必须大于等于指定的最小值    
@DecimalMax(value)  被注释的元素必须是一个数字，其值必须小于等于指定的最大值    
@Size(max=, min=)   被注释的元素的大小必须在指定的范围内    
@Digits (integer, fraction)     被注释的元素必须是一个数字，其值必须在可接受的范围内    
@Past   被注释的元素必须是一个过去的日期    
@Future     被注释的元素必须是一个将来的日期    
@Pattern(regex=,flag=)  被注释的元素必须符合指定的正则表达式

# Hibernate Validator提供的校验注解
@NotBlank(message =)   验证字符串非 null，且长度必须大于 0    
@Email  被注释的元素必须是电子邮箱地址    
@Length(min=,max=)  被注释的字符串的大小必须在指定的范围内    
@NotEmpty   被注释的字符串的必须非空    
@Range(min=,max=,message=)  被注释的元素必须在合适的范围内

```



## 相关文章

[使用 spring validation 完成数据后端校验]: https://www.cnkirito.moe/spring-validation/
[@Valid和@Validated的总结区分]: https://www.cnblogs.com/guchunchao/p/9860337.html	"讲得还可以"





# 通配符

## /*  和  /**  的区别

/*  只有后面的一级

/**  可包含多级





# 注解

## @Target

```java
public enum ElementType {
    /** 
    	Class, interface (including annotation type), or enum declaration 
     	类，接口（包括注释类型）或枚举声明
     */
    TYPE,

    /** Field declaration (includes enum constants) */
    FIELD,

    /** Method declaration */
    METHOD,

    /** 
    	Formal parameter declaration 
    	正式的参数声明
    */
    PARAMETER,

    /** Constructor declaration */
    CONSTRUCTOR,

    /** Local variable declaration */
    LOCAL_VARIABLE,

    /** Annotation type declaration */
    ANNOTATION_TYPE,

    /** Package declaration */
    PACKAGE,

    /**
     * Type parameter declaration
     * 类型参数声明
     */
    TYPE_PARAMETER,

    /**
     * Use of a type
     * 使用的类型
     */
    TYPE_USE
}
```





## @Retention

```java
public enum RetentionPolicy {
    /**
     * Annotations are to be discarded by the compiler.
     * 注释只在源代码级别被保留，编译时被忽略
     */
    SOURCE,

    /**
     * Annotations are to be recorded in the class file by the compiler
     * but need not be retained by the VM at run time.  This is the default
     * behavior.
     * 注释将被编译器在类文件中记录，但在运行时不需要JVM保留（默认）
     */
    CLASS,

    /**
     * Annotations are to be recorded in the class file by the compiler and
     * retained by the VM at run time, so they may be read reflectively.
     * 注释将被编译器记录在类文件中，在运行时保留VM，因此可以反读
     */
    RUNTIME
}
```





## @Documented

表名这个注解是由JavaDoc记录，如果一个类型声明被注释了文档化，它的注释成为公共API的一部分





## @JsonFormat

> 将 `Date` 转化成 `String`，一般是后台传值给前台

```yaml
spring:
	jackson:
		date-format: yyyy-MM-dd HH:mm:ss
		time-zone: GMT+8
```







## @DatetimeFormat

> 将 `String` 转化成 `Date` ，一般是前台给后台传值使用

```java
@DatetimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
private Date sendTime;
```



## @Import





# RestFul 风格API设计规范

## URI规范

- 不用大写
- 用 `-` 不用 `_`
- 参数列表要 encode
- URI中的名词表示资源集合，使用复数形式
- 避免层级过深的URI。过深的导航容易导致url膨胀，不易维护，如 `GET /zoos/1/areas/3/animals/4`，尽量使用查询参数代替路径中的实体导航，如`GET /animals?zoo=1&area=3`





## 查询  GET

```markdown
GET /zoo
GET /zoos/1
GET /zoos/1/employees
```





## 创建  POST

> 创建单个资源。POST一般向“资源集合”型uri发起

```markdown
# 新增动物
POST /animals
# 为id为1的动物园雇佣员工
POST /zoos/1/employees
```







## 更新  PUT

> 更新单个资源（全量），客户端提供完整的更新后的资源。与之对应的是 PATCH，PATCH 负责部分更新，客户端提供要更新的那些字段。PUT/PATCH一般向“单个资源”型uri发起

```markdown
PUT /animals/1
PUT /zoos/1
```







## 删除   DELETE

```markdown
DELETE /zoos/1/employees/2
DELETE /zoos/1/employees/2;4;5
# 删除id为1的动物园内的所有动物
DELETE /zoos/1/animals
```



## 常用的HTTP状态码

| 状态码                    | 使用场景                                                     |
| ------------------------- | ------------------------------------------------------------ |
| 400 bad request           | 常用在参数校验                                               |
| 401 unauthorized          | 未经校验的用户，常用于未登录。如果经过验证后依然没有权限，应该403 |
| 403 forbidden             | 无权限                                                       |
| 404 not found             | 资源不存在                                                   |
| 500 internal server error | 非业务类异常                                                 |
| 503 service unavailable   | 由容器抛出，自己的代码不要抛这个异常                         |







[RESTful API接口设计标准及规范]:https://blog.csdn.net/qq_41606973/article/details/86352787







# 异常

> 相关文章

[Spring的@ControllerAdvice注解作用原理探究]:"https://zhuanlan.zhihu.com/p/73087879"







# Mybatis Generator

> 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <properties resource="generator.properties"/>
    <context id="MySqlContext" targetRuntime="MyBatis3" defaultModelType="flat">
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
        <property name="javaFileEncoding" value="UTF-8"/>
        <!-- 为模型生成序列化方法-->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
        <!-- 为生成的Java模型创建一个toString方法 -->
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>
        <!--可以自定义生成model的代码注释-->
        <commentGenerator type="com.vanessa.tripback.mbg.CommentGenerator">
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
            <property name="suppressDate" value="true"/>
            <property name="addRemarkComments" value="true"/>
        </commentGenerator>
        <!--配置数据库连接-->
        <jdbcConnection driverClass="${jdbc.driverClass}"
                        connectionURL="${jdbc.connectionURL}"
                        userId="${jdbc.username}"
                        password="${jdbc.password}">
            <!--解决mysql驱动升级到8.0后不生成指定数据库代码的问题-->
            <property name="nullCatalogMeansCurrent" value="true" />
        </jdbcConnection>
        <!--指定生成model的路径-->
        <javaModelGenerator targetPackage="com.vanessa.tripback.mbg.model" targetProject="D:\project\trip-recommend\trip-back\src\main\java"/>
        <!--指定生成mapper.xml的路径-->
        <sqlMapGenerator targetPackage="com.vanessa.tripback.mapper" targetProject="D:\project\trip-recommend\trip-back\src\main\resources"/>
        <!--指定生成mapper接口的的路径-->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.vanessa.tripback.mbg.mapper"
                             targetProject="D:\project\trip-recommend\trip-back\src\main\java"/>
        <!--生成全部表tableName设为%-->
        <table tableName="%">
            <generatedKey column="id" sqlStatement="MySql" identity="true"/>
        </table>
    </context>
</generatorConfiguration>
```



> 自定义注释生成器   - - - -  结合Swagger使用

```java
/**
 * 自定义注释生成器
 * Created by macro on 2018/4/26.
 */
public class CommentGenerator extends DefaultCommentGenerator {
    private boolean addRemarkComments = false;
    private static final String EXAMPLE_SUFFIX="Example";
    private static final String API_MODEL_PROPERTY_FULL_CLASS_NAME="io.swagger.annotations.ApiModelProperty";

    /**
     * 设置用户配置的参数
     */
    @Override
    public void addConfigurationProperties(Properties properties) {
        super.addConfigurationProperties(properties);
        this.addRemarkComments = StringUtility.isTrue(properties.getProperty("addRemarkComments"));
    }

    /**
     * 给字段添加注释
     */
    @Override
    public void addFieldComment(Field field, IntrospectedTable introspectedTable,
                                IntrospectedColumn introspectedColumn) {
        String remarks = introspectedColumn.getRemarks();
        //根据参数和备注信息判断是否添加备注信息
        if(addRemarkComments&&StringUtility.stringHasValue(remarks)){
//            addFieldJavaDoc(field, remarks);
            //数据库中特殊字符需要转义
            if(remarks.contains("\"")){
                remarks = remarks.replace("\"","'");
            }
            //给model的字段添加swagger注解
            field.addJavaDocLine("@ApiModelProperty(value = \""+remarks+"\")");
        }
    }

    /**
     * 给model的字段添加注释
     */
    private void addFieldJavaDoc(Field field, String remarks) {
        //文档注释开始
        field.addJavaDocLine("/**");
        //获取数据库字段的备注信息
        String[] remarkLines = remarks.split(System.getProperty("line.separator"));
        for(String remarkLine:remarkLines){
            field.addJavaDocLine(" * "+remarkLine);
        }
        addJavadocTag(field, false);
        field.addJavaDocLine(" */");
    }

    @Override
    public void addJavaFileComment(CompilationUnit compilationUnit) {
        super.addJavaFileComment(compilationUnit);
        //只在model中添加swagger注解类的导入
        if(!compilationUnit.isJavaInterface()&&!compilationUnit.getType().getFullyQualifiedName().contains(EXAMPLE_SUFFIX)){
            compilationUnit.addImportedType(new FullyQualifiedJavaType(API_MODEL_PROPERTY_FULL_CLASS_NAME));
        }
    }
}
```



> 生成器

```java
public class Generator {
    public static void main(String[] args) throws Exception {
        //MBG 执行过程中的警告信息
        List<String> warnings = new ArrayList<String>();
        //当生成的代码重复时，覆盖原代码
        boolean overwrite = true;
        //读取我们的 MBG 配置文件
        InputStream is = Generator.class.getResourceAsStream("/generatorConfig.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(is);
        is.close();

        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        //创建 MBG
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        //执行生成代码
        myBatisGenerator.generate(null);
        //输出警告信息
        for (String warning : warnings) {
            System.out.println(warning);
        }
    }
}
```





# Swagger

> 配置文件

```java
@Configuration
@EnableSwagger2
public class Swagger2Configuration {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.vanessa.tripback.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    /**
     * 创建基础信息
     * @return
     */
    public ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("旅游后端项目API接口")
                .contact("vanessa")
                .version("1.0")
                .build();
    }

}
```



> 一些常用的Swagger注解

| Swagger注解                                            | 简单说明                                             |
| ------------------------------------------------------ | ---------------------------------------------------- |
| @Api(tags = "xxx模块说明")                             | 作用在模块类上                                       |
| @ApiOperation("xxx接口说明")                           | 作用在接口方法上                                     |
| @ApiModel("xxxPOJO说明")                               | 作用在模型类上：如VO、BO                             |
| @ApiModelProperty(value = "xxx属性说明",hidden = true) | 作用在类方法和属性上，hidden设置为true可以隐藏该属性 |
| @ApiParam("xxx参数说明")                               | 作用在参数、方法和字段上，类似@ApiModelProperty      |

[Swagger使用说明]: "https://mp.weixin.qq.com/s/0-c0MAgtyOeKx6qzmdUG0w"







# Spring Security









# TOKEN 令牌的设计

> 相关的文章

[如何无痛的刷新token]:"https://www.jianshu.com/p/58f05bf13b7d"



> 令牌解析的错误

- MalformedJwtException-如果指定的JWT构造错误（因此无效）。 无效的JWT不应该被信任，应该被丢弃。
- SignatureException-如果发现JWS签名，但无法验证。 签名验证失败的JWT不应该被信任，应该被丢弃。
- ExpiredJwtException-如果指定的JWT是Claims JWT，并且Claims在调用此方法之前有一个过期时间。
- IllegalArgumentException-如果指定的字符串为null或为空，或者只有空白。











# AOP切面的使用

- `@Aspect`：用于定义切面
- `@Before`：通知方法会在目标方法调用之前执行
- `@After`：通知方法会在目标方法返回或抛出异常后执行
- `@AfterReturning`：通知方法会在目标方法返回后执行
- `@AfterThrowing`：通知方法会在目标方法抛出异常后执行
- `@Around`：通知方法会将目标方法封装起来
- `@Pointcut`：定义切点表达式







# 跨域

>  后端配置跨域

```java
/**
 * 全局跨域配置
 * Created by macro on 2019/7/27.
 */
@Configuration
public class GlobalCorsConfig {

    /**
     * 允许跨域调用的过滤器
     */
    @Bean
    public CorsFilter corsFilter() {
        CorsConfiguration config = new CorsConfiguration();
        //允许所有域名进行跨域调用
        config.addAllowedOrigin("*");
        //允许跨越发送cookie
        config.setAllowCredentials(true);
        //放行全部原始头信息
        config.addAllowedHeader("*");
        //允许所有请求方法跨域调用
        config.addAllowedMethod("*");
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", config);
        return new CorsFilter(source);
    }
}
```



> 配置了全局跨域后，如果手动response也要配置跨域

```java
//跨域
String headerOrigin = request.getHeader("origin");
if(headerOrigin != null){
    response.setHeader("Access-Control-Allow-Origin",headerOrigin);
}
response.setHeader("Access-Control-Expose-Headers","page, per_page,status,total_count");
response.setHeader("Access-Control-Allow-Credentials", "true");
response.setHeader("Access-Control-Allow-Headers",request.getHeader("Access-Control-Request-Headers"));
response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE,PUT");

//响应json
response.setCharacterEncoding("utf-8");
response.setContentType("application/json");

```







> Thumbnailator







# Thumbnailator

<a hreaf="https://www.cnblogs.com/linkstar/p/7412012.html">Java中使用Thumbnailator压缩图片</a>