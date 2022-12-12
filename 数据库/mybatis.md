# MyBatis

## 使用

- 添加MyBatis的依赖

![image-20220114134705157](C:\Users\BDA\Documents\Note\pic\image-20220114134705157.png)

- 创建user数据表

- 编写User实体类

- 创建UserMapper.xml

  ![image-20220114105552990](C:\Users\BDA\Documents\Note\pic\image-20220114105552990.png)

- 创建mybatis.xml

![image-20220114134633386](C:\Users\BDA\Documents\Note\pic\image-20220114134633386.png)

- 测试

![image-20220114134559769](C:\Users\BDA\Documents\Note\pic\image-20220114134559769.png)

## 增删改

### 增

![image-20220114213526300](C:\Users\BDA\Documents\Note\pic\image-20220114213526300.png)

![image-20220114213455777](C:\Users\BDA\Documents\Note\pic\image-20220114213455777.png)

### 删

![image-20220114220447737](C:\Users\BDA\Documents\Note\pic\image-20220114220447737.png)

![image-20220114220520027](C:\Users\BDA\Documents\Note\pic\image-20220114220520027.png)

### 改

![image-20220114220502273](C:\Users\BDA\Documents\Note\pic\image-20220114220502273.png)

![image-20220114220530524](C:\Users\BDA\Documents\Note\pic\image-20220114220530524.png)

## 配置文件

- environments
  - transactionManager
    - JDBC
    - MANAGED
  - dataSource
    - UNPOOLED
    - POOLED
    - JNDI
- Mapper
- Properties
  - 开发中习惯将数据源的配置信息抽取出来

- typeAliases 
  - 起别名

## API

- SqlSessionFactoryBuilder
  - 用 build() 方法构建一个SqlSessionFactory对象
  - SqlSessionFactory常用的创建SqlSession实例的方法有以下两个
    - openSession()
      - 会默认开启一个事务，但事务不会自动提交，需要手动提交
    - openSession(boolean autoCommit)
      - 参数为是否自动提交
- SqlSession
  - 增删改查

## Dao层实现

- 代理开发实现
  - ![image-20220115112741790](C:\Users\BDA\Documents\Note\pic\image-20220115112741790.png)
  - ![image-20220115113017435](C:\Users\BDA\Documents\Note\pic\image-20220115113017435.png)
  - ![image-20220115113057015](C:\Users\BDA\Documents\Note\pic\image-20220115113057015.png)
    - UserMapper.xml 中的命名空间需要是接口的全路径
    - 方法id需要和接口方法名一致
    - 放回值和参数也需要一致

## 返回值中有集合

```java
```



## 映射文件深入

### 动态Sql

- if

![image-20220115155256987](C:\Users\BDA\Documents\Note\pic\image-20220115155256987.png)

- foreach
  - ![image-20220115162729908](C:\Users\BDA\Documents\Note\pic\image-20220115162729908.png)

### Sql语句抽取

- ```xml
  <sql id="">常用sql语句</sql>	
  ```

## 配置文件深入

### TypeHandlers<T>

- T是想要转换的类型

- 自定义类型转换器
  - 继承BaseTypeHandler，实现四个方法
    - setNonNullParameter()
      - 将Java类型转换成数据库需要的类型
    - getNullableResult()
      - 有三个重载
      - 将数据库中的数据转换成Java类型

### Plugins

#### PageHelper

- 导入依赖

  - ![image-20220115192044967](C:\Users\BDA\Documents\Note\pic\image-20220115192044967.png)

- 配置

  - ![image-20220115194004451](C:\Users\BDA\Documents\Note\pic\image-20220115194004451.png)

- 使用

  - 显示数据

    - ```java
      PageHelper.startPage(第几页,一页几条数据);
      ```

  - ![image-20220115220614663](C:\Users\BDA\Documents\Note\pic\image-20220115220614663.png)

## 多表操作

### 一对一

将查询的结果和实体中的元素一一对应

![image-20220115230034427](C:\Users\BDA\Documents\Note\pic\image-20220115230034427.png)

另一种写法

![image-20220115230713501](C:\Users\BDA\Documents\Note\pic\image-20220115230713501.png)

### 一对多

![image-20220115235315315](C:\Users\BDA\Documents\Note\pic\image-20220115235315315.png)

### 多对多

​	![image-20220116093848663](C:\Users\BDA\Documents\Note\pic\image-20220116093848663.png)

## 注解开发

![image-20220116094116313](C:\Users\BDA\Documents\Note\pic\image-20220116094116313.png)

### 步骤

- 在接口上写注解，在注解中写sql语句
- 在mybatis.xml 中配置映射关系![image-20220116100229537](C:\Users\BDA\Documents\Note\pic\image-20220116100229537.png)

### 一对一

![image-20220116112244407](C:\Users\BDA\Documents\Note\pic\image-20220116112244407.png)

方式二

![image-20220116112337018](C:\Users\BDA\Documents\Note\pic\image-20220116112337018.png)

### 一对多

![image-20220116114540981](C:\Users\BDA\Documents\Note\pic\image-20220116114540981.png)

![image-20220116114638002](C:\Users\BDA\Documents\Note\pic\image-20220116114638002.png)

### 多对多

![image-20220116115750164](C:\Users\BDA\Documents\Note\pic\image-20220116115750164.png)

![image-20220116115809117](C:\Users\BDA\Documents\Note\pic\image-20220116115809117.png)

# MyBatisPlus

## 使用步骤

1. 引入依赖

```xml
<!-- mybatis-plus -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.3.1</version>
</dependency>
<!-- mybatis-plus代码生成器 -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.3.1.tmp</version>
</dependency>
<!-- mybatisPlus Freemarker 模版引擎 -->
<dependency>
    <groupId>org.freemarker</groupId>
    <artifactId>freemarker</artifactId>
</dependency>
<!-- freemarker 作为 MyBatis-Plus 自动生成代码时作为模板使用，还可选用 velocity 作为模板 -->
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.2</version>
</dependency>
```

2. 配置信息

```yaml
# mybatis配置
mybatis-plus:
  # xml文件路径
  mapper-locations: classpath:mapper/*.xml
  # 实体类路径
  type-aliases-package: com.xjh.entity
  configuration:
    # 驼峰转换
    map-underscore-to-camel-case: true
    # 是否开启缓存
    cache-enabled: false
    # 打印sql
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  # 全局配置
  global-config:
    # 数据库字段驼峰下划线转换
    db-column-underline: true
    # id自增类型(数据库id自增)
    id-type: 0
    	# 0 为数据库自增id
    	# 1 为不设置自增id
    	# 2 为用户输入id
    	# 3 为当插入对象为空时自动填充id
    	# 4 为分配字符串型的UUID
```

- MyBatis-Plus 默认的id生成算法是 雪花算法 ，缺点是比较长

3. 配置启动类

```java
@MapperScan("")
@ComponentScan(basepackages = {""})
public void 启动类(){
    
}
```

## 代码生成器

```java
public class CodeGenerator {
    /**
     * <p>
     * 读取控制台内容
     * </p>
     */
    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotEmpty(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    /**
     * 自动生成代码
     */
    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();
        // TODO 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        // 生成文件的输出目录【默认 D 盘根目录】
        gc.setOutputDir(projectPath + "/src/main/java");
        // 作者
        gc.setAuthor("lizhou");
        // 是否打开输出目录
        gc.setOpen(false);
        // controller 命名方式，注意 %s 会自动填充表实体属性
        gc.setControllerName("%sController");
        // service 命名方式
        gc.setServiceName("%sService");
        // serviceImpl 命名方式
        gc.setServiceImplName("%sServiceImpl");
        // mapper 命名方式
        gc.setMapperName("%sMapper");
        // xml 命名方式
        gc.setXmlName("%sMapper");
        // 开启 swagger2 模式
        gc.setSwagger2(true);
        // 是否覆盖已有文件
        gc.setFileOverride(true);
        // 是否开启 ActiveRecord 模式
        gc.setActiveRecord(true);
        // 是否在xml中添加二级缓存配置
        gc.setEnableCache(false);
        // 是否开启 BaseResultMap
        gc.setBaseResultMap(true);
        // XML columList
        gc.setBaseColumnList(false);
        // 全局 相关配置
        mpg.setGlobalConfig(gc);

        // TODO 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://127.0.0.1:3306/sbm?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC&useSSL=true&characterEncoding=UTF-8");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("123456");
        mpg.setDataSource(dsc);

        // TODO 包配置
        PackageConfig pc = new PackageConfig();
        // 父包名。如果为空，将下面子包名必须写全部， 否则就只需写子包名
        pc.setParent("com.zyxx.sbm");
        // Entity包名
        pc.setEntity("entity");
        // Service包名
        pc.setService("service");
        // Service Impl包名
        pc.setServiceImpl("service.impl");
        mpg.setPackageInfo(pc);

        // TODO 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };
        // 输出文件配置
        List<FileOutConfig> focList = new ArrayList<>();
        focList.add(new FileOutConfig("/templates/mapper.xml.ftl") {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输入文件名称
                return projectPath + "/src/main/resources/mapper/" + tableInfo.getEntityName() + "Mapper.xml";
            }
        });
        // 自定义输出文件
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);
        mpg.setTemplate(new TemplateConfig().setXml(null));

        // TODO 策略配置
        StrategyConfig strategy = new StrategyConfig();
        // 数据库表映射到实体的命名策略，驼峰原则
        strategy.setNaming(NamingStrategy.underline_to_camel);
        // 字数据库表字段映射到实体的命名策略，驼峰原则
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        // 实体是否生成 serialVersionUID
        strategy.setEntitySerialVersionUID(false);
        // 是否生成实体时，生成字段注解
        strategy.setEntityTableFieldAnnotationEnable(true);
        // 使用lombok
        strategy.setEntityLombokModel(true);
        // 设置逻辑删除键
        strategy.setLogicDeleteFieldName("del_flag");
        // TODO 指定生成的bean的数据库表名
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
        // 驼峰转连字符
        strategy.setControllerMappingHyphenStyle(true);
        mpg.setStrategy(strategy);
        // 选择 freemarker 引擎需要指定如下加，注意 pom 依赖必须有！
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }
}
```

- 使用 MyBatisPlus 提供的 SqlSessionFactoryBuilder 去创建sqlSession实例
- mapper 类继承 BaseMapper 类

## 常用注解

- TableField(value = "")
  - 对象中的属性名和字段名不一致
    - TableField(value = "")
  - 对象中的属性字段在表中不存在
    - TableField(exist = false)
  - 想让对象中的属性不被查询出来
    - TableField(select = false)
  - 自动填充
    - `@TableField(fill = FieldFill.INSERT_UPDATE)`

## Wrapper

- QueryWrapper<User>

- UpdateWrapper<User>

- 基本操作字符

  | 操作符                              | 作用                   |
  | ----------------------------------- | ---------------------- |
  | wrapper.eq("字段名",值)             | 等于                   |
  | wrapper.set("字段名",值)            | 设置                   |
  | wrapper.ne("字段名",值)             | 不等于                 |
  | wrapper.gt("字段名",值)             | 大于                   |
  | wrapper.ge("字段名",值)             | 大于等于               |
  | wrapper.lt("字段名",值)             | 小于                   |
  | wrapper.le("字段名",值)             | 小于等于               |
  | wrapper.between(值1, 值2)           | 在值1和值2之间         |
  | wrapper.notBetween(值1, 值2)        | 不在值1和值2之间       |
  | wrapper.in(值1, 值2)                | 值1或值2               |
  | wrapper.notIn(值1, 值2)             | 不是值1或值2           |
  | wrapper.alleq(map, 是否处理null值)  | 全相等                 |
  | wrapper.like("字段名",值)           | like   %值%            |
  | wrapper.notLike("字段名",值)        | not like  %值%         |
  | wrapper.likeLeft("字段名",值)       | %值  ---> 以这个值结尾 |
  | wrapper.likeLeft("字段名",值)       | 值%  ---> 以这个值开头 |
  | wrapper.orderBy("字段名")           | 升序排序               |
  | wrapper.orderByAsc("字段名")        | 升序排序               |
  | wrapper.orderByDesc("字段名")       | 降序排序               |
  | wrapper.or()                        | or                     |
  | wrapper.and()                       | and                    |
  | wrapper.select("字段名1","字段名2") | 指定输出的字段         |
  
- 可以在参数中写连接条件，返回值是boolean

## Wrappers

- 可替代wrapper对象使用

### 方法

- `<>query()` 
- `lambdaQuery()` 

### CRUD

#### 增

- userMapper.insert(user)

#### 删

- userMapper.deleteById(id)
- userMapper.deleteByMap(map) 根据map删除数据，and关系
  - map.put("字段名",值)
- userMapper.delete(wrapper)
- userMapper.deleteBatchIds(Arrays.asList(id1,id2)) 根据id批量删除 

#### 改

- userMapper.updateById(user)
  - 会根据user中id的值去查找

#### 查

- selectById(id)         根据id查询
- selectBatchIds(Arrays.asList(id1,id2))  根据id批量查询
- selectOne(wrapper) 只能查询一条数据，超出会报错
- selectCount(wrapper)  返回数据的条数
- selectList(wrapper)      查询全部记录
- selectPage(page,wrapper)    分页查询
  - 在配置类中配置分页插件
    - ![image-20220116223437589](C:\Users\BDA\Documents\Note\pic\image-20220116223437589.png)
  - 创建一个Page<User>对象     

###  配置

- ConfigLocation

![image-20220117020109642](C:\Users\BDA\Documents\Note\pic\image-20220117020109642.png)

- MapperLocations  自定义sql语句

![image-20220117020519130](C:\Users\BDA\Documents\Note\pic\image-20220117020519130.png)

- TypeAliasesPackage         全局包扫描路径，别名扫描

![image-20220117020925098](C:\Users\BDA\Documents\Note\pic\image-20220117020925098.png)

- MapUnderscoreToCamelCase         驼峰命名自动映射
  - 在mybatis 中默认是false的，plus 中默认是true的
  - ![image-20220117022359340](C:\Users\BDA\Documents\Note\pic\image-20220117022359340.png)
- CacheEnabled    缓存
  - 默认是true 
  - ![image-20220117022409501](C:\Users\BDA\Documents\Note\pic\image-20220117022409501.png)

### DB策略

- idType
  - 设置全局默认主键类型
  - ![image-20220117022435398](C:\Users\BDA\Documents\Note\pic\image-20220117022435398.png)
- tablePrefix
  - 设置表名前缀
  - ![image-20220117022902070](C:\Users\BDA\Documents\Note\pic\image-20220117022902070.png)

## 常见问题

### 找不到resource下的文件，在pom以下内容

![image-20220114232916497](C:\Users\BDA\Documents\Note\pic\image-20220114232916497.png)

![image-20220116161753476](C:\Users\BDA\Documents\Note\pic\image-20220116161753476.png)

### 插入数据时id自增长异常

- 在实体类的id属性上添加@TableId(type = IdType.AUTO)

### 找不到与数据库表匹配的实体类

- 在实体类上添加@TableName("表名")

# Druid

## 简介

- 是一个JDBC组件，包含三个部分
  - DruidDriver：代理Driver，能够提供基于Filter－Chain模式的插件体系
  - DruidDataSource ：高效可管理的数据库连接池
  - SQLParser ：
- 用途
  - 监控数据库访问性能，Druid内置提供了一个功能强大的StatFilter插件，能够详细统计SQL的执行性能，这对于线上分析数据库访问性能有帮助
  - 高效、功能强大、可扩展性好
  - 数据库密码加密。直接把数据库密码写在配置文件中，这是不好的行为，容易导致安全问题。DruidDruiver和DruidDataSource都支持PasswordCallback
  - SQL执行日志，Druid提供了不同的 LogFilter，能够支持 Common-Logging、Log4j 和 JdkLog
  - 可以通过Druid提供的Filter-Chain机制，很方便编写JDBC层的扩展插件

## 使用步骤

1. 导入依赖
2. 单数据源配置配置

```yaml
spring:
  application:
    name: druidDemo
  datasource:
    url: jdbc:mysql://localhost:3306/test?characterEncoding=UTF-8
    driver-class-name: com.mysql.jdbc.Driver
    username: xxx # 数据库账号
    password: xxx@ # 数据库密码
    type: com.alibaba.druid.pool.DruidDataSource # 设置类型为 DruidDataSource
    # Druid 自定义配置，对应 DruidDataSource 中的 setting 方法的属性
    druid: # 设置 Druid 连接池的自定义配置。然后 DruidDataSourceAutoConfigure 会自动化配置 Druid 连接池。
      min-idle: 0 # 池中维护的最小空闲连接数，默认为 0 个。
      max-active: 20 # 池中最大连接数，包括闲置和使用中的连接，默认为 8 个。
```

3. 多数据源配置

```yaml
spring:
  application:
    name: druidDemo
  datasource:
    mall:
      url: jdbc:mysql://localhost:3306/test?characterEncoding=UTF-8
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root # 数据库账号
      password: root0319@ # 数据库密码
      type: com.alibaba.druid.pool.DruidDataSource # 设置类型为 DruidDataSource
      min-idle: 0 # 池中维护的最小空闲连接数，默认为 0 个。
      max-active: 20 # 池中最大连接数，包括闲置和使用中的连接，默认为 8 个。
    # 用户数据源配置
    users:
      url: jdbc:mysql://localhost:3306/test?characterEncoding=UTF-8
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root # 数据库账号
      password: root0319@ # 数据库密码
      type: com.alibaba.druid.pool.DruidDataSource # 设置类型为 DruidDataSource
      min-idle: 0 # 池中维护的最小空闲连接数，默认为 0 个。
      max-active: 20 # 池中最大连接数，包括闲置和使用中的连接，默认为 8 个。
```

4. 配置监控台

```yaml
druid: # 设置 Druid 连接池的自定义配置。然后 DruidDataSourceAutoConfigure 会自动化配置 Druid 连接池。
	filter:
		stat: # 配置 StatFilter 
			log-slow-sql: true # 开启慢查询记录
			slow-sql-millis: 5000 # 慢 SQL 的标准，单位：毫秒
			merge-sql: true # SQL合并配置
	stat-view-servlet: # 配置 StatViewServlet
		enabled: true # 是否开启 StatViewServlet
		login-username: root # 账号
		login-password: root # 密码
```

- stat文档：https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE_StatFilter
- stat-view-servlet文档：https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE_StatViewServlet%E9%85%8D%E7%BD%AE
- Druid监控台：http://127.0.0.1:8082/druid/sql.html 

使用

```java
public static Connection getConnection(){
    try {
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("Druid.Properties");
        Properties prop = new Properties();
        prop.load(is);
        DataSource source = DruidDataSourceFactory.createDataSource(prop);
        return source.getConnection();
    } catch (Exception e) {
        e.printStackTrace();
    }
    return null;
}
```

# 获取DataSource对象

- @Autowired注入DataSource

# 缓存机制

### 二级缓存

- 本地缓存（SqlSession）
- 

## Plan

- [ ] 了解velocity
- [ ] 基于Filter－Chain模式的插件体系
- [ ] SQLParser 
- [ ] StatFilter插件
- [ ] 线上分析数据库

