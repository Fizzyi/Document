
# 依赖
```yml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.3.1</version>
</dependency>
```

# mapper.java 
让mapper 继承 BaseMapper,其中的泛型为具体需要操作的 po，非常重要。
```java
public interface UserMapper extends BaseMapper<User>{

}
```

这个泛型中的 User 就是与数据库对应的 po。
默认情况下：
- MybatisPlus 会把 PO 实体的类名驼峰转下划线作为表名。
- MybatisPlus 会把 PO 实体的所有变量名转下划线作为表的字段名，并根据变量类型推断字段类型
- MybatisPlus 会把名为 id 的字段作为主键。

# 常见注解
## @TableName
说明： 表名注解，标识实体类对应的表。使用位置为实体类。
```java
@TableName("user")
public class User {
    private Long id;
    private String name;
}
```
理解：如果表名和实体类的名称不符合默认条件，使用该注解进行映射。

## @TableId
说明：主键注解，标识是贴类中的主键字段。。使用位置为实体类型的主键字段。
```Java
@TableName("user")
public class User {
    @TableId
    private Long id;
    private String name;
}
```

## @TableField
说明：普通字段注解
```java
@TableName("user")
public class User {
    @TableId
    private Long id;
    private String name;
    private Integer age;
    @TableField("isMarried")
    private Boolean isMarried;
    @TableField("concat")
    private String concat;
}
```
- 成员变量名与数据库字段名不一致
- 成员变量是以 `isXXX`命名的，按照规范，MybatisPlus 识别字段时会把 `is`去除。
- 成员变量名与数据库一致，但是与数据库的关键字冲突。

# 手写sql
mapper 文件的读取地址默认为 `classpath*:/mapper/**/*.xml`


# 复杂查询
## 条件构造器
### QueryWrapper

### UpdateWrapper
### LambdaQueryWrapper


## Service接口
通用接口为 IService，默认实现为 ServiceImpl
### save
 - save 新增单个元素
 - saveBatch 批量新增
 - saveOrUpdate 根据id 判断，如果数据存在就更新，不存在就新增
 - saveOrUpdateBatch 批量的新增或者修改
### remove
 - removeById 根据id 删除
 - removeByIds 根据id 批量删除
 - removeByMap 根据Map中的键值对为条件删除
 - remove(Wrapper<T>) 根据 Wrapper 条件删除
### update
 - updateById 根据id 修改
 - update(Wrapper<T>) 根据 UpdateWrapper 修改，Wrapper 中包含set 和 where 部分。
 - update(T,Wrapper<T>) 根据 T 内的数据修改与Wrapper 匹配到的数据。
 - updateBatchById: 根据id 批量修改。
### Get
 -  getById: 根据id 查询一条数据
 -  getOne(Wrapper<T>) 根据 Wrapper 查询1 条数据。
### List
 - listByIds：根据id 批量查询
 - list(Wrapper<T>:根据 Wrapper 条件查询多条数据)
 - list() 查询所有
- list
- count
- page
