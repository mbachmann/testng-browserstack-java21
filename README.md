# testng-browserstack-java21


[TestNG](http://testng.org) Integration with BrowserStack.


## Using Maven

### Run sample build

- Clone the repository
- Replace YOUR_USERNAME and YOUR_ACCESS_KEY with your BrowserStack access credentials in browserstack.yml.
- Install dependencies `mvn compile`
- To run the test suite having cross-platform with parallelization, run `mvn test -P sample-test`
- To run local tests, run `mvn test -P sample-local-test`

For local tests install the browserstack app: https://www.browserstack.com/docs/live/local-testing/set-up-local-testing 

Understand how many parallel sessions you need by using our [Parallel Test Calculator](https://www.browserstack.com/automate/parallel-calculator?ref=github)

### Integrate your test suite

This repository uses the BrowserStack SDK to run tests on BrowserStack. Follow the steps below to install the SDK in your test suite and run tests on BrowserStack:

* Create sample browserstack.yml file with the browserstack related capabilities with your [BrowserStack Username and Access Key](https://www.browserstack.com/accounts/settings)
  and place it in your root folder: [browserstack.yml](browserstack.yml)
* Add maven dependency of browserstack-java-sdk in your pom.xml file
* 
```xml
<dependency>
    <groupId>com.browserstack</groupId>
    <artifactId>browserstack-java-sdk</artifactId>
    <version>${browserstack.version}</version>
    <scope>compile</scope>
</dependency>
```

Adjust the version of the SDK in the pom.xml file to the latest version available on the [Maven Repository](https://mvnrepository.com/artifact/com.browserstack/browserstack-java-sdk)

```xml
<properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <testng.version>7.11.0</testng.version>
        <surefire.version>3.5.2</surefire.version>
        <selenium.version>4.29.0</selenium.version>
        <json-simple.version>1.1.1</json-simple.version>
        <browserstack.version>1.30.7</browserstack.version>
        <config.file>config/sample-test.testng.xml</config.file>
</properties>
```

* Modify your build plugin to run tests by adding argLine `-javaagent:${settings.localRepository}/com/browserstack/browserstack-java-sdk/${browserstack.version}/browserstack-java-sdk-${browserstack.version}.jar` and `maven-dependency-plugin` for resolving dependencies in the profiles `sample-test` and `sample-local-test`.
```xml
            <plugin>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>getClasspathFilenames</id>
                        <goals>
                            <goal>properties</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>${surefire.version}</version>
                <configuration>
                    <suiteXmlFiles>
                        <suiteXmlFile>${config.file}</suiteXmlFile>
                    </suiteXmlFiles>
                    <argLine>
                        <!-- Add BrowserStack Java SDK as a Java agent -->
                        <!-- C:\Users\MBach\.m2\repository\com\browserstack\browserstack-java-sdk\1.30.7\browserstack-java-sdk-1.30.7.jar -->
                        -javaagent:${settings.localRepository}/com/browserstack/browserstack-java-sdk/${browserstack.version}/browserstack-java-sdk-${browserstack.version}.jar
                    </argLine>
                    <forkCount>1</forkCount>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.14.0</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                </configuration>
            </plugin>
```
* Install dependencies `mvn compile`


```yaml


## Notes
* You can view your test results on the [BrowserStack Automate dashboard](https://www.browserstack.com/automate)
