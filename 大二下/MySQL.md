**DDL：Data Definition Language**

DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。通常，DDL由数据库管理员执行。

**DML：Data Manipulation Language**

DML为用户提供添加、删除、更新数据的能力，这些是应用程序对数据库的日常操作。

**DQL：Data Query Language**

DQL允许用户查询数据，这也是通常最频繁的数据库日常操作。



## 关系模型

### 主键

- 作为主键最好是完全业务无关的字段，我们一般把这个字段命名为`id`。常见的可作为`id`字段的类型有：

  - 自增整数类型：数据库会在插入数据时自动为每一条记录分配一个自增整数，这样我们就完全不用担心主键重复，也不用自己预先生成主键；

  - 全局唯一GUID类型：使用一种全局唯一的字符串作为主键，类似`8f55d96b-8acc-4636-8cb8-76bf8abc2f57`。GUID算法通过网卡MAC地址、时间戳和随机数保证任意计算机在任意时间生成的字符串都是不同的，大部分编程语言都内置了GUID算法，可以自己预算出主键。

- 对于大部分应用来说，通常自增类型的主键就能满足需求。
- 联合主键
  - 关系数据库实际上还允许通过多个字段唯一标识记录，即两个或更多的字段都设置为主键，这种主键被称为联合主键。
  - 对于联合主键，允许一列有重复，只要不是所有主键列都重复即可。

### 外键

- 外键：通过表中的某个与其他表相关联的字段，将本表的某条记录与外部表的一条记录关联起来的键。
  - 如在student表中增添一个class_id字段，将每一个学生都与一个班级关联起来，student与class两张表就关联起来了。
  - 通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果`classes`表不存在`id=99`的记录，`students`表就无法插入`class_id=99`的记录。

```sql
ALTER TABLE students
ADD CONSTRAINT fk_class_id -- 外键约束的名称fk_class_id可以任意
FOREIGN KEY (class_id) -- FOREIGN KEY (class_id)指定了class_id作为外键
REFERENCES classes (id);-- REFERENCES classes (id)指定了这个外键将关联到classes表的id列（即classes表的主键）。

-- 要删除一个外键约束，也是通过ALTER TABLE实现的：
ALTER TABLE students
DROP FOREIGN KEY fk_class_id;
-- 注意：删除外键约束并没有删除外键这一列。删除列是通过DROP COLUMN ...实现的。
```

### 一对一、多对多

- 一对一：
  - 一个表的记录对应到另一个表的唯一一个记录。**两个表的记录的个数肯定是相同的？**
  - 把一个大表拆成两个一对一的表，目的是把经常读取和不经常读取的字段分开，以获得更高的性能。
- 多对多：比如一个班级对应多个老师（比如九个科目），一个老师也会对应多个班级（比如一个老师教三个班的语文）。
  - 通过中间表来关联两个多对多的表。
  - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203182005072.png" alt="image-20220318200540975" style="zoom:50%;" />



### 索引

- 索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。
- 索引的效率取决于索引列的值是否散列，即该列的值如果越互不相同，那么索引效率越高。

```sql
-- 如果要经常根据score列进行查询，就可以对score列创建索引：
ALTER TABLE students
ADD INDEX idx_score (score); -- ADD INDEX idx_score (score)就创建了一个名称为idx_score，使用列score的索引。索引名称是任意的
-- 多列的情况：
ALTER TABLE students
ADD INDEX idx_name_score (name, score);
```

- 唯一索引：限制某一字段不能出现重复，比如限制不能重名。

```sql
ALTER TABLE students
ADD UNIQUE INDEX uni_name (name);-- 通过UNIQUE关键字我们就添加了一个唯一索引。
-- 也可以只对某一列添加一个唯一约束而不创建唯一索引：
ALTER TABLE students
ADD CONSTRAINT uni_name UNIQUE (name); -- 这种情况下，name列没有索引，但仍然具有唯一性保证。
```

## 查询数据

```sql
SELECT <字段名1，字段名2，字段名3...> -- SELECT * 表示查询所有
FROM <表名>
```

# MySQL必知必会

## 第1章

- 数据库：保存有组织的数据的容器。
  - 通常我们使用的是数据库管理系统DBMS，而不是直接使用数据库本身，DBMS代替我们访问数据库。

- 表的名称一定是唯一的，表是某种特定类型数据的结构化清单。
  - 表的完全限定名应该是唯一的，但是不同的数据库中可以有同名的表，相同数据库之中不能有相同名字的表。
- 模式(schema)：关于数据库和表的布局及特性的信息。
- 列(column)：表中的一个字段。所有的表都由一个或多个列组成。
- 行(row)：表中的数据是按行存储的，所保存的每个记录存储在自己的行内，行有时候也称作数据库记录(record)。
- 主键：一列或一组列，其值可以唯一区分表中每个行。
  - 任意两行都不具有相同的主键值。
  - 主键值不能为NULL。
  - 可以使用多个键联合作为主键，此时只要保证列植的组合唯一即可，单个列的值可以出现重复。
- 关于主键的一些习惯：
  - 不更新主键列中的值。
  - 不重用主键列的值。
  - 不在主键列中使用可能会更改的值。

## 第2章

- MySQL、Oracle、MicroSoft SQL Sever等数据库是基于客户机-服务器的数据库。
  - 服务器部分是负责所有数据访问和处理的一个软件，运行在数据库服务器上。
  - 与数据文件打交道的只有服务器软件，客户机负责发出请求，服务器返回结果。

## 第3章 使用MySQL

- ```sql
  SHOW TABLES -- 返回当前选中的数据库中的所有表，注意首先要选定使用(USE)哪一个数据库，才能SHOW TABLES
  ```

  - 可以使用`USE`指定某个数据库。

- 

- ```sql
  SHOW COLUMNS FROM customers-- 显示表列，但是要指定显示哪一个表的所有列，可以用FROM来指定
  ```

  - 每个字段返回一行。这一行包含字段名、数据类型、是否允许NULL、键信息、默认值等。
  - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203201438852.png" alt="image-20220320143817754" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203201440624.png" alt="image-20220320144058489" style="zoom: 50%;" />

## 第4章 检索数据

- 使用`SELECT`选择多个列时，列名之间用逗号分割，但是**最后一个列名后面不需要逗号**。
  - 使用通配符可能会使自己省事，毕竟不需要自己去检索列，但是检索不需要的列通常会降低检索和应用程序的性能。
  - 使用通配符能检索出名字未知的列。

- ```sql
  SELECT DISTINCT vend_id -- distinct只能放在列名前面。
  FROM products
  ```

  - 使用`DISTINCT`可以去重。
  - **`DISTINCT`应用于所有列而不仅是前置它的列。不能部分地使用`DISTINCT`**

 

- ```sql
  SELECT prod_name
  FROM products
  LIMIT 5
  ```

  - 可以使用`LITMIT`限制返回的行数。
  - 可以给LIMIT一个偏移量。`LIMIT 起始位置,最大行数`，注意两个值直接用逗号啊啊啊，**逗号**！！
  - 带一个值的`LIMIT`从第一行开始，给出的数为返回的行数，带两个值的LIMIT可以指定从行号为第一个值的位置开始。
  - 行数不够时，能返回多少就返回多少。
  - **检索出来的第一行为行0，而不是行1。**

- 可以使用完全限定的表名，形式是`数据库名.表名`。
- 可以使用完全限定的列名，形式是`表名.列名`。不同表中含有同名列时有作用。

## 第5章 排序检索数据

- 如果不明确规定排序顺序，则不应该假定检索出的数据的顺序有意义。

- ```sql
  SELECT prod_name AS 'name'
  FROM products
  ORDER BY 'name';
  ```

  - `ORDER BY`不一定要根据所选择的列来排序，也可以选择使用非选择列来排序，如上述可以使用`prod_id`来排列也可以。

- `ORDER BY`按多个列排序，多个列的列名之间用逗号分割即可。比如首先按prod_id排列，相同id之间使用名字的字典序排列。

  - 按多个列排序时，排序完全按所规定的顺序执行。

  - ```sql
    SELECT *
    FROM products
    ORDER BY prod_price,prod_name;-- 相同价格时，才按name进行排序，如果价格不存在相同的，则直接按照price排序。
    ```

- 指定排序方向。**默认的排序是升序的**

  - 使用`DESC`关键字指定降序排列。

  - `DESC`**关键字只应用到直接位于其前面的列名。**

  - **如果想在多个列上进行降序排列，必须对每一个列都使用DESC关键字。**

  - ```sql
    SELECT *
    FROM products
    ORDER BY prod_price DESC,prod_name;-- 对价格进行降序排列，而对名字使用默认的升序排列。
    ```

- 使用`LIMIT`和`ORDER BY`来检索最值。

  - 注意各个子句的顺序，LIMIT子句一定要在ORDER BY的后面，ORDER BY一定要在FROM后面。次序不对会报错。

    - ```sql
      SELECT *
      FROM products
      ORDER BY prod_price DESC-- 降序
      LIMIT 1; -- 限制为一个，得出的就是最大值
      ```

## 第6章 过滤数据

- `WHERE`子句

  - 要在`FROM`之后
  - 要在`ORDER BY`之前

- 各种大于等于小于小于等于，不等于之类的和Java没什么不同，**注意一下等于是**`=`不是`==`。

- BETWEEN：AND左边是左端点，右边是右端点，两边都取闭区间。

  - ```sql
    SELECT prod_name,prod_price
    FROM products
    WHERE prod_price BETWEEN 5 AND 10;-- BETWEEN 小端点值 AND 大端点值
    ```

- **空值检查：`IS NULL` 子句**

  - ```sql
    SELECT cust_id
    FROM customers
    WHERE cust_email IS NULL;-- 空值检查
    ```

## 第7章 数据过滤

- 且或非，AND OR NOT。都是用于WHERE，用来构建更复杂的条件，比如多个命题的复合。

  - **NOT否定跟在它之后的条件**

    ```sql
    SELECT *
    FROM customers
    WHERE cust_id NOT IN (10002, 10005)-- 既不是10002也不是10005
    ORDER BY cust_name;
    ```

    

- 运算顺序啥的，用括号括起来指定就好了。

- `IN`操作符：用来指定条件范围，范围中的每个条件都可以进行匹配。

  - 将条件放在括号中，用逗号隔开。

  - **IN操作符本身执行的就是`OR`操作符的功能，只不过它更清楚直观。**

  - ```sql
    SELECT *
    FROM customers
    WHERE cust_id IN (10002, 10005);-- 10002 OR 10005
    ```

## 第8章 用通配符进行过滤

- 通配符(wildcard)：用来匹配值的一部分的特殊字符。

- 搜索模式：由字面值、通配符或两者的结合构成的搜索条件。

- `%`通配符：用来表示 ：任何字符出现任意次数（包括零次）。

- 通配符可在搜索模式中任意位置使用，并且可以使用多个通配符。

- ```sql
SELECT *
FROM products
WHERE prod_name LIKE "jet%" -- 以jet开头的字符串
OR prod_name LIKE "%ton%"-- ton为子串的字符串
OR prod_name LIKE "s%e"-- 以s开头，e结尾的字符串
ORDER BY prod_name;
  ```

- ##### 下划线`_`通配符：

  - 一个下划线匹配一个字符，单个下划线为单个字符占位。

  - ```sql
    SELECT *
    FROM products
    WHERE prod_name LIKE "_ton"-- 长度为4个字符且最后三个字符为ton的字符串
    ORDER BY prod_name;
    ```

- 使用通配符的技巧：

  - 不要过度使用通配符。
  - 尽量不要把通配符用在搜索模式的开始处，这样是最慢的。

## 第9章 用正则表达式进行搜索

- 剩下的都比较细，不想看了，直接下一章。

- `REGEXP`：正则表达式关键字

  - `.`是正则表达式语言中一个特殊的字符，表示匹配任意一个字符。

  - ```sql
    SELECT *
    FROM products
    WHERE  prod_name REGEXP ".000"
    ORDER BY prod_name;
    ```

  - 进行OR匹配：匹配两个串之一，使用`|`

  - ```sql
    SELECT *
    FROM products
    WHERE  prod_name REGEXP "ton|jet"-- 对ton或者jet进行模式匹配，只要串中含有ton或者jet的都符合条件。
    ORDER BY prod_name;
    ```

  - 匹配几个字符之一：

  - **"1|2|3| ton"的匹配是"1"或者"2"或者"3"或者" ton",而不是1或2或3中的某一个再加上ton，注意区分。**

  - ```sql
    SELECT *
    FROM products
    WHERE  prod_name REGEXP "[1,2,3] ton"-- 方括号中的某一个字符加上ton作为匹配的模式。方括号中相当于OR
    ORDER BY prod_name;
    ```

    ```sql
    SELECT *
    FROM products
    WHERE  prod_name REGEXP "[^1,2,3] ton"-- 字符集合也可以被否定,加上 ^ 对"1或2或3"取了否定
    ORDER BY prod_name;
    ```

  - 匹配范围

    - 匹配0-9：[0-9]，范围不限于0-9.
    - 匹配a-1：[a-z]

    ```sql
    SELECT *
    FROM products
    WHERE  prod_name REGEXP "[1-3] ton"
    ORDER BY prod_name;
    ```

  - 匹配特殊字符：正则表达式内所有具有特殊含义的字符，用于匹配时都必须使用转义

  - ```sql
    SELECT *
    FROM products
    WHERE  prod_name REGEXP "\\."-- 对点(.)进行匹配。
    ORDER BY prod_name;
    ```

  - 匹配字符类：<img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203202054089.png" alt="image-20220320205458797" style="zoom:50%;" />



## 第10章 计算字段

- 只有数据库知道`SELECT`哪些语句中哪些列是实际的表列，哪些是计算字段，在客户机看来，计算字段的数据和其他列的数据返回的方式并没有什么不同。

- 使用`Concat()`将两个列的名字拼接：可以拼接多个串，各个串之间用逗号分割。

  - ```sql
    SELECT Concat(vend_name, "(", vend_country, ")")
    FROM vendors
    ORDER BY vend_name;
    ```

  - 效果：<img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203202114962.png" alt="image-20220320211421863"  />

  - 还可以使用别名，以方便客户机引用收到的数据。关键字`AS`

  - ![image-20220320211733454](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203202117541.png)

  - ```sql
    SELECT Concat(vend_name, "(", vend_country, ")") AS vend_title
    FROM vendors
    ORDER BY vend_name;
    ```

- 就是类似`order_item  * order_price AS total_price`，这个SQL也会帮你计算好。四则运算都可以

## 第11章 使用数据处理函数

- 处理文本串
- 处理数值数据
- 处理日期和时间值并从其中提取特定成分
- 返回DBMS正在使用的信息。
- 不想看细节了，大概看一下，然后当做字典一样来使用吧‘
- **日期使用yyyy-mm-dd的格式，如2000-11-21，首选，因为排除多义性。**
- <img src="C:/Users/jiangyiqing/AppData/Roaming/Typora/typora-user-images/image-20220320214358953.png" alt="image-20220320214358953" style="zoom:67%;" />
- 日期库<img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203202143175.png" alt="image-20220320214326927" style="zoom:67%;" />

- 数学库<img src="C:/Users/jiangyiqing/AppData/Roaming/Typora/typora-user-images/image-20220320215604983.png" alt="image-20220320215604983" style="zoom:50%;" />





## 第12章 汇总数据

<img src="C:/Users/jiangyiqing/AppData/Roaming/Typora/typora-user-images/image-20220320215958916.png" alt="image-20220320215958916" style="zoom: 50%;" />

- AVG()，参数是某一列，可以通过WHERE子句将列精准定义为这一列的某一行或多行。
- 为了获得多个列的均值，必须使用多个AVG()

## 第13章 数据分组



- 可以将数据按列分为多个逻辑组，以对每个组进行聚集计算。

- ```sql
  SELECT vend_id, COUNT(*) AS prods_num
  FROM products
  GROUP BY vend_id; -- GROUP BY指示MySQL按照分组来进行聚集计算，而不是对整个结果进行。
  ```

- 关于`GROUP BY`：

  - 子句可以包含任意数目的列，使得分组可以进行嵌套，为数据分组提供更细致的控制。
  - 如果子句中嵌套了分组，那么数据将在最后规定的分组上进行汇总。
  - 子句中列出的每个列都必须是检索列或有效的非聚集函数的表达式。
  - 如果在SELECT中使用表达式，在GROUP BY中也要使用相同的表达式。不能使用别名。
  - `GROUP BY`在`WHERE`之后，`ORDER BY`之前。

- 过滤分组：

  - WHERE过滤指定的是行而不是分组。

  - HAVING支持WHERE的所有操作符。

  - 可以看做WHERE是分组前过滤，而HAVING是分组后过滤。

  - ```SQL
    SELECT cust_id, COUNT(*) AS "orders"
    FROM orders
    GROUP BY cust_id
    HAVING COUNT(*) >= 2;-- 过滤分组中订单数大于等于2的
    ```

- **不要依赖ORDER BY来排序，ORDER BY是确保排序正确的唯一方法**

- SQL子句的顺序：

- ```sql
  SELECT -- 要返回的列或者表达式
  FROM -- 从哪一张表检索数据
  WHERE -- 对行进行过滤
  GROUP BY -- 对数据进行分组
  HAVING -- 对组进行过滤
  ORDER BY -- 对输出进行排序
  LIMIT -- 限制返回的结果的行数
  ```



## 第14章 使用子查询

- 子查询：嵌套在查询中的查询。
  - 对于查询的子查询的数目是没有限制的。
  - 但是实践中由于性能的限制，不会嵌套太多的子查询。
  - **子查询除了与IN操作符结合使用，也可以与等于、不等于等结合使用。**

- 情境构建：查询购买了昏睡红茶的所有客户。

  - 查询包含昏睡红茶的所有订单的编号（FROM order_items）。

  - 查询前一步的订单中的所有的客户ID（FROM orders）。

  - 根据客户ID查询客户信息（FROM customers）。

  - 每一步都可以作为一个单独的查询。

  - 使用良好的缩进更有可读性。

    ```SQL
    SELECT cust_id-- 将前两步借助子查询组合起来
    FROM orders
    WHERE order_num IN (SELECT order_num
                        FROM orderitems
                        WHERE prod_id = '114514'
                       );
    -- 将剩下的两步又嵌套起来：总共嵌套三层
    SELECT cust_name, cust_contact
    FROM customers
    WHERE cust_id IN (SELECT cust_id-- 将前两步借助子查询组合起来
                      FROM orders
                      WHERE order_num IN (SELECT order_num
                                          FROM orderitems
                                          WHERE prod_id = '114514'));
    ```

- 作为计算字段使用子查询，就是可以用`AS`。

- 注意可能需要使用完全限定列名来避免二义性。



## 第15章 联结表

- 外键：外键为某个表中的某一列，它包含另一个表的主键值，定义了两个表之间的关系。

- 可伸缩性（scale）能够适应不断增加的工作量而不失败。设计良好的应用程序或数据库称为可伸缩性好(scale well)

- 联结解决的问题：如果需要的数据存储在多个表中，怎样用单条SELECT语句检索出数据？

- 应该叫做，隐式联结？毕竟没有用到`JOIN`关键字，直接借助`WHERE`来联结，**没有WHERE的话，第一张表的每一行将与第二张表的每一行匹配。**

  - ```sql
    SELECT vend_name, prod_name, prod_price -- 选择的三个列并不都属于同一张表 
    FROM vendors, products-- FROM这里列出了两个表
    WHERE vendors.vend_id = products.vend_id -- 这里使用了完全限定名，避免二义性
    ORDER BY vend_name, prod_name;
    ```

- 内部联结：也称作等值联结，基于两个表之间的相等测试。

  - 用`JOIN ON` 来写一下上面那个例子

  - ```sql
    SELECT vend_name, prod_name,prod_price
    FROM vendors 
    INNER JOIN products
    ON vendors.vend_id = products.vend_id;
    ```

- 联结多个表：SELECT对联结的表的数目没有限制，首先列出所有表，然后定义表之间的关系。

  - ```sql
    SELECT vend_name, prod_name, prod_price,quantity
    FROM vendors, products, orderitems 
    WHERE vendors.vend_id = products.vend_id
          AND products.prod_id = orderitems.prod_id
          AND order_num = 20005;
    -- 等价的JOIN貌似是这样
    SELECT vend_name, prod_name,prod_price,quantity
    FROM vendors
    JOIN products
    ON vendors.vend_id = products.vend_id
    JOIN orderitems 
    ON products.prod_id = orderitems.prod_id;
    WHERE order_num = 20005;
    ```

  - **联结的表越多，性能下降越厉害。不要联结不必要的表。**





## 第16章 创建高级联结

- SQL允许给计算字段、表名、列名取别名。

  - ```sql
    SELECT vend_name, prod_name, prod_price,quantity
    FROM vendors v, products p, orderitems o
    WHERE v.vend_id = p.vend_id
          AND p.prod_id = o.prod_id
          AND order_num = 20005;
    -- 要AS的版本和不要AS的版本
    SELECT vend_name, prod_name, prod_price,quantity
    FROM vendors AS v, products AS p, orderitems AS o
    WHERE v.vend_id = p.vend_id
          AND p.prod_id = o.prod_id
          AND order_num = 20005;
    ```

- 自联结：

  - ```sql
    -- 使用子查询
    SELECT prod_id, prod_name
    FROM products
    WHERE vend_id = (SELECT vend_id
                     FROM products
                     WHERE pro_id = "DTNTR"
                    );
    -- 使用自联结来达到相同的效果
    SELECT p1.prod_id, p1.prod_name
    FROM products AS p1 , products AS p2 -- 同一张表使用了两个别名，避免对products的引用具有二义性，可以看做是一张表和其副本。
    WHERE p1.vend_id = p2.vend_id
    AND   p2.prod_id = 'DTNTR';
    ```

- 自然联结：自然联结排除多次出现，对于重复出现的列，只保留一次。这个工作需要我们手动完成。

  - 一般是对一个表使用通配符，对其他联结的表的列都明确指出需要哪个列来联结。

- 外部联结：联结包含了那些在相关表中没有关联行的行。

  - `LEFT`指明选择`OUTER JOIN`左边的表的所有行，`RIGHT`以此类推。

  - **左联结和右联结完全可以通过调转`OUTER JOIN`两边的表名的位置直接相互转换。**

  - ```sql
    SELECT customers.cust_id, orders.order_num
    FROM customers LEFT OUTER JOIN orders -- 选择customers表的所有行
    ON customers.cust_id = orders.cust_id;
    ```

- **联结与聚集函数也可以一起使用**。



## 第17章 组合查询

需要使用组合查询的情况：

- 在单个查询中从不同的表返回类似结构的数据。

- 对单个表执行多个查询，按单个查询返回数据。

- ```sql
  SELECT vend_id, prod_id, prod_price
  FROM products
  WHERE prod_price <= 5
  UNION
  SELECT vend_id, pro_id,prod_price-- 两个SELECT必须要有完全相同的列
  FROM products
  WHERE vend_id IN (10001, 10002);
  ```

- 使用`UNION`的规则：
  - UNION必须由两条或两条以上`SELECT`语句组成，语句之间用关键字UNION分割。
  - UNION中的每个查询必须包含相同的列、表达式或聚集函数（但是不要求列出的顺序相同）
  - 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐含地转换的类型。
- `UNION`默认会去除重复的行，想返回所有匹配行，使用`UNION ALL`
- 只允许使用一条`ORDER BY`子句，必须出现在最后一条`SELECT`语句之后。

## 第18章 全文本搜索（先留坑吧）

## 第19章 插入数据

- 插入完整的行：这是最安全的做法，第一个列名对应VALUE中的第一个列植，以此类推。**这里列出个字段的顺序与表中实际的顺序无关**

  - ```sql
    -- 即使实际的表的字段的存储顺序改变，这个插入也可以正常工作
    INSERT INTO customers(cust_name, -- 这里这个列名可以按照任意顺序列出，只要保证VALUE里的值和各个字段对应就可以。
                          cust_address,
                          cust_city,
                          cust_state,
                          cust_zip,
                          cust_country,
                          cust_contact,
                          cust_email)
                          VALUES(
                              'Pep E.Lapew',-- 这里都用单引号，不要用ESC下面那么反引号
                              '1000 Main Street',
                              'LA',
                              'CA',
                              '90046',
                              'USA',
                              NULL,
                              NULL
                          );
    ```

  - 可以在插入时省略某些列，如果这个列定义为允许NULL值或者是给出默认值。否则插入不成功。

  

- 插入行的一部分

- 插入多行

  - 可以使用多条`INSERT`，每条语句用一个分号结尾：

  - ```sql
    INSERT INTO customers(cust_name, 
                          cust_address,
                          cust_city,
                          cust_state,
                          cust_zip,
                          cust_country,
                          cust_contact,
                          cust_email)
                          VALUES(
                              'Pep E.Lapew',-- 这里都用单引号，不要用ESC下面那么反引号
                              '1000 Main Street',
                              'LA',
                              'CA',
                              '90046',
                              'USA',
                              NULL,
                              NULL
                          );
    INSERT INTO customers(cust_address,
                          cust_name,
                          cust_city,
                          cust_state,
                          cust_zip,
                          cust_country,)
                          VALUES(
                              '1000 Avenue Street',
                              'Troye Sivan',-- 这里都用单引号，不要用ESC下面那么反引号
                              'LA',
                              'CA',
                              '90046',
                              'USA',
                          );
    ```

  - **如果每条INSERT语句中的列名和次序相同，可以将句子合并：**可以提高性能。

  - ```sql
    INSERT INTO customers(cust_name, 
                          cust_address,
                          cust_city,
                          cust_state,
                          cust_zip,
                          cust_country,
                          cust_contact,
                          cust_email)
                          VALUES( -- 合并之后就只有一个VALUES
                              'Pep E.Lapew',-- 这里都用单引号，不要用ESC下面那么反引号
                              '1000 Main Street',
                              'LA',
                              'CA',
                              '90046',
                              'USA'
                          ), -- 这里是逗号不是分号
                          (
                              '1000 Avenue Street',
                              'Troye Sivan',-- 这里都用单引号，不要用ESC下面那么反引号
                              'LA',
                              'CA',
                              '90046',
                              'USA'
                          );
    ```

- 插入检索出的数据：利用INSERT插入一条SELECT语句检索的数据。

  - **MySQL使用的是列的位置来对应填充，SELECT中第一列将会填充表列中的第一列，以此类推。**
  - `SELECT`中可以包含`WHERE`子句来过滤插入的数据。
  
  ```sql
  INSERT INTO customers(cust_id,
                        cust_email,
                        cust_name,
                        cust_address,
                        cust_city,
                        cust_state,
                        cust_zip,
                        cust_country
                       )
                       SELECT-- 上下列出的列要一一对应上，并且SELECT会把custnew中的所有数据都插入到customers中
                       cust_id,
                       cust_email,
                       cust_name,
                       cust_address,
                       cust_city,
                       cust_state,
                       cust_zip,
                       cust_country
                       FROM custnew;
  ```
  
  

## 第20章 更新和删除数据

- `UPDATE`语句由三部分组成：

  - 要更新的表

  - 列名和它们的新值

  - 确定要更新的行的过滤条件。

  - **如果用UPDATE语句更新多行，并且在更新的过程中有一行以上发生错误，则整个UPDATE操作被取消（并且回滚出错前的所有更新）**

  - **要告知MySQL即使发生错误也继续更新，使用`IGNORE`关键字。**

  - 删除某个列的值，可以将它设置为NULL。

    ```sql
    UPDATE (IGNORE) customers
    SET cust_name = "New_Name",-- 更新多个列时，每个“列 = 值”对之间用逗号分隔。
        cust_email = "New_email"
    WHERE cust_id = 10005;-- 如果没有WHERE子句，MySQL将会用新的值更新表中所有的行
    ```

- `DELETE`语句由两部分构成：

  - 要从中删除的表（**DELETE可能删除表的每一行，但是并不删除表本身**）

  - 过滤条件

  - **`DELETE`删除整行而不是删除列。**

  - **为了删除指定的列，使用`UPDATE`**

  - 如果想更快地删除表中所有行，使用`TRUNCATE TABLE`（它直接删除表，然后创建一个新的空表，而不是逐行删除）

  - ```sql
    DELETE FROM customers
    WHERE cust_id = 10005; -- 如果省去WHERE子句，MySQL将删除表中每一行
    ```

- **更新和删除的指导原则：**

  - 除非确实打算更新或删除每一行，否则绝对不要使用不带`WHERE`子句的`UPDATE`或`DELETE`。
  - 保证每个表都有主键，尽可能像`WHERE`子句那样使用它。
  - 在对`UPDATE`或`DELETE`语句使用`WHERE`子句之前，应该先用`SELECT`进行测试，保证`WHERE`过滤的是正确的记录。



## 第21章 创建和操纵表

- 表创建基础：

- ```SQL
  -- 教材创建customers表时所用的
  CREATE  IF   NOT EXISTS customers -- 表名
  (cust_id 	  int NOT NULL AUTO_INCREMENT, -- 列名 列的数据类型 是否可以为空 是否自增
   cust_name    char(50) NOT NULL,
   cust_address char(50) NULL,
   cust_city	  char(50) NULL,
   cust_state   char(50) NULL,
   cust_zip     char(50) NULL,
   cust_country char(50) NULL,
   cust_contact char(50) NULL,
   cust_email   char(255) NULL,
   PRIMARY KEY (cust_id) -- 主键
  )ENGINE = InnoDB;
  ```

- 使用NULL：
  - 被定义为`NOT NULL`的字段，如果在插入时没有给定值，会插入失败。
  - 如果不指定`NOT NULL`，创建表时将会默认字段可以为`NULL`。

- 要创建由多个列组成的主键，应该以逗号分隔的列表给出各列名。

  - ```sql
    PRIMARY KEY (cust_id, cust_name);
    ```

- `AUTO_INCREMENT`告知MySQL，每当本列增加一行时自动增量。**每个表只允许一个`AUTO_INCREMENT`列，而且它必须被索引。**

- 可以使用`DEFAULT`关键字给字段指定默认值

  - ```sql
    CREATETABLE orders
    (
        order_quantity int NOT NULL DEFAULT 1
        ......
    ).....
    ```

- 引擎类型：不同表之间可以混用引擎，外键不能跨引擎

  - InnoDB是一个可靠的事务处理引擎，但不支持全文本搜索。
  - MyISAM是一个性能极高的引擎，支持全文本搜索，但不支持事务处理。
  - MEMORY在功能上等同于MyISAM，但由于数据存储在内存而不是硬盘中，速度很快，特别适合于临时表。

- 更新表：

  - ```sql
    ALTER TABLE vendors
    ADD vend_Phone char(20);-- 给表添加一个列
    
    ALTER TABLE vendors
    DROP vend_Phone;-- 给表删除一个列
    
    ALTER TABLE orderitems
    ADD CONSTRAINT fk_orderitems_products -- 给表添加一个外键
    FOREIGN KEY (prod_id)
    REFERENCES products(prod_id)
    ```

- 复杂的表结构更改步骤：

  - 用新的列布局创建一个新表
  - 使用`INSERT SELECT`从旧表复制数据到新表。如果有必要可以使用转换函数和计算字段
  - 检验包含所需数据的新表
  - 重命名旧表
  - 用旧表原来的名字重命名新表
  - 根据需要，重新创建触发器、存储过程、索引和外键。

- 删除整个表本身：

  - ```sql
    DROP TABLE newcustomers; -- 删除表没有确认，也不能撤销
    ```

- 重命名表：

  - ```sql
    RENAME TABLE
                Backup_customers TO Customers,
                Backup_vendors TO Vendors,
                Backup_products TO Products;
    ```

## 第22章 使用视图

- 视图是虚拟的表，它不包含表中应该有的任何列或数据，它包含的是一个SQL查询。

- 视图的用处：

  - 重用SQL语句。利用视图，可以一次性编写基础的SQL，然后根据需要多次使用。其实类似自定义的方法。
  - 简化复杂的SQL操作。
  - 使用表的组成部分而不是整个表。
  - 保护数据。可以授予用户对表的部分的访问权限而不是整个表的访问权限。

  - 更改数据格式和表示。视图可返回于底层表的表示和格式不同的数据。

- 视图创建后可以当做一个正常的表来使用。

- 视图的规则和限制：

  - 唯一的命名。不能于表或其他视图重名。
  - 可创建的视图数目没有限制。
  - 为了创建视图，要有足够的访问权限。
  - 视图可以嵌套。
  - `ORDER BY`可以用在视图中，也可以被SELECT FROM视图中的排序覆盖。
  - 视图不能索引，也不能有关联的触发器或默认值。
  - 视图可以和表一起使用。

- 使用视图：

  - ```sql
    CREATE VIEW viewName -- 创建视图
    SHOW CREATE VIEW viewName -- 查看创建视图的语句
    DROP VIEW ViewName -- 删除视图
    CREATE OR REPLACE VIEW -- 更新视图
    ```

- 创建视图：

  - ```sql
    CREATE VIEW Products_And_Customers AS -- 创建视图
    SELECT cust_name, cust_contact, prod_id
    FROM customers, orders, orderItems
    WHERE customers.cust_id = orders.cust_id
    	  AND orderItems.order_num = orders.order_num;
    -- 从视图中查询数据
    SELECT cust_name,cust_contact
    FROM products_and_customers
    WHERE prod_id = 'tnt2';
    ```

- 使用视图重新格式化检索出的数据：

  - 如果那行格式化的数据经常需要使用的话，可以创建一个视图，每次都使用它。

  - ```SQL
    CREATE VIEW vendor_Locations AS -- 创建视图，格式化数据
    SELECT ConCat(Rtrim(vend_name), "(", Rtrim(vend_country), ")") AS vend_title
    FROM vendors
    ORDER BY vend_name;
    
    -- 使用视图
    SELECT *
    FROM Vendor_Locations;
    ```

- 使用视图过滤：

  - ```sql
    CREATE VIEW customer_email_list AS
    SELECT cust_id, cust_email, cust_name
    FROM customers
    WHERE cusm_mail IS NOT NULL;
    -- 使用过滤后的视图
    SELECT *
    FROM customer_email_list;
    ```

- 使用视图与计算字段

  - ```sql
    CREATE VIEW order_items_expanded AS
    SELECT order_num, prod_id, quantity, item_price,
    		quantity * item_price AS expanded_price
    FROM orderitems;
    -- 计算字段将会成为一列
    SELECT *
    FROM order_items_expanded
    WHERE order_num = 20005;
    ```

- 更新视图：留个坑。一般不去更新视图。



## 第23章 使用存储过程

- 存储过程简单来说，就是为以后的使用而保存的一条或多条MySQL语句的集合，可将其是为批文件。实际上是一个函数。

- 为什么要使用存储过程：

  - 通过把处理封装在容易使用的单元中，简化复杂的操作。
  - 防止错误，保证数据的一致性。
  - 简化对变动的管理。
  - 通过存储过程限制对基础数据的访问减少了数据讹误。
  - 提高性能。使用存储过程比使用单独的SQL语句快。

- 但是需要创建存储过程的安全访问权限。

- 执行存储过程：

  - MySQL将存储过程的执行称为调用(`CALL`)。
  - `CALL`接受存储过程的名字以及需要传递给它的任意参数。

- 创建存储过程：存储过程实际上就是函数，括号是参数列表。

  - ```SQL
    DELIMITER // -- 使用MySQL workbench的时候要加上分隔符覆盖，重新定义分隔符
    CREATE PROCEDURE productpricing()
    BEGIN 
    	SELECT Avg(pro_price) AS priceAverage
    	FROM products;
    END //
    DELIMITER ; -- 这里又重新恢复分隔符的定义为分号。
    ```

- 调用存储过程

  - ```SQL
    CALL productpricing();
    ```

- 删除存储过程

  - ```SQL
    DROP PROCEDURE productpricing; -- 可选 DROP PROCEDURE IF EXISTS
    ```

- 变量：内存中的一个特定位置，用来临时存储数据。

  - **所有MySQL变量都必须以@开始。**

- 使用参数：

  - `OUT`指出相应的参数用来从存储过程传出一个值，给调用者。

  - `IN`：传递给存储过程

  - `INOUT`：对存储过程传入和传出

  - ```sql
    DELIMITER //
    CREATE PROCEDURE productpricing(-- 这里是函数的参数列表
        OUT p1 DECIMAL(8, 2), -- OUT指出相应的参数用来从存储过程传出一个值，给调用者。
        OUT ph DECIMAL(8, 2), --  
        OUT pa DECIMAL(8, 2)
    )
    BEGIN-- 存储过程的代码
    	SELECT Min(prod_price)
    	INTO p1 -- INTO指示将值保存到相应的变量
    	FROM products;
    	SELECT Max(prod_price)
    	INTO ph
    	FROM products;
    	SELECT Avg(prod_price)
    	INTO pa
    	FROM products;
    END //
    DELIMITER ;
    ```

  - 使用`OUT`的栗子：

  - ```SQL
    DELIMITER //
    CREATE PROCEDURE ordertotal(
        IN onumber INT,
        OUT ototal DECIMAL(8, 2)
    )
    BEGIN
    SELECT Sum(item_price * quantity)
    FROM orderitems
    WHERE order_num = onumber
    INTO ototal;
    END //
    DELIMITER ;
    -- 使用上述存储过程
    CALL ordertotal(20005, @total);
    SELECT @total;
    ```

- 建立智能存储过程（开始将代码推向循环和跳转了）

  - ```SQL
    -- NAME: ordertotal
    -- parameters: onumber = order number
    -- 			  taxable = 1 if taxable
    -- 			  ototal = order total variable
    DELIMITER //
    CREATE PROCEDURE ordertotal(
        IN onumber INT,
        IN taxable BOOLEAN,
        OUT ototal DECIMAL(8, 2)
    )COMMENT "Obtain order total, optionally adding tax" -- 一个注释
    BEGIN
    	-- Declare variable for total
    	DECLARE total DECIMAL(8, 2); --  声明一个局部变量
    	-- Declare tax percengate
    	DECLARE taxRate INT DEFAULT 6;
    	
    	-- Get the order total
    	SELECT Sum(item_price * quantity)
    	FROM orderitems
    	WHERE order_num = onumber
    	INTO total;
    	
    	-- Is it taxable?
    	IF taxable THEN
    		SELECT total + (total / 100 * taxRate) INTO total;
    	END IF;
    	SELECT total INTO otatal; -- 将total的值赋给ototal
    END //
        DELIMITER ;
    -- 调用过程
    CALL ordertotal(20005, true, @total); -- 非0都视作真
    SELECT @total;
    ```

- 检查存储过程

  - ```SQL
    SHOW CREATE PROCEDURE ordertotal; -- 显示创建一个存储过程的语句
    SHOW PROCEDURE STATUS ; -- 列出所有存储过程。
    SHOW PROCEDURE STATUS LIKE "ordertotal"; -- 使用LIKE来过滤。
    ```

    





## 第24章 使用游标

- 游标(cursor)：是一个存储在MySQL服务器上的数据库查询，它不是一条`SELECT`语句，而是被该语句检索出来的结果集。
- **在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。**
- 游标主要用于交互式应用，其中用户需要滚动屏幕上的数据，并对数据进行浏览或做出更改。
- **MySQL的游标只能用于存储过程和函数。**所以存储过程完成后，游标就消失。
- 使用游标的步骤：
  - 声明一个游标。这个过程实际上没有检索数据，它只是定义要使用的`SELECT`语句。
  - 打开游标以供使用。这个过程使用第一步定义的`SELECT`语句把数据实际检索出来。
  - 对于填有数据的游标，根据需要取出行。
  - 结束游标使用时，必须关闭游标。
  - 在声明游标后可根据需要频繁打开和关闭游标。游标打开后可根据需要频繁执行取操作。

- 创建游标：
  - **存储过程处理完成后，游标就消失。**
  - `CLOSE`释放游标所使用的所有内部资源和内存，每个游标在不再需要时都应该关闭。
  - 使用声明过的游标不需要再声明，直接`OPEN` 
  - 如果不明确关闭游标，MySQL会在到达`END`语句时隐式关闭它。

- 

- ```sql
  CREATE PROCEDURE processorders()
  BEGIN 
  	DECLARE ordernumbers CURSOR -- 声明一个游标
  	FOR
  	SELECT order_num FROM orders; -- 可以根据需要带WHERE和其他子句
  	-- 打开游标
  	OPEN ordernumbers -- 此时执行查询
  	-- 中间应该还有一些对游标操作的语句..
  	CLOSE ordernumbers -- 关闭游标
  END;
  
  ```

- 使用游标数据：

  - 当一个游标被打开后，使用`FETCH`语句分别访问它的每一行。`FETCH`指定检索什么数据，检索出来的数据存储在什么地方。

  - `FETCH`会向前移动游标中的内部行指针，使得不会重复读取同一行。

  - 循环检索数据的栗子：

    - **局部变量必须在定义任何游标或句柄之前定义**
    - **句柄必须在游标之后定义。**

  - ```SQL
    DELIMITER //
    CREATE PROCEDURE processorders()
    BEGIN
    	-- 声明局部变量
    	DECLARE done BOOLEAN DEFAULT 0;
    	DECLARE o INT;
    	DECLARE t DECIMAL(8, 2);
    	DECLARE ordernumbers CURSOR
    	FOR
    	SELECT order_num FROM orders;
    
        DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done = 1; -- SQLSTATE '02000'是一个未找到条件，当没有更多行时出现
        CREATE TABLE IF NOT EXISTS ordertotals ( -- 新建了一个表，存储过程创建和填充这个表
            order_num INT,
            total DECIMAL(8, 2)
        );
    	-- 打开游标
    	OPEN ordernumbers;
    	-- 循环
    	REPEAT
    		FETCH ordernumbers INTO o;
    		CALL ordertotal(o, 1, t); -- 存储过程调用其他存储过程
    		INSERT INTO ordertotals(order_num, total)
    		VALUES(o, t);
    		UNTIL done 
    	END REPEAT;
    	CLOSE ordernumbers;
    END //
        DELIMITER ;
    ```

## 第25章 使用触发器（留部分坑）

- 触发器是MySQL响应以下任意语句而自动执行的一条MySQL语句（或一组语句，比如一个过程？）
  - DELETE
  - INSERT
  - UPDATE
  - 其他MySQL语句不支持触发器

- 创建触发器时需要给出的信息：
  - 唯一的触发器名（**最好这个名字是数据库范围内唯一的）**
  - 触发器关联的表（只有表才支持触发器，视图不支持）
  - 触发器应该响应的活动（DELETE、UPDATE、INSTER）
  - 触发器何时执行（处理之前还是处理之后）

```SQL
CRCREATE TRIGGER newproduct 
AFTER INSERT 
ON products
FOR EACH ROW  SELECT "Procudt Added" INTO @ee;
```

- 触发器按照每个表每个时间每次地定义，每个表每次事件每次只允许一个触发器。因此单表最多6个触发器。
- 触发器只能与一个事件一个表关联。
- 前置触发器失败，则不执行请求的事件。请求的事件失败，则不执行后置触发器。

```SQL
-- 删除触发器
DROP TRIGGER newproduct; -- 触发器只能先删除然后再重新创建以达到修改触发器的效果
```

- 

## 第26章 事务处理

