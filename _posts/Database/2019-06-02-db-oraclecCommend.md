---
layout: post
title: 将一个表字段复制到另一个表中
category: other
tags: [other]
---

## 将一个表字段复制到另一个表中
````  
-- Created on 2016/10/12 by jiangyuan
declare 
-- 将一个表字段复制到另一个表中 
i integer; 
item varchar2(2000); 
r_arc010 varchar2(2000);
begin 
-- Test statements here 
for item in ( select aac002 from fc02) loop   
select count(1) into i  from hc02_sb where aac002 = item.aac002;   
if (i = 1)  then       select arc010 into  r_arc010 from fc02 where aac002 = item.aac002 and aae100 = '1';     
update  hc02_sb set arc010 = r_arc010 where aac002 = item.aac002;     
end if;   
end loop;
end;

````  

如果两列数据重复的话，可以通过count方法，找出计算条数大于1的，那么表示此条数据重复：<br/>
>sql: select district() from products having count()>1;
备注：实际上两列重复的话，都是针对某个字段比较有意:<br/>
>sql:select name,count() from usertable group by name having count()>1;