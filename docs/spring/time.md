# Time

## JPA

TO save dates in UTC in database in spring boot you can set the following:

```shell
spring.jpa.properties.hibernate.jdbc.time_zone=UTC
```

## ZonedDateTime and daylight saving

- [List of time format strings](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)
- [List of valid zoneID for ZonedDateTime](https://mkyong.com/java8/java-display-all-zoneid-and-its-utc-offset/)

## date parsing for java time

[helpful](https://stackoverflow.com/questions/27952472/serialize-deserialize-java-8-java-time-with-jackson-json-mapper)
