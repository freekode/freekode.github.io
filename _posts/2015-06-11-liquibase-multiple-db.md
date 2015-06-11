---
layout: post
title: Liquibase, multiple databases
---

I have several databases and would like to update them in one command.

Directory structure:
{% highlight text %}
pom.xml
database/
    first/
        changelog-master.xml
        dev.properties
    second/
        changelog-master.xml
        dev.properties
{% endhighlight %}

dev.properties contains connection parameters:
{% highlight text %}
driver=org.postgresql.Driver
url=jdbc:postgresql://127.0.0.1/first
username=test
password=test
{% endhighlight %}

I don't know why, but here we can't write path to changelog file, so we write it in pom.xml

Maven config looks like that:
{% highlight xml %}
<plugin>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-maven-plugin</artifactId>
    <version>3.3.5</version>
    <executions>
        <execution>
            <id>first</id>
            <phase>process-resources</phase>
            <configuration>
                <propertyFile>database/first/dev.properties</propertyFile>
                <changeLogFile>database/first/changelog-master.xml</changeLogFile>
            </configuration>
            <goals>
                <goal>update</goal>
            </goals>
        </execution>
        <execution>
            <id>second</id>
            <phase>process-resources</phase>
            <configuration>
                <propertyFile>database/second/dev.properties</propertyFile>
                <changeLogFile>database/second/changelog-master.xml</changeLogFile>
            </configuration>
            <goals>
                <goal>update</goal>
            </goals>
        </execution>
    </executions>
    <dependencies>
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>9.3-1103-jdbc41</version>
        </dependency>
    </dependencies>
</plugin>
{% endhighlight %}


And...
{% highlight text %}
$ mvn install
{% endhighlight %}

#### Conclusion

Liquibase very weird product, xml config, many problems with relative path and so on. In databasechangelog table there is filename field, this field contains not filename, it contains relative path to changelog. And if you move this file to another folder without any changes, liquibase will try to apply it, just because your changelog in another folder. To avoid it use `logicalFilePath="<file name>"` parameter in `<changeSet>` tag.

I recommend to use [flyway](http://flywaydb.org) migration system. Liquibase have only one feature - supporting multiple database engines.
