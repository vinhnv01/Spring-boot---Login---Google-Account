<p align="center">
 <img width="300px" src="https://www.trustedreviews.com/wp-content/uploads/sites/54/2022/04/How-to-make-google-account-920x613.jpg" align="center" alt="GitHub Readme Stats" />
 <h2 align="center">Spring boot - Login với Google Account</h2>
</p>

Thông qua hướng dẫn Spring Boot này, bạn sẽ học cách triển khai chức năng đăng nhập một lần bằng tài khoản Google cho ứng dụng web Spring Boot hiện có, sử dụng thư viện Spring OAuth2 Client - cho phép người dùng cuối đăng nhập bằng tài khoản Google của chính họ thay vì thông tin đăng nhập do ứng dụng quản lý .
Giả sử rằng bạn có một dự án Spring Boot hiện có với chức năng xác thực đã được triển khai bằng Spring Security và thông tin người dùng được lưu trữ trong cơ sở dữ liệu MySQL (Nếu không, hãy tải xuống dự án mẫu trong hướng dẫn này ).<br>
Sau đó, chúng tôi sẽ cập nhật trang đăng nhập cho phép người dùng đăng nhập bằng tài khoản Google của chính họ như sau:<br> bạc.

<div align="center">
 <img width="600px" src="https://www.codejava.net/images/articles/frameworks/springboot/oauth-google/Login_Google.png" align="center" alt="GitHub Readme Stats" />
</div>

# 1. Tạo thông tin đăng nhập Google OAuth

Trước tiên, hãy làm theo video này để tạo ID ứng dụng khách Google OAuth nhằm nhận các khóa truy cập của Google đăng nhập một lần trên API (ID ứng dụng khách và bí mật ứng dụng khách). Lưu ý rằng bạn cần thêm một URI chuyển hướng được ủy quyền như sau:
<br> <b> <i> http: // localhost: 8080 / login / oauth2 / code / google </b> </i> 
<br> Trong trường hợp ứng dụng của bạn được lưu trữ với đường dẫn ngữ cảnh riêng, ví dụ / Shopme - thì hãy chỉ định URI chuyển hướng như sau:
<br> <b> <i> http: // localhost: 8080 / Shopme / login / oauth2 / code / google </b> </i> 

# 2. Khai báo sự phụ thuộc cho ứng dụng khách Spring Boot OAuth2
Bên cạnh phần phụ thuộc Spring Security, bạn cần thêm một phần phụ thuộc mới vào tệp dự án Maven để sử dụng Spring Boot OAuth2 Client API giúp đơn giản hóa đáng kể việc tích hợp một lần cho các ứng dụng Spring Boot.<br> <br>
Vì vậy, hãy khai báo phụ thuộc sau: <br>
```md
        <!-- https://mvnrepository.com/artifact/org.springframework.security.oauth.boot/spring-security-oauth2-autoconfigure -->
        <dependency>
            <groupId>org.springframework.security.oauth.boot</groupId>
            <artifactId>spring-security-oauth2-autoconfigure</artifactId>
            <version>2.1.3.RELEASE</version>
        </dependency>
```
 # 3. Định cấu hình thuộc tính Spring OAuth2 cho Google
 
 Tiếp theo, mở tệp cấu hình Spring Boot ( application.yml ) và chỉ định các thuộc tính cho đăng ký Máy khách OAuth2 cho nhà cung cấp có tên google, như sau: <br>
  <h3> &nbsp; 3.1 Tạo dự án Spring Boot </h3> 
  <h3> &nbsp; Trên Eclipse, tạo một dự án Spring Boot : </h3>
   <img width="500px" src="https://s1.o7planning.com/vi/11823/images/17226826.png" align="center" alt="GitHub Readme Stats" /> <br>
  <h3> &nbsp; Cơ sở dữ liệu sử dụng trong ứng dụng này là MySQL, SQL Server, PostGres hoặc Oracle hoặc bất kỳ một cơ sở dữ liệu nào khác mà bạn quen thuộc. </h3> <br>
   <img width="500px" src="https://s1.o7planning.com/vi/11823/images/17226880.png" align="center" alt="GitHub Readme Stats" /> <br>
  <h3> &nbsp; Khai báo các thư viện Spring Social vào project của bạn: </h3>
    <div class="df-fragment df-cktext-default">
    <ol>
      <li>spring-security-oauth2</li>
      <li>spring-social-core</li>
      <li>spring-social-config</li>
      <li>spring-social-security</li>
      <li>spring-social-facebook</li>
      <li>spring-social-twitter</li>
      <li>spring-social-github</li>
      <li>spring-social-linkedin</li>
      <li>spring-social-google</li>
 </ol>

</div>
<h3> &nbsp; Nội dung đầy đủ của tập tin pom.xml: </h3><br>
<h4> &nbsp; pom.xml </h4> <br>

```md
<pre><code class="language-xml hljs">
<span class="hljs-meta">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">project</span> <span class="hljs-attr">xmlns</span>=<span class="hljs-string">"http://maven.apache.org/POM/4.0.0"</span>
    <span class="hljs-attr">xmlns:xsi</span>=<span class="hljs-string">"http://www.w3.org/2001/XMLSchema-instance"</span>
    <span class="hljs-attr">xsi:schemaLocation</span>=<span class="hljs-string">"http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">modelVersion</span>&gt;</span>4.0.0<span class="hljs-tag">&lt;/<span class="hljs-name">modelVersion</span>&gt;</span>
  <groupId>org.o7planning</groupId>
    <artifactId>SpringBootSocialJPA</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>SpringBootSocialJPA</name>
    <description>Spring Boot + Oauth2 Social + JPA</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.0.RELEASE</version>
        <relativePath /> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>


        <!-- http://mvnrepository.com/artifact/org.springframework.security/spring-security-config -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.security.oauth/spring-security-oauth2 -->
        <dependency>
            <groupId>org.springframework.security.oauth</groupId>
            <artifactId>spring-security-oauth2</artifactId>
            <version>2.2.1.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-core -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-core</artifactId>
            <version>1.1.5.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-config -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-config</artifactId>
            <version>1.1.5.RELEASE</version>
        </dependency>
 

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-web -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-web</artifactId>
            <version>1.1.5.RELEASE</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-security -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-security</artifactId>
            <version>1.1.5.RELEASE</version>
        </dependency>



        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-facebook -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-facebook</artifactId>
            <version>2.0.3.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-twitter -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-twitter</artifactId>
            <version>1.1.2.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-github -->
        <!-- <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-github</artifactId>
            <version>1.0.0.M4</version>
        </dependency> -->

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-linkedin -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-linkedin</artifactId>
            <version>1.0.2.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.social/spring-social-google -->
        <dependency>
            <groupId>org.springframework.social</groupId>
            <artifactId>spring-social-google</artifactId>
            <version>1.0.0.RELEASE</version>
        </dependency>


        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>com.oracle</groupId>
            <artifactId>ojdbc6</artifactId>
            <version>11.2.0.3</version>
        </dependency>

        <!-- SQL Server Mssql-Jdbc Driver -->
        <dependency>
            <groupId>com.microsoft.sqlserver</groupId>
            <artifactId>mssql-jdbc</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- SQL Server JTDS Driver -->
        <!-- https://mvnrepository.com/artifact/net.sourceforge.jtds/jtds -->
        <dependency>
            <groupId>net.sourceforge.jtds</groupId>
            <artifactId>jtds</artifactId>
            <version>1.3.1</version>
        </dependency>


        <!-- Email validator,... -->
        <!-- http://mvnrepository.com/artifact/commons-validator/commons-validator%20 -->
        <dependency>
            <groupId>commons-validator</groupId>
            <artifactId>commons-validator</artifactId>
            <version>1.5.0</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <repositories>
        <!-- Repository for ORACLE JDBC Driver -->
        <repository>
            <id>codelds</id>
            <url>https://code.lds.org/nexus/content/groups/main-repo</url>
        </repository>
        
    </repositories>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>    
     
<span class="hljs-tag">&lt;/<span class="hljs-name">project</span>&gt;</span>
</code></pre>
```
  <div>  &nbsp; <h3>SpringBootSocialJpaApplication.java</h3></div>
 <img width="600px" src="https://user.oc-static.com/upload/2019/11/21/15742940638228_pasted%20image%200%20%2810%29.png" align="center" alt="GitHub Readme Stats" />
<h3> &nbsp; 3.2 Cấu hình DataSource </h3> 
<h4> &nbsp; Các thông tin về cơ sở dữ liệu cần được cấu hình trong tập tin application.properties: </h4> 
<h4> &nbsp; &nbsp; 3.2.1 : application.properties (MySQL) </h4> <br>

```md

        spring.thymeleaf.cache=false
        # ===============================
        # DATABASE
        # ===============================

        spring.datasource.driver-class-name=com.mysql.jdbc.Driver
        spring.datasource.url=jdbc:mysql://localhost:3306/social
        spring.datasource.username=root
        spring.datasource.password=12345

        # ===============================
        # JPA / HIBERNATE
        # ===============================

        spring.jpa.show-sql=true
        spring.jpa.hibernate.ddl-auto=update
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
```
<br>
<h4> &nbsp;  3.2.2 : application.properties (Mssql-Jdbc Driver) </h4> <br>
        
```md
     
        spring.thymeleaf.cache=false      
        # ===============================
        # DATABASE
        # ===============================

        spring.datasource.driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver
        spring.datasource.url=jdbc:sqlserver://tran-vmware-pc\\SQLEXPRESS:1433;databaseName=social
        spring.datasource.username=sa
        spring.datasource.password=12345


        # ===============================
        # JPA / HIBERNATE
        # ===============================

        spring.jpa.show-sql=true
        spring.jpa.hibernate.ddl-auto=update
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.SQLServer2012Dialect
```

<br> 
<h4>  &nbsp; 3.2.3 : application.properties (PostGres) </h4><br>
 
```md
        spring.thymeleaf.cache=false

        # ===============================
        # DATABASE CONNECTION
        # ===============================

        spring.datasource.driver-class-name=org.postgresql.Driver
        spring.datasource.url=jdbc:postgresql://tran-vmware-pc:5432/social
        spring.datasource.username=postgres
        spring.datasource.password=12345

        # ===============================
        # JPA / HIBERNATE
        # ===============================

        spring.jpa.show-sql=true
        spring.jpa.hibernate.ddl-auto=update
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect


        # Fix Postgres JPA Error:
        # Method org.postgresql.jdbc.PgConnection.createClob() is not yet implemented.
        spring.jpa.properties.hibernate.temp.use_jdbc_metadata_defaults=false
```

<br>
<h4> &nbsp; 3.2.4 : application.properties (Oracle) </h4> <br>
        
```md
     
        spring.thymeleaf.cache=false

        # ===============================
        # DATABASE
        # ===============================

        spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver

        spring.datasource.url=jdbc:oracle:thin:@localhost:1521:db12c
        spring.datasource.username=social
        spring.datasource.password=12345


        # ===============================
        # JPA / HIBERNATE
        # ===============================

        spring.jpa.show-sql=true
        spring.jpa.hibernate.ddl-auto=update
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.Oracle10gDialect
```
<br> 

   <h3> &nbsp; 3.3  Cấu hình Security & Spring Social </h3> 
   <br> <img width="700px" src="https://s1.o7planning.com/vi/11823/images/17246902.png" align="center" alt="GitHub Readme Stats" />
  
  # 4.  Cập nhật lớp thực thể người dùng và bảng người dùng  <br>
 
   
   
   
   
