---
layout:       post
title:        "sqlserver常见的问题"
author:       "史密斯"
header-style: text
catalog:      true
tags:
    - C#
    - Bank-end
    - sqlserver
---


数据库三范式是什么？

第一范式：字段不能有冗余信息，所有字段都是必不可少的。

第二范式：满足第一范式并且表必须有主键。

第三范式：满足第二范式并且表引用其他的表必须通过主键引用。

1、索引的作用？和它的优点缺点是什么？

索引就一种特殊的查询表，数据库的搜索引擎可以利用它加速对数据的检索。它很类似与现实生活中书的目录，不需要查询整本书内容就可以找到想要的数据。索引可以是唯一的，创建索引允许指定单个列或者是多个列。缺点是它减慢了数据录入的速度，同时也增加了数据库的尺寸大小。



2、说一下SQLServer中索引的两种类型？

聚簇(或者叫做聚集，cluster)索引和非聚簇索引。

字典的拼音目录就是聚簇(cluster)索引，笔画目录就是非聚簇索引。这样查询“G到M的汉字”就非常快，而查询“6划到8划的字”则慢。

聚簇索引是一种特殊索引，它使数据按照索引的排序顺序存放表中。聚簇索引类似于字典，即所有词条在字典中都以字母顺序排列。聚簇索引实际上重组了表中的数据，所以你只能在表中建立一个聚簇索引。

3、触发器的作用？

触发器是一中特殊的存储过程，主要是通过事件来触发而被执行的。它可以强化约束，来维护数据的完整性和一致性，可以跟踪数据库内的操作从而不允许未经许可的更新和变化。可以联级运算。如，某表上的触发器上包含对另一个表的数据操作，而该操作又会导致该表触发器被触发。

4、什么是事务？什么是锁？

事务就是被绑定在一起作为一个逻辑工作单元的SQL语句分组，如果任何一个语句操作失败那么整个操作就被失败，以后操作就会回滚到操作前状态，或者是上有个节点。为了确保要么执行，要么不执行，就可以使用事务。要将有组语句作为事务考虑，就需要通过ACID测试，即原子性，一致性，隔离性和持久性。

锁：在DBMS中，锁是实现事务的关键，锁可以保证事务的完整性和并发性。与现实生活中锁一样，它可以使某些数据的拥有者，在某段时间内不能使用某些数据或数据结构。当然锁还分级别的。

5、事务是什么？

事务是作为一个逻辑单元执行的一系列操作，一个逻辑工作单元必须有四个属性，称为 ACID（原子性、一致性、隔离性和持久性）属性，只有这样才能成为一个事务：

1)原子性

事务必须是原子工作单元；对于其数据修改，要么全都执行，要么全都不执行。

2)一致性

事务在完成时，必须使所有的数据都保持一致状态。在相关数据库中，所有规则都必须应用于事务的修改，以保持所有数据的完整性。事务结束时，所有的内部数据结构（如 B 树索引或双向链表）都必须是正确的。

3)隔离性

由并发事务所作的修改必须与任何其它并发事务所作的修改隔离。事务查看数据时数据所处的状态，要么是另一并发事务修改它之前的状态，要么是另一事务修改它之后的状态，事务不会查看中间状态的数据。这称为可串行性，因为它能够重新装载起始数据，并且重播一系列事务，以使数据结束时的状态与原始事务执行的状态相同。

4)持久性

事务完成之后，它对于系统的影响是永久性的。该修改即使出现系统故障也将一直保持。

6、什么叫视图？游标是什么？

视图是一种虚拟的表，具有和物理表相同的功能。可以对视图进行增，改，查，操作，试图通常是有一个表或者多个表的行或列的子集。对视图的修改不影响基本表。它使得我们获取数据更容易，相比多表查询。

游标：是对查询出来的结果集作为一个单元来有效的处理。游标可以定在该单元中的特定行，从结果集的当前行检索一行或多行。可以对结果集当前行做修改。一般不使用游标，但是需要逐条处理数据的时候，游标显得十分重要。

7、什么是SQL注入式攻击？

所谓SQL注入式攻击，就是攻击者把SQL命令插入到Web表单的输入域或页面请求的查询字符串，欺骗服务器执行恶意的SQL命令。在某些表单中，用户输入的内容直接用来构造（或者影响）动态SQL命令，或作为存储过程的输入参数，这类表单特别容易受到SQL注入式攻击。

8、写出一条Sql语句：取出表A中第31到第40记录（SQLServer,以自动增长的ID作为主键,注意：ID可能不是连续的）

解1: select top 10 * from A where id not in (select top 30 id from A)

解2: select top 10 * from A where id > (select max(id) from (select top 30 id from A )as A)


9、横表、纵表转换

纵表结构 TableA 

Name

Course

Grade

张三

语文

75

张三

数学

80

张三

英语

90

李四

语文

95

李四

数学

55

 横表结构 TableB

Name

语文

数学

英语

张三

75

80

90

李四

95

55

0



先理解：

select Name，

 (case Course when ‘语文‘ then Grade else 0 end) as 语文，

 (case Course when ‘数学‘ then Grade else 0 end) as 数学，

 (case Course when ‘英语‘ then Grade else 0 end) as 英语

from TableA

然后理解标准答案：

select Name，

sum(case Course when ‘语文‘ then Grade else 0 end) as 语文，

sum(case Course when ‘数学‘ then Grade else 0 end) as 数学，

sum(case Course when ‘英语‘ then Grade else 0 end) as 英语

from TableA

group by Name




横表转纵表的"SQL"示例

横表结构: TEST_H2Z

      ID      姓名    语文        数学       英语     

      1       张三     80         90         70           

      2       李四     90         85         95         

      3       王五     88         75         90         




转换后的表结构: 

      ID     姓名     科目     成绩 

      1       张三     语文     80 

      2       张三     数学     90 

      3       张三     英语     70 

      4       李四     语文     90 

      5       李四     数学     80   

      6       李四     英语     99 

      7       王五     语文     85 

      8       王五     数学     96 

      9       王五     英语     88  





横表转纵表SQL示例:
SELECT   姓名,'语文'   AS     科目,语文   AS   成绩   FROM   TEST_H2Z   UNION   ALL 
SELECT   姓名,'数学'   AS     科目,数学   AS   成绩   FROM   TEST_H2Z   UNION   ALL 
SELECT   姓名,'英语'   AS     科目,英语   AS   成绩   FROM   TEST_H2Z
ORDER BY 姓名,科目 DESC;




10、删除姓名、年龄重复的记录

Id  name  age  salary

1   yzk    80  1000

2   yzk    80  2000

3   tom    20  20000

4   tom    20  20000

5   im     20  20000

//取得不重复的数据

select * from Persons where Id in(SELECT   MAX(Id) AS Expr1 FROM Persons GROUP BY Name, Age)

根据姓名、年龄分组，取出每组的Id最大值，然后将Id最大值之外的排除。

删除重复的数据：

delete from Persons where Id not in(SELECT  MAX(Id) AS Expr1 FROM  Persons GROUP BY Name, Age)






11、

表一：student_info

学号

姓名

性别

出生年月

家庭住址

备注

0001

张三

男

1981-8-9

北京

NULL





表二：curriculum

课程编号

课程名称

学分

0001

计算机基础

2

0002

C语言

2






表三：grade
学号

课程编号

分数

0001

0001

80

0001

0002

90





题目：

条件查询：

1、在GRADE表中查找80-90份的学生学号和分数

    select 学号,分数 from grade where 分数 between 80 and 90

2、在GRADE 表中查找课程编号为003学生的平均分

   select avg(分数) from grade where 课程编号='003'

3、在GRADE 表中查询学习各门课程的人数

    select课程编号,count(学号) as 人数from grade group by 课程编号

4、查询所有姓张的学生的学号和姓名

   select  姓名,学号 from student_info where 姓名 like '张%'


嵌套查询：

1、 查询和学号’0001’的这位同学性别相同的所有同学的姓名和出生年月

      select 姓名,出生年月 from student_info where 性别 in(select 性别 from student_info where sno='0001')

2、 查询所有选修课程编号为0002 和0003的学生的学号、姓名和性别

     select 学号,姓名,性别 from student_info where 学号 in(select 学号 from grade where 课程编号='0002' and 学号 in(select 学号 from grade where 课程编号='0001'))

3、 查询出学号为0001的学生的分数比0002号学生最低分高的课程编号的课程编号和分数

     select 课程编号, 分数 from grade where 学号='0001' and 分数>(select min(分数) from grade where 学号='0002')




多表查询：

1、 查询分数在80-90分的学生的学号、姓名、分数

select student_info.学号,student_info.姓名,grade.分数 from student_info,grade where grade.分数 between 80 and 90

2、 查询学习了’C语言’课程的学生学号、姓名和分数

select student_info.学号,student_info.姓名,grade.成绩from student_info,grade,curriculum where student_info.学号=grade.学号and grade.课程号=curriculum.课程号and curriculum.课程名='C语言'

3、 查询所有学生的总成绩，要求列出学号、姓名、总成绩，没有选课的学生总成绩为空。

select grade.学号,student_info.姓名,sum(grade.成绩) as 总成绩from student_info,grade where grade.学号=student_info.学号group by grade.学号,student_info.姓名





12、本题用到下面三个关系表：

CARD     借书卡:   (CNO 卡号，NAME  姓名，CLASS 班级)

BOOKS    图书:     (BNO 书号，BNAME 书名,AUTHOR 作者，PRICE 单价，QUANTITY 库存册数 )

BORROW   借书记录: (CNO 借书卡号，BNO 书号，RDATE 还书日期

备注：限定每人每种书只能借一本；库存册数随借书、还书而改变。

要求实现如下处理：

写出自定义函数，要求输入借书卡号能得到该卡号所借书金额的总和：

CREATE FUNCTION getSUM

(

@CNO int

)

RETURNS int

AS

BEGIN

    declare @sum int

    select @sum=sum(price) from BOOKS where bno in (select bno from BORROW where cno=@CNO)

    return @sum

END

GO




找出借书超过5本的读者,输出借书卡号及所借图书册数。

select CNO,count(BNO) as 借书数量from BORROW group by CNO having count(BNO)>3

查询借阅了"水浒"一书的读者，输出姓名及班级。

select name,class from card where cno in( select cno from borrow where bno in(select bno from BOOKS where bname='水浒'))

查询过期未还图书，输出借阅者（卡号）、书号及还书日期。

select CNO,BNO,RDATE from borrow where getdate()>RDATE

查询书名包括"网络"关键词的图书，输出书号、书名、作者。

select bno,bname,author from books where bname like '网络%'

查询现有图书中价格最高的图书，输出书名及作者。

select bname,author from books where price in(select max(price) from books  )

查询当前借了"计算方法"但没有借"计算方法习题集"的读者，输出其借书卡号，并按卡号降序排序输出。

select cno from borrow where bno in (select bno from books where bname='计算方法') and cno not in ( select cno from borrow where bno in(select bno from books where bname='计算方法习题集')) order by cno desc

或

SELECT a.CNO

FROM BORROW a,BOOKS b

WHERE a.BNO=b.BNO AND b.BNAME=N'计算方法'

    AND NOT EXISTS(

        SELECT * FROM BORROW aa,BOOKS bb

        WHERE aa.BNO=bb.BNO

            AND bb.BNAME=N'计算方法习题集'

            AND aa.CNO=a.CNO)

ORDER BY a.CNO DESC

将"C01"班同学所借图书的还期都延长一周。


update borrow set rdate=dateadd(day,7,rdate) from BORROW where cno in(select cno from card where class='一班')

从BOOKS表中删除当前无人借阅的图书记录。

DELETE A FROM BOOKS a

WHERE NOT EXISTS(

    SELECT * FROM BORROW

    WHERE BNO=a.BNO)

如果经常按书名查询图书信息，请建立合适的索引。

CREATE CLUSTERED INDEX IDX_BOOKS_BNAME ON BOOKS(BNAME)




在BORROW表上建立一个触发器，完成如下功能：如果读者借阅的书名是"数据库技术及应用"，就将该读者的借阅记录保存在BORROW_SAVE表中（注ORROW_SAVE表结构同BORROW表）

CREATE TRIGGER TR_SAVE ON BORROW

FOR INSERT,UPDATE

AS

IF @@ROWCOUNT>0

INSERT BORROW_SAVE SELECT i.*

FROM INSERTED i,BOOKS b

WHERE i.BNO=b.BNO

AND b.BNAME=N'数据库技术及应用'



建立一个视图，显示"力01"班学生的借书信息（只要求显示姓名和书名）。

CREATE VIEW V_VIEW

AS

select name,bname

from  books,card,borrow

where borrow.cno=card.cno and borrow.bno=books.bno and class='一班'

查询当前同时借有"计算方法"和"组合数学"两本书的读者，输出其借书卡号，并按卡号升序排序输出。

select a.cno from borrow a,borrow b

where a.cno=b.cno and

 a.bno in(select bno from books where bname='计算方法') and

b.bno in(select bno from books where bname='组合数学')

order by a.cno asc

或

SELECT a.CNO

FROM BORROW a,BOOKS b

WHERE a.BNO=b.BNO

 AND b.BNAME IN('计算方法','组合数学')

GROUP BY a.CNO

HAVING COUNT(*)=2

ORDER BY a.CNO asc




用事务实现如下功能：一个借书卡号借走某书号的书，则该书的库存量减少1，当某书的库存量不够1本的时候，该卡号不能借该书

alter PROCEDURE pro_jieshu

 @cno int,

 @bno int,

 @date datetime

AS

BEGIN

begin tran

declare @quantity int

select @quantity=quantity from books where bno=@bno

  insert into borrow values(@cno,@bno,@date)

  update books set quantity=@quantity-1 where bno=@bno

if(@quantity>0)

  begin

   commit tran

  end

else

  begin

   print '已无库存'

   rollback

  end

END

GO





用游标实现将书号为‘A001’的书本的价格提高10元

declare @bno int
declare @bname nvarchar(50)
declare @author nvarchar(50)
declare @price int
declare @quantity int
declare mycursor cursor for select * from books
open mycursor
fetch next from mycursor into @bno,@bname,@author,@price,@quantity
while(@@fetch_status=0)
  begin
      if(@bno=2)
       begin
        update books set price=@price+10 where current of mycursor
       end
     fetch next from mycursor into @bno,@bname,@author,@price,@quantity
  end
close mycursor
deallocate mycursor




13、

Student(S#,Sname,Sage,Ssex) 学生表

Course(C#,Cname,T#) 课程表

SC(S#,C#,score) 成绩表

Teacher(T#,Tname) 教师表

查询“001”课程比“002”课程成绩高的所有学生的学号；

select a.S# from (select s#,score from SC where C#='001') a,(select s#,score

  from SC where C#='002') b

  where a.score>b.score and a.s#=b.s#;

查询平均成绩大于60分的同学的学号和平均成绩；

select S#,avg(score) from sc group by S# having avg(score) >60;



查询所有同学的学号、姓名、选课数、总成绩；

select Student.S#,Student.Sname,count(SC.C#),sum(score) from Student left Outer join SC on Student.S#=SC.S# group by Student.S#,Sname

查询姓“李”的老师的个数；

select count(distinct(Tname)) from Teacher where Tname like '李%';

查询没学过“叶平”老师课的同学的学号、姓名；

select Student.S#,Student.Sname  from Student 

where S# not in (select distinct( SC.S#) from SC,Course,Teacher where  SC.C#=Course.C# and Teacher.T#=Course.T# and Teacher.Tname='叶平');

查询学过“001”并且也学过编号“002”课程的同学的学号、姓名；

select Student.S#,Student.Sname from Student,SC where Student.S#=SC.S# and SC.C#='001'and exists( Select * from SC as SC_2 where SC_2.S#=SC.S# and SC_2.C#='002');

查询学过“叶平”老师所教的所有课的同学的学号、姓名；

select S#,Sname from Student where S# in (select S# from SC ,Course ,Teacher where SC.C#=Course.C# and Teacher.T#=Course.T# and Teacher.Tname='叶平'

group by S# having count(SC.C#)=(select count(C#) from Course,Teacher  where Teacher.T#=Course.T# and Tname='叶平'));

查询课程编号“002”的成绩比课程编号“001”课程低的所有同学的学号、姓名；

Select S#,Sname from (select Student.S#,Student.Sname,score ,(select score from SC SC_2 where SC_2.S#=Student.S# and SC_2.C#='002') score2

from Student,SC where Student.S#=SC.S# and C#='001') S_2 where score2 <score;



查询所有课程成绩小于60分的同学的学号、姓名；

select S#,Sname from Student where S# not in (select Student.S# from Student,SC where S.S#=SC.S# and score>60);

查询没有学全所有课的同学的学号、姓名；

select Student.S#,Student.Sname from Student,SC

where Student.S#=SC.S# group by  Student.S#,Student.Sname having count(C#) <(select count(C#) from Course);

查询至少有一门课与学号为“1001”的同学所学相同的同学的学号和姓名；

select S#,Sname from Student,SC where Student.S#=SC.S# and C# in select C# from SC where S#='1001';

查询至少学过学号为“001”同学所有一门课的其他同学学号和姓名；

select distinct SC.S#,Sname from Student,SC where Student.S#=SC.S# and C# in (select C# from SC where S#='001');

把“SC”表中“叶平”老师教的课的成绩都更改为此课程的平均成绩；

update SC set score=(select avg(SC_2.score) from SC SC_2 where SC_2.C#=SC.C# ) from Course,Teacher where Course.C#=SC.C# and Course.T#=Teacher.T# and Teacher.Tname='叶平');



查询和“1002”号的同学学习的课程完全相同的其他同学学号和姓名；

 

select S# from SC where C# in (select C# from SC where S#='1002')

 

    group by S# having count(*)=(select count(*) from SC where S#='1002');



删除学习“叶平”老师课的SC表记录；

 

Delete SC

 

    from course ,Teacher 

 

    where Course.C#=SC.C# and Course.T#= Teacher.T# and Tname='叶平';

 

向SC表中插入一些记录，这些记录要求符合以下条件：没有上过编号“003”课程的同学学号、2、号课的平均成绩；

 

Insert SC select S#,'002',(Select avg(score)

 

from SC where C#='002') from Student where S# not in (Select S# from SC where C#='002');




按平均成绩从高到低显示所有学生的“数据库”、“企业管理”、“英语”三门的课程成绩，按如下形式显示： 学生ID,,数据库,企业管理,英语,有效课程数,有效平均分

 

SELECT S# as 学生ID

 

        ,(SELECT score FROM SC WHERE SC.S#=t.S# AND C#='004') AS 数据库

 

        ,(SELECT score FROM SC WHERE SC.S#=t.S# AND C#='001') AS 企业管理

 

        ,(SELECT score FROM SC WHERE SC.S#=t.S# AND C#='006') AS 英语

 

        ,COUNT(*) AS 有效课程数, AVG(t.score) AS 平均成绩

 

    FROM SC AS t

 

    GROUP BY S#

 

ORDER BY avg(t.score)







 查询各科成绩最高和最低的分：以如下形式显示：课程ID，最高分，最低分

 

SELECT L.C# As 课程ID,L.score AS 最高分,R.score AS 最低分

 

    FROM SC L ,SC AS R

 

    WHERE L.C# = R.C# and

 

        L.score = (SELECT MAX(IL.score)

 

                      FROM SC AS IL,Student AS IM

 

                      WHERE L.C# = IL.C# and IM.S#=IL.S#

 

                      GROUP BY IL.C#)

 

        AND

 

        R.Score = (SELECT MIN(IR.score)

 

                      FROM SC AS IR

 

                      WHERE R.C# = IR.C#

 

                  GROUP BY IR.C#

 

                    );

 



按各科平均成绩从低到高和及格率的百分数从高到低顺序

 

SELECT t.C# AS 课程号,max(course.Cname)AS 课程名,isnull(AVG(score),0) AS 平均成绩

 

        ,100 * SUM(CASE WHEN  isnull(score,0)>=60 THEN 1 ELSE 0 END)/COUNT(*) AS 及格百分数

 

    FROM SC T,Course

 

    where t.C#=course.C#

 

    GROUP BY t.C#

 

ORDER BY 100 * SUM(CASE WHEN  isnull(score,0)>=60 THEN 1 ELSE 0 END)/COUNT(*) DESC

 



查询如下课程平均成绩和及格率的百分数(用"1行"显示): 企业管理（001），马克（002），OO&UML （003），数据库（004）

 

SELECT SUM(CASE WHEN C# ='001' THEN score ELSE 0 END)/SUM(CASE C# WHEN '001' THEN 1 ELSE 0 END) AS 企业管理平均分

 

        ,100 * SUM(CASE WHEN C# = '001' AND score >= 60 THEN 1 ELSE 0 END)/SUM(CASE WHEN C# = '001' THEN 1 ELSE 0 END) AS 企业管理及格百分数

 

        ,SUM(CASE WHEN C# = '002' THEN score ELSE 0 END)/SUM(CASE C# WHEN '002' THEN 1 ELSE 0 END) AS 马克思平均分

 

        ,100 * SUM(CASE WHEN C# = '002' AND score >= 60 THEN 1 ELSE 0 END)/SUM(CASE WHEN C# = '002' THEN 1 ELSE 0 END) AS 马克思及格百分数

 

        ,SUM(CASE WHEN C# = '003' THEN score ELSE 0 END)/SUM(CASE C# WHEN '003' THEN 1 ELSE 0 END) AS UML平均分

 

        ,100 * SUM(CASE WHEN C# = '003' AND score >= 60 THEN 1 ELSE 0 END)/SUM(CASE WHEN C# = '003' THEN 1 ELSE 0 END) AS UML及格百分数

 

        ,SUM(CASE WHEN C# = '004' THEN score ELSE 0 END)/SUM(CASE C# WHEN '004' THEN 1 ELSE 0 END) AS 数据库平均分

 

        ,100 * SUM(CASE WHEN C# = '004' AND score >= 60 THEN 1 ELSE 0 END)/SUM(CASE WHEN C# = '004' THEN 1 ELSE 0 END) AS 数据库及格百分数

 

  FROM SC

 

查询不同老师所教不同课程平均分从高到低显示

 

SELECT max(Z.T#) AS 教师ID,MAX(Z.Tname) AS 教师姓名,C.C# AS 课程ＩＤ,MAX(C.Cname) AS 课程名称,AVG(Score) AS 平均成绩

 

    FROM SC AS T,Course AS C ,Teacher AS Z

 

    where T.C#=C.C# and C.T#=Z.T#

 

  GROUP BY C.C#

 

  ORDER BY AVG(Score) DESC





  查询如下课程成绩第 3 名到第 6 名的学生成绩单：企业管理（001），马克思（002），UML （003），数据库（004） [学生ID],[学生姓名],企业管理,马克思,UML,数据库,平均成绩

 

SELECT  DISTINCT top 3

 

      SC.S# As 学生学号,

 

        Student.Sname AS 学生姓名 ,

 

      T1.score AS 企业管理,

 

      T2.score AS 马克思,

 

      T3.score AS UML,

 

      T4.score AS 数据库,

 

      ISNULL(T1.score,0) + ISNULL(T2.score,0) + ISNULL(T3.score,0) + ISNULL(T4.score,0) as 总分

 

      FROM Student,SC  LEFT JOIN SC AS T1

 

                      ON SC.S# = T1.S# AND T1.C# = '001'

 

            LEFT JOIN SC AS T2

 

                      ON SC.S# = T2.S# AND T2.C# = '002'

 

            LEFT JOIN SC AS T3

 

                      ON SC.S# = T3.S# AND T3.C# = '003'

 

            LEFT JOIN SC AS T4

 

                      ON SC.S# = T4.S# AND T4.C# = '004'

 

      WHERE student.S#=SC.S# and

 

      ISNULL(T1.score,0) + ISNULL(T2.score,0) + ISNULL(T3.score,0) + ISNULL(T4.score,0)

 

      NOT IN

 

      (SELECT

 

            DISTINCT

 

            TOP 15 WITH TIES

 

            ISNULL(T1.score,0) + ISNULL(T2.score,0) + ISNULL(T3.score,0) + ISNULL(T4.score,0)

 

      FROM sc

 

            LEFT JOIN sc AS T1

 

                      ON sc.S# = T1.S# AND T1.C# = 'k1'

 

            LEFT JOIN sc AS T2

 

                      ON sc.S# = T2.S# AND T2.C# = 'k2'

 

            LEFT JOIN sc AS T3

 

                      ON sc.S# = T3.S# AND T3.C# = 'k3'

 

            LEFT JOIN sc AS T4

 

                      ON sc.S# = T4.S# AND T4.C# = 'k4'

 

      ORDER BY ISNULL(T1.score,0) + ISNULL(T2.score,0) + ISNULL(T3.score,0) + ISNULL(T4.score,0) DESC);

 




统计列印各科成绩,各分数段人数:课程ID,课程名称,[100-85],[85-70],[70-60],[ <60]

 

SELECT SC.C# as 课程ID, Cname as 课程名称

 

        ,SUM(CASE WHEN score BETWEEN 85 AND 100 THEN 1 ELSE 0 END) AS [100 - 85]

 

        ,SUM(CASE WHEN score BETWEEN 70 AND 85 THEN 1 ELSE 0 END) AS [85 - 70]

 

        ,SUM(CASE WHEN score BETWEEN 60 AND 70 THEN 1 ELSE 0 END) AS [70 - 60]

 

        ,SUM(CASE WHEN score < 60 THEN 1 ELSE 0 END) AS [60 -]

 

    FROM SC,Course

 

    where SC.C#=Course.C#

 

GROUP BY SC.C#,Cname;






查询学生平均成绩及其名次

 

SELECT 1+(SELECT COUNT( distinct 平均成绩)

 

              FROM (SELECT S#,AVG(score) AS 平均成绩

 

                      FROM SC

 

                  GROUP BY S#

 

                  ) AS T1

 

            WHERE 平均成绩 > T2.平均成绩) as 名次,

 

      S# as 学生学号,平均成绩

 

    FROM (SELECT S#,AVG(score) 平均成绩

 

            FROM SC

 

        GROUP BY S#

 

        ) AS T2

 

ORDER BY 平均成绩 desc;




 查询各科成绩前三名的记录:(不考虑成绩并列情况)

 

SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数

 

      FROM SC t1

 

      WHERE score IN (SELECT TOP 3 score

 

              FROM SC

 

              WHERE t1.C#= C#

 

            ORDER BY score DESC

 

              )

 

      ORDER BY t1.C#;






查询每门课程被选修的学生数

 

select c#,count(S#) from sc group by C#;

 

 

 

查询出只选修了一门课程的全部学生的学号和姓名

 

select SC.S#,Student.Sname,count(C#) AS 选课数

 

  from SC ,Student

 

  where SC.S#=Student.S# group by SC.S# ,Student.Sname having count(C#)=1;








  查询男生、女生人数

 

Select count(Ssex) as 男生人数 from Student group by Ssex having Ssex='男';

 

Select count(Ssex) as 女生人数 from Student group by Ssex having Ssex='女'；

 

 

 

查询姓“张”的学生名单

 

SELECT Sname FROM Student WHERE Sname like '张%';

 

 

 

查询同名同性学生名单，并统计同名人数

 

select Sname,count(*) from Student group by Sname having  count(*)>1;






 1981年出生的学生名单(注：Student表中Sage列的类型是datetime)

 

select Sname,  CONVERT(char (11),DATEPART(year,Sage)) as age

 

    from student

 

where  CONVERT(char(11),DATEPART(year,Sage))='1981';

 

 

 

查询每门课程的平均成绩，结果按平均成绩升序排列，平均成绩相同时，按课程号降序排列

 

Select C#,Avg(score) from SC group by C# order by Avg(score),C# DESC ;






查询平均成绩大于85的所有学生的学号、姓名和平均成绩

 

select Sname,SC.S# ,avg(score)

 

    from Student,SC

 

where Student.S#=SC.S# group by SC.S#,Sname having    avg(score)>85;

 

 

 

查询课程名称为“数据库”，且分数低于60的学生姓名和分数

 

Select Sname,isnull(score,0)

 

    from Student,SC,Course

 

where SC.S#=Student.S# and SC.C#=Course.C# and  Course.Cname='数据库'and score <60;







 查询所有学生的选课情况；

 

SELECT SC.S#,SC.C#,Sname,Cname

 

    FROM SC,Student,Course

 

where SC.S#=Student.S# and SC.C#=Course.C# ;

 

 

 

查询任何一门课程成绩在70分以上的姓名、课程名称和分数；

 

SELECT  distinct student.S#,student.Sname,SC.C#,SC.score

 

    FROM student,Sc

 

WHERE SC.score>=70 AND SC.S#=student.S#;







查询不及格的课程，并按课程号从大到小排列

 

select c# from sc where scor e <60 order by C# ;

 

 

 

查询课程编号为003且课程成绩在80分以上的学生的学号和姓名；

 

select SC.S#,Student.Sname from SC,Student where SC.S#=Student.S# and Score>80 and C#='003';

 

 

 

求选了课程的学生人数

 

select count(*) from sc;






查询选修“叶平”老师所授课程的学生中，成绩最高的学生姓名及其成绩

 

select Student.Sname,score from Student,SC,Course C,Teacher

 

where Student.S#=SC.S# and SC.C#=C.C# and C.T#=Teacher.T# and Teacher.Tname='叶平' and SC.score=(select max(score)from SC where C#=C.C# );

 

 

 

查询各个课程及相应的选修人数

 

select count(*) from sc group by C#;

 

 

 

查询不同课程成绩相同的学生的学号、课程号、学生成绩

 

select distinct  A.S#,B.score from SC A  ,SC B where A.Score=B.Score and A.C# <>B.C# ;






查询每门功成绩最好的前两名

 

SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数

 

      FROM SC t1

 

      WHERE score IN (SELECT TOP 2 score

 

              FROM SC

 

              WHERE t1.C#= C#

 

            ORDER BY score DESC

 

              )

 

      ORDER BY t1.C#;









统计每门课程的学生选修人数（超过10人的课程才统计）。要求输出课程号和选修人数，查询结果按人数降序排列，查询结果按人数降序排列，若人数相同，按课程号升序排列

 

select  C# as 课程号,count(*) as 人数

 

    from  sc 

 

    group  by  C#

 

order  by  count(*) desc,c#

 

 

 

检索至少选修两 门课程的学生学号

 

select  S# 

 

    from  sc 

 

    group  by  s#

 

having  count(*)  >  =  2

 

 

 

查询全部学生都选修的课程的课程号和课程名

 

select  C#,Cname 

 

    from  Course 

 

    where  C#  in  (select  c#  from  sc group  by  c#)

 

 

 

查询没学过“叶平”老师讲授的任一门课程的学生姓名

 

select Sname from Student where S# not in (select S# from Course,Teacher,SC where Course.T#=Teacher.T# and SC.C#=course.C# and Tname='叶平');

 



查询两门以上不及格课程的同学的学号及其平均成绩

 

select S#,avg(isnull(score,0)) from SC where S# in (select S# from SC where score <60 group by S# having count(*)>2)group by S#;

 

 

 

检索“004”课程分数小于60，按分数降序排列的同学学号

 

select S# from SC where C#='004'and score <60 order by score desc;

 

 

 

删除“002”同学的“001”课程的成绩

 

delete from Sc where S#='002'and C#='001';



























