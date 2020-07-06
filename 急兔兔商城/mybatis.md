# Mybatis:

#### 1.遍历jtb_item发现出先下划线的属性无法适配，所以使用了resultMap.

#### 2.发现mybatis的insert方法并不会插入数据库默认值，可以改用insertSelective方法，具有动态sql判断。

### 2.尽量使用Selective后缀的方法：

1.能自动将null值忽略不更新到数据库

2.Selective更新和删除时，会触发数据库的级联(cacsade)删除或更新。

3.example真的特别好用，帅!

```
JtbItemcontentExample jtbItemcontentExample=new JtbItemcontentExample();
jtbItemcontentExample.createCriteria().andIdEqualTo(cNumber);
jtbItemcontentMapper.updateByExampleSelective(jtbItemcontent,jtbItemcontentExample);
//开始更新jtbitem
JtbItemExample jtbItemExample=new JtbItemExample();
jtbItemExample.createCriteria().andCNumberEqualTo(cNumber);
jtbItemMapper.updateByExampleSelective(jtbItem,jtbItemExample);
return;
```

