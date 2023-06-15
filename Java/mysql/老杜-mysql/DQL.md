[toc]



# DQL: 查询语句
## 单表查询

### 简单查询

- 查询一个字段

  `select 字段名 from 表名;`

  其中要注意：**select 和 from 都是关键字，字段名和表名都是标识符**

- 查询多个字段

  `select 字段1,字段2,字段3 from 表名;`

  **使用逗号隔开**

- 查询所有字段

  - 可以把所有字段都写上

  - `select * from 表名;`

    这种方式的缺点是效率低，可读性差，在实际开发中不建议

#### 给列起别名

`select 字段1,字段2 as 别名 from 表名;`

使用 as 关键字起别名

- 注意：只是将显示的查询结果列名显示为 别名，原表列名该叫什么还是叫什么

- 记住：select 语句是永远都不会进行修改操作的

- 省略 as 关键字：可以省略 使用 空格 代替

  `select 字段1,字段2 别名 from 表名;`

- 假如别名里面有空格，这时候会报错

  解决办法：可以使用引号把别名括起来

  注意：在所有的数据库当中，字符串统一使用单引号括起来，单引号是标准，双引号在 `oracle` 数据库中用不了，但是在 `mysql` 中可以使用

#### 列参与数学运算

`select 字段1,字段2*12 from 表名;`

字段可以使用数学表达式

### 条件查询

不是将表中所有数据都查出来,是查询出来符合条件的

#### 语法格式

`select 字段 from 表名 where 条件 `

| 条件                  | 含义                                  |
| --------------------- | ------------------------------------- |
| =                     | 等于                                  |
| <> 或 !=              | 不等于                                |
| <                     | 小于                                  |
| <=                    | 小于等于                              |
| >                     | 大于                                  |
| >=                    | 大于等于                              |
| `between ... and ...` | 两个值之间,等同于 `>= and <=`         |
| `is null`             | 为 `null`  ( `is not null` 不为空 )   |
| and                   | 并且                                  |
| or                    | 或者                                  |
| in                    | 包含                                  |
| not                   | not 可以取非,主要用在 `is` 或 `in` 中 |
| like                  | 模糊查询,支持 `%` 或 `_` 匹配         |
| %                     | 匹配任意个字符                        |
| 下划线                | 一个下划线只匹配一个字符              |

注意:

- 数据库中 `null` 不能使用 `=` 来衡量,需要使用 `is null`

- `and` 和 `or` 同时出现时, `and` 的优先级比 `or` 高, 这时可以通过加 `()` 解决

- `in`不是一个区间 `in ` 后面跟的是具体的值,例如:

  查询一个薪资是 800 和 5000 的员工

  `select ename,sal from emp where sal in(800, 5000)`

### 模糊查询

`like` 称为模糊查询,支持 `%` 或 `_` 匹配

- `%`：匹配任意多个字符
- `_`：任意一个字符

#### 例子

- 找出名字中带有 o 的

  `select ename from emp where ename like %o%`

- 找出名字以 T 结尾的

  `select ename from emp where ename like %T`

- 找出名字以 K 开始的

  `select ename from emp where ename like K%`

- 找出第二个字母是 A 的

  `select ename from emp where ename like _A%`

- 找出第三个字母是 R 的

  `select ename from emp where ename like __R%`

- 找出名字中带有下划线的

  `select ename from emp where ename like %\_%`

  `\`：转义字符

### 排序

语法：

`select 字段 from 表名 order by 规则`

#### 根据单个字段排序

- `select 字段 from emp order by sal`：默认升序
- `select 字段 from emp order by sal desc`：指定降序
- `select 字段 from emp order by sal asc`：指定升序

#### 根据多个字段排序

- 按两个字段排序，查询员工名字和薪资，要求使用薪资升序，如果薪资一样的话，再按照名字升序排列

  `select ename,sal from emp order by sal asc, ename asc`：sal 再前面，起主导作用，只有 sal 相等的时候，才会考虑使用 ename 排序

#### 根据字段位置排序：了解，不建议写

`select ename,sal from emp order by 2`：2 表示第二列，第二列是 sal

按照查询结果的 第 2 列 sal 排序

不建议在开发中这样写，因为不健壮。因为列的顺序很容易发生改变，列顺序修改之后，这个 SQL 语句就没有用了

### 综合案例

- 找出工资再 1250 到 3000 之间的员工信息，要求按照薪资降序排列

  `select ename,sal from emp where sal between 1250 and 3000 order by desc`

  **关键字顺序不能变**：`select ... from ... where ... order by ...`

### 数据处理函数

- 数据处理函数又称为 单行处理函数，单行处理函数的特点是：一个输入对应一个输出
- 和单行处理函数相对的是 多行处理函数，多行处理函数的特点是：多个输入，对应一个输出

| 函数名称                                        | 作用                                                         |
| ----------------------------------------------- | ------------------------------------------------------------ |
| `Lower`                                         | 转换小写                                                     |
| `upper`                                         | 转换大写                                                     |
| `substr`                                        | 取子串 ( `substr(被截取的字符串，起始下标，截取的长度)` )，注意：MySQL的起始下标从 1 开始 |
| `concat`                                        | 进行字符串的拼接                                             |
| `length`                                        | 取长度                                                       |
| `trim`                                          | 去空格                                                       |
| `str_to_date`                                   | 将字符串转换成日期                                           |
| `date_format`                                   | 格式化日期                                                   |
| `format`                                        | 设置千分位                                                   |
| `round`                                         | 四舍五入                                                     |
| `rand`                                          | 生成随机数                                                   |
| `ifnull`                                        | 可以将 `null` 转换成一个具体值，`ifnull(数据，被当作哪个值)` |
| `case...when...then...when...then...else...end` | 当什么时候该干什么 ( 类似 `switch` )                         |

#### 数字，日期的格式化

- 数字格式化：`format(数字，格式)`

  `select ename,format(sal, '$999.999') from emp`

- 字符串转换日期：`str_to_date` 将字符串 varchar 类型转换成 date 类型

  * `create table t_user (id int, name varchar(32), birth date)`

    `insert into t_user(id,name,birth) values(1, '张三', '01-10-1990')`

    这里使用这个插入语句会报错，因为类型不匹配，数据库的 birth 是 date 类型，而这里给了一个字符串类型

    这里就可以使用 str_to_date 函数

     语法格式：`str_to_date('字符串日期','日期格式')`

  * `insert into t_user(id,name,birth) values(1,'张三',str_to_date('01-10-1990','%d-%m-%Y'))`

    **但是如果这里的 字符串是 '1990-10-01' 那么就不用 str_to_date 这个函数，会自动进行转换**

  * `insert into t_user(id,name,birth) values(2,'李四','1990-10-01')`

  #### mysql 日期格式

  - `%Y`：年
  - `%m`：月
  - `%d`：日
  - `%h`：时
  - `%i`：分
  - `%s`：秒

- 日期转换字符串：`date_format` 将 date 类型转换成具有一定格式的 varchar 字符串类型

  想要查询时以某个特定的日期格式展示，可以使用 `date_format`

  语法：`select id,name,date_form(birth, '%m/%d/%Y') from emp`

  `select id,name,birth from emp`

  **这个 SQL 语句实际上是进行了默认的日期格式化，自动将数据库中的 date 类型转换成 varchar 类型，并且采用的格式是 mysql 默认的日期格式 '%Y-%m-%d'**

#### 案例

- 将员工名称转换成小写

  `select lower(ename) as enmae from emp`

- 将员工名称转换成大写

  `select upper(ename) as ename from emp` 

- 找出员工名字第一个字母是 A 的员工信息

  `select ename from emp where substr(ename 1, 1) = 'A'`

- 首字母大写

  `select concat(upper(substr(ename, 1, 1)), substr(name, 2, lengt(name) - 1)) as result from emp`
  
- `select round(1236.577,0) as result from emp` 

  **round第二个参数 0 表示保留到整数位，1 表示保留一位小数，-1 表示保留到十位**

- 当员工的工作岗位是 `MANAGER` 的时候，工资上调 `10%` , 当工作岗位是 `SALESMAN` 的时候，工资上调 `50%`

  `select ename,job,sal as oldSal,(case job when 'MANAGER' then sal * 1.1 when 'SALESMAN' then sal-1.5 else sal end) as newSal from emp`

### 分组函数 ( 多行处理函数 )

多行处理函数的特点：输入多行最终输出一行

| 作用       | 函数名  |
| ---------- | ------- |
| 取得记录值 | `count` |
| 求和       | `sum`   |
| 取平均数   | `avg`   |
| 取最大的数 | `max`   |
| 取最小的数 | `min`   |

分组函数再使用的时候必须先进性分组，然后才能使用，如果没有对数据进行分组，整张表默认为一组

#### 案例

- 找出最高工资

  `select max(sal) from emp`

- `计算工资和`

  `select sum(sal) from emp`

- 计算平均工资

  `select avg(sal) from emp`

- 计算员工数量

  `select count(ename) from emp`

#### 分组函数使用注意事项

- 分组函数自动忽略 null，不需要提前对 null 进行处理

- 分组函数中 `count(*)` 和 `count(具体字段)` 的区别

  - `count(*)`：统计表当中的总行数。( 只要有一行记录数据 count 则++ )，因为每一行记录不可能都为 null，一行数据中有一列不为 null，则这行数据就是有效的
  - `count(具体字段)`：表示统计该字段下所有不为 null 的元素的总数

- 分组函数不能够直接使用在 `where` 子句中

  因为分组函数再使用的时候必须先分组之后才能使用，`where` 执行的时候，还没有分组，所以 `where` 后面不能出现分组函数

  `select sum(sal) from emp` 这个没有分组，为啥 `sum()` 函数可以调用呢，是因为 `select` 在 `group by` 之后执行

-  所有的分组函数可以组合起来一起用

  `select sum(sal),min(sal),max(sal),avg(sal),count(*) from emp`

#### 分组查询

##### 介绍

在实际的运用中，可能有这样的需求，需要先进行分组，然后对每一组的数据进行操作。这个时候我们就需要使用分组查询

##### 语法

`select ... from ... group by...`

##### 使用场景

- 计算每个部门的工资和
- 计算每个工作岗位的平均薪资
- 计算每个工作岗位的最高薪资

##### having

使用 having 可以对分完组之后的数据进一步过滤，having 不能单独使用，having 不能代替 where，hacing 必须和 group by 联合使用

- 找出每个部门最高薪资，要求显示最高薪资大于 3000 的

  `select deptno,max(sal) from emp group by deptno having max(sal) > 3000`

  这样写效率低，我们可以先找出最高薪资大于 3000 的再进行分组

  `select deptno,max(sal) where sal > 3000 group by deptno`

###### 优化策略：where 和 having，优先选择 where，where 实在完成不了了，再选择 having

- 找出每个部门平均薪资，要求显示平均薪资高于 2500 的，这时就是用不了 where

  `select deptno,avg(sal) from emp group by deptno having avg(sal) > 2500`

##### 案例

- 找出每个工作岗位的工资和

  实现思路：按照工资岗位分组，然后对工资求和

  `select job,sum(sal) from emp group by job`

  **这时就不能在加上 ename 了， 因为 ename 并没有进行分组**

- 找出每个部门的最高薪资

  `select deptno,max(sal) from emp by group by deptno`

- 找出每个部门不同工作岗位的最高薪资

  `select deptno,job,max(max) from emp group by deptno,job`

### 注意

- 以上语句的执行顺序
  1. `from`
  2. `where`
  3. `group by`
  4. `having`
  5. `select`
  6. `order by`：排序总在最后执行
- 一般来说 `select` 后面跟的是一个字段名，但是如果跟的是一个数据的话，会把这个值根据表的长度返回这个表长度的值
- 在数据库当中，只要有 Null 参与的数学运算，最终结果就是 Null
- 在一条 `select` 语句当中，如果有 `group by` 语句的话， `select` 后面只能有
  - 参加分组的字段
  - 分组函数

### distinct ---> 去除重复记录

**把查询结果去除重复记录，distinct 只能出现在所有字段的最前方**

- `select distinct job from emp`：查询工作岗位并去重

- `select distinct ename,job from emp`：查询 名称和工作岗位并进行 联合去重

统计工作岗位的数量

- `select count(distinct job) from emo`

### 综合案例

- 找出每个岗位的平均薪资，要求显示平均薪资大于 1500 的，除 `MANAGER` 岗位之外，要求按照平均薪资降序排列

  `select job,avg(sal) from emp where jon <> 'MANAGER' group by job having avg(sal) > 1500 order by avg(sal) desc`

### 总结

- 书写顺序：

  `select ... from ... where ... group by ... having ... order by ...`：不能颠倒，没用到可以不写

  **从某张表中查询数据，先经过 where 条件筛选出有价值的数据，对这些有价值的数据进行分组，分组之后可以使用 having 继续筛选，select 查询出来，最后排序输出**

## 连接查询

### 什么是连接查询

- 从一张表中单独查询，称为单表查询
- `emp` 表和 `dept`表联合起来查询数据，从 `emp` 表中取员工名字，从 `dept` 表中取部门名字，这种跨表查询，多张表联合起来查询数据，被称为连接查询

### 连接查询的分类

#### 根据年代分类

- `SQL92`：1992 年的时候出现的语法
- `SQL99`：1999 年的时候出现的语法

这里重点学习 `SQL99` 

#### 根据表连接的方式分类

- 内连接
  - 等值连接
  - 非等值连接
  - 自连接
- 外连接
  - 左外连接 ( 左连接 )
  - 右外连接 ( 右连接 )
- 全连接 ( 几乎不用 )

### 笛卡尔积现象

当两张表进行连接查询时，没有任何条件的限制会发生什么现象

案例：查询每个员工所在的部门名称

- 两张表连接没有条件限制

  `select ename,dname from emp,dept `

  当两张表进行连接查询，没有任何条件限制的时候，最终查询结果条数，是两张表条数的乘积，这种现象被称为笛卡尔积现象，( 笛卡尔发现的，这是一个数学现象 )

**要想避免笛卡尔积现象，连接时需要加条件，满足这个条件的的记录i被筛选出来**

- 两张表连接有条件限制

  `select ename,dname from emp,dept where emp.deptno = dept.deptno`

  这样 select 后的两个字段再两个表中都会进行查询，效率不高，所以加上表名

  `select emp.ename,dept.dname from emp,dept where emp.deptno = dept.deptno`

  这样写法很不优雅，所以也可以对 表 起一个别名

  `select e.ename, d.dname from emp e,dept d where e.deptno = d.deptno`：这是 `SQL92`的语法

  注意：**最终查询的结果条数是 xx 条，但是匹配的过程中，匹配的次数并没有减少**

 **通过笛卡尔积现象得出，表的连接次数越多效率越低，尽量避免表的连接次数**

### 内连接

特点：**将完全能够匹配上条件的数据查询出来**

#### 等值连接

**条件是等值关系，称为等值连接**

**查询每个员工所在部门名称，显示员工名和部门名**

- SQL92 的语法: `select e.ename,d.dname from emp e, dept d where e.deptnp = d.deptnp`

  **结构不清晰，表的连接挑战和后期进一步筛选的条件，都放到了 where 后面**

- SQL99 的语法：`select e.ename,d.dname from emp e join dept d on e.deptno = d.deptno`

  **表连接的操作是独立的，连接之后，如果还需要进一步筛选。再往后继续添加 where **

##### SQL99 等值连接语法

`select 字段1，字段2... from 表1 inner join 表2 on 连接条件`

`inner`可以省略，带着 `inner` 可读性更好，一眼可以看出来是内连接

#### 非等值连接

**条件不是一个等量关系，称为非等值连接**

**找出每个员工的薪资等级，要求显示员工名、薪资、薪资等级**

`select e.ename,e.sal,s.grade from emp e inner join salgrade s on e.sal between s.losal and s.hisal`

#### 自连接

**自己和自己连接**

**查询员工的上级领导，要求显示员工名和对应的领导名**

技巧：一张表看成两张表

`select e1.ename,e2.ename from emp e1 inner join emp e2 on e1.mgr = e2.empno`

### 外连接

#### 与内连接的区别

- 外连接中，两张表连接，产生了主次关系
- 内连接中，两张表是平等的，没有主次关系

#### 右外连接 ( 右连接 )

**查询每个员工所在部门名称，显示员工名和部门名**

`select e.ename,d.dname from emp e right outer join dept d on e.deptnp = d.deptno`

- `right`：表示将 join 关键字右边的这张表看成主表，主要是为了将这张表的数据全部查询出来，捎带着关联查询左边的表

- `outer`：同内连接的 inner

#### 左外连接 ( 左连接 )

问题同上

`select e.ename,d.dname from dept d left outer join emp e on e.deptnp = d.deptno`

- 查询每个员工的上级领导，要求显示所有员工的名字和领导名

  `select e1.ename,e2.ename from emp e1 left outer join emp e2 on el.mgr= e2.empno`

### 三张表、四张表连接

#### 语法

`select ... from a join b on a 和 b 的连接条件 join c on a 和 c 的连接条件 right join d on a 和 d 的连接条件`

**一条SQL中内连接和外连接可以混合，都可以出现**

- **找出每个员工的部门名称，以及工资等级，要求显示员工名、部门名、薪资、薪资等级**

~~~sql
select 
	e.ename,e.sal,d.dname,s.grade 
from 
	emp e 
join 
	dept d 
on 
	e.deptno = d.deptno 
join 
	salgrade s 
on 
	e.sal between s.losal and s.hisal
~~~

- **找出每个员工的部门名称，还有上级名称，以及工资等级，要求显示员工名、领导名、部门名、薪资、薪资等级**

~~~sql
select 
	e1.ename,e1.sal,e2.ename,d.dname,s.grade 
from 
	emp e1
join 
	dept d 
on 
	e.deptno = d.deptno 
join 
	salgrade s 
on 
	e.sal between s.losal and s.hisal
left join 
	emp e2
on 
	e1.mgr = e2.empno
~~~

### 子查询

#### 什么是子查询

**select 语句中嵌套 select 语句，被嵌套的 select 语句被称为子查询**

#### 子查询都可以出现在哪些地方

- select 后面
- from 后面
-  where 后面

#### where 子句中的子查询

- 找出比最低工资高的员工姓名和工资

  实现思路

  1. 查询最低工资是多少

     `select min(sal) from emp`

  2. 找出 > 800 的

     `select ename,sal from emp where sal > 800`

  3. 合并

     `select ename,sal from emp where sal > (select min(sal) from emp)` 

#### from 子句中的子查询

技巧：**from 后面的子查询，可以将子查询的查询结果当做一张临时表**

**找出每个工作岗位的平均工资的薪资等级**

实现思路：

1. 找出每个岗位的平均工资

   `select job,avg(sal) from emp group by job`

2. 将上一步查询结果当做一个临时表

   `select t.*,s.grade from (select job,avg(sal) as salgrade from emp group by job) t join salgrade s on t.salgrade between  s.losal and s.hisal`

#### select 子句中的子查询 ( 了解即可 )

找出每个员工的部门名称，要求显示员工名，部门名

~~~sql
select
	e.ename,(select d.dname from dept d where e.deptno = d.deptno) as dname
from 
	emp e;
# 对于 select 后面的子查询来说，这个子查询只能一次返回一条结果，多余一条就报错
~~~

### union 合并查询结果集

- 查询工作岗位是 MANAGER 和 SALESMAN 的员工
  - `select ename,job from emp where job = 'MANAGER' or job = 'SALESMAN'`
  - `select ename,job from emp where job in('MANAGER', 'SALESMAN ')`
  - `select ename,job from emp where job = 'MANAGER' union select ename,job from emp where job = 'SALESMAN'`

**union 的效率要高一些，对于表连接来说，每连接一次新表，则匹配的次数满足笛卡尔积，成倍的翻，但是 union 可以减少匹配的次数。在减少匹配次数的情况下，还可以完成两个结果集的拼接**

假如有 a，b，c 三张表，假如每个表的数据都是 10 条，a 连接 b 连接 c

- 不使用 union：匹配 1000 次
- 使用 union：a 连接 b 100 次 加上  a 连接 c 100 次 就是 200 次

#### 注意事项

- union 在进行结果集合并的识货，要求两个结果集的列数相同

  **mysql 中只要列数相同就可以，而 oracle 不可以，要求集合合并时列和列的数据类型也要一致**

### limit 分页查询

#### limit 的作用

**limit 是将查询结果集的一部分查询出来，通常使用在分页查询当中，分页的作用是为了提高用户的体验，因为一次全部都查出来，用户体验差，可以一页一页翻页看**

#### limit 语法

- 完整用法：`limit startIndex length`

  - startIndex：起始下标，从 0 开始
  - length：长度

- 缺省用法：`limit 5`：这是取前 5

- 按照薪资降序，取出排名在前 5 名的员工

  `select ename,sal from emp order by sal desc limit 5`：取前 5 个

注意：mysql 当中 limit 在 order by 之后执行

- 取出工资排名在 3 到 5 名的员工

  `select ename,sal from emp order by sal desc limit 2 3`

#### 通用分页

- 每页显示 3 条数据

  - 第一页：limit 0,3	[0, 1, 2]
  - 第二页：limit 3,3    [3, 4, 5]
  - 第三页：limit 6,3    [6, 7, 8]
  - 第四页：limit 9,3    [9, 10, 11]

  每页显示 pageSize 条记录

##### 分页公式

  ​	第 n 页：limit ( n - 1 ) * pageSize, pageSize

## 大总结

### 语法

~~~sql
select 
	...
from
	...
where
	...
group by
	...
having
	...
order by
	...
limit
	...
~~~

### 执行顺序

1. from
2. where
3. group by
4. having
5. select 
6. order by
7. limit.