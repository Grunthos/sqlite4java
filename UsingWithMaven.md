sqlite4java has a number of artifacts: the main artifact `com.almworks.sqlite4java:sqlite4java` is a jar file with Java classes, and one binary artifact for each target platform/architecture. You need to add dependency on the main artifact and at least one binary artifact.

Furthermore, when sqlite4java initializes, it will not be able to automatically find binary artifacts in the Maven repository, so you either need to add goals to somehow place `sqlite4java-*.jar` in the same directory with `*.dll/*.so/*.jnilib` artifacts, or pass `sqlite4java.library.path` system property to the process using sqlite4java, or use `SQLite.setLibraryPath()` as the first method you call when working with sqlite4java.

List of artifacts:
| **groupId** | **artifactId** | **type** |
|:------------|:---------------|:---------|
| com.almworks.sqlite4java | sqlite4java | jar |
| com.almworks.sqlite4java | libsqlite4java-linux-amd64 | so |
| com.almworks.sqlite4java | libsqlite4java-linux-i386 | so |
| com.almworks.sqlite4java | libsqlite4java-osx | jnilib |
| com.almworks.sqlite4java | libsqlite4java-osx-10.4 | jnilib |
| com.almworks.sqlite4java | libsqlite4java-osx-ppc | jnilib |
| com.almworks.sqlite4java | sqlite4java-win32-x64 | dll |
| com.almworks.sqlite4java | sqlite4java-win32-x86 | dll |

Sample POM:
```
<project>
  ...
  <properties>
    <sqlite4java.version>1.0.392</sqlite4java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>com.almworks.sqlite4java</groupId>
      <artifactId>sqlite4java</artifactId>
      <type>jar</type>
      <version>${sqlite4java.version}</version>
    </dependency>
    <dependency>
      <groupId>com.almworks.sqlite4java</groupId>
      <artifactId>libsqlite4java-linux-i386</artifactId>
      <type>so</type>
      <version>${sqlite4java.version}</version>
    </dependency>
  </dependencies> 

 <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
        <executions>
          <execution>
            <id>copy</id>
            <phase>compile</phase>
            <goals>
              <goal>copy</goal>
            </goals>
            <configuration>
              <artifactItems>
                <artifactItem>
                  <groupId>com.almworks.sqlite4java</groupId>
                  <artifactId>libsqlite4java-linux-i386</artifactId>
                  <version>${sqlite4java.version}</version>
                  <type>so</type>
                  <overWrite>true</overWrite>
                  <outputDirectory>${project.build.directory}/lib</outputDirectory>
                </artifactItem>
              </artifactItems>
            </configuration>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <configuration>
          <systemProperties>
            <property>
              <name>sqlite4java.library.path</name>
              <value>${project.build.directory}/lib</value>
            </property>
          </systemProperties>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>appassembler-maven-plugin</artifactId>
        <version>1.0-alpha-2</version>
        <executions>
          <execution>
            <id>assemble</id>
            <phase>package</phase>
            <goals>
              <goal>assemble</goal>
            </goals>
            <configuration>
              <extraJvmArguments>-Dsqlite4java.library.path=lib</extraJvmArguments>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>
```