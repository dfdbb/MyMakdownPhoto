### RANK()函数可排名
`="第"&RANK(F2,$F$2:$F$45)&"名"`
### Lookup()函数

### Mid()函数

### VLOOKUP函数
=VLOOKUP(根据什么找、在哪里找、返回哪列、返回表示为 1/TRUE 或 0/FALSE 的近似或精确匹配项)。

## SUMIF&SUMIFS()函数 
数据源如图：
![输入图片说明](/imgs/2022-11-14/kGoiSYXG8BaTI9QQ.png)
SUMIF:
求A组总销量:
`=sumif(条件范围，条件，求和区域)`

SUMIFS:
求A组所有男性销量:
`=sumifs(示例区域,条件区域1,条件1，条件区域2，条件2.....`

ISODD函数判断奇偶

判断男女：`=IF(ISODD(MID([@身份证号码],17,1)),"男","女")`

取出生日期:`=TEXT(MID([@身份证号码],7,8),"0000年00月00日")`

取年龄:`=DATEDIF([@出生日期],TODAY(),"y")`
> 隐藏函数
datedif(起始日期，终止日期，日期单位)


RANK()排名: `=rank(值，范围，升或降)`

字符串拼接：`="第"&RANK(F2,$F$2:$F$45)&"名" `

![输入图片说明](/imgs/2022-11-21/s2JY2dcCsTR1DH5B.png)

![输入图片说明](/imgs/2022-11-21/schA2qN12oJTr2UP.png)

# DATEDIF(H3，“)

# LEN(H3)





<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxNDY2NzI4ODksLTExNjg2MzQwODcsMj
A3MDI0NDU0MSwtMTA0ODIwNDY4NSwtOTcwNTI2Njk3LC00ODc4
NzU2OTUsLTE3OTAwNjU2MjUsLTE2NzQxODA0MjAsLTEwODUyMj
gyODAsLTIwNzAyODM2MTFdfQ==
-->