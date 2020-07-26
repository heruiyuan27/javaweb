#JDBC基础

概念：JDBC=Java DataBase Connectivity  Java 数据库连接， Java语言操作数据库

其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即==接口==。各个数据库厂商去实现这套接口，提供==数据库驱动jar包==。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的==实现类==。

<img src="C:\Users\Hery\Desktop\GitHub\javaweb\JDBC.assets\image-20200409200000460.png" alt="image-20200409200000460" style="zoom:80%;" />

- 导入驱动jar包 mysql-connector-java-5.1.37-bin.jar
  1.复制mysql-connector-java-5.1.37-bin.jar到项目的libs目录下
  2.右键-->Add As Library

#对象

##DriverManager：驱动管理对象

1. 注册驱动：告诉程序该使用哪一个数据库驱动jar:注册与给定的驱动程序 DriverManager 。 
	
	反射使用：  Class.forName("com.mysql.jdbc.Driver");
	
	```java
	//静态块调用静态方法：static void registerDriver(Driver driver) 完成注册 com.mysql.jdbc.Driver
	static {
		try {
			java.sql.DriverManager.registerDriver(new Driver());
		} catch (SQLException E) {
			throw new RuntimeException("Can't register driver!");
		}
	}
	//了解
	```
	

注意：mysql5之后的驱动jar包可以省略注册驱动的步骤。
					
2. 获取数据库连接：conn
	static Connection getConnection(String url, String user, String password) 
	
	![image-20200409200643152](C:\Users\Hery\Desktop\GitHub\javaweb\JDBC.assets\image-20200409200643152.png)
	
	jdbc:mysql://ip地址(域名):端口号/数据库名称
	
	```
	jdbc:mysql://localhost:3306/db3
	```
	
	特殊的，本机mysql服务器+端口3306
	
	```
	jdbc:mysql:///db3
	```
	
##Connection：数据库连接对象

1. 获取执行sql 的对象 pstm或 ==pstmt==		

Statement createStatement()
==PreparedStatement prepareStatement(String sql)==

2. 管理事务：
开启事务：.setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务
提交事务：.commit() 
回滚事务：.rollback() 

##Statement：执行sql的对象

1. boolean execute(String sql) ：可以执行任意的sql（了解） 
2. int executeUpdate(String sql) ：执行DML（insert、update、delete）语句、DDL(create，alter、drop)语句。返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值>0的则执行成功，反之，则失败。
3. ResultSet executeQuery(String sql)  ：执行DQL（select)语句

```java
public class JDBCDemo1 {
    public static void main(String[] args){

        Statement stmt=null;
        Connection conn=null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
            conn = DriverManager.getConnection("jdbc:mysql:///db5", "root", "root");
            stmt = conn.createStatement();
            String sql="insert into account values(null,'wangwu',3200)";
            int count = stmt.executeUpdate(sql);
            System.out.println(count);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            if(stmt!=null){//如果连接失败，stmt可能空指针异常
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn!=null){//如果连接失败，conn可能空指针异常
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

        }


    }
}
```
##PreparedStatement：预执行sql的对象√

1. prepareStatement()会先将 SQL 语句发送给数据库预编译。 PreparedStatement 会引用着预编译后的结果。
  可以多次传入不同的参数给 PreparedStatement 对象并执行。减少 SQL 编译次数，提高效率。

2. 安全性更高，没有 SQL 注入的隐患。

   | 方法                     | 说明                                                         |
   | ------------------------ | ------------------------------------------------------------ |
   | int executeUpdate()      | 用于发送 DML 语句，增删改的操作， insert、update、delete参数： SQL 语句返回值：返回对数据库影响的行数 |
   | ResultSet executeQuery() | 用于发送 DQL 语句，执行查询的操作。select 参数：SQL 语句返回值：查询的结果集 |


步骤：

```java
String sql = "select * from user where username = ? and password =?";
pstmt=conn.prepareStatement(sql);
pstmt.setString(1,username);
pstmt.setString(2,password);
rs = pstmt.executeQuery();
```

##ResultSet：结果集对象

boolean next(): 游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，如果是，则返回false，如果不是则返回true

```
rs.next()
```

getXxx(参数):获取数据：int getInt() ,String getString()
参数：可以是列序号，可以是列字段名称

    rs.getString(1)
    rs.getDouble("balance")

1. 游标向下移动一行
2. 判断是否有数据
3. 获取数据

```java
public static List<Emp> findAll() {
    Connection conn = null;
    Statement stmt = null;
    ResultSet rs = null;
    List<Emp> emplist = null;
    try {
        Class.forName("com.mysql.jdbc.Driver");
        conn = DriverManager.getConnection("jdbc:mysql:///db4", "root", "root");
        stmt = conn.createStatement();
        String sql = "select * from emp";
        rs = stmt.executeQuery(sql);
        emplist = new ArrayList<Emp>();
        Emp emp = null;
        while (rs.next()) {
            int id = rs.getInt("id");
            String ename = rs.getString("ename");
            int job_id = rs.getInt("job_id");
            int mgr = rs.getInt("mgr");
            Date joindate = rs.getDate("joindate");
            double salary = rs.getDouble("salary");
            double bonus = rs.getDouble("bonus");
            int dept_id = rs.getInt("dept_id");
            emp = new Emp();
            emp.setId(id);
            emp.setEname(ename);
            emp.setJob_id(job_id);
            emp.setMgr(mgr);
            emp.setJoindate(joindate);
            emp.setSalary(salary);
            emp.setBonus(bonus);
            emp.setDept_id(dept_id);
            emplist.add(emp);
        }
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    } finally {
        if (rs != null) {//如果连接失败，rs可能空指针异常
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (stmt != null) {//如果连接失败，stmt可能空指针异常
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {//如果连接失败，conn可能空指针异常
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    return emplist;
```

# JDBC工具类1：普通版

* 目的：简化书写

1. 静态块：获取url,user,password,driver,同时注册驱动
	配置文件src目录下jdbc.properties
url=
user=
password=
driver=

2. 获取connection对象方法

3. 释放资源-重载
	public static void close(ResultSet rs,Statement stmt, Connection conn)
	public static void close(Statement stmt, Connection conn)

```java
public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;

static{

    try {
        Properties pro=new Properties();
        ClassLoader classLoader = JDBCUtils.class.getClassLoader();
        String path = classLoader.getResource("jdbc.properties").getPath();
        pro.load(new FileReader(path));
        url = pro.getProperty("url");
        user = pro.getProperty("user");
        password = pro.getProperty("password");
        driver = pro.getProperty("driver");
        Class.forName(driver);
    } catch (IOException e) {
        e.printStackTrace();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
}
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url,user,password);
    }

    public static void close(ResultSet rs,Statement stmt, Connection conn){
        if (rs != null) {//如果连接失败，rs可能空指针异常
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (stmt != null) {//如果连接失败，stmt可能空指针异常
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {//如果连接失败，conn可能空指针异常
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

    }
    public static void close(Statement stmt, Connection conn){
        if (stmt != null) {//如果连接失败，stmt可能空指针异常
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {//如果连接失败，conn可能空指针异常
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
```

# JDBC事务

使用Connection对象来管理事务

1. 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务
  在执行sql之前开启事务
2. 提交事务：commit() 
  当所有sql都执行完提交事务
3.  回滚事务：rollback() 
  在catch中回滚事务

```java
try {
    conn=JDBCUtils.getConnection();
    conn.setAutoCommit(false);
    String sql1 = "update account set balance = balance - ? where id = ?";
    String sql2 = "update account set balance = balance + ? where id = ?";
    pstmt1=conn.prepareStatement(sql1);
    pstmt2=conn.prepareStatement(sql2);
    pstmt1.setDouble(1,500);pstmt1.setInt(2,1);
    pstmt2.setDouble(1,500);pstmt2.setInt(2,2);
    pstmt1.executeUpdate();
    pstmt2.executeUpdate();
    conn.commit();

} catch (Exception e) {
    try {
        if(conn != null) {
            conn.rollback();
        }
    } catch (SQLException e1) {
        e1.printStackTrace();
    }
    e.printStackTrace();
}
```

# 数据库连接池

概念：一个容器(集合)，存放数据库连接的容器。

当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。

好处：
1. 节约资源
2. 用户访问高效

标准接口：DataSource   （javax.sql）
1. 方法：
  获取连接：getConnection()

  归还连接：Connection.close()   如果连接对象Connection是从连接池中获取的。

2. 一般我们不去实现它，有数据库厂商来实现
    1. C3P0：数据库连接池技术

    2. Druid：数据库连接池实现技术，由阿里巴巴提供的

## C3P0

1. 导入jar包 (两个) c3p0-0.9.5.2.jar 	mchange-commons-java-0.2.12.jar ，
	* 不要忘记导入数据库驱动jar包
2. 定义配置文件：
	* 名称： c3p0.properties 或者 c3p0-config.xml
	* 路径：直接将文件放在src目录下即可。
3. 创建核心对象 数据库连接池对象 ComboPooledDataSource
4. 获取连接： getConnection

```java
DataSource ds = new ComboPooledDataSource();
Connection conn = ds.getConnection();
```

## Druid√

1. 导入jar包 druid-1.0.9.jar
2. 定义配置文件：
* 是properties形式的
* 可以叫任意名称，可以放在任意目录下
3. 加载配置文件。Properties
4. 获取数据库连接池对象：通过工厂来来获取  DruidDataSourceFactory
5. 获取连接：getConnection

```java
Properties pro=new Properties();
InputStream is =DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
pro.load(is);
DataSource ds = DruidDataSourceFactory.createDataSource(pro);
Connection conn = ds.getConnection();
```

#JDBC工具类2：Druid√

```java
public class JDBCUtils {
    private static DataSource ds;
    static{
        try {
            Properties pro=new Properties();
            InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
            pro.load(is);
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }
    public static void close(ResultSet rs , Statement stmt, Connection conn){
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    public static void close(Statement stmt, Connection conn){
        close(null,stmt,conn);
    }
    public static DataSource getDataSource(){
        return  ds;
    }
}
```

#SpringJDBC√

Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发。

```
JdbcTemplate template = new JdbcTemplate(ds);
```

调用JdbcTemplate的方法来完成CRUD的操作：

* update():
	* 执行DML语句。增、删、改语句
* queryForMap():
	* 查询结果将结果集封装为map集合，将列名作为key，将值作为value ，将这条记录封装为一个map集合【注意：这个方法查询的结果集长度只能是1】
* queryForList():
	* 查询结果将结果集封装为list集合【多条记录，Map→List】
* query():查询结果，将结果封装为JavaBean对象
	* query的参数：RowMapper
	* 一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
	* **new BeanPropertyRowMapper<类型>(类型.class)**
* queryForObject：查询结果，将结果封装为对象
	* 一般用于聚合函数的查询

```java
@Test
public void test2(){
	String sql="insert into emp (id,ename,dept_id) values(?,?,?) ";
	System.out.println(template.update(sql,1015, "郭靖", 10));
}
```

```java
@Test
public void test5(){
    String sql="select * from emp";
    List<Map<String, Object>> list = template.queryForList(sql);
    for(Map<String, Object> m:list){
        System.out.println(m);
    }
}
```

```java
@Test
public void test6(){
    String sql="select * from emp";
    List<Emp> list=template.query(sql, new BeanPropertyRowMapper<Emp>(Emp.class));
    for(Emp e:list){
        System.out.println(e);
    }
}
```

```java
@Test
public void test7(){
    String sql="select count(id) from emp";
    Integer i=template.queryForObject(sql,Integer.class);
    System.out.println(i);

}
```