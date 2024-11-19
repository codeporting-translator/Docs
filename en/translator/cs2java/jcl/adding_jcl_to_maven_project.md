## Adding CodePorting.Translator JCL to the Maven Project
You can easily add JCL directly to your Maven project with simple configurations.
### 1. Specifying Maven Repository Configuration
First, you need to specify the CodePorting Maven Repository configuration/location in your Maven pom.xml as follows:
```xml
<repositories>
    <repository>
        <id>codeporting</id>
        <name>CodePorting Maven Repository</name>
        <url>https://products.codeporting.com/translator/csharp-to-java/repo/</url>
    </repository>
</repositories>
```
### 2. Defining CodePorting.Translator JCL Dependency
Then, based on the namespaces used in the original C# project, define CodePorting.Translator JCL dependencies in your pom.xml as follows:
```xml
<dependencies>
    <dependency>
        <groupId>com.aspose.ms.jdk.NetFramework</groupId>
        <artifactId>mscorlib</artifactId>
        <version>24.7.0</version>
    </dependency>

    <dependency>
        <groupId>com.aspose.ms.jdk.NetFramework</groupId>
        <artifactId>System</artifactId>
        <version>24.7.0</version>
    </dependency>

    <dependency>
        <groupId>com.aspose.ms.jdk.NetFramework</groupId>
        <artifactId>System.Security</artifactId>
        <version>24.7.0</version>
    </dependency>

    <dependency>
        <groupId>com.aspose.ms.jdk.NetFramework</groupId>
        <artifactId>System.Xml</artifactId>
        <version>24.7.0</version>
    </dependency>

    <dependency>
        <groupId>com.aspose.ms.jdk.NetFramework</groupId>
        <artifactId>System.Net</artifactId>
        <version>24.7.0</version>
    </dependency>

    <dependency>
        <groupId>com.aspose.ms.jdk.NetFramework</groupId>
        <artifactId>System.Drawing</artifactId>
        <version>24.7.0</version>
    </dependency>
</dependencies>
```
