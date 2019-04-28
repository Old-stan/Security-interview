* HQL注入特性
* 带有order by的limit，在limit语句后面的注入方法

  * 使用procedure analyse进行报错注入，只适用于5.0.0<mysql<5.6.6
* SQL注入写一句话语句
  * mssql
  * mysql
* mysql延时注入函数
  * sleep()
  * benchmark()
  * get_lock()：延时精确可控，利用环境有限，需要两个session
  * 笛卡尔积：

    ```sql
    count(*) FROM information_schema.columns A, information_schema.columns B, information_schema.columns C;
    ```

  * 通过rpad或repeat构造长字符串，加以计算量大的pattern，通过repeat的参数可以控制延时长短
    ```sql
    select rpad('a',4999999,'a') RLIKE concat(repeat('(a.*)+',30),'b');
    concat(rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a'),rpad(1,999999,'a')) RLIKE '(a.*)+(a.*)+(a.*)+(a.*)+(a.*)+(a.*)+(a.*)+b'
    ```

* 注入绕过
  * 宽字符：%df
  * 将字符串转为16进制或使用char函数（十进制）进行转化（数据库会自动把16进制转化）
  * 用注释符去掉输入密码部分如 “– / * #” 
  * 内联注释
  * 换行符代替空格。Windows换行符为%0A%0D，Linux为%0A

* PHP防御SQL注入
  * php.ini设置display_errors=Off，关闭错误提示
  * php.ini设置magic_quotes_gpc=On，开启魔术引号，5.3.0版本废弃，5.4.0版本移除
  * addslashes()函数，作用和魔术引号相同，两者冲突
  * mysql_real_escape_string()函数，转义SQL语句中的特殊字符，5.5.0版本废弃，7.0.0版本移除
  * htmlspecialchars()函数，把预定义的字符转换为HTML实体
  * 正则替换：preg_match、preg_match_all()、preg_replace
  * 转换数据类型
  * 预编译语句
  * PDO (使用序数参数）

