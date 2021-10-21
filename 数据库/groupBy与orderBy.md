group by与order by有什么区别，order by就是排序。而group by就是分组，举个例子好说点，group by 单位名称 

这样的运行结果就是以“单位名称”为分类标志统计各单位的职工人数和工资总额。

WHERE语句在GROUP BY语句之前；SQL会在分组之前计算WHERE语句。   

HAVING语句在GROUP BY语句之后；SQL会在分组之后计算HAVING语句。

group by会删掉重复的数据。