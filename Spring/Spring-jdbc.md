

# 1、JdbcTemplate概念及使用

- Spring 框架对 JDBC 进行封装，使用 JdbcTemplate 方便实现对数据库操作

- 引入相关 jar 包

~~~xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.5</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.22</version>
    </dependency>
</dependencies>
~~~



~~~java
package com.feige.jdbc.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

/**
 * @author feige<br />
 * @ClassName: JdbcConfig <br/>
 * @Description: <br/>
 * @date: 2021/4/9 10:28<br/>
 */
@Configuration
@ComponentScan(basePackages = {"com.feige.jdbc"})
@EnableTransactionManagement
public class JdbcConfig {

    /**
     * @description: 往sprig容器注册一个德鲁伊数据源
     * @author: feige
     * @date: 2021/4/9 10:41
     * @return: com.alibaba.druid.pool.DruidDataSource
     */
    @Bean
    public DruidDataSource getDruidDataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setUrl("jdbc:mysql://localhost:3306/movie_db?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=UTF-8");
        druidDataSource.setUsername("root");
        druidDataSource.setPassword("123456");
        druidDataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        return druidDataSource;
    }

    /**
     * @description: 往sprig容器注册一个JdbcTemplate
     * @author: feige
     * @date: 2021/4/9 10:42
     * @return: org.springframework.jdbc.core.JdbcTemplate
     */
    @Bean
    public JdbcTemplate getJdbcTemplate(){
        return new JdbcTemplate(getDruidDataSource());
    }

    /**
     * @description: 往sprig容器注册一个事务管理器
     * @author: feige
     * @date: 2021/4/9 10:43
     * @return: org.springframework.jdbc.datasource.DataSourceTransactionManager
     */
    @Bean
    public DataSourceTransactionManager getDataSourceTransactionManager(){
        return new DataSourceTransactionManager(getDruidDataSource());
    }
}

~~~





```java
package com.feige.jdbc.dao;

import com.feige.jdbc.entity.User;

import java.util.List;

/**
 * @author feige<br />
 * @ClassName: UserDao <br/>
 * @Description: <br/>
 * @date: 2021/4/9 10:54<br/>
 */
public interface UserDao {

    boolean add(User user);

    /**
     * 测试事务
     * @param user
     * @return
     */
    boolean testTransactional(User user);

    boolean update(User user);

    boolean delete(Integer id);

    User getUserById(Integer id);

    List<User> getUsers();

}
```





```java
package com.feige.jdbc.dao;


import com.feige.jdbc.entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;


/**
 * @author feige<br />
 * @ClassName: UserDao <br/>
 * @Description: <br/>
 * @date: 2021/4/9 10:43<br/>
 */
@Repository
public class UserDaoImpl implements UserDao{

    @Autowired
    private JdbcTemplate jdbcTemplate;



    @Override
    public boolean add(User user) {
        //1 创建 sql 语句
        String sql = "insert into user values(?,?,?)";
        //2 调用方法实现
        Object[] args = {user.getId(),user.getUsername(),user.getPassword()};
        int update = jdbcTemplate.update(sql,args);
        return update > 0;
    }

    /**
     * @description: 测试事务
     * @author: feige
     * @date: 2021/4/9 11:12
      * @param user
     * @return: boolean
     */
    @Override
    @Transactional(rollbackFor = Exception.class)
    public boolean testTransactional(User user) {
        //1 创建 sql 语句
        String sql = "insert into user values(?,?,?)";
        //2 调用方法实现
        Object[] args = {user.getId(),user.getUsername(),user.getPassword()};
        int update = jdbcTemplate.update(sql,args);
        int update1 = jdbcTemplate.update(sql,args);
        return update > 0;
    }

    @Override
    public boolean update(User user) {
        //1 创建 sql 语句
        String sql = "update user set username = ?,password = ? where id = ?";
        //2 调用方法实现
        Object[] args = {user.getUsername(),user.getPassword(),user.getId()};
        int update = jdbcTemplate.update(sql,args);
        return update > 0;
    }

    @Override
    public boolean delete(Integer id) {
        //1 创建 sql 语句
        String sql = "delete from user where id = ?";
        //2 调用方法实现
        Object[] args = {id};
        int update = jdbcTemplate.update(sql,args);
        return update > 0;
    }

    @Override
    public User getUserById(Integer id) {
        String sql = "select id,username,password from user where id = ?";
        return jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<User>(User.class), id);
    }

    @Override
    public List<User> getUsers() {
        String sql = "select id,username,password from user";
        return jdbcTemplate.query(sql, new BeanPropertyRowMapper<User>(User.class));
    }

}
```





```java
package com.feige.jdbc.entity;

import java.io.Serializable;
import java.util.Date;

/**
 * (User)实体类
 *
 * @author makejava
 * @since 2021-04-09 10:47:48
 */
public class User implements Serializable {


    private static final long serialVersionUID = 830109298741667900L;

    public User(Integer id, String username, String password) {
        this.id = id;
        this.username = username;
        this.password = password;
    }

    public User() {
    }

    private Integer id;


    private String username;


    private String password;



    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```





```java
package com.feige.jdbc.spring;

import com.feige.jdbc.dao.UserDao;
import com.feige.jdbc.entity.User;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import java.util.List;

/**
 * @author feige<br />
 * @ClassName: SpringJdbcTest <br/>
 * @Description: <br/>
 * @date: 2021/4/9 10:28<br/>
 */
public class SpringJdbcTest {

    @Test
    public void test1(){
        ApplicationContext context = new AnnotationConfigApplicationContext("com.feige.jdbc");
        UserDao userDao = context.getBean(UserDao.class);
        boolean add = userDao.add(new User(3,"feige","123456"));
        System.out.println(add);
    }

    @Test
    public void test2(){
        ApplicationContext context = new AnnotationConfigApplicationContext("com.feige.jdbc");
        UserDao userDao = context.getBean(UserDao.class);
        User user = userDao.getUserById(1);
        System.out.println(user);
    }

    @Test
    public void test3(){
        ApplicationContext context = new AnnotationConfigApplicationContext("com.feige.jdbc");
        UserDao userDao = context.getBean(UserDao.class);
        List<User> users = userDao.getUsers();
        System.out.println(users);
    }


}
```