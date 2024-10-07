# Logging with Java

## overview

- **loggers** define which package and which log level you want to use
- **appenders** define how messages are logged (File, Console, RollingFile etc.)
- you can connect one or more appenders to one logger
- if no appender 'catches' the log it will be handled by the root logger (which you need to connect to a logger as well as set a log level)

## scafffolding

- use slf4j with simple-logger to print to console
- if you want to configure loggers and appenders use log4j2 or logback
- if you have no strong opinion on that use logback because it rolling filling appender config is way more convenient
- **if you use spring boot** 
  - just use slf4j loggers in code (Spring boot uses logback under the hood)
  - if you want to configuration
    - configure with logging.level.* in properties.yaml
        - e.g. ogging.level.org=DEBUG  
    - or add a logback-spring.xml to resources folder
  - if you use logback-spring.xml you can use properties from appliyation.yaml with `<springProperty name="targetname" source="spring.property.name"/>`

### slf4j + simple logger

This setting does not need a configuration to work.

### slf4j + logback + spring

- remember it is called **logback not lockback**
- Paste listing content into src/main/resources/logback-spring.xml
- add app.name Property to src/main/resources/application.properties

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    
    <!-- to load default spring layout -->
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    
    <!-- to load properties from application.properties -->
    
    <property resource="application.properties" />
    
    <!-- app.name needs to be defined in application.properties (otherwise it will use '${app.name}' as folder name)-->

    <property name="APPLICATION_NAME" value="${app.name}"/>
    <property name="PATH_LOGS" value="/tmp/${APPLICATION_NAME}/logs" />
    <property name="LAYOUT" value="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!-- default spring layout -->
            <Pattern>${CONSOLE_LOG_PATTERN}</Pattern>
        </encoder>
    </appender>
    
    <appender name="RollingFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${PATH_LOGS}/${APPLICATION_NAME}.log</file>
        <encoder>
            <Pattern>${LAYOUT}</Pattern>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>${PATH_LOGS}/archived/${APPLICATION_NAME}-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <maxFileSize>10MB</maxFileSize>
            <!-- deletes oldest files when totalSizeCap or maxHostory is reached -->
            <totalSizeCap>100MB</totalSizeCap>
            <maxHistory>30</maxHistory> 
        </rollingPolicy>
    </appender>

    <logger name="de.weyrich" level="debug" additivity="false">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </logger>
    
    <root level="info">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </root>
    
</configuration>    
```

### slf4j + log4j2

- does not need a configuration to funciton
- add **log4j2.xml** to resources folder (not log4j.xml!)
- the name of the logger defines which packages are targeted. You do not need wildcards, you just end with the package part you want to include (e.g. de.weyrich) 
- you cann access system properties with `${sys:propertyName}`
Maven:

```xml

    <properties>
        <slf4j.version>1.7.5</slf4j.version>
        <log4j.version>2.13.3</log4j.version>
    </properties>
    ...
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>${slf4j.version}</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-slf4j-impl</artifactId>
        <version>${log4j.version}</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>${log4j.version}</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>${log4j.version}</version>
    </dependency>

```

log4j2.xml

```xml

<Configuration status="info">
    <Appenders>
        <File name="FILE" fileName="application.log">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </File>
        <Console name="CONSOLE">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="de" level="DEBUG">
            <AppenderRef ref="CONSOLE"/>
            <AppenderRef ref="FILE"/>
        </Logger>
         <!-- addtional loggers e.g. for used libraries-->

        <Root level="WARN">
            <AppenderRef ref="CONSOLE"/>
        </Root>
    </Loggers>
</Configuration>

```
