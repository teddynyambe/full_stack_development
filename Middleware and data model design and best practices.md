# 1. Middleware design and best practices
The middle design is brocken down into **presentation** (exposing the API), **business layer** (logic to perform a specific task - these are exposed to the outside world by presentation), and finally **persistence layer** these provide abstraction of the database. More details [here](https://stackoverflow.com/questions/34084203/spring-entities-should-convert-to-dto-in-service)

In view of the aforemention its necessary to look at it as having the following layers
* A persistence layer to store data
* Business layer to operate on data
* A presentation layerto expose data to the outside of the application

Therefore each layer defines its own objects as it will be discussed futher below down:

* Persistence Layer: Repositories, Entities
* Business Layer: Services, Domain Objects
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
The busines layer models the business domain under consideration, it has services representing the business logic. It includes manipulation of the persitence layer that are masked using data transfer models (DTOs). The shared DTOs are used to transfer data between the business model and the presentation model. Normally there will be a lot of data conversion between the business layer and the presentation layer. This practically mean that data managed in the business layer will be entities from the persistence there. It is not advisable to work directly with entities in the business layer. Therefore entities data need need to be transfered to DTOs. This can easily be achieved through the use of the model mapper. Spring provides an implemenation of the model matter by adding its dependency as follows:

```xml
<dependency>
	<groupId>org.modelmapper</groupId>
	<artifactId>modelmapper</artifactId>
	<version>2.3.8</version>
</dependency>
```
The typical use case and best practice for model mapper is to provide for it globally as a bean. This can be done under a utility class annotated as a @component as follows:
```java
@Component
public class StartUps {
    @Bean
    ModelMapper modelMapper() {
        return new ModelMapper();
    }
}
```

This makes it available in the container at statrup. It can be used in any class for ease conversion as follows:
```java
@Service
public class UserRegistrationServiceImpl implements UserRegistrationService{
    ModelMapper modelMapper; 
    UserRegistrationServiceImpl(ModelMapper modelMapper){ // model mapper bean injected here
        this.modelMapper = modelMapper;
        this.userRepository = userRepository;
    }

    @Override
    public UserDTO createUser(NewUserDTO newUserDTO) {
        User user = modelMapper.map(newUserDTO, User.class); // Usage
        user.setUserId(UUID.randomUUID().toString());
        user.setEncryptedPasswoprd("Testing");
        User savedUser = userRepository.save(user);

        return modelMapper.map(savedUser, UserDTO.class); // Usage
    }
  }
```

The interface decribing the contract
```java
public interface UserRegistrationService {
    UserDTO createUser(NewUserDTO newUserDTO);
    UserDTO findByUsername(String username);
}
```

Concrete class showing the implementation

```java
@Service
public class UserRegistrationServiceImpl implements UserRegistrationService{
    ModelMapper modelMapper;
    UserRepository userRepository;
    UserRegistrationServiceImpl(ModelMapper modelMapper, UserRepository userRepository){
        this.modelMapper = modelMapper;
        this.userRepository = userRepository;
    }

    @Override
    public UserDTO createUser(NewUserDTO newUserDTO) {
        User user = modelMapper.map(newUserDTO, User.class);
        user.setUserId(UUID.randomUUID().toString());
        user.setEncryptedPasswoprd("Testing");
        User savedUser = userRepository.save(user);
        return modelMapper.map(savedUser, UserDTO.class);
    }

    @Override
    public UserDTO findByUsername(String username) {
        User userByUsername = userRepository.findByUsername(username);
        if(userByUsername == null)
            return null;
        return modelMapper.map(userByUsername, UserDTO.class);
    }
}
```
## 3.0. Application/Presentation Layer
This layer should abstract the business and the peresistence layer to the viewer of the application, here data should be hidden as much as possible, only exposing what is necessary through the use of data models and shared data transfer objects.

The controller class is a s follows:

```java
@RestController
@RequestMapping("/user")
public class Auth {
    private RegisterUserService registerUserService;
    private final ModelMapper modelMapper;

    public Auth(RegisterUserService registerUserService, ModelMapper modelMapper) {
        this.registerUserService = registerUserService;
        this.modelMapper = modelMapper;
    }

    @PostMapping
    public CreatedUserDTO ceate(@RequestBody NewUserDTO newUserDTO) {
        User user = registerUserService.addNew(dtoToEntity(newUserDTO));
        CreatedUserDTO createdUserDTO = entityToDto(user);
        createdUserDTO.setCreationStatus(true);
        return createdUserDTO;
    }

    private CreatedUserDTO entityToDto(User user) {
        return modelMapper.map(user, CreatedUserDTO.class);
    }

    private User dtoToEntity(NewUserDTO newUserDTO) {
        return modelMapper.map(newUserDTO, User.class);
    }
}
```
### What makes a controller a REST service
The above controller is annotated by @RestController indicating to the spring framework that the annoted class runs as a REST service during runtime. 

* @RestController => @Controller and @RequestMapping
* @Controller => @Component and includies capabilities of being scanned to include client presentation service via @RequestMapping
* @GetMapping is a shortcut for @RequestMapping(method = RequestMethod.GET)
* @ResponseBody serializes a return object with JSON before being returned
* @RestController => @Controller and @ResponseBody, @ResponseBody has now become redunant.


### 3.1. Exception Handline
Many thoughts are available on how to handle exceptions. I am currently handling exceptions in the controller proecting the business layer and the persistence. I am using the @ControllerAdvise which is handling errors across controllers. Custome error handlers are defined with overrideing spring exception handling mechanism. 

#### Overide messga structure
Agree on business or domain area structure of what your error structure should look like and represent it in class model as follows:

```java
public class FAPPExceptionResponse {
    private  Date timestamp;
    private String msg;
    private String details;

    public FAPPExceptionResponse(Date timestamp, String message, String details) {
        super();
        this.timestamp = timestamp;
        this.msg = message;
        this.details = details;
	
/* getters */

}
```
Then then create anticipated error classes that will handle specific excptions in the application. Say for instance user not found scenerio

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class FAPPUserNotFoundException extends RuntimeException {
    public FAPPUserNotFoundException(String message) {
        super(message);
    }
}
```
This class will be used in a controller to throw an exception once a user is not found as follows:

```java
    @GetMapping(/{id})
    public ResponseEntity<CreatedUserModel> findUser(@PathVariable String id) {
        UserDTO userDto = userRegistrationService.findById(id);
    
        if(userDto == null) {
           // Code here
        } else {
            throw new FAPPUserNotFoundException("User not found");
        }
    }
```
To glue the above configuration and setup, override the defualt spring reaction to exception so that custome error structure and handler setup above can be invoked instead of the defualt spring exception scheme. The glue is as follows:

```java
@ControllerAdvice //This informs all the controllers to use this for exception response
@RestController //Its a rest controller and will send exceptions to clients
public class FAAPCustomizedResponseEntityExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(FAPPUserNotFoundException.class) //Exception handler
    public final ResponseEntity<Object> handleAllExceptions(Exception e, WebRequest request) {
        FAPPExceptionResponse exceptionResponse = new FAPPExceptionResponse(new Date(), e.getMessage(), request.getDescription(false)); //Specicify exception response structure
        return new ResponseEntity<>(exceptionResponse, HttpStatus.NOT_FOUND);
    }
}
```

