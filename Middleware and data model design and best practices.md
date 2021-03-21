# 1. Middleware design and best practices
The middle design is brocken down into **presentation** (exposing the API), **business layer** (logic to perform a specific task - these are exposed to the outside world by presentation), and finally **persistence layer** these provide abstraction of the database. More details [here](https://stackoverflow.com/questions/34084203/spring-entities-should-convert-to-dto-in-service)

In view of the aforemention its necessary to look at it as having the following layers
* A persistence layer to store data
* Business layer to operate on data
* A presentation layerto expose data to the outside of the application

Therefore each layer defines its own objects as it will be discussed futher below down:

* Persistence Layer: Repositories, Entities
* Business Layer: Services, Domian Objects
* Presentation Layer: Controllers, DTOs

## 1.0. Persistence (Entity/Repository): Design thought
Think its best to start from the data (model) and group your way up to the busines and presentation layers.
### 1.1. Create Spring boot project
First head to [start.spring.io] and enter the project details
* Group name - 
* Artifact -
* Name - 
* Decription -
* Package name -
* Packaging -
* Dependencies -
### 1.2. Data Model
#### Database connection and configuration
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
 By default H2 database comes with "sa" as username and blank database, this can be changed in the application.properties file as follows:
 ```properties
  spring.datasource.url=jdbc:h2:mem:fappdb
  spring.datasource.driverClassName=org.h2.Driver
  spring.datasource.username=admin
  spring.datasource.password=p@ssw0rd
  spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
 ```    
+ Configure Database (deployment testing) - MySQL

#### How to pre-load data in the database
place a _data.sql_ file in the _src/main/resources_ with SQL table definitions like

```sql 
DROP TABLE IF EXISTS user;

CREATE TABLE user (
  id INT AUTO_INCREMENT  PRIMARY KEY,
  username VARCHAR(250) NOT NULL,
  password VARCHAR(250) NOT NULL,
  full_name VARCHAR(255) NOT NULL,
  mobile_number VARCHAR(250) DEFAULT NULL
);

INSERT INTO billionaires (username, password, full_name, mobile_number) VALUES
  ('teddy', 'p@ssw0rd', 'Teddy Nyambe', '+260977760473'),
  ('sharon','p@ssw0rd', 'Sharon Nyambe', '+26097777777'),
  ('edward','p@ssw0rd', 'Edward Lubasi', '+2766879900');
```
when you restart the project look out for this line

```log
H2 console available at '/h2-console'. Database available at 'jdbc:h2:mem:fappdb'
```
#### View database and data - Via h2 web console
Head out to the browser and point the browser to https://localhost:[port]/h1-console. Note the database name, username and passord entered in the properties file

### 1.3. Entity (Persistence Layer)
#### JPA/
Good practice to add it under package name entity, why? [here](https://stackoverflow.com/questions/8743995/what-is-difference-between-a-model-and-an-entity)

```java
@Entity
public class User {
    @Id
    private Long id;
    private String username;
    private String password;
    @Column(name = "full_name")
    private String fullName;
    @Column(name = "mobile_phone")
    private String mobilePhone;

    public User(Long id, String username, String password, String fullName, String mobilePhone) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.fullName = fullName;
        this.mobilePhone = mobilePhone;
    }

    public User() {
    }
    // Setters & Getters
 }
```
### 1.4. Repository (Persistence Layer)
Define a repository. It will do all magic with the database. A number of methods are abstracted and available for use. But custom methods can be defined.

```java
public interface UserRepository extends JpaRepository<User, Long> {

}
```
#### Custome queries
In case there is need to perform queries not provided by the repository, system has flexibility to define custom quries in the repository as follows:

```java
TO-DO
```

The way the Enity and Repository have been defined, they automatically communicate with the underlying database server. 

## 2.0. Business Layer


