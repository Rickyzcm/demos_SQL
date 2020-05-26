MySQL_Note_0
===

**提要**  
* 内容目前到书籍《MySQL必知必会》第九章"使用正则表达式搜索"前为止
* 知识点顺序与书本不完全一致：先是命题，然后是SQL语句
* 表达方式可能不适应一般的阅读习惯  
* 对 SQL 语句的书写方式，更推荐该书上的写法

**内容概览**

 关键字 | 功能介绍 | 性质 | 注意点(规则) |
 -|-|-|-
 USE | 选择数据库 | 语句 |
 SHOW | 了解数据库 | 语句 |
 SELECT | 检索数据库 | 语句 | 
 DISTINCT | 检索不同的行 | 关键字 | 放置在要修饰的列名之前  
 LIMIT | 限定结果 | 子句 | 后跟数字x解释为限定x行，跟(x,y)则解释为限定从行x开始的y行  
 ORDER BY | 指定排序方向 | 子句 | 应放置在 WHERE 子句的后方  
 ASC | 升序排序 | 关键字 | 可作为缺省值不写，MySQL也将按照默认(升序)排序
 DESC | 降序排序 | 关键字 | 不可缺省，当有多列作为排序依据时，每一列都需要有一个`DESC`关键字在列名之后修饰
 WHERE | 指定搜索条件 | 子句 | 应放置在 ORDER BY 子句的前方  
 BETWEEN | 范围值检查 | 操作符 | 与 `AND`搭配使用-- BETWEEN a AND b(在a和b之间，包括a、b)
 AND | 联结前后两个条件 | 操作符 | 前后联结的条件必须同时成立
 NULL | 表示为非0不存在任何元素 | 值 | 特殊的空值即不是0也不是空字符的意思
 IN | 指定条件范围 | 操作符 | 后跟的条件范围可看做合法值清单
 NOT | 否定之后所有条件 | 操作符 | 有且只有一个否定之后所跟条件的功能
 LIKE | 模糊搜索模式 | 操作符 | 将字面值、通配符或两者组合构成搜索条件
  `*`| 不指定解释为任意 | 通配符 | 不应经常使用，有时可以检索出未知名的列
 `%` | 与`*`类似，匹配任意个任意字符 | 通配符 | 表示任意个任意字符，但不能`NULL`比较
 `_` | 与`%`类似，匹配单个字符 | 通配符 | 表示单个任意字符，不能与`NULL`比较

开始使用数据库(不使用可视化操作数据库管理工具时)
---

1. Win+R键
1. cmd内输入`MySQL -uroot -p`
1. 键入密码(输入过程中不可见)，回车

**-注-**
> 其中的`uroot`中间不要有空格，root 是在安装 MySQL时设置的管理员用户名

>在使用MySQL的语句大小写均可，但是为了养成好习惯，我们应当尽可能将MySQL的内置词纯大写和可以自行定义的词句纯小写区分开来

>每个语句必须以“;”结尾才会被识别为语句结束

开始使用MySQL语句
---
1. 进入MySQL后显示所有可用(当前用户权限范围内)数据库
    ```sql
    SHOW DATABASES;
    ```

1. 使用数据库,假设库名为 database_name

    ```sql
    USE database_name;
    ```

1. 显示数据库 database_name 中可用数据表
    ```sql
    SHOW TABLES FROM database_name;
    ```

1. 显示数据表 table_name 中所有列的信息
    ```sql
    SHOW COLUMNS FROM table_name;
    ```

    *以上语句也可用 `DESCRIBE`语句来代替*

    ```sql
    DESCRIBE table_name;
    ```

1. 显示数据表 table_name 中所有数据项
    ```sql
    SHOW * FROM table_name;
    ```
    *`*`称为通配符*

1. 检索数据表 table_name 中 column_1,column_2 两列的所有数据项
    ```sql
    SELECT column_1,column_2 FROM table_name;
    ```
    *检索多列时每两个列名之间都用“,”隔开*

1. 检索数据表 table_name 中 column 列中值不同的数据项
    ```sql
    SELECT DISTINCT column FROM table_name;
    ```

1. 检索数据表 table_name 中 column 列中所有的数据项并显示不多于5行
    ```sql
    SELECT column FROM table_name LIMIT 5;
    ```

1. 检索数据表 table_name 中 column 列中所有的数据项并显示从行5开始的5行
    ```sql
    SELECT column FROM table_name LIMIT 5,5;
    ```

1. 检索数据表 table_name 中 column 列中所有的数据项并显示从第5行开始的5行(注意 *行5* 和 *第5行* 的区别)
    ```sql
    SELECT column FROM table_name LIMIT 4,5;
    ```

1. 检索数据表 table_name 中 column 列中所有内容(table_name在database_name数据库中)
    ```sql
    SELECT table_name.column FROM database_name.table_name;
    ```

1. 以 column_1 列字母顺序检索table_name 中 column_1,column_2 列所有数据项
    ```sql
    SELECT column_1,column_2 FROM table_name ORDER BY column_1;
    ```

1. 检索 table_name 中 column_1,column_2 列所有数据项，首先按照 column_2 列排列，然后按照 column_1 列排列
    ```sql
    SELECT column_1,column_2 FROM table_name ORDER BY column_2,column_1;
    ```


### 指定排序方向

1. 检索 table_name 中 column 列所有数据项，按照 column 列升序排列
    ```sql
    SELECT column FROM table_name ORDER BY column [ASC];
    ```

1. 检索 table_name 中 column 列所有数据项，按照 column 列降序排列
    ```sql
    SELECT column FROM table_name ORDER BY column DESC;
    ```
    *以上的1,2 中 ASC关键字可缺省，DESC 必须标出，如若多列时，每个列名之后均要修饰关键词*

1. 检索 table_name 中 column_1,column_2 列所有数据项，首先按照 column_1 降序排列,column_1值相同时按照 column_2 列降序排列
    ```sql
    SELECT column_1,column_2 FROM table_name ORDER BY column_1 DESC,column_2 DESC;
    ```

1. 检索 table_name 中 column_1,column_2 列所有数据项，首先按照 column_1 降序排列,column_1值相同时按照 column_2 列升序排列
    ```sql
    SELECT column_1,column_2 FROM table_name ORDER BY column_1 DESC,column_2 [ASC];
    ```

### 检索出该列中最值
*将顺序排列和限制结果结合，即 `ORDER BY`,`LIMIT`结合效果*

1. 检索 table_name 中 column 列中的最大值
    ```sql
    SELECT column FROM table_name ORDER BY column_1 DESC LIMIT 1;
    ```

1. 检索 table_name 中 column 列中的最小值
    ```sql
    SELECT column FROM table_name ORDER BY column_1 [ASC] LIMIT 1;
    ```

### 指定搜索条件

1. 检索 table_name 的 column 列中的值为 para_value 的数据项的所有信息
    ```sql
    SELECT * FROM table_name WHERE column = para_value;
    ```

1. 检索 table_name 的 column 列中的值不为 para_value 的数据项的所有信息
    ```sql
    SELECT * FROM table_name WHERE column <> para_value;
    ```

1. 检索 table_name 的 column 列中值不为 "String" 的数据项的所有信息
    ```sql
    SELECT * FROM table_name WHERE column = "String";
    ```
    *其中在 MySQL 中 String 和 string 字符串值是相同的，执行匹配时默认不区分大小写*

### 范围值检查

1. 检索 table_name 的 column 列中值在为 x-y (包括x，y)之间的数据项的所有信息
    ```sql
    SELECT column FROM table_name BETWEEN x AND y;
    ```

### 空值检查

1. 检索 table_name 的column_2 列值为空(column_2字段为NULL)的数据项的 column_1,column_3 列的所有信息
    ```sql
    SELECT column_1,column_3 FROM table_name WHERE column_2 IS NULL;
    ```

*在这里需要区别空值NULL和不匹配的概念*

### 组合WHERE子句加强数据过滤
+ *为了对不只一个列进行过滤，使用 `AND` 和 `OR` 操作符进行过滤控制*

1. 检索 table_name 的 column_1 列值为空，且 column_2 列的值小于x的所有行的所有信息
    ```sql
    SELECT * FROM table_name WHERE column_1 IS NULL AND column_2 < x ;
    ```

    *当使用超过2个需要同时成立的过滤条件时，每加一条就要增加一个 `AND`*

1. 检索 table_name 的 column_1 列值为空，且 column_2 列的值小于 x 的所有行的所有信息  

    ```sql
    SELECT * FROM table_name WHERE column_1 IS NULL AND column_2 < x ;
    ```

1. 检索 table_name 的 column_1 列值为空，或者 column_2 列的值不小于 x 的所有行的所有信息
    ```sql
    SELECT * FROM table_name WHERE column_1 IS NULL OR column_2 >= x ;
    ```

+ *WHERE 子句内可以包含任意数目的 `AND` 和 `OR` 操作符，并组合使用*

1. 检索 table_name 的 column_1 列值为空或等于 para_value ,并且 column_2 列的值不小于 x 的所有行的所有信息
    ```sql
    SELECT * FROM table_name WHERE ( column_1 IS NULL OR column_1 = para_value ) AND column_2 >= x ;
    ```

    *以上的圆括号运算优先级较 `AND` 和 `OR` 更高，能够使column_1的两个数据过滤条件先行，操作符不会被错误地组合*

+ *IN 操作符指定条件范围，范围中的每个条件都可以进行匹配*

1. 检索 table_name 的 column_1 列值为 value_0 和等于 value_1 的所有行的所有信息
    ```sql
    SELECT * FROM table_name WHERE column_1 IN ( value_0 , value_1 );
    ```

    *也可以将 `IN` 操作符后面的括号内看成是由逗号间隔的过滤条件合法值清单*

+ *将 OR 和 ORDER BY 操作符进行组合*

1. 检索 table_name 的 column_1 列值为 value_0 或等于 value_1 的所有行的所有信息按照 column_3 列的顺序进行排列  

    ```sql
    SELECT * FROM table_name WHERE column_1 = value_0 OR column_1 = value_1  ORDER BY column_3;
    ```  

    *实际上也可以继续回到 `IN` 操作符的用法来实现 `OR` 的功能*
    ```sql    
    SELECT * FROM table_name WHERE column_1 IN ( value_0 , value_1 ) ORDER BY column_3;
    ```  

**可以看出实际上 IN 操作符的合法值清单要比 OR 更直观易读，不失为 IN 的优点之一**

### `NOT`操作符:否定之后所跟的任何条件

1. 检索 table_name 的 column_1 列值不为 value_0 或 value_1 的所有行的所有信息
    ```sql    
    SELECT * FROM table_name WHERE column_1 NOT IN ( value_0 , value_1 );
    ```  

### `LIKE` 操作符  

+ 引入 **通配符** 和 **搜索模式** 
    * 用来匹配值的一部分的特殊字符 —— **通配符**
    * 由字面值、通配符或两者结合组成的"搜索"条件

#### 百分号`%`通配符  

`%`表示任何字符出现任意次数  
* 以字母`xyz`开头的所有值就可写作 `xyz%`  
* `%xyz%`——匹配任何位置包含`xyz`的文本   
* `%xyz`——匹配文本以`xyz`为结尾 的文本
* `x%z`——匹配以`x`开头以`z`结尾的文本

*这里的搜索条件是区分大小写的，`xyz%`和 `Xyz%`是不同的值*  
*`%`能够匹配0个，1个或是多个字符,而匹配0个字符的时候涉及到**尾空格**的问题*  

*`%`不能够匹配`NULL`,即*
```sql
SELECT * FROM table_name WHERE column LIKE '%';
```  
是无法匹配搜索到值为`NULL`的行

#### 下划线`_`通配符 
用途与`%`类似，但是只为匹配单个字符

#### 通配符的注意事项
  * 处理时间较其他操作符更长
  * 不要过度使用，尽能使用其他操作符代替
  * 注意使用的位置正确与否