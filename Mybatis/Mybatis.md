# 			Mybatis 

### 1、 动态SQL

#### 一、if

PS: 

​	**if** 标签通常用于WHERE语句中，通过判断参数值来决定是否使用某个查询条件，它也经常用于**UPDATE**语句中判断是否更新某一字段，还可以在INSERT语句中用来判断是否插入某个字段值

```xml
 SELECT * FROM employee_library
   <where>
      <if test="employeeName !=null and employeeName!=''">
           AND EmployeeName <![CDATA[ = ]]>#{employeeName,jdbcType=INTEGER}
      </if>
   </where>
 ORDER BY EmployeeNo ASC
```



#### 二、choose（when、otherwise）

PS:

​     **if** 条件无法实现 **if....else** 逻辑，**choose** 可以完成

​     **choose** 元素 中包含 **when**和 **otherwise** 两个标签，一个 **choose** 中至少有一个 **when**，有 0 个或者 l 个 **otherwise**

```xml
SELECT * FROM employee_library
   WHERE 1=1
     <choose>
        <when test="employeeName !=null and employeeName!=''">
           AND EmployeeName <![CDATA[ = ]]> #{employeeName}
        </when>
        <otherwise>
           AND EmployeeName <![CDATA[ = ]]> '赵云'
        </otherwise>
     </choose>
ORDER BY EmployeeNo ASC
```



####三、where 

PS:

​	where 标签的作用：如果该标签包含的元素中有返回值，就插入一个where,如果where后面的字符串是以AND 或者 OR 开头的，就将他们踢掉

```xml
<where>
    <if test="userName!=null and userName!=''">
        and user name like concat('%',#{userName},'%')
    </if>
    <if test="userName!=null and userName!=''"> 
        and user email = #{userEmail}
    </if>
</where>
```

* 当 if 条件都不满足的时候， where 元素中没有内容，所以在 SQL 中不会出现 where
* 如果 if 条件满足， where 元素的内容就是以 and 开头 的条件， where 会自动去掉开头的 and，这也能保证 where 条件正确。

#### 四、set 

PS:

​     set标签的作用：如果该标签包含的元素中有返回值，就插入一个set,如果set后面的字符串是以逗号结尾的，就将这个逗号踢掉

```xml
update sys_user
<set>
    <if test="userName!=null and userName!=''"> 
        user name= #{userName},
    </if>
    <if test="userPassword!=null and userPassword!=''"> 
        user password= #{userPassword},
    </if>
    <if test="userEmail!=null and userEmail!=''"> 
        user email = #{userEmail},
    </if> 
    <if test="userinfo!=null and userinfo!=''"> 
        user info = #{userinfo}, 
    </if>
    <if test="headimg!=null"> 
        head_img = #{headimg, jdbcType=BLOB},
    </if > 
    <if test="createTime!=null">
        create_time = #{createTime, jdbcType=TIMESTAMP}, 
	</if> 
    id = # {id}, 
</set>
where id = #{id}
```

#### 五、trim

PS：

​	**where** 和 **set** 标签的功能都可以用 **trim** 标签来实现，并且在底层就是通过TrimSqlNode实现的。

**where** 标签对应trim的实现如下：

```xml
<trim prefix="SET" suffixOverrides=",">
	.........
</trim>
```

**trim** 标签有如下属性：

* prefix：当 **trim** 元素内包含内容时，会给内容增加 **prefix** 指定的前缀
* prefixOverrides：当 **trim** 元素内包含内容时，会把内容中匹配的前缀字符去掉
* suffix：当**trim** 元素内包含内容时，会给内容增加**suffix**指定的后缀
* suffixOverrides：当**trim**元素内包含内容时，会把内容中匹配的后缀字符串去掉

#### 六、foreach

PS：

​	可以对数组、Map或实现了Iterable接口（如：List、Set）的对象进行遍历。

```xml
<select id="selectByidList" resultType="tk.mybatis.simple.model.SysUser">
    select id,
    user_name userName,
    user_password userPassword,
    user_email userEmail,
    user_info userinfo,
    head_img headimg,
    create_time createTime
    from sys_user
    where id in 
    <foreach collection="list" open="(" close=")" separator="," item="id" index="i"> 
        #{id} 
	</foreach> 
</select>
```

foreach包含以下属性：

* collection： 必填，值为要选代循环的属性名。这个属性值的情况有很多。 
* item：变量名，值为从迭代对象中取出的每一个值。
* index：索引的属性名，在集合数组情况下值为当前索引值，当迭代循环的对象是Map类型时，这个值为Map的Key（键值）
* open：整个循环内容开头的字符串
* close：整个循环内容结尾的字符串
* separator：每次循环的分隔符



案例：**foreach** 实现批量添加

```xml
<insert id="insertList">
    insert into sys_user
    ( 
     user_name,
     user_password,
     user_email,
     user_info,
     head_img,    
     create_time
    ) 
	values     
    <foreach collection="list" item="user" separator=",">
        (
          #{user.userName},
          #{user.userPassword},
          #{user.userEmail}, 
          #{user.userlnfo},
          #{user.headlmg, jdbcType=BLOB},
          #{user.createTime, jdbcType=TIMESTAMP}
        ) 
    </foreach> 
</insert>  
```



案例：**foreach** 实现动态UPDATE

```XML
<update id="updateByMap">
    update sys_user
     set
    <foreach collection="_parameter" item="val" index="key" separator=",">
        #{key}=#{val}
    </foreach>
    where id=#{id}
</update>
```



#### 七、bind

PS：

​	bind标签的作用：可以使用OGNL表达式创建一个变量并将其绑定到上下文中

```xml
<if test="userNarne !=null and userNarne!=''">
    and user_name like concat('%',#{userName},'%')
</if> 

<if test="userNarne !=null and userNarne!=''">
    <bind name="userNameLike" value="'%'+userName+'%'"/>
    and user_name like #{userNameLike}
</if>
```



### 2、SQL建表

PS：

​	基于MySql建立 【用户表】、【角色表】、【权限表】

【用户表】

```mysql
CREATE TABLE sys_user
(
   id 	BIGINT NOT NULL AUTO_INCREMENT COMMENT '用户ID',
   user_name 	VARCHAR(50) COMMENT '用户名',
   user_password VARCHAR(50) COMMENT '密码',
   user_email 	VARCHAR(50) COMMENT '邮箱',
   user_info 	TEXT COMMENT '简介',
   head_img 	BLOB COMMENT '头像',
   create_time 	DATETIME COMMENT '创建时间',
   PRIMARY KEY (id)  
 );
ALTER TABLE sys_user COMMENT '用户表'
```

【角色表】

```mysql
CREATE TABLE sys_role
(
   id BIGINT NOT NULL AUTO_INCREMENT COMMENT '角色ID',
   role_name VARCHAR(50) COMMENT '角色名',
   enabled INT COMMENT '有效标志',
   create_by BIGINT COMMENT '创建人',
   create_time DATETIME COMMENT '创建时间',
   PRIMARY KEY(id)
);
ALTER TABLE sys_role COMMENT '角色表'
```

【权限表】

```mysql
CREATE TABLE sys_privilege
(
   id  BIGINT NOT NULL AUTO_INCREMENT COMMENT '权限ID',
   privilege_name VARCHAR(50) COMMENT '权限名称',
   privilege_url VARCHAR(200) COMMENT '权限URL',
   PRIMARY KEY(id)
);
ALTER TABLE sys_privilege COMMENT '权限表'
```

【用户与角色关联表】

```mysql
CREATE TABLE sys_user_role
(
   user_id BIGINT COMMENT '用户ID',
   role_id BIGINT COMMENT '角色ID'
);
ALTER TABLE sys_user_role COMMENT '用户角色关联表'
```

【角色与权限关联表】

```mysql
CREATE TABLE sys_role_privilege
(
   role_id BIGINT COMMENT '角色ID',
   privilege_id BIGINT COMMENT '权限ID'
);
ALTER TABLE sys_role_privilege COMMENT '角色权限关联表'
```



### 3、高级查询



#### 1、Association 一对一

PS：一对一关系映射

```java
public class SysRole {
    private int id;
    private String roleName;
    private int enabled;
    // 一对一的映射关系 association标签代表将数据映射到 CreateUserInfo对象中
    private CreateUserInfo createUserInfo;
    // 可访问权限组
    private List<SysPrivilege> privilegeList;
    
    .........
}
```



```xml
<association property="createUserInfo" javaType="MySpring.SpringDao.CreateUserInfo">
            <result column="create_by" property="createBy"></result>
            <result column="create_time" property="createTime" jdbcType="TIMESTAMP"></result>
 </association>
```

association标签包含的属性：

* property：对应实体类中的属性名，必填项。
* javaType：属性对应的java类型。
* resultMap：可以直接使用现有的resultMap,而不需要在这里配置
* columnPrefix：查询类的前缀，配置前缀后，在子标签配置result的column时可以省略前缀。
* select：另一个映射查询的id，Mybatis会额外执行这个查询获取嵌套对象的结果
* column：列名（别名），将主查询中列的结果作为嵌套查询的参数，配置方式如column={prop1=col1,prop2=col2},prop1和prop2将作为嵌套查询的参数
* fetchType：数据加载方式，可选值为lazy(延迟加载)、eager(积极加载)，这个配置会覆盖全局的lazyLoadingEnabled配置



#### 2、Collection 一对多关系映射

PS：

​	通过一次SQL查询将所有的结果查询出来，然后通过配置的结果映射，将数据映射到不同的对象中。

首先在Model中添加一对多的属性关系

【SysUser.class】文件添加角色属性字段

```java
// 一个用户有多种身份
public class SysUser {
    private int id;
    private String userName;
    private String userPassword;
    private String userEmail;
    private String userInfo;
    private byte[] headImg;
    private Date createTime;
    // 一对多的角色信息
    private List<SysRole> roleList;
    
    .........
        
   public List<SysRole> getRoleList() {
        return roleList;
    }

    public void setRoleList(List<SysRole> roleList) {
        this.roleList = roleList;
    }   
}

```

【SysUserMapper.xml】resultMap配置

```xml
 <resultMap id="sysUserBySysRoleList" type="MySpring.SpringDao.SysUser">
        <result column="id" property="id"></result>
        <result column="user_name" property="userName"></result>
        <result column="user_password" property="userPassword"></result>
        <result column="user_email" property="userEmail"></result>
        <result column="user_info" property="userInfo"></result>
        <result column="head_img" property="headImg"></result>
        <result column="create_time" property="createTime" jdbcType="TIMESTAMP"></result>
        <collection property="roleList" columnPrefix="role_" ofType="MySpring.SpringDao.SysRole">
            <result column="id" property="id"></result>
            <result column="role_name" property="roleName"></result>
            <result column="enabled" property="enabled"></result>
            <result column="create_by" property="createBy"></result>
            <result column="create_time" property="createTime" jdbcType="TIMESTAMP"></result>
        </collection>
    </resultMap>
```

【SysUserMapper.xml】一对多查询,

```xml
// resultMap="sysUserBySysRoleList"
<select id="selectSysUserBySysRoleList" resultMap="sysUserBySysRoleList">
         SELECT      
     	 u.id,
         u.user_name,
         u.user_password,
         u.user_email,
         u.user_info,
         u.head_img,
         u.create_time,
         ur.id role_id,
         ur.role_name role_role_name,
         ur.enabled role_enabled,
         ur.create_by role_create_by,
         ur.create_time role_create_time
         FROM Sys_user u
         INNER JOIN sys_user_role uur
         ON u.id=uur.user_id
         INNER JOIN sys_role ur
         ON uur.role_id=ur.id
         WHERE u.id=#{id}
         ORDER  by uur.role_id ASC
    </select>
```

```json
// 一对多关系映射结果如下
{
"id":826481371,
"userName":"诺",
"userPassword":"admin.999",
"userEmail":"826481371@qq.com",
"userInfo":"个性签名不能忘记哦!",
"headImg":null,
"createTime":1570854278000,
"roleList":[
                {
                "id":80001,
                "roleName":"系统管理员",
                "enabled":1,
                "createBy":826481371,
                "createTime":1570897636000
                },
                {
                "id":80002,
                "roleName":"业务管理员",
                "enabled":1,
                "createBy":826481371,
                "createTime":1571102899000
                },
                {
                "id":80003,
                "roleName":"普通管理员",
                "enabled":1,
                "createBy":826481371,
                "createTime":1571102937000
                },
                {
                "id":80004,
                "roleName":"字典管理员",
                "enabled":1,
                "createBy":826481371,
                "createTime":1571102965000
                },
                {
                "id":80005,
                "roleName":"门户管理员",
                "enabled":1,
                "createBy":826481371,
                "createTime":1571102983000
                }
            ]
}
```

PS:

​	MyBatis一对多数据映射根据resultMap中配置的ID来合并，上面的数据在SQL中的表现形式如下

<img src=".././Img/一对多关系数据映射MYSQL.jpg"/>

根据ID【826481371】将数据合并到一条，**MyBatis数据一对多合并根据ID进行**



#### 3、一对一，一对多的混合使用

一个JAVA 的POJO模型

```java
// 一个角色对应一个创建者，一个角色有多个权限
public class SysRole {
    private int id;
    private String roleName;
    private int enabled;
    // 一对一的创建者
    private CreateUserInfo createUserInfo;
    // 一对多的访问权限
    private List<SysPrivilege> privilegeList;
    .........
}
```

Mapper.xml配置

```xml
<resultMap id="sysRoleAndSysPrivilegeListMap" type="MySpring.SpringDao.SysRole">
        <id column="id" property="id"></id>
        <result column="role_name" property="roleName"></result>
        <result column="enabled" property="enabled"></result>
        // 一对一创建者
        <association property="createUserInfo" javaType="MySpring.SpringDao.CreateUserInfo">
            <result column="create_by" property="createBy"></result>
            <result column="create_time" property="createTime" jdbcType="TIMESTAMP"></result>
        </association>
        // 一对多的权限信息
        <collection property="privilegeList" columnPrefix="privilege_" ofType="MySpring.SpringDao.SysPrivilege" resultMap="MySpring.SpringMapper.SysPrivilegeMapper.sysPrivilegeMap">
        </collection>
    </resultMap>
```

Mapper.xml查询

```XML
// resultMap="sysRoleAndSysPrivilegeListMap"
<select id="getSysRoleAndSysPrivilegeList" resultMap="sysRoleAndSysPrivilegeListMap">
       SELECT a.id,a.role_name,a.enabled,a.create_by,a.create_time,
       c.id privilege_id,
       c.privilege_name privilege_privilege_name,
       c.privilege_url privilege_privilege_url
       FROM sys_role a
       LEFT JOIN sys_role_privilege b
       ON a.id=b.role_id
       LEFT JOIN sys_privilege c
       ON b.privilege_id=c.id
       WHERE a.id=#{id}
 </select>
```

查询映射结果如下:

```json
{
"id":80001,
"roleName":"系统管理员",
"enabled":1,
"createUserInfo":{
"createBy":826481371,
"createTime":1570897636000
},
"privilegeList":[
{
"id":10001,
"privilegeName":"C++VIP课程专区",
"privilegeUrl":"C++"
}
]
}
```



#### 4、嵌套查询中的Column

PS：

​	Association 和 Collection 标签中使用Column，语法

        ```xml
<resultMap id="sysRoleListMap" type="MySpring.SpringDao.SysRole">
        <id column="id" property="id"></id>
        <result column="role_name" property="roleName"></result>
        <result column="enabled" property="enabled"></result>
        <association property="createUserInfo" columnPrefix="create_" javaType="MySpring.SpringDao.CreateUserInfo">
            <result column="create_by" property="createBy"></result>
            <result column="create_time" property="createTime" jdbcType="TIMESTAMP"></result>
        </association>
        <collection property="privilegeList" column="{sysRoleId=id}" select="MySpring.SpringMapper.SysPrivilegeMapper.getSysPrivilegeByRoleId" ></collection>
    </resultMap>
        ```

**column="{sysRoleId=id}"** 的含义：

**sysRoleId**：表示 SysPrivilegeMapper.getSysPrivilegeByRoleId的参数

**id**：SysRole的主键ID

```xml
<select id="getSysRoleListBySysUserId" resultMap="sysRoleListMap">
        SELECT a.id,a.role_name,a.enabled,a.create_by create_create_by,a.create_time 			    create_create_time
        FROM sys_role a
        LEFT JOIN sys_user_role b
        ON a.id=b.role_id
        LEFT JOIN sys_user c
        ON c.id=b.user_id
        WHERE c.id=#{SysUserId}
    </select>
```

PS：

​     将查询出来用户 **ID** 当作参数传递给getSysPrivilegeByRoleId方法查询角色信息

















