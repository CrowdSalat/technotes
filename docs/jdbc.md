# jdbc

## db2

- i highly recommend to activate jdbc traces while developing to see how the driver butchers your sql statements.
- maven dependecy: https://mvnrepository.com/artifact/com.ibm.db2/jcc
- does not like semicolons in queries
- you may need a space at the end of every line. New lines are ignored by the driver.
- complete query will executed in lowerCase. Casesensitive string compares may therefore be effected. You may want to use the lower() to workaround this issue. 
- you may need to execute ``Class.forName("com.ibm.db2.jcc.DB2Driver");`` before calling the connection if you are using jdbc version < 4.0
- example from ibm: https://www.ibm.com/support/knowledgecenter/SSEPEK_10.0.0/java/src/tpc/imjcc_cjvjdbas.html
- if you want to use Hibernate UUID in DB2, create it in DB with varchar36 so you can edit it by yourself (see @JdbcTypeCode(SqlTypes.VARCHAR))

## druid/ avatica core

[official doc](https://druid.apache.org/docs/latest/querying/sql.html#jdbc)

- **you can not use all druid sql language features over jdbc so you should consider using the rest api for queries instead**
- jdbc url against broker(default :8082) or router (default :8888) : `jdbc:avatica:remote:url=http://localhost:8888/druid/v2/sql/avatica/`
- Drivername: `org.apache.calcite.avatica.remote.Driver`
- user: if default not needed
- pw: if default not needed
- add driver to classpath:

```xml
<dependency>
    <groupId>org.apache.calcite.avatica</groupId>
    <artifactId>avatica-core</artifactId>
    <version>1.17.0</version>
</dependency>

```

## oracle thin

If you want to query a table which has lower case letters in it, you need to add quotes to the query e.g.:

´´´sql
select + from "flyway_schema_history"
´´´

Same goes for fieldnames. By default oracle thin driver send every table and field name in uppercase even if you wrote lowercase letter.

## integration into spring boot

- For mininal setup use `spring-boot-starter-jdbc`.
- If you want spring data like access use `spring-boot-starter-data-jdbc`, but then you may just use spring-data-jpa

### maven

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

### Config

```java
@Configuration
public class JdbcConfig{

    private String driverName;
    private String jdbcUrl;
    private String dbUser;
    private String dbPw;

    @Bean
    public Datasource datasource(){
    DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(driverName);
        dataSource.setUrl(jdbcUrl);
        dataSource.setUsername(dbUser);
        dataSource.setPassword(dbPw);
        return dataSource;
    }
}

```

### usage

Init JdbcTemplate with Datasource

```java

@Autowired
public YourDao(Datasource datasource){
    this.jdbcTemplate = new JdbcTemplate(datasource);
}

```

