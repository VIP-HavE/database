>>在数据库的使用过程中，大概可以分为数据库的初级操作、中级操作、高级操作
>>初级操作即为查询、增加、删除、更新
>>中级操作即为联表操作与事务操作
>>高级操作为性能优化
>
>由此，开始数据的初级操作
># 一、初步使用
>## 1、数据库操作
>### 1）、创建数据库CREATE
>在开始，我们首先创建一个数据库
>
>```sql
>CREATE TABLE `user_test` (
>  `id` 			bigint(20)	 unsigned NOT NULL AUTO_INCREMENT COMMENT '主键id',
>  `user_name` 	varchar(255) DEFAULT NULL COMMENT '用户名',
>  `pass_word` 	varchar(255) DEFAULT NULL COMMENT '密码',
>  PRIMARY KEY (`id`)
>) ENGINE=InnoDB AUTO_INCREMENT=2424797 DEFAULT CHARSET=utf8 COMMENT='用户信息表'
>```
>运行结果如下：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/37a7cfed73cb4ae7a1fcbf016bb48f51.png)
>那如果创建的数据库我们不满意呢，最简单的即为直接销毁
>### 2）、删除数据库DROP
>```sql
>drop table user_test 
>```
>如果创建的数据库暂时是满意的，那么可以对数据库的内容进行操作
>现在开始操作
>## 2、数据库内容操作
>### 1）、增加 INSERT
>现在将数据库表user_test中添加三条信息，分别为
><table>
>	<tr>
>	    <th>id</th>
>	    <th>user_name</th>
>	    <th>pass_word</th>  
>	</tr >
>	<tr >
>	    <td>1</td>
>	    <td>name1</td>
>	    <td>password1</td>
>	</tr>
>	<tr>
>	    <td>2</td>
>	    <td>name2</td>
>	    <td>password2</td>
>	</tr>
>	<tr>
>	    <td>3</td>
>	    <td>name3</td>
>	    <td>password3</td>
>	</tr>
></table>
>那么可以使用INSET语句进行如下操作
>
>```sql
>INSERT INTO test.user_test(id, user_name, pass_word)VALUES(1, 'name1', 'password1');
>INSERT INTO test.user_test(id, user_name, pass_word)VALUES(2, 'name2', 'password2');
>INSERT INTO test.user_test(id, user_name, pass_word)VALUES(3, 'name3', 'password3');
>```
>当前，也可以接在VALUES后面一次性写完
>
>```sql
>INSERT INTO 
>	test.user_test(id, user_name, pass_word)
>VALUES
>	(1, 'name1', 'password1'), (2, 'name2', 'password2'), (3, 'name3', 'password3');
>```
>
>### 2）、查询 SELECT
>无论是对数据库内容如何的操作，查询都是我们必要的验证方式之一 
>1、整体的查询
>```sql
>SELECT * FROM test.user_test;
>```
>![在这里插入图片描述](https://img-blog.csdnimg.cn/b9e2bc56df194d65a83a6bfba52ce0c6.png)
>
>2、具体字段的查询
>
>```sql
>SELECT id, user_name, pass_word FROM test.user_test WHERE id=1;
>```
>3、查询的模糊
>>用%代替不确定的部分
>```sql
>SELECT * FROM test.user_test WHERE user_name LIKE 'name%';
>```
>![在这里插入图片描述](https://img-blog.csdnimg.cn/8e5f66cb628a419ea6cb6a830bdf8ba2.png)
>LIKE 的用法只是用于表示泛指，并不只是在查询中可用，但是建议只在查询中使用，毕竟数据库的安全性是最重要的
>
>
>### 3）、删除 DELETE
>
>比如我们想删除字段user_name 为name3的这条数据，可以如下操作
>
>```sql
>DELETE FROM test.user_test WHERE user_name = 'name3';
>```
>![在这里插入图片描述](https://img-blog.csdnimg.cn/159efd1f3c344f298b1c9117b5dc7d2c.png)
>
>
>
>
>### 4）、更新 UPDATE
>比如我们要更新user_name 为name2时的pass_word内容为password222
>
>```sql
>UPDATE test.user_test SET pass_word='password222' WHERE user_name='name2';
>```
>![在这里插入图片描述](https://img-blog.csdnimg.cn/4e430790ff9c417f842d7ae0da88ab10.png)
># 二、常用操作方式
>为了方便演示，将表中数据增加为如下
>![在这里插入图片描述](https://img-blog.csdnimg.cn/bd7e5563d5dd4d6084ee287605a3e0c8.png)
>
>## 1、排序order by
>在使用order by进行排序操作时，默认情况下完成的是升序的操作，
>* asc:是默认的排序方式，表示升序
>* desc：降序的排序方式
>
>排序的方式是按照自然顺序进行排序的
>如果是数值，那么按照从大到小
>如果是字符串，那么按照字典序排序
>
>在进行排序的时候可以指定多个字段，而且多个字段可以使用不同的排序方式
>
>每次在执行order by的时候相当于是做了全排序，思考全排序的效率
>会比较耗费系统的资源，因此选择在业务不太繁忙的时候进行
>## 2、去重DISTINCT
>为了方便研究，插入一条与id为8相同的数据，结果如下
>![在这里插入图片描述](https://img-blog.csdnimg.cn/03fffb919c5646f18acba22c10aae03d.png)
>两种方式查看查询user_name为name8的结果
>使用distinct 前
>
>```sql
>SELECT  pass_word FROM test.user_test WHERE user_name = 'name8'
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/e811197cf95346c6b7b2060b858fd6c2.png)
>
>使用distinct 后
>```sql
>SELECT distinct pass_word FROM test.user_test WHERE user_name = 'name8'
>```
>![在这里插入图片描述](https://img-blog.csdnimg.cn/97b244eca5f24e0eaca1df7d7fba1c7c.png)
>## 3、限制LIMIT 
>有时为了一些原因，或许是为了美观，也或许是在大量数据时分批次删除更新避免锁表的发生，我们会需要对当前操作进行一些限制
>
>select * from test.user_test limit 3
>![在这里插入图片描述](https://img-blog.csdnimg.cn/5446a7f0b4174a08922705ceac27a120.png)
>## 4、且AND
>```sql
>SELECT  * FROM test.user_test WHERE user_name = 'name8' and id = '8'
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/00b876d80dc444889400f26c8c5df5ca.png)
>## 5、或OR
>
>```sql
>SELECT  * FROM test.user_test WHERE user_name = 'name8' or id = '8'
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/597c3de81fc34418b7898297222d9cb3.png)
>## 6、并集union
>将两个集合中的所有数据都进行显示，但是不包含重复的数据
>
>```sql
>select * from test.user_test where id < 7 union
>select * from test.user_test where id > 2;
>```
>![在这里插入图片描述](https://img-blog.csdnimg.cn/70b8d645de664366949d1740e552dcfa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oqA5pyv5bCW5YiA,size_20,color_FFFFFF,t_70,g_se,x_16)
>## 7、全集union all
>将两个集合的数据全部显示，不会完成去重的操作
>
>```sql
>select * from test.user_test where id < 7 union all
>select * from test.user_test where id > 2;
>```
>![在这里插入图片描述](https://img-blog.csdnimg.cn/f818787e1ad54c8d916d4c972b283ea7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oqA5pyv5bCW5YiA,size_20,color_FFFFFF,t_70,g_se,x_16)
>
>
>## 8、交集（实验未成功，报错，Mysql不支持）
>两个集合中交叉的数据集，只显示一次
>
>```sql
>select * from test.user_test where id < 7 intersect 
>select * from test.user_test where id > 2;
>```
>
>## 9、差集（实验未成功，报错，Mysql不支持）
>包含在A集合而不包含在B集合中的数据，跟A和B的集合顺序相关
>
>```sql
>select * from test.user_test where id < 7 minus 
>select * from test.user_test where id > 2;
>```
># 三、函数
>## 1、行数COUNT()
>
>```sql
>select COUNT(*) from test.user_test
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/9eef94acbeb140caadb47720810eb47c.png)
>## 2、最大值MAX()
>
>```sql
>select MAX(id) from test.user_test
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/1b3c55965d91463ca8b76976c5d5070a.png)
>
>## 3、最小值MIN()
>
>```sql
>select MIN(id) from test.user_test
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/1f6cb3893f814da796f0fff3423a1416.png)
>
>## 4、平均值AVG()
>select AVG(id) from test.user_test
>![在这里插入图片描述](https://img-blog.csdnimg.cn/053f959c8c194d3db16fac0ed905a322.png)
>
>## 5、求和SUM()
>select SUM(id) from test.user_test
>![在这里插入图片描述](https://img-blog.csdnimg.cn/51a2fff4eb544b749828d6d99e74a63f.png)
>
># 四、不常用操作
>## 1、对表结构的操作
>### 1). 添加列
>
>```sql
>ALTER TABLE user_test
>ADD remarks char(20);
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/2bed02dc6e1245bebf90dfc94881d7c4.png)
>
>### 2). 删除列
>
>```sql
>ALTER TABLE user_test
>DROP COLUMN remarks;
>```
>![在这里插入图片描述](https://img-blog.csdnimg.cn/4f12d24a70354b2e92d7b5318f98805a.png)
>## 2、导表
>
>```sql
>CREATE TABLE newtable AS SELECT * FROM user_test;
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/bf32c820651045dbbea6fa3b0bf0fc73.png)
>**注意**：经实验发现，此方法不需要重新建表，似乎是对表的直接复制
>## 3、连接字段
>
>```sql
>SELECT CONCAT(user_name, pass_word) AS concat_col FROM user_test;
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/aa8304dc096d479290d001e961e17554.png)
>## 4、去空
>许多数据库会使用空格把一个值填充为列宽，因此连接的结果会出现一些不必要的空格，使用 TRIM() 可以去除首尾空格
>
>```sql
>SELECT CONCAT(TRIM(user_name), '(', TRIM(pass_word), ')') AS example FROM user_test;
>```
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/75ef260907c1435d9fffbf5e86c0b882.png)select * from test.user_test where id < 7 minus 
>select * from test.user_test where id > 2;
