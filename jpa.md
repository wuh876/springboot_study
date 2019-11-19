### 1、导入依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```
### 2、添加配置文件
```
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://39.105.167.131:3306/smile_boot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8&useSSL=true
    username: root
    password: 

  jpa:
    properties:
      hibernate:
        hbm2ddl:
          auto: update
    show-sql: true
```
```
hibernate.hbm2ddl.auto 参数的作用主要用于：自动创建、更新、验证数据库表结构，有四个值。

create：每次加载 Hibernate 时都会删除上一次生成的表，然后根据 model 类再重新来生成新表，哪怕两次没有任何改变也要这样执行，
    这就是导致数据库表数据丢失的一个重要原因。
create-drop：每次加载 Hibernate 时根据 model 类生成表，但是 sessionFactory 一关闭，表就自动删除。
update：最常用的属性，第一次加载 Hibernate 时根据 model 类会自动建立起表的结构（前提是先建立好数据库），
    以后加载 Hibernate 时根据 model 类自动更新表结构，即使表结构改变了，但表中的行仍然存在，不会删除以前的行。要注意的是当部署到服务器后，
    表结构是不会被马上建立起来的，是要等应用第一次运行起来后才会。
validate ：每次加载 Hibernate 时，验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值。
```
### 3、新建bean
```
@Entity
public class User{
    @Id
    Long id;
    String name;
    
    //setter and getter
}

#### 注解:

@Entity(name="EntityName") 必须，用来标注一个数据库对应的实体，数据库中创建的表名默认和类名一致。其中，name 为可选，
    对应数据库中一个表，使用此注解标记 Pojo 是一个 JPA 实体。
@Table(name=""，catalog=""，schema="") 可选，用来标注一个数据库对应的实体，数据库中创建的表名默认和类名一致。
    通常和 @Entity 配合使用，只能标注在实体的 class 定义处，表示实体对应的数据库表的信息。
@Id 必须，@Id 定义了映射到数据库表的主键的属性，一个实体只能有一个属性被映射为主键。
@GeneratedValue(strategy=GenerationType，generator="") 可选，strategy: 表示主键生成策略，
    有 AUTO、INDENTITY、SEQUENCE 和 TABLE 4 种，分别表示让 ORM 框架自动选择，generator: 表示主键生成器的名称。
@Column(name = "user_code"， nullable = false， length=32) 可选，@Column 描述了数据库表中该字段的详细定义，
    这对于根据 JPA 注解生成数据库表结构的工具。
    name: 表示数据库表中该字段的名称，默认情形属性名称一致；
    nullable: 表示该字段是否允许为null，默认为 true；
    unique: 表示该字段是否是唯一标识，默认为 false；
    length: 表示该字段的大小，仅对 String 类型的字段有效。
@Transient可选，@Transient 表示该属性并非一个到数据库表的字段的映射，ORM 框架将忽略该属性。
@Enumerated 可选，使用枚举的时候，我们希望数据库中存储的是枚举对应的 String 类型，而不是枚举的索引值，
    需要在属性上面添加 @Enumerated(EnumType.STRING) 注解。
```
