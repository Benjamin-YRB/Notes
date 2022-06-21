# Spring5整合Log4j2

1. 引入jar包
   1. <!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
      <dependency>
          <groupId>org.apache.logging.log4j</groupId>
          <artifactId>log4j-core</artifactId>
          <version>2.13.3</version>
      </dependency>
   2. <!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-api -->
      <dependency>
          <groupId>org.apache.logging.log4j</groupId>
          <artifactId>log4j-api</artifactId>
          <version>2.13.3</version>
      </dependency>
   3. <!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-slf4j-impl -->
      <dependency>
          <groupId>org.apache.logging.log4j</groupId>
          <artifactId>log4j-slf4j-impl</artifactId>
          <version>2.13.3</version>
          <scope>test</scope>
      </dependency>
   4. <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
      <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-api</artifactId>
          <version>1.7.30</version>
      </dependency>
2. 创建log4j2.xml配置文件

