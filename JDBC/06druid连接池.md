# 06druid连接池

## 硬编码

```java
/*
* 硬编码
* 1.创建一个druid连接池对象
* 2.设置连接池参数
* 3.获取连接
* 4.回收连接
* */
public void testHard() throws SQLException {
    DruidDataSource dataSource = new DruidDataSource();
    dataSource.setUrl("jdbc:mysql://127.0.0.1:3306/atguigu");
    dataSource.setUsername("root");
    dataSource.setPassword("085595");
    dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");

    dataSource.setInitialSize(5);//初始化连接数量
    dataSource.setMaxActive(10);//最大的数量

    Connection connection = dataSource.getConnection();

    //数据库操作

    connection.close();//可能是向上转型，重写了回收连接
}
```

## 软编码

```java
/*
* 通过读取外部配置文件的方式，实例化druid连接池对象
* */
@Test
public void testSoft() throws Exception {
    //读取外部配置文件 Properties
    Properties properties =new Properties();
	//src下的文件，可以使用类加载器提供的方法
    InputStream ips = druidUse.class.getClassLoader().getResourceAsStream("druid.properties");
    properties.load(ips);

    //使用连接池的工具类的工程模式
    DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);

    Connection connection = dataSource.getConnection();

    //数据库操作

    connection.close();
}
```

外部配置文件druid.properties

```
# druid配置文件
# druid连接池需要的配置参数,key固定命名
driverClassName=com.mysql.cj.jdbc.Driver
username=root
password=085505
url=jdbc:mysql://127.0.0.1:3306/atguigu
```

