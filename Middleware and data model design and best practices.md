# 1. Middleware design and best practices
The middle design is brocken down into **controllers** (exposing the API), **services** (business logic to perform a specific task - these are exposed to the outside world by controllers, **repositories** these provide abstraction of the database (**model**).

## 1.1. Design thought
Think its best to start from the data (model) and group your way up to the controllers.
## 1.2. Create Spring boot project
First head to [start.spring.io] and enter the project details
* Group name - 
* Artifact -
* Name - 
* Decription -
* Package name -
* Packaging -
* Dependencies -
## 1.3. Data Model
### Database connection and configuration
During design/testing start with in-memory database such as h2, however after development you can now test the deployment with server based database like mysql. H2 can also be configured to be a server based database.

* Adding h2 to spring-boot project
+ Add dependency in pom.xml file of your project
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```
+ Configure Database (design and testing) - H2
 By default H2 database comes with "sa" as username and blank database, this can be changed in the properties file as follows:
 ```properties
  spring.datasource.url=jdbc:h2:mem:myappdb
  spring.datasource.driverClassName=org.h2.Driver
  spring.datasource.username=admin
  spring.datasource.password=p@ssw0rd
  spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
 ```
+ Configure Database (deployment testing) - MySQL


  
