# Spring Boot Launch Demo

In addition to running Spring Boot applications by using java -jar, it is also possible to make fully executable applications for Unix systems. 
A fully executable jar can be executed like any other executable binary or it can be registered with init.d or systemd. 

Details on this are described in the [Spring Boot reference docs](https://docs.spring.io/spring-boot/docs/current/reference/html/deployment.html#deployment-install).

## Using Customized Launch Script

To achieve this the Spring Boot Maven or Gradle plugins repackage the jar file and add an embedded shell script to the jar.
This default shell script is provided by Spring Boot and should run on most Unix systems without any issues.

But sometimes the script does not work on your operating system for any reason 
(i.e. using a legacy version or the shell is not compatible with the default shell script)

This project shows how to use a customized launch script to create an executable jar. The _custom-launch-script.sh_ used here 
just adds an additional line ```echoRed "My custom script is running!!!!"``` to the script to show that the executable really
uses the custom one instead of the default script.

The required steps are shown for 

* Maven based Spring Boot projects
* Gradle based Spring Boot projects

## Maven Build

To use the custom script you need to add this snippet to your maven pom to the spring boot plugin configuration section.

```
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <executable>true</executable>
        <embeddedLaunchScript>custom-launch-script.sh</embeddedLaunchScript>
    </configuration>
</plugin>
```

Now perform a ```./mvnw clean package``` and after this execute the app:

```shell script
./target/boot-launch-demo-1.0.jar
```

## Gradle Build

To use the custom script you need to add this snippet to your _build.gradle_ file.

```
bootJar {
    launchScript {
        script = file("$projectDir/custom-launch-script.sh")
    }
}
```

Now perform a ```./gradlew clean build``` and after this execute the app:

```shell script
./build/libs/boot-launch-demo-1.0.jar
``` 

### Reference Documentation
For further reference, please consider the following sections:

* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
* [Spring Boot Maven Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/maven-plugin/)
* [Spring Boot Gradle Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html/)

