# Gradle

## local repository

Gradle stores dependecies under ~/.gradle/caches. 

If uses ~/.m2 if you configure:

```groovy
repositories {
    mavenLocal()
}
```

## on macos m2

Getting the following error when running `./gradlew bootBuildImage` on apple m2: `Connection to the Docker daemon at 'localhost' failed with error "[2] No such file or directory"; ensure the Docker daemon is running and accessible`

[Solution](https://stackoverflow.com/questions/71618429/building-a-docker-image-with-the-spring-boot-gradle-plugin-and-colima)
