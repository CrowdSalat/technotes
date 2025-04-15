# Spring Soap client

- add dependecy spring-boot-starter-web-services
- add maven plugin to generate classes from wsdl
- add client
- add client and marshaller as spring bean
- Wrap Request in JAXBElement if error "unable to marshal type "xyz" as an element because it is missing an @XmlRootElement annotation" happended

## add dependecy spring-boot-starter-web-services

Add to pom: 

```shell
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web-services</artifactId>
</dependency>
```

## add maven plugin to generate classes from wsdl

Add to pom:

```xml
   <!-- SOAP client-->

   ....

            <plugin>
                <groupId>org.jvnet.jaxb</groupId>
                <artifactId>jaxb-maven-plugin</artifactId>
                <version>4.0.8</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <schemaLanguage>WSDL</schemaLanguage>
                    <generateDirectory>${project.basedir}/src/main/java</generateDirectory>
                    <!-- also set in spring config -->
                    <generatePackage>de.weyrich.soapclient.gen</generatePackage>
                    <schemaDirectory>${project.basedir}/src/main/resources</schemaDirectory>
                    <schemaIncludes>
                         <include>meine.wsdl</include>
                    </schemaIncludes>
                </configuration>
            </plugin>

        </plugins>
    </build>
</project>
```

## add client

```java
package de.weyrich;
 
import de.weyrich.soapclient.gen.BeispielRequest;
import de.weyrich.soapclient.gen.BesipielResponse;
import jakarta.xml.bind.JAXBElement;
import org.springframework.ws.client.core.support.WebServiceGatewaySupport;
 
public class SoapClientExamle extends WebServiceGatewaySupport {
 
    public BeispielRequestResponse callBeispielRequest(String typeName) {
        BeispielRequestRequest request = new BeispielRequestRequest();
        request.setTypeName(typeName);
        return (BeispielRequestResponse) getWebServiceTemplate().marshalSendAndReceive(request);
    }
}

```

see last example if this thtows a @XMLRootElement error or an error that JAXB cannot find BeispielRequest.


## add client and marshaller as spring bean

```java
package de.weyrich;
 
import de.weyrich.SoapClientExamle;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.oxm.jaxb.Jaxb2Marshaller;
 
@Configuration
public class SoapClientConfig {
 
    @Bean
    public Jaxb2Marshaller marshaller() {
        Jaxb2Marshaller marshaller = new Jaxb2Marshaller();
        // must match plugin in pom
        marshaller.setContextPath("de.weyrich.soapclient.gen");
        return marshaller;
    }
    @Bean
    public SoapClientExamle SoapClientExamle(Jaxb2Marshaller marshaller) {
        SoapClientExamle client = new SoapClientExamle();
        client.setDefaultUri("https://example.de/soapservice");
        client.setMarshaller(marshaller);
        client.setUnmarshaller(marshaller);
        return client;
    }
}
```

## Wrap Request in JAXBElement if error "unable to marshal type "xyz" as an element because it is missing an @XmlRootElement annotation" happended

Use ObjectFactory to wrap request in JAXBElement. Needs to be done when @XmlRootElement is missing in generated classes which might happen because of leagcy WSDL stuff.


```java
package de.weyrich;
 
import de.weyrich.soapclient.gen.BeispielRequest;
import de.weyrich.soapclient.gen.BesipielResponse;
import de.weyrich.soapclient.gen.ObjectFactory;
import jakarta.xml.bind.JAXBElement;
import org.springframework.ws.client.core.support.WebServiceGatewaySupport;
 
public class SoapClientExamle extends WebServiceGatewaySupport {
 
    public BeispielRequestResponse callBeispielRequest(String typeName) {
        BeispielRequestRequest request = new BeispielRequestRequest();
        request.setTypeName(typeName);
        JAXBElement<BeispielRequestRequest> requestWrapped = new ObjectFactory().createBeispielRequestRequest(request);
        return (BeispielRequestResponse) getWebServiceTemplate().marshalSendAndReceive(requestWrapped);
    }
}
