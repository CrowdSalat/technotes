# Misc reverse preoxy stuff

## Terms Reverseproxy vs API Gateway

Reverseproxy handels the following things

1. Load balancing policy
2. SSL/TLS Termination
3. SSL/TLS Encryption
4. Name based virtual hosting

API Gateway can additionally handle the following things:

1. API request validation
2. Payload transformation
3. rate limiting, quotas and throttling
4. retry policy

## reverse proxy in front of tomcat 9

By default tomcat 9 ignores x-forwarded header which can be a issue if you happen to have redirects in your app. To adress this you need to add [org.apache.catalina.valves.RemoteIpValve](https://tomcat.apache.org/tomcat-9.0-doc/config/valve.html) to the <TOMCAT_HOME>/conf/server.xml of the tomcat and add the needed fields. But beware, if the proxy does not run on the same host or the same local network you also need to configure the [trustedProxies](https://tomcat.apache.org/tomcat-9.0-doc/config/valve.html) field inside the Valve.

```xml
....
# in Server.Service.Engine.Host

      <Valve className="org.apache.catalina.valves.RemoteIpValve" remoteIpHeader="x-forwarded-for" proxiesHeader="x-forwarded-by" protocolHeader="x-forwarded-proto" portHeader="x-forwarded-port" />

        </Host>
    </Engine>
  </Service>
</Server>
``````

Here is a rudimetary ansible task to add this:

```yaml
    name: Configure Tomcat to work behind a reverse proxy
    lineinfile:
        state: present
        path: '{{ tomcat_home_dir }}/conf/server.xml'
        regexp: 'className="org.apache.catalina.valves.RemoteIpValve"'
        insertbefore: '</Host>'
        # line breaks will break the regex and therefore the task
        line: |
        <Valve className="org.apache.catalina.valves.RemoteIpValve" remoteIpHeader="x-forwarded-for" proxiesHeader="x-forwarded-by" protocolHeader="x-forwarded-proto" portHeader="x-forwarded-port" />
```
