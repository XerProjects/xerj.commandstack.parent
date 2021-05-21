# Xer Projects Parent POM 

# Usage

Define as parent to project's pom.xml:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>io.github.xerprojects</groupId>
        <artifactId>xerj.parent</artifactId>
        <version>...</version>
    </parent>
    ...
</project>
```

# Profiles
- ## snapshots
    Allows maven to download packages from the snapshots repository:
    https://oss.sonatype.org/content/repositories/snapshots

    **Usage:** 
    - `mvn verify -Psnapshots`

- ## code-coverage
    Enables jacoco code coverage and reports for unit tests.  
    
    By default, jacoco will require a minimum of 80% coverage. This san be configured by `jacoco.instruction.coverage` property.

    **Usage:** 
    - `mvn verify -Pcode-coverage`
    - `mvn verify -Pcode-coverage -Djacoco.instruction.coverage=0.50`
        - This will override coverage requirement to 50%.

    **Note:**  
    If a child pom needs to define a custom `argLine` configuration in `maven-surefire-plugin`, it will need to add a `@{surefire.jacoco.args}` the the `argLine` in order for jacoco to work correctly. Otherwise, jacoco should just work out of the box.  
    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <configuration>
            <argLine>
                @{surefire.jacoco.args}
                ...
            </argLine>
        </configuration>
    </plugin>
    ```

- ## integration-test
    Enable maven-failsafe-plugin integration test and jacoco reports.

    **Usage:** 
    - `mvn verify -Pintegration-test`

    **Note:**  
    If a child pom needs to define a custom `argLine` configuration in `maven-failsafe-plugin`, it will need to add a `@{failsafe.jacoco.args}` to the `argLine` in order for jacoco to work correctly. Otherwise, jacoco should just work out of the box.  
    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-failsafe-plugin</artifactId>
        <configuration>
            <argLine>
                @{failsafe.jacoco.args}
                ...
            </argLine>
        </configuration>
    </plugin>
    ```

- ## release
    Enables release plugins such as:
    - maven-source-plugin
    - maven-javadocs-plugin
    - maven-gpg-plugin

    **Usage:** 
    - `mvn release:clean release:prepare release:perform -Prelease`

## Note:
Profiles can be combined such as:  

- `mvn clean verify -Pcode-coverage,integration-test` 
    - this will run both code coverage and integration tests for the project. It will also generate jacoco coverage reports.