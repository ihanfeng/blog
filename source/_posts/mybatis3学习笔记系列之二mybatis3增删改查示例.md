title: "MyBatis学习笔记系列之二：MyBatis增删改查示例"
id: 883
date: 2013-06-19 11:06:29
tags: 
- mybatis
- mybatis增删改查
categories: 
- mybatis
---

在系列一的基础上，我们实现mybatis的增删改查操作。查询操作在上一节已经实现，可以先去看看哈。

1、为了方便地获取`SqlSessionFactory`实例，先写一个工具类`SqlSessionFactoryGen`，用以生成SqlSessionFactory实例，代码如下：
<!-- more -->
{%codeblock lang:java%}
package com.hanfeng.demo.utils;

import java.io.Reader;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

/**
* 工具类SqlSessionFactoryGen，用以生成SqlSessionFactory实例
* @author hanfeng
*
*/
public class SqlSessionFactoryGen {
 private static SqlSessionFactory factory;
 //静态代码块。在类初始化时被执行，如第一次
 //引用类的静态变量，创建类的第一个实例
 static{
 //与configuration.xml中的mapper配置类似，告诉MyBatis应读取的核心配置文件
 String resource = &quot;main/resources/config/configuration.xml&quot;;
 Reader reader = null;
 try {
 reader = Resources.getResourceAsReader(resource);
 } catch (Exception e) {
 e.printStackTrace();
 }
 factory = new SqlSessionFactoryBuilder().build(reader);
 }

public static SqlSessionFactory getSqlSessionFactory()
 {
 return factory;
 }
}
{%endcodeblock%}

由于整个程序只需要一个SqlSessionFactory实例，因此通过调用SqlSessionFactoryGen的`getSqlSessionFactory()`方法获取的是同一个SqlSessionFactory实例。

2、新建执行测试类StudentManagerTest

{%codeblock lang:java%}
package com.hanfeng.demo.pojo.test;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import com.hanfeng.demo.dao.StudentMapper;
import com.hanfeng.demo.pojo.Student;
import com.hanfeng.demo.utils.SqlSessionFactoryGen;
public class StudentManagerTest {
 //获取SqlSessionFactory实例
 private static SqlSessionFactory factory = SqlSessionFactoryGen.getSqlSessionFactory();
 public static void main(String[] args) {
 }
}
{%endcodeblock%}

我们会在测试类的基础上逐步增加方法哈，查询方法见上一节。

3、增加方法
首先在接口`StudentMapper`中声明执行增加、编辑、删除操作的方法，代码如下所示：

{%codeblock lang:java%}
//添加新的学生
 public void add(Student student);
 //编辑学生信息
 public void update(Student student);
 //根据ID删除信息
 public void delete(int id);
{%endcodeblock%}

接着在`StudentMapper.xml`中编写相应的SQL语句

{%codeblock lang:xml%}
<!—与StudentMapper接口中的getById方法对应，包括方法名和参数类型。SQL语句中以“#{}”的形式引用参数—>
<select id="getById" parameterType="int" resultMap="studentResultMap">
SELECT *
FROM student WHERE id = #{id}
</select>
<!— 新增学生 —>
<!—执行增加操作的SQL语句。id和parameterType分别与StudentMapper接口中的add方法的名字和
参数类型一致。以#{name}的形式引用Student参数的name属性，MyBatis将使用反射读取Student参数
的此属性。#{name}中name大小写敏感。引用其他 的gender等属性与此一致。seGeneratedKeys设置
为"true"表明要MyBatis获取由数据库自动生成的主 键；keyProperty="id"指定把获取到的主键值注入
到Student的id属性—>
<insert id="add" parameterType="Student" useGeneratedKeys="true" keyProperty="id">
insert into student(name,gender,major,grade)values(#{name},#{gender},#{major},#{grade})
</insert>
<!— 编辑学生信息 —>
<!—执行修改操作的SQL语句。id和parameterType属性以及“#{}”的形式的含义与上述insert语句一致。—>
<update id="update" parameterType="Student">
update student set name=#{name},
gender=#{gender},
major=#{major},
grade=#{grade}
where id=#{id}
</update>
<!— 删除学生信息 —>
<delete id="delete" parameterType="int">
delete from student where id = #{id}
</delete>
{%endcodeblock%}

测试类完整代码：

{%codeblock lang:java%}
package com.hanfeng.demo.pojo.test;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import com.hanfeng.demo.dao.StudentMapper;
import com.hanfeng.demo.pojo.Student;
import com.hanfeng.demo.utils.SqlSessionFactoryGen;
public class StudentManagerTest {
 //获取SqlSessionFactory实例
 private static SqlSessionFactory factory = SqlSessionFactoryGen.getSqlSessionFactory();

 //实现增加学生
 public void add(){
 SqlSession sqlSession = factory.openSession();
 try {
 Student student = new Student();
 student.setName(&quot;陈静&quot;);
 student.setGender(&quot;女&quot;);
 student.setMajor(&quot;计算机科学与技术&quot;);
 student.setGrade(&quot;2008&quot;);

 StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
 mapper.add(student);

 //提交事务，否则无法实际保存到数据库
 sqlSession.commit();

 System.out.println(&quot;数据库生成ID:&quot;+student.getId());
 } catch (Exception e) {
 e.printStackTrace();
 }finally{
 sqlSession.close();
 }
 }
 //根据ID查询学生
 public void QueryStudentById(){
 SqlSession sqlSession = factory.openSession();
 try {
 StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
 Student student = mapper.getById(4);
 if (student != null) {
 System.out.println(&quot;姓名：&quot;+student.getName()+&quot;\n专业：&quot;+student.getMajor());
 }else{
 System.out.println(&quot;没有找到相关记录！&quot;);
 }
 } catch (Exception e) {
 e.printStackTrace();
 }finally{
 sqlSession.close();
 }
 }

 //编辑学生信息
 public void updateInfo(){
 SqlSession sqlSession = factory.openSession();
 try {
 StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
 Student student = mapper.getById(3);
 System.out.println(&quot;修改前信息：&quot;+student.getName()+&quot;--&quot;+student.getGender()+&quot;--&quot;+student.getMajor()+&quot;--&quot;+student.getGrade());
 student.setMajor(&quot;数控技术&quot;);
 mapper.update(student);
 sqlSession.commit();
 System.out.println(&quot;修改后信息：&quot;+student.getName()+&quot;--&quot;+student.getGender()+&quot;--&quot;+student.getMajor()+&quot;--&quot;+student.getGrade());
 } catch (Exception e) {
 e.printStackTrace();
 }finally{
 sqlSession.close();
 }
 }

 //删除学生信息
 public void deleteInfo(){
 SqlSession sqlSession = factory.openSession();
 try {
 StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
 mapper.delete(4);
 System.out.println(&quot;删除成功&quot;);
 sqlSession.commit();
 } catch (Exception e) {
 e.printStackTrace();
 }finally{
 sqlSession.close();
 }
 }
 public static void main(String[] args) {
 StudentManagerTest test = new StudentManagerTest();
 //执行增加学生的方法
// test.add();
 //执行学生查询
// test.QueryStudentById();
 //执行学生信息修改
// test.updateInfo();
 // 执行学生信息删除
 test.deleteInfo();
 }
}

{%endcodeblock%}

源代码：[http://pan.baidu.com/share/link?shareid=1347967662&amp;uk=4161616584](http://pan.baidu.com/share/link?shareid=1347967662&amp;uk=4161616584)