# SQL Dialects

## Translate one dialect to another

- For non sensitive information use: https://www.jooq.org/translate/
- Otherwise use [CLI locally](https://www.jooq.org/doc/latest/manual/sql-building/sql-parser/sql-parser-cli/):

```shell
mkdir jooq-cli && cd jooq-cli   
curl -O -J -L https://repo1.maven.org/maven2/org/jooq/jooq/3.19.7/jooq-3.19.7.jar
curl -O -J -L https://repo1.maven.org/maven2/org/reactivestreams/reactive-streams/1.0.3/reactive-streams-1.0.3.jar
curl -O -J -L https://repo1.maven.org/maven2/io/r2dbc/r2dbc-spi/1.0.0.RELEASE/r2dbc-spi-1.0.0.RELEASE.jar
java -cp jooq-3.19.7.jar:reactive-streams-1.0.3.jar:r2dbc-spi-1.0.0.RELEASE.jar org.jooq.ParserCLI -h


--from-dialect=PostgresSQL
--to-dialect=
```
