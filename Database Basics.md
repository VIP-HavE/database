>在数据库的使用过程中，大概可以分为数据库的初级操作、中级操作、高级操作
>初级操作即为查询、增加、删除、更新
>中级操作即为联表操作与事务操作
>高级操作为性能优化

由此，开始数据的初级操作
# 一、大致使用
## 1、数据库操作
### 1）、创建数据库CREATE
在开始，我们首先创建一个数据库

```sql
CREATE TABLE `user_test` (
  `id` 			bigint(20)	 unsigned NOT NULL AUTO_INCREMENT COMMENT '主键id',
  `user_name` 	varchar(255) DEFAULT NULL COMMENT '用户名',
  `pass_word` 	varchar(255) DEFAULT NULL COMMENT '密码',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2424797 DEFAULT CHARSET=utf8 COMMENT='用户信息表'
```
运行结果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/37a7cfed73cb4ae7a1fcbf016bb48f51.png)
那如果创建的数据库我们不满意呢，最简单的即为直接销毁
### 2）、删除数据库DROP
```sql
drop table user_test 
```
如果创建的数据库暂时是满意的，那么可以对数据库的内容进行操作
现在开始操作
## 2、数据库内容操作
### 1）、增加 INSERT
现在将数据库表user_test中添加三条信息，分别为
<table>
	<tr>
	    <th>id</th>
	    <th>user_name</th>
	    <th>pass_word</th>  
	</tr >
	<tr >
	    <td>1</td>
	    <td>name1</td>
	    <td>password1</td>
	</tr>
	<tr>
	    <td>2</td>
	    <td>name2</td>
	    <td>password2</td>
	</tr>
	<tr>
	    <td>3</td>
	    <td>name3</td>
	    <td>password3</td>
	</tr>
</table>
那么可以使用INSET语句进行如下操作

```sql
INSERT INTO test.user_test(id, user_name, pass_word)VALUES(1, 'name1', 'password1');
INSERT INTO test.user_test(id, user_name, pass_word)VALUES(2, 'name2', 'password2');
INSERT INTO test.user_test(id, user_name, pass_word)VALUES(3, 'name3', 'password3');
```
当前，也可以接在VALUES后面一次性写完

```sql
INSERT INTO 
	test.user_test(id, user_name, pass_word)
VALUES
	(1, 'name1', 'password1'), (2, 'name2', 'password2'), (3, 'name3', 'password3');
```

### 2）、查询 SELECT
无论是对数据库内容如何的操作，查询都是我们必要的验证方式之一 
1、整体的查询
```sql
SELECT * FROM test.user_test;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/b9e2bc56df194d65a83a6bfba52ce0c6.png)

2、具体字段的查询

```sql
SELECT id, user_name, pass_word FROM test.user_test WHERE id=1;
```
3、查询的模糊
>用%代替不确定的部分
```sql
SELECT * FROM test.user_test WHERE user_name LIKE 'name%';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/8e5f66cb628a419ea6cb6a830bdf8ba2.png)
LIKE 的用法只是用于表示泛指，并不只是在查询中可用，但是建议只在查询中使用，毕竟数据库的安全性是最重要的


### 3）、删除 DELETE

比如我们想删除字段user_name 为name3的这条数据，可以如下操作

```sql
DELETE FROM test.user_test WHERE user_name = 'name3';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/159efd1f3c344f298b1c9117b5dc7d2c.png)




### 4）、更新 UPDATE
比如我们要更新user_name 为name2时的pass_word内容为password222

```sql
UPDATE test.user_test SET pass_word='password222' WHERE user_name='name2';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4e430790ff9c417f842d7ae0da88ab10.png)
# 二、一些使用的方式
为了方便演示，将表中数据增加为如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/bd7e5563d5dd4d6084ee287605a3e0c8.png)

## 1、排序order by
在使用order by进行排序操作时，默认情况下完成的是升序的操作，
* asc:是默认的排序方式，表示升序
* desc：降序的排序方式

排序的方式是按照自然顺序进行排序的
如果是数值，那么按照从大到小
如果是字符串，那么按照字典序排序

在进行排序的时候可以指定多个字段，而且多个字段可以使用不同的排序方式

每次在执行order by的时候相当于是做了全排序，思考全排序的效率
会比较耗费系统的资源，因此选择在业务不太繁忙的时候进行
## 2、并集
将两个集合中的所有数据都进行显示，但是不包含重复的数据

```sql
select * from test.user_test where id < 7 union
select * from test.user_test where id > 2;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/bd7e5563d5dd4d6084ee287605a3e0c8.png)

## 3、全集
将两个集合的数据全部显示，不会完成去重的操作

```sql
select * from test.user_test where id < 7 union all
select * from test.user_test where id > 2;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/deddd3fd5b9b4075ab2a75f6fe2c8b95.png)

## 4、交集
两个集合中交叉的数据集，只显示一次

```sql
select * from test.user_test where id < 7 intersect 
select * from test.user_test where id > 2;
```

## 5、差集
包含在A集合而不包含在B集合中的数据，跟A和B的集合顺序相关

```sql
select * from test.user_test where id < 7 minus 
select * from test.user_test where id > 2;
```