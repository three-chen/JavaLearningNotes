# 02preparedstatement

## statement存在的问题

需要字符串拼接，比较麻烦；

只能拼接字符串类型，其它的数据库类型无法处理；

可能发生sql注入，例：

```java
//                                            account改成' or '1' = '1
                                               //前面为空，后面恒成立
String sql = "select * from t_user WHERE account = '" + account +"' ;";
```

## preparedstatement

```java
//3.编写sql语句
String sql = "select * from t_user WHERE account = ? AND PASSWORD = ? ;";
//4.创建预编译statement并且设置SQL语句结构
PreparedStatement preparedStatement = connection.prepareStatement(sql);

//5.单独的占位符进行赋值
/*
* 参数1：index 占位符的位置 从左向右数 从 1 开始 账号 ? 1
* 参数2：object 占位符的值 可以设置任何类型的数据，避免我们拼接，让类型更加丰富
* */
preparedStatement.setObject(1,account);
preparedStatement.setObject(2,password);

//6.执行数据库操作
ResultSet resultSet = preparedStatement.executeQuery();
```

