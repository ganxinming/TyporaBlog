# MYSQL 各字段执行顺序

select * from employees
where emp_no % 2=1 and last_name!='Mary'
order by hire_date desc;

1、FORM: 对FROM左边的表和右边的表计算笛卡尔积，产生虚表VT1。
2、ON: 对虚表VT1进行ON过滤，只有那些符合<join-condition>的行才会被记录在虚表VT2中。
3、JOIN： 如果指定了OUTER JOIN（比如left join、 right join），那么保留表中未匹配的行就会作为外部行添加到虚拟表VT2中，产生虚拟表VT3。
4、WHERE： 对虚拟表VT3进行WHERE条件过滤。只有符合<where-condition>的记录才会被插入到虚拟表VT4中。
5、GROUP BY: 根据group by子句中的列，对VT4中的记录进行分组操作，产生VT5。
6、HAVING： 对虚拟表VT5应用having过滤，只有符合<having-condition>的记录才会被 插入到虚拟表VT6中。
7、SELECT： 执行select操作，选择指定的列，插入到虚拟表VT7中。
8、DISTINCT： 对VT7中的记录进行去重。产生虚拟表VT8.
9、ORDER BY: 将虚拟表VT8中的记录按照<order_by_list>进行排序操作，产生虚拟表VT9.
10、LIMIT：取出指定行的记录，产生虚拟表VT10, 并将结果返回。

技巧：反正写sql语句的时候，习惯，写完select后，后面的参数先留着。先写from，也是从这开始，然后进行表的连接join，有on执行on条件。连接完表后，通过where进行条件筛选，之后group by 进行分组，根据having的条件。之后select 选择要显示的数据，形成了一张基表。之后的order by 就是在这基础上进行的。 最后控制要显示的数据，通过limit控制。

# 小特点

group by 可以按属性不同值进行分组，只要我输出那个值，可以达到去重的效果。

order by x desc,y  ase 。可以多个排序，升序或者降序。

limit 0,1  0代表从0+1行开始，也就是第一个开始，显示1条数据。 可直接写成 limit 1

SELECT/FROM/WHERE/ORDER BY/GROUP BY等子句应独占一行，即分成另一段写。

