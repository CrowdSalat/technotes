# Jackson 

## ObjectMapper

[OVerview](https://www.baeldung.com/jackson-object-mapper-tutorial)

```java

ObjectMapper om = new ObjectMapper();

// create json string form object
String json = om.writeValueAsString (myJavaObject)

```

## annotation

- @JsonProperty
- @JsonAlias
- @JsonFormat e.g. parse a Date format

### it is your class

- setter/getter overload: does not work out of the box, one need to be annotated with @JsonSetter

### not your class

- you can not annotate the field, but you have no setter for a field: annotate the getter with @JsonProperty(access = JsonProperty.Access.READ_ONLY) ([But there is a bug in it](https://github.com/FasterXML/jackson-databind/issues/935))
- add @JsonIgnoreProperties(value="field", allowGetters = true, allowSetters = false) to class

## date parsing for java time

[helpful](https://stackoverflow.com/questions/27952472/serialize-deserialize-java-8-java-time-with-jackson-json-mapper)

#java #json #mapping
