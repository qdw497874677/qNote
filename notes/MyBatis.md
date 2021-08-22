[**首页**](https://github.com/qdw497874677/myNotes/blob/master/首页检索.md)

# 面试问题

## 是什么

ORM框架。可以通过XML或者注解

## #{}和 ${}的区别是什么？

\#{}是预编译处理，${}是字符替换。 在使用 #{}时，MyBatis 会将 SQL 中的 #{}替换成“?”，配合 PreparedStatement 的 set 方法赋值，自动进行java类型和jdbc类型转换，这样可以有效的防止 SQL 注入，保证程序的运行安全。



## 缓存

目的就是提升查询效率和减少数据库压力。MyBatis有一级缓存、二级缓存、预留了集成第三方缓存的接口

### 一级缓存

也叫本地缓存。在会话（SqlSession）层面进行缓存。默认是开启的。MyBatis在开启一个数据库会话时会创建一个新的SqlSession对象，在一个SqlSession中对每次查询的结果缓存起来。会话结束时释放。每一个SqlSession都有一个自己Executor，每一个Executor都有一个自己的的缓存

### 二级缓存

二级缓存用来解决一级缓存不能跨会话共享的问题，范围是namespace级别的，可以被多个SqlSession共享（调用的是同一个接口中的相同方法就能用到缓存）。

开启了二级缓存后，SqlSession之间可以共享缓存。



## 接口和xml怎么进行绑定





# 多数据源







# 注解增删改

在mapper中方法添加注解，实现增删改查。

如果存在多个参数，必须参数前面加注解。

~~~java
@Select("select * from user where id=#{id}")
User getUserById(@Param("id") Integer id);
~~~



# 分页

## **pagehelper**

### 导入依赖

~~~xml
<!--pagehelper-->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.3</version>
</dependency>
~~~

### 查询

~~~java
@Override
public PageInfo<Tag> getBlogPageHelper(Integer page, Integer size) {
    PageHelper.startPage(page,size);
    List<Tag> tags = tagMapper.qureyTagList();
    PageInfo pageInfo = new PageInfo(tags);
    return pageInfo;
}
~~~

### 分页的字段

~~~java
//当前页
private int pageNum;
//每页的数量
private int pageSize;
//当前页的数量
private int size;

//由于startRow和endRow不常用，这里说个具体的用法
//可以在页面中"显示startRow到endRow 共size条数据"

//当前页面第一个元素在数据库中的行号
private int startRow;
//当前页面最后一个元素在数据库中的行号
private int endRow;
//总记录数
private long total;
//总页数
private int pages;
//结果集
private List<T> list;

//前一页
private int prePage;
//下一页
private int nextPage;

//是否为第一页
private boolean isFirstPage = false;
//是否为最后一页
private boolean isLastPage = false;
//是否有前一页
private boolean hasPreviousPage = false;
//是否有下一页
private boolean hasNextPage = false;
//导航页码数
private int navigatePages;
//所有导航页号
private int[] navigatepageNums;
//导航条上的第一页
private int navigateFirstPage;
//导航条上的最后一页
private int navigateLastPage;
~~~

### **排序**

~~~java
PageHelper.startPage(pageNum , pageSize);
PageHelper.orderBy("A B");
//其中A为排序依据的字段名，B为排序规律，desc为降序，asc为升序

//或者一步到位
String orderBy="字段名 排序规律";
PageHelper.startPage(pageNum, pageSize, orderBy);
~~~



# ResultMap结果集映射

实体类的属性名和数据库查询的到结果的字段名不一致，需要进行映射

~~~xml
<resultMap id="blogtype" type="Blog">
        <result property="id" column="bi"/>
        <result property="authorName" column="ba"/>
        <result property="title" column="bt"/>
    </resultMap>
~~~



## 多对一

### 嵌套查询

~~~xml
<select id="getStudents" resultMap="StudentTeacher">
        select * from student;
    </select>
    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
<!--        复杂的属性需要单独处理-->
<!--        对象使用association-->
<!--        集合使用collection-->
        <!-- 通过结果中的一个字段的值去执行另一个select，把select的结果作为实体类的一个属性，这个select的结果一般对应着属性的类 -->
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
<!--        <collection property=""-->
    </resultMap>
    <select id="getTeacher" resultType="Teacher">
        select * from teacher where id=#{id};
    </select>
~~~



### 结果嵌套

~~~xml
<select id="getStudents2" resultMap="StudentTeacher2">
    select s.id sid,s.name sname,t.name tname
    from student s,teacher t
    where s.tid=t.id;
</select>
<resultMap id="StudentTeacher2" type="Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <!-- 联表把所有需要的字段都查出来，对于属性Teacher，把对应的结果字段映射到他的属性的字段上 -->
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
~~~





## 一对多

### 嵌套查询

students属性的类型为List<Student>

~~~xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from teacher where id = #{id}
</select>
<resultMap id="TeacherStudent2" type="Teacher">
    <!-- 集合类型用collection -->
    <collection property="students" ofType="Student" column="id" select="getStudent"/>
</resultMap>
<select id="getStudent" resultType="Student">
    select * from student where tid = #{id};
</select>
~~~





### 结果嵌套

~~~xml
	<select id="getTeacher" resultMap="TeacherStudent">
        select t.id tid,t.name tname,s.id sid,s.name sname
        from teacher t,student s
        where t.id = s.tid and t.id = #{id}
    </select>
    <resultMap id="TeacherStudent" type="Teacher">
        <result property="id" column="tid"/>
        <result property="name" column="tname"/>
<!--        集合要用collection，用ofType表示集合中的类型-->
        <collection property="students" ofType="Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
        </collection>
    </resultMap>
~~~





~~~java
    <resultMap id="ProductAttr" type="com.mi.sc.sop.model.Product">
        <result property="productCode" column="productCode"/>
        <result property="productName" column="productName"/>
        <!--        集合要用collection，用ofType表示集合中的类型-->
        <collection property="attrVals" ofType="com.mi.sc.sop.model.AttributeVal">
            <result property="attrId" column="sid"/>
            <result property="attrCode" column="attrCode"/>
            <result property="attrValCode" column="attrValCode"/>
        </collection>
    </resultMap>

<!--    <select id="listProduct2" resultType="com.mi.sc.sop.model.Product">-->
    <select id="listProduct2" resultMap="ProductAttr">
        SELECT
            product.id pid,
            product.product_code productCode,
            product.product_name productName,
            product.service_line_id serviceLineId,
            product.STATUS,
            product.delete_reason deleteReason,
            product.create_time createTime,
            product.create_user createUser,
            product.update_time updateTime,
            product.update_user updateUser,
            sa.attr_id attrId,
            sa.attr_code attrCode,
            attrval.attr_val_name attrValCode
        FROM
-- 	sc_product product
            <choose>
                <when test="page != null and page.thisPage != null and page.pageSize != null">
                    (SELECT * FROM sc_product WHERE
                    <if test="query.serviceLineId!=null">
                        AND sc_product.service_line_id=#{query.serviceLineId}
                    </if>
                    AND sc_product.tenant_code = '1' 
                    LIMIT #(query.page.startNumber), #(query.page.pageSize)) product
                </when>
                <otherwise> sc_product sp </otherwise>
            </choose>
    LEFT JOIN sc_product_attr_val_rela rela ON product.id = rela.product_id
    AND rela.tenant_code = '1'
    LEFT JOIN sc_attribute_val attrval ON rela.attr_val_id = attrval.id
    AND attrval.tenant_code = '1'
    LEFT JOIN sc_attribute sa ON sa.id = attrval.attr_id
    AND sa.tenant_code = '1'
    LEFT JOIN sc_product ON sc_product.id = product.id
        WHERE
            1 = 1
        <if test="query.appendSql!=null">
            ${query.appendSql}
        </if>
          AND product.service_line_id = 1
          AND product.tenant_code = '1'
-- 	AND product.id IN ( SELECT id FROM sc_product WHERE sc_product.service_line_id = 1 AND sc_product.tenant_code = '1' LIMIT 20, 10 )
    </select>

~~~









# 动态SQL

可以根据条件动态的拼接sql语句



## IF

~~~xml
	<select id="qureyBlogList" resultMap="blogtype">
        select b.id bi,b.authorname ba,b.title bt,b.content bc,b.firstpicture bfp,b.summary bs,b.typeid bti,b.flag bf,b.recommend br,b.published bp,t.name tn,b.create_time bcr,b.update_time bu,b.views bv from blog b left join type t on t.id=b.typeid
        
        <if test="typeId!=null">
            and b.typeid = #{typeId}
        </if>

        
    </select>
~~~

如果map中存在typeId的key，就执行拼接



## where

用<where></where>标签包裹，如果存在拼接就自动添加where，如果没有拼接就不加where。就是为了防止自己写where但是后面没有过滤条件导致错误。

## choose

~~~xml
<choose>
    <when test="id != null">
        id = #{id}
    </when>
    <when test="typeId != null">
        typeId = #{typeId}
    </when>
</choose>
~~~

其中用when，满足其中一个就不执行后面的拼接了。



## set

~~~xml
<update id="uodateBlog" parameterType="map">
    update blog
    <set>
    	<if test="title != null">
        	title = #{title},
        </if>
        <if test="author != null">
        	author = #{author},
        </if>
    </set>
</update>
~~~

更新的时候自动删除最后的“，”，还要保证至少有一个更新字段，否则出错







## foreach

~~~xml
<select id="qureyBlogList" resultMap="blogtype">
        select b.id bi,b.authorname ba,b.title bt,b.content bc,b.firstpicture bfp,b.summary bs,b.typeid bti,b.flag bf,b.recommend br,b.published bp,t.name tn,b.create_time bcr,b.update_time bu,b.views bv from blog b left join type t on t.id=b.typeid
    	<!-- 如果传入的map中有名为ids的key，就会在后面添加if里面的内容 -->
        <if test="ids!=null">
            <!-- 对于value为集合，自定index表示迭代的位置，自定item表示集合中的每一个元素，open表示以什么字符串开始，close表示以什么字符串结束，separator表示每个表达式中间用什么间隔 -->
            <foreach  collection="ids" index="index" item="item" open="and b.id in (" close=")" separator=",">
                #{item}
            </foreach>
        </if>
        

        
    </select>
~~~



对于collection有三种情况

- 对于单参数且List类型：collection的值为list
- 对于单参数且Array类型：collection的值为array
- 对于用Map类型传递多参数（想遍历map中的一个key对应的集合）：collection的值为这个集合的key名



# 重要概念

- SqlSession：表示和数据库的一次会话，向用户提供了操作数据库的方法
- MappedStatement：表示要发往数据库执行的指令，可以理解为是SQL的嘲笑表示
- Executor：具体和数据库交互的执行器，接收MappedStatement作为参数
- 映射接口：接口中的方法表示要执行的SQL，具体SQL在映射文件中表示
- 映射文件：MyBatis编写SQL的文件，通常每个表对应一个映射文件



# 获取主键



