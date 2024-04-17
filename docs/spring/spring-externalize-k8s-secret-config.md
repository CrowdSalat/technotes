# Load mounted secrets as spring boot properties

## motivation

Sometimes it is forbidden by platform or security teams to consume secrets as environments varibales (which would be the easiest way).
Mounting secrets is generally considered (a little bit) more secure than using them as environmental variables in Kubernetes. Here's why:

Environmental Variables:

- **Exposure in Logs**: When an application logs information about its environment, which can be common during debugging or errors, any secret values set as environment variables could be accidentally leaked.
- **Process Visibility**: Any process running within the pod can potentially access environment variables, which could be a security risk if a process has elevated privileges.

Mounted Secrets:

**Limited Access**: Secrets mounted as files are typically only accessible to the application process itself, reducing the risk of unauthorized access.
**No Accidental Leaks**: Since the secret value isn't directly embedded in the application code or configuration, accidental leaks through logging or code exposure are less.

## solutions utilizing secrets

You can test the following solutions in the [PoprertyLogger projekt inside your boilerplate repo](https://github.com/CrowdSalat/boilerplate/tree/main/springboot/PropertyLogger).

You got the following secrets:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-properties
stringData:
  spring.datasource.url: jdbc://bla.de
  password.super: secret###########
---
apiVersion: v1
kind: Secret
metadata:
  name: secret-properties-2
stringData:
  second_secret_entry: also_secret
```

### Import with k spring.cloud.kubernetes.secrets.paths

noteworthy:

- The spring boot application needs the following dependency: `org.springframework.cloud:spring-cloud-starter-kubernetes-client-config`
- `spring.cloud.kubernetes.secrets.paths` searches recursivly so you can create one mountpoint where every secret is mounted

Example yaml and conf:

```yaml
# e.g. Deplyoment
#...
          env:
             # to prevent automatic config map discovery and stacktraces
            - name: SPRING_CLOUD_KUBERNETES_CONFIG_ENABLED
              value: "false"
            # searches recursivly
            - name: SPRING_CLOUD_KUBERNETES_SECRETS_PATHS
              value: /app/secrets/
          volumeMounts:
            # this create one file per secret entry
            - name: secret-properties
              mountPath: /app/secrets/firstsecret/
            - name: secret-properties-2
              mountPath: /app/secrets/secondsecret/
      volumes:
        - name: secret-properties
          secret:
            secretName: secret-properties
        - name: secret-properties-2
          secret:
            secretName: secret-properties-2
```

### Import with with spring.config.import=configtree:/

noteworthy:

- runs out of the box
- you need to map every secret entry to a path when defining `volumes:`


```yaml
# e.g. Deplyoment
#...
            env:
              - name: SPRING_CONFIG_IMPORT
                # use * to use all directories on this level and start searching one layer deeper 
                value: "optional:configtree:/app/config/*/"
            volumeMounts:
              - mountPath: /app/config/firstsecret/
                name: secret-properties
              - mountPath: /app/config/secondsecret/
                name: secret-properties-2
        volumes:
        - name: secret-properties
            secret:
            secretName: secret-properties
            items:
                # use same property
              - key: spring.datasource.url
                path: spring/datasource/url
                # remap property
              - key: password.super
                path: spring/datasource/password
        - name: secret-properties-2
            secret:
            secretName: secret-properties-2
            items:
              # remap property
              - key: second_secret_entry
                path: my/own/property
```

### Addtional application.yaml with spring.config.additional-location

TODO

### programmatically with @ConfigurationProperties

TODO

## alternatives without secrets 

### spring cloud config server

TODO but my own expirience with this is approach is bulky.

### ?

TODO what other mechanismn/operators are there?
