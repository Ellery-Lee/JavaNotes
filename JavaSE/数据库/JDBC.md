# JDBC

Java Database Connectivity（Java语言连接数据库）

**JDBC是SUN公司制定的一套接口。**接口都有调用者和实现者。

**java.sql.；**这个软件包下有很多接口

- 为什么要面向接口编程？

**解耦合：**降低程序的耦合度，提高程序的扩展力。

多态机制就是非常典型的面向抽象编程。（不要面向具体编程）

- 为什么SUN制定一套JDBC接口呢？

因为每一个数据库的底层实现原理不一样。

![image-20200831112403126](D:\JavaHub\学习相关\Java笔记\pictures\image-20200831112403126.png)



**模拟JDBC实现：**

![image-20200831113512864](D:\JavaHub\学习相关\Java笔记\pictures\image-20200831113512864.png)

## 一、JDBC开发前准备工作

- 先从官网下载对应的驱动jar包，然后将其配置到环境变量classpath当中。

- classpath=.;E:\JAVA\mysql-connector-java-8.0.21\mysql-connector-java-8.0.21.jar

**以上的配置是针对于文本编辑器的方式开发，使用IDEA工具的时候，不需要配置以上的环境变量。IDEA有自己的配置方式**

## 二、JDBC编程六步（背会）

1. 注册驱动（告诉Java程序，即将要连接的是哪个品牌的数据库）
2. 获取连接（表示JVM的进程和数据库进程之间的通道打开了，这属于进程间通信，重量级的，使用完一定要关闭）
3. 获取数据库操作对象（专门执行sql语句的对象）
4. 执行sql语句（DQL、DML...）
5. 处理查询结果集（只有当第四步执行的是select语句的时候，才有这五步处理查询结果集）
6. 释放资源（使用完资源之后一定要关闭（先判断是不是null），Java和数据库属于进程间通信，开启后一定要关闭，**顺序由里到外：结果集--->数据库操作对象--->数据库连接**。）



## 三、IDEA配置JDBC

- 在Project下的Module右键

  ![Snipaste_2020-08-31_13-40-01](D:\JavaHub\学习相关\Java笔记\pictures\Snipaste_2020-08-31_13-40-01.png)

- 选择Java

  ![Snipaste_2020-08-31_13-43-36](D:\JavaHub\学习相关\Java笔记\pictures\Snipaste_2020-08-31_13-43-36.png)

- 找到jar包，点OK

  ![Snipaste_2020-08-31_13-44-24](D:\JavaHub\学习相关\Java笔记\pictures\Snipaste_2020-08-31_13-44-24.png)

- Choose Module(如果project下有多个模块，每个都需要导入)

  ![Snipaste_2020-08-31_13-45-04](D:\JavaHub\学习相关\Java笔记\pictures\Snipaste_2020-08-31_13-45-04.png)

## 四、使用StatementSQL执行对象（注入问题）

```java
package Login;

/*
实现功能：
    1.需求：模拟用户登录功能的实现
    2.业务描述：
        程序运行的时候，提供一个输入的入口，可以让用户输入用户名和密码。
        用户输入用户名和密码后，提交信息，java程序收集到用户信息。
        java程序连接数据库验证用户名和密码是否合法。
        合法：显示登录成功
        不合法：显示登陆失败
    3.数据的准备：
        在实际开发中，表的设计会使用专业的建模工具，这里使用建模工具:PowerDesigner
        使用PD工具来进行数据库表的设计（参见user_login.sql脚本）

    4.当前程序存在的问题：
        用户名：das
        密码：dsa' or '1'='1
        登陆成功
        这种现象被称为SQL注入（黑客经常使用）
    5.导致SQL注入的根本原因是什么？
        用户输入的信息中含有sql语句的关键字，并且这些关键字参与sql语句的编译过程，
        导致sql语句的原义被扭曲，进而达到sql注入。
 */

import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class JDBCtest {
    public static void main(String[] args) {
        //初始化一个界面
        Map<String, String>userLoginInfo = initUI();
        //验证用户名和密码

        boolean loginSuccess = login(userLoginInfo);

        //最后输出结果
        System.out.println(loginSuccess ? "登陆成功" : "登陆失败");
    }

    /**
     * 用户登录
     * @param userLoginInfo 用户登录信息
     * @return false表示失败，true表示成功
     */

    private static boolean login(Map<String, String> userLoginInfo) {
        //打标记
        boolean loginSuccess = false;
        //JDBC代码
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;


        try {
            //注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //获取连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc?serverTimezone=GMT","root", "Li19980503");
            //获取数据库操作对象
            stmt = conn.createStatement();
            //执行sql语句
            String sql = "select * from t_user where loginName = '"+userLoginInfo.get("loginName")+"' and loginPwd = '"+userLoginInfo.get("loginPwd")+"'";
            rs = stmt.executeQuery(sql);
            //处理查询结果集
            if(rs.next()){
                loginSuccess = true;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //释放资源
            if(rs != null){
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
                try{
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return loginSuccess;
    }

    /**
     * 初始化用户界面
     * @return 用户输入的用户名和密码等登录信息
     */

    private static Map<String, String> initUI() {
        Scanner s = new Scanner(System.in);

        System.out.println("用户名：");
        String userName = s.nextLine();

        System.out.println("密码: ");
        String passWord = s.nextLine();

        Map<String, String> userLoginInfo = new HashMap<>();
        userLoginInfo.put("loginName", userName);
        userLoginInfo.put("loginPwd", passWord);

        return userLoginInfo;
    }


}
```

## 五、使用PreparedStatement SQL执行对象（解决注入问题）

```java
package Login;

import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

/*主要解决SQL注入问题
    只要用户提供的信息不参与SQL语句的编译过程，问题就解决了
    要想用户信息不参与SQL语句的编译，必须使用java.sql.PreparedStatement
    PreparedStatement接口继承了java.sql.Statement
    PreparedStatement是属于预编译的数据库操作对象
    PreparedStatement的原理是：预先对SQL语句的框架进行编译，然后在给SQL语句传值
  测试结果：
    注入失败
  解决SQL注入的关键：用户提供的信息中即使含有sql语句关键字，不参与编译，不起作用
*/

public class JDBCtest01 {
    public static void main(String[] args) {
        //初始化一个界面
        Map<String, String>userLoginInfo = initUI();
        //验证用户名和密码

        boolean loginSuccess = login(userLoginInfo);

        //最后输出结果
        System.out.println(loginSuccess ? "登陆成功" : "登陆失败");
    }

    /**
     * 用户登录
     * @param userLoginInfo 用户登录信息
     * @return false表示失败，true表示成功
     */

    private static boolean login(Map<String, String> userLoginInfo) {
        //打标记
        boolean loginSuccess = false;
        //JDBC代码
        Connection conn = null;
        PreparedStatement ps = null; //这里使用PreparedStatement（预编译的数据库操作对象）
        ResultSet rs = null;


        try {
            //注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //获取连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc?serverTimezone=GMT","root", "Li19980503");
            //获取预编译的数据库操作对象
            //SQL语句的框架,其中一个？代表一个占位符，一个？将来接收一个值，注意占位符不能使用单引号括起来
            String sql = "select * from t_user where loginName = ? and loginPwd = ?";
            //程序执行到此处，会发送sql语句框子给DBMS，然后DBMS进行sql语句的预先编译
            ps = conn.prepareStatement(sql);
            //给占位符传值，第一个问号下表是1，第二个问号下标是2，JDBC中所有下标从1开始
            ps.setString(1,userLoginInfo.get("loginName"));
            ps.setString(2,userLoginInfo.get("loginPwd"));
            //执行sql语句
            rs = ps.executeQuery();
            //处理查询结果集
            if(rs.next()){
                loginSuccess = true;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //释放资源
            if(rs != null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(ps != null){
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try{
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return loginSuccess;
    }

    /**
     * 初始化用户界面
     * @return 用户输入的用户名和密码等登录信息
     */

    private static Map<String, String> initUI() {
        Scanner s = new Scanner(System.in);

        System.out.println("用户名：");
        String userName = s.nextLine();

        System.out.println("密码: ");
        String passWord = s.nextLine();

        Map<String, String> userLoginInfo = new HashMap<>();
        userLoginInfo.put("loginName", userName);
        userLoginInfo.put("loginPwd", passWord);

        return userLoginInfo;
    }
}
```



## 六、对比PreparedStatement和Statement

- Statement存在SQL注入问题，PreparedStatement解决了SQL注入问题
- Statement是编译一次执行一次，PreparedStatement是编译一次执行N次，效率更高
- PreparedStatement会在编一阶段做安全检查

**综上：PreparedStatement使用较多（例如传值），极少数使用Statement（业务方面要求必须sql注入，例如需要拼接sql语句）**



## 七、JDBC事务问题

1. JDBC的事务是自动提交的，什么事自动提交？

   **只要执行任意一条DML语句，则自动提交一次，这是JDBC默认的行为。**但是实际中，通常是N条DML语句共同联合才能完成，必须保证他们这些DML语句在同一个事务中同时成功或者失败。

2. 重点三行代码

   ```java
   conn.setAutoCommit(false);//禁用自动提交
   conn.commit();//提交
   conn.rollback();//出现异常，进行回滚
   ```

   ```java
   package ustc.java.jdbc;
   
   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.PreparedStatement;
   import java.sql.SQLException;
   
   public class JDBCTest12 {
       public static void main(String[] args) {
           Connection conn = null;
           PreparedStatement ps = null;
           try {
               // 注册驱动
               Class.forName("com.mysql.jdbc.Driver");
   
               // 获取连接
               conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase","root","146");
   
               // 将自动提交改为手动提交
               conn.setAutoCommit(false);
   
               // 获取预编译的数据库操作对象
               String sql = "update t_act set balance = ? where actno = ? ";
               ps = conn.prepareStatement(sql);
               ps.setInt(1,10000);
               ps.setDouble(2,111);
   
               // 执行sql语句
               int count = ps.executeUpdate();
   
               /*String s = null;
               s.toString();*/
   
               ps.setInt(1,10000);
               ps.setDouble(2,222);
               count += ps.executeUpdate();
   
               System.out.println(count == 2 ? "转账成功" : "转账失败");
   
               // 程序能执行到此处，说明没有异常，事务结束，手动提交数据
               conn.commit();
           } catch (Exception e) {
               // 遇到异常，回滚
               if (conn != null) {
                   try {
                       conn.rollback();
                   } catch (SQLException throwables) {
                       throwables.printStackTrace();
                   }
               }
               e.printStackTrace();
           }  finally {
               // 释放资源
               if (ps != null) {
                   try {
                       ps.close();
                   } catch (SQLException throwables) {
                       throwables.printStackTrace();
                   }
               }
               if (conn != null) {
                   try {
                       conn.close();
                   } catch (SQLException throwables) {
                       throwables.printStackTrace();
                   }
               }
           }
       }
   }
   ```



## 八、JDBC封装类的实现

在JDBC实现中，有很多代码写起来很繁琐，可以封装成工具类，简化JDBC编程。

工具类中的构造方法都是私有的，因为工具类当中的方法都是静态的，不需要new对象，直接采用类名调用。

## 九、悲观锁和乐观锁

**悲观锁：**事务必须排队执行。数据锁住了，不允许并发。（行级锁：select语句后面添加for update）

**乐观锁：**支持并发，事务也不需要排队，只不过需要一个版本号。